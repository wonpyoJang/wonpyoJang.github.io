---
title: "GCP Compute Engine에 Hyperledger Fabric 1.4 설치하기"
date: 2019-12-01 18:20:00 -0400
categories: GCP, Hyperledger Fabric
---

## 인스턴스 스펙
이 포스트에서 사용할 VM의 스펙은 다음과 같다.
+ GCP Compute Engine N1
- vCPU 1개
- 3.75GB 메모리
- __HDD 30GB__
- __CentOS 7__

## 프로그램 버전 정보
이 포스트에서 사용할 프로그램 버전 정보는 아래와 같다.
+ CentOS 7
+ curl 7.29.0
+ docker 19.03.3 
+ docker-compose 1.24.1 
+ go 1.12.10 
+ nodejs 8.9.4 
+ npm 5.6.0 
+ python 2.7.5 
+ git 1.8.3.1 
+ gcc 4.8.5 
+ g++ 4.8.5 
+ jq 1.5

---

## 도커 설치

+ Dependency 설치
yum을 이용해 다음 프로그램을 설치한다.

```
$ sudo yum install -y yum-utils \
device-mapper-persistent-data \
lvm2
```
cf) yum-utils : yum 저장소를 관리하기 위한 도구
    device-mapper-persistent-data : 가상화를 위한 도구. docker 설치에 필요
    lvm2 : Logical Volume Manager, docker 설치에 필요.

+ 저장소 설정
도커 저장소를 yum 저장소에 추가한다.
```
$ sudo yum-config-manager \
--add-repo \
https://download.docker.com/linux/centos/docker-ce.repo
```

+ 도커 엔진 설치
```
$ sudo yum install docker-ce docker-ce-cli containerd.io [--nobest]
```
cf) dcoker-ce : Docker Community Edition
    docker-ce-cli : Docker Community Edition CLI

+ 도커 실행
```
$ sudo systemctl start docker
```

+ 도커 설치 확인
```
$ sudo docker run hello-world
```

## docker-compose 설치

+ docker-compose 1.24.1 다운로드
```
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

+ 실행권한 추가
```
sudo chmod +x /usr/local/bin/docker-compose
```

+ 설치확인
```
$ docker-compose --version
```

---

## Go 설치

+ wget 설치
```
$ sudo yum install wget
```

+ 실행파일 다운로드
```
$ wget https://dl.google.com/go/go1.12.10.linux-amd64.tar.gz
```

+ 압축해제
```
$ sudo tar -C /usr/local -xzf go1.12.10.linux-amd64.tar.gz
```

+ /etc/profile에 아래 내용 추가
```
sudo vi /etc/profile
```
```
export PATH=$PATH:/usr/local/go/bin
```

+ 환경변수 적용 및 확인
```
$ source /etc/profile
$ echo $PATH
```

+ go 설치 확인
hello.go 생성

```
$ mkdir -p go/src/hello
$ cd go/src/hello
$ vi hello.go
```

아래 내용 입력

```
package main

import "fmt"

func main() {
	fmt.Printf("hello, world\n")
}
```

빌드 테스트

```
$ go build
$ ./hello
```

+ GOPATH 추가
    
```
$ cd ~
$ vi ~/.bashrc
```

아래 내용 입력

```
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```

## Node.js 설치

+ 실행 파일 다운로드

```
$ wget https://nodejs.org/download/release/v8.9.4/node-v8.9.4-linux-x86.tar.xz
```

+ 설치 폴더 생성

```
$ sudo mkdir -p /usr/local/lib/nodejs
```

+ 압축 해제 및 설치

```
$ sudo tar -xJvf node-$VERSION-$DISTRO.tar.xz -C /usr/local/lib/nodejs 
```

+ 환경 변수 설정

/etc/profile 에 아래 내용 입력

```
$ sudo vi /etc/profile
```

```
VERSION=v8.9.4
DISTRO=linux-x64
export PATH=/usr/local/lib/nodejs/node-$VERSION-$DISTRO/bin:$PATH
```

환경변수 적용

```
source /etc/profile
```

+ 설치 확인

```
$ node -v
$ npm version
$ npx -v
```

+ 심볼릭 링크 생성

```
$ sudo ln -s /usr/local/lib/nodejs/node-$VERSION-$DISTRO/bin/node /usr/bin/node
$ sudo ln -s /usr/local/lib/nodejs/node-$VERSION-$DISTRO/bin/npm /usr/bin/npm
$ sudo ln -s /usr/local/lib/nodejs/node-$VERSION-$DISTRO/bin/npx /usr/bin/npx

```

+ npm 다운그레이드 ?

```
$ npm install npm@5.6.0 -g
```

## 하이퍼레저 패브릭 설치

+ 깃 설치

```
$ sudo yum -y install git
```

+ 현재 접속중인 계정에 도커 사용 권한 주기

```
$ sudo usermod -aG docker $USER
```

+ 도커 재시작

```
$ sudo service docker restart
```

+ 새 콘솔창을 연다.

+ Hyperledger Fabric 1.4.3 다운로드

```
$ sudo curl -sSL http://bit.ly/2ysbOFE | bash -s -- 1.4.3 1.4.3 0.4.15
```

+ 환경변수 설정

/etc/profile 에 다음 내용 추가

```
export PATH=~/fabric_samples/bin:$PATH
```

변경 내용 적용

```
$ source /etc/profile
```

## Node.js SDK 설치

Node.js SDK는 Node.js로 체인코드를 작성할 수 있게 해주는 툴이다.

```
$ npm install --save fabric-contract-api
$ npm install --save fabric-shim
```

## 설치 확인

+ byfn 예제 동작 확인

```
$ cd ~
$ cd fabric-samples/first-network/
$ ./byfn.sh generate
$ ./byfn.sh up -l node
```
+ reference
Hyperledger fabric prerequisite : https://hyperledger-fabric.readthedocs.io/en/latest/prereqs.html
docker 설치(Cent OS) : https://docs.docker.com/install/linux/docker-ce/centos/
docker compose 설치 : https://docs.docker.com/compose/install/
go 설치 : https://golang.org/doc/install
nodejs 설치 : https://github.com/nodejs/help/wiki/Installation
byfn 예제 : https://hyperledger-fabric.readthedocs.io/en/release-1.4/build_network.html
balance transfer 예제 : https://github.com/hyperledger/fabric-samples/tree/release-1.4/balance-transfer


