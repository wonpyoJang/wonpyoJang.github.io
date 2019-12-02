---
title: "GCP에 Hyperledger Fabric 설치하기"
date: 2019-12-01 18:20:00 -0400
categories: GCP, Hyperledger Fabric
---

# GCP Compute Engine에 Hyperledger Fabric 1.4 설치하기

## GCP 인스턴스 스펙
이 포스트에서 사용할 GPU Compute Engine VM 인스턴스의 스펙은 다음과 같다.
+ GCP Compute Engine N1 (vCPU 1개, 3.75GB 메모리, __HDD 30GB__, Cent)
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

## 도커 설치
yum package manager를 활용해 docker를 설치한다.

### + Dependency 설치
yum을 이용해 다음 프로그램을 설치한다.
```
$ sudo yum install -y yum-utils \
device-mapper-persistent-data \
lvm2
```
cf) yum-utils : yum 저장소를 관리하기 위한 도구
    device-mapper-persistent-data : 가상화를 위한 도구. docker 설치에 필요
    lvm2 : Logical Volume Manager, docker 설치에 필요.

### + 저장소 설정
도커 저장소를 yum 저장소에 추가한다.
```
$ sudo yum-config-manager \
--add-repo \
https://download.docker.com/linux/centos/docker-ce.repo
```

### + 도커 엔진 설치
```
$ sudo yum install docker-ce docker-ce-cli containerd.io [--nobest]
```
cf) dcoker-ce : Docker Community Edition
    docker-ce-cli : Docker Community Edition CLI

### + 도커 실행
```
$ sudo systemctl start docker
```

### + 도커 설치 확인
```
$ sudo docker run hello-world
```








