- **목차**

### 사용 버전

가상환경 : VMware Workstation 15.5.6 버전

리눅스 : Ubuntu 20.04.2.0 LTS 버전 

[Download Ubuntu Desktop | Download | Ubuntu](https://ubuntu.com/download/desktop)

### 진행

1. **VMware에 Ubuntu 가상 머신 설치**

    네트워크 : NAT

    MAC 주소 Generate

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/97331666-e267-4eb5-bd04-fddf6af07b90/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/97331666-e267-4eb5-bd04-fddf6af07b90/Untitled.png)

2. **sudo apt-get upgrade 진행**

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8be1a0b6-407f-4e9a-91d1-9f0eaf1c74f9/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8be1a0b6-407f-4e9a-91d1-9f0eaf1c74f9/Untitled.png)

3. **AWS Console 로그인**
4. **IAM 사용자 생성**

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fb11017b-01ba-43b2-8023-22fb17c706d4/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fb11017b-01ba-43b2-8023-22fb17c706d4/Untitled.png)

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7073a1b8-f691-4e0e-a4f4-e060fed0ae8c/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7073a1b8-f691-4e0e-a4f4-e060fed0ae8c/Untitled.png)

    이름 : ecruser

    방식 : 프로그래밍 방식

    권한 : SecretsManageReadWrite, EC2InstanceProfileForImageBuilderECRContainerBuilds

5. **ECR Repository 생성**

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/27b98dcf-ea27-4dc1-bce0-01893228473e/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/27b98dcf-ea27-4dc1-bce0-01893228473e/Untitled.png)

    표시 여부 : 프라이빗

    리포지토리 이름 : skecr

6. **우분투에서 golang 설치**

    ```bash
    ~$ wget https://golang.org/dl/go1.16.3.linux-amd64.tar.gz
    ~$ sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.16.3.linux-amd64.tar.gz
    ~$ export PATH=$PATH:/usr/local/go/bin
    ~$ go version
    go version go1.16.3 linux/amd64
    ============================================================
    환경변수 VM 껐다 켜도 적용 되도록
    ~$ vi ~/.profile
    ...
    export PATH=$PATH:/usr/local/go/bin
    ```

    - .profile

    ```bash
    ~$ mkdir go
    ~$ cd go
    ~/go$ mkdir bin pkg src
    ~/go$ ls
    bin  pkg  src
    ```

    ```bash
    ~$ mkdir -p ~/goproject/webhandler
    ~$ cd ~/goproject/webhandler
    ~/goproject/webhandler$ go env -w GO111MODULE=auto
    ~/goproject/webhandler$ go mod init myapp.com/webhandler
    go: creating new go.mod: module myapp.com/webhandler
    ```

7. **개발툴 설치(Visual Studio Code)**

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/94c4b2ca-be53-4d76-b9b8-c6faa56b95d4/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/94c4b2ca-be53-4d76-b9b8-c6faa56b95d4/Untitled.png)

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8f0be929-5ea2-4bf5-8356-0a49728839f5/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8f0be929-5ea2-4bf5-8356-0a49728839f5/Untitled.png)

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/deb1b507-a4f1-4008-a02e-f1804708ae78/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/deb1b507-a4f1-4008-a02e-f1804708ae78/Untitled.png)

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6a8a8965-326c-4751-b235-c82215916cb1/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6a8a8965-326c-4751-b235-c82215916cb1/Untitled.png)

8. **개발 환경 설정**

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/27ce5240-512f-452e-843c-e7a82752a6c7/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/27ce5240-512f-452e-843c-e7a82752a6c7/Untitled.png)

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e3c18c41-4a37-4215-85dd-ccff43ab06ec/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e3c18c41-4a37-4215-85dd-ccff43ab06ec/Untitled.png)

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1b0f968e-bc71-4a35-86a9-fb927eaf79da/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1b0f968e-bc71-4a35-86a9-fb927eaf79da/Untitled.png)

    ```bash
    ~$ sudo apt install curl
    ~$ sudo apt-get install build-essential
    ~$ curl localhost:5000/hello
    Your Request Path is /hello
    ```

    - build-essential 설치 안할 시 오류

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e0bee84f-a226-43b5-8f4a-fb8f27a8dae3/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e0bee84f-a226-43b5-8f4a-fb8f27a8dae3/Untitled.png)

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/edbbbb65-ebfe-4323-b34a-f65a22a43c47/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/edbbbb65-ebfe-4323-b34a-f65a22a43c47/Untitled.png)

9. **Docker 설치**

    [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)

    도커와 쿠버네티스를 사용하기 위해서 **최신 버전이 아닌 특정 버전(5:19.03.15)을 설치**해준다.

    ```bash
    ~$ sudo apt-get update
    ~$ sudo apt-get install \
    >     apt-transport-https \
    >     ca-certificates \
    >     curl \
    >     gnupg \
    >     lsb-release
    ~$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    ~$ echo \
    >   "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
    >   $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    ~$ sudo apt-get update
    ~$ apt-cache madison docker-ce
     docker-ce | 5:20.10.6~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     docker-ce | 5:20.10.5~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     docker-ce | 5:20.10.4~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     docker-ce | 5:20.10.3~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     docker-ce | 5:20.10.2~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     docker-ce | 5:20.10.1~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     docker-ce | 5:20.10.0~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     docker-ce | 5:19.03.15~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     docker-ce | 5:19.03.14~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     docker-ce | 5:19.03.13~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     docker-ce | 5:19.03.12~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     docker-ce | 5:19.03.11~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     docker-ce | 5:19.03.10~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     docker-ce | 5:19.03.9~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
    ~$ sudo apt-get install docker-ce=5:19.03.15~3-0~ubuntu-focal docker-ce-cli=5:19.03.15~3-0~ubuntu-focal containerd.io
    ~$ sudo docker run hello-world
    ~$ docker version
    ```

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8636c10d-5484-4e89-8e24-0b64e8661511/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8636c10d-5484-4e89-8e24-0b64e8661511/Untitled.png)

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/84b5eae2-e063-4392-839d-a05f45f39395/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/84b5eae2-e063-4392-839d-a05f45f39395/Untitled.png)

10. **AWS CLI 설치**

    [Linux에서 AWS CLI 버전 2 설치, 업데이트 및 제거](https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/install-cliv2-linux.html)

    ```bash
    ~$ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
    ~$ unzip awscliv2.zip
    ~$ sudo ./aws/install
    ~$ aws --version
    aws-cli/2.1.39 Python/3.8.8 Linux/5.8.0-50-generic exe/x86_64.ubuntu.20 prompt/off
    ```

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/44c7c2d2-fab1-4b48-9d6c-9c76073ed79e/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/44c7c2d2-fab1-4b48-9d6c-9c76073ed79e/Untitled.png)

    ```bash
    ~$ aws configure
    Access Key ID : IAM "ecruser"의 액세스 키
    Secret Access Key : IAM "ecruser"의 시크릿 키
    region : ap-northeast-2
    format : json
    ```

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/675a73d7-17b5-4a0e-ba79-9dd71fcec20f/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/675a73d7-17b5-4a0e-ba79-9dd71fcec20f/Untitled.png)

    - 이후 **[에러 발생](https://www.notion.so/Ubuntu-59cf8e7cd5714d6e9b6ec077e032ea5a)**
11. **Dockerfile 생성 및 ECR 배포 Shell Script 작성**

    ```bash
    ~/goproject/webhandler$ go build -o webhandler
    ```

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8d569341-4cb9-4ec0-b33f-a2ab9b5332f5/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8d569341-4cb9-4ec0-b33f-a2ab9b5332f5/Untitled.png)

    ```bash
    FROM alpine:latest
    LABEL maintainer="SK9604 tjsrud428@gmail.com"
    RUN mkdir /lib64 && \
    ln -s /lib/libc.musl-x86_64.so.1 /lib64/ld-linux-x86-64.so.2
    COPY ./webhandler /root
    EXPOSE 5000
    WORKDIR /root
    ENTRYPOINT ["/bin/sh", "-c"]
    CMD ["./webhandler"]
    ```

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fae38531-be04-4ffe-95a9-73335e6d27ec/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fae38531-be04-4ffe-95a9-73335e6d27ec/Untitled.png)

    ```bash
    version="1.0"
    aws ecr get-login-password --region ap-northeast-2 | docker login --username AWS --password-stdin 778893145182.dkr.ecr.ap-northeast-2.amazonaws.com/skecr
    docker build -t webhandler:$version .
    docker tag webhandler:$version 778893145182.dkr.ecr.ap-northeast-2.amazonaws.com/skecr:$version
    docker push 778893145182.dkr.ecr.ap-northeast-2.amazonaws.com/skecr:$version
    ```

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0ae2fd9c-7a23-4ec2-9c8a-baf1093de1af/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0ae2fd9c-7a23-4ec2-9c8a-baf1093de1af/Untitled.png)

    - 코드의 빨간 네모 안에는 개인 ECR 프라이빗 리포지토리의 URI를 복사하여 붙여 넣는다

    ```bash
    ~/goproject/webhandler$ chmod 770 dockerpush.sh
    ~/goproject/webhandler$ sudo ./dockerpush.sh
    ~/goproject/webhandler$ sudo docker images
    ```

    - **에러 발생**

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1d632ae6-fc50-4f0a-ad5e-9064805d69b8/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1d632ae6-fc50-4f0a-ad5e-9064805d69b8/Untitled.png)

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0e2bce86-7d64-4ff3-b485-58552b6587da/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0e2bce86-7d64-4ff3-b485-58552b6587da/Untitled.png)
