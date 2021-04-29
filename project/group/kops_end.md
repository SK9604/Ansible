1. **클러스터 생성**

    ```bash
    ~$ export NAME=mycluster.k8s.local
    ~$ export KOPS_STATE_STORE=s3://sk-k8s-bucket

    ~$ kops create cluster \
      --zones=ap-northeast-2c \
      --state s3://sk-k8s-bucket \
      --master-zones=ap-northeast-2c \
      --node-count=2 \
      --node-size=t3a.large \
      --node-volume-size=8 \
      --master-count=1 \
      --master-size=t3a.large \
      --master-volume-size=8 \
      --topology=public \
      --api-loadbalancer-type=public \
      --image='ami-021fbe1c44132d837' \
      --networking=calico \
      --cloud-labels "Owner=SK9604" \
      --ssh-public-key ./id_rsa.pub \
      $NAME

    ~$ kops update cluster --yes $NAME
    ~$ kops validate cluster
    Using cluster from kubectl context: mycluster.k8s.local

    Validating cluster mycluster.k8s.local

    INSTANCE GROUPS
    NAME                    ROLE    MACHINETYPE     MIN     MAX     SUBNETS
    master-ap-northeast-2c  Master  t3a.large       1       1       ap-northeast-2c
    nodes                   Node    t3a.large       2       2       ap-northeast-2c

    NODE STATUS
    NAME                                                    ROLE    READY
    ip-172-20-33-3.ap-northeast-2.compute.internal          node    True
    ip-172-20-43-19.ap-northeast-2.compute.internal         master  True
    ip-172-20-52-149.ap-northeast-2.compute.internal        node    True

    Your cluster mycluster.k8s.local is ready
    ```

2. **VPC 엔드포인트 생성**

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0244d88d-df59-421e-8d97-a07745dcf2aa/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0244d88d-df59-421e-8d97-a07745dcf2aa/Untitled.png)

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5f2ad47c-10fd-461f-bb35-3efd094a387d/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5f2ad47c-10fd-461f-bb35-3efd094a387d/Untitled.png)

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a410168d-4860-4322-aa08-c374d99e8562/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a410168d-4860-4322-aa08-c374d99e8562/Untitled.png)

3. **YAML 파일 수정**

    deploymentwebapp.yml

    ```bash
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my-webapp-deployment
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: my-webapp
      template:
        metadata:
          name: urmywebapp
          labels:
            app: my-webapp
        spec:
          containers:
          - name: urmywebapp
            image: 778893145182.dkr.ecr.ap-northeast-2.amazonaws.com/skecr:1.0
            ports:
            - containerPort: 5000
              name: flask-port
          imagePullSecrets:
          - name: regcred
    ```

    app-svc.yml

    ```bash
    apiVersion: v1
    kind: Service
    metadata:
      name: urmywebapp-svc-clusterip
    spec:
      ports:
        - name: webapp-port
          port: 80
          targetPort: flask-port
      selector:
        app: my-webapp
      type: ClusterIP
    ```

    ingress.yml

    ```bash
    apiVersion: networking.k8s.io/v1beta1
    kind: Ingress
    metadata:
      name: myingress
      annotations:
        nginx.ingress.kubernetes.io/rewrite-target: /
        kubernetes.io/ingress.class: "nginx"
    spec:
      rules:
      - host: a948f1e332f324a0ea33522416751dd9-659e07d158005953.elb.ap-northeast-2.amazonaws.com
        http:
          paths:
          - path: /login
            pathType: Prefix
            backend:
              serviceName: urmywebapp-svc-clusterip
              servicePort: 80
          - path: /register
            pathType: Prefix
            backend:
              serviceName: urmywebapp-svc-clusterip
              servicePort: 80
          - path: /friendlist
            pathType: Prefix
            backend:
              serviceName: urmywebapp-svc-clusterip
              servicePort: 80
    ```

    ingress-nginx.yml 은 삭제

    ```bash
    ~/yaml$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.45.0/deploy/static/provider/aws/deploy.yaml
    namespace/ingress-nginx created
    serviceaccount/ingress-nginx created
    configmap/ingress-nginx-controller created
    clusterrole.rbac.authorization.k8s.io/ingress-nginx created
    clusterrolebinding.rbac.authorization.k8s.io/ingress-nginx created
    role.rbac.authorization.k8s.io/ingress-nginx created
    rolebinding.rbac.authorization.k8s.io/ingress-nginx created
    service/ingress-nginx-controller-admission created
    service/ingress-nginx-controller created
    deployment.apps/ingress-nginx-controller created
    validatingwebhookconfiguration.admissionregistration.k8s.io/ingress-nginx-admission created
    serviceaccount/ingress-nginx-admission created
    clusterrole.rbac.authorization.k8s.io/ingress-nginx-admission created
    clusterrolebinding.rbac.authorization.k8s.io/ingress-nginx-admission created
    role.rbac.authorization.k8s.io/ingress-nginx-admission created
    rolebinding.rbac.authorization.k8s.io/ingress-nginx-admission created
    job.batch/ingress-nginx-admission-create created
    job.batch/ingress-nginx-admission-patch created
    ```

    ```bash
    $ kubectl apply -f deploymentwebapp.yml     # 모든 야물파일 적용
    deployment.apps/my-webapp-deployment created

    ~/yaml$ kubectl get pods,services
    NAME                                        READY   STATUS    RESTARTS   AGE
    pod/my-webapp-deployment-5bd6cf5bb5-4bckm   1/1     Running   0          3s
    pod/my-webapp-deployment-5bd6cf5bb5-d8526   1/1     Running   0          3s
    pod/my-webapp-deployment-5bd6cf5bb5-ftw68   1/1     Running   0          3s

    NAME                               TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
    service/kubernetes                 ClusterIP   100.64.0.1      <none>        443/TCP   61m
    service/urmywebapp-svc-clusterip   ClusterIP   100.65.186.23   <none>        80/TCP    22m

    ~/yaml$ kubectl get pods,deployment -n ingress-nginx
    NAME                                            READY   STATUS      RESTARTS   AGE
    pod/ingress-nginx-admission-create-6z7vq        0/1     Completed   0          11m
    pod/ingress-nginx-admission-patch-m4qrp         0/1     Completed   0          11m
    pod/ingress-nginx-controller-6c94f69c74-b6gzl   1/1     Running     0          11m

    NAME                                       READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/ingress-nginx-controller   1/1     1            1           11m

    ~/yaml$ kubectl get svc -n ingress-nginx
    NAME                                 TYPE           CLUSTER-IP       EXTERNAL-IP                                                                          PORT(S)                      AGE
    ingress-nginx-controller             LoadBalancer   100.67.237.30    a948f1e332f324a0ea33522416751dd9-659e07d158005953.elb.ap-northeast-2.amazonaws.com   80:32564/TCP,443:32146/TCP   35m
    ingress-nginx-controller-admission   ClusterIP      100.64.155.225   <none>                                                                               443/TCP                      35m

    ~/yaml$ kubectl get ingress
    NAME        CLASS    HOSTS                                                                                ADDRESS                                                                              PORTS   AGE
    myingress   <none>   a948f1e332f324a0ea33522416751dd9-659e07d158005953.elb.ap-northeast-2.amazonaws.com   a948f1e332f324a0ea33522416751dd9-659e07d158005953.elb.ap-northeast-2.amazonaws.com   80      2m54s
    ```

4. **cmd창에서 curl로 확인**

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4d2ca268-9ddd-49f7-9b42-d02a311b9259/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4d2ca268-9ddd-49f7-9b42-d02a311b9259/Untitled.png)
