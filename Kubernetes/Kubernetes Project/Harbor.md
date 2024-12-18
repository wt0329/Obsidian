## 💡 목표
- Helm을 이용하여 Kubernetes cluster에 Harbor 배포
- Namespace : ci-cd
- Service type : LoadBalancer

## ✔ 조건
- Harbor version : 2.10

## 💬 준비사항
- Helm 설치
- NFS 설치
- NFS Provisioner 설치
- storageClass 설치
- Metallb 설치

## ☸ 설치 프로세스
- Harbor를 위한 인증서 생성
- Harbor Helm Repo 다운로드
- Helm을 이용한 Harbor 배포
- 설치한 Habor Docker Image 사용을 위한 설정

---


## ✅ Harbor를 위한 인증서 생성

**📍 인증 기관 인증서 생성**
  <font color="#000000">1. </font>CA 인증서 개인 키 생성
```css
openssl genrsa -out ca.key 4096
```

  <font color="#000000">2.</font> CA 인증서 생성
```css
openssl req -x509 -new -nodes -sha512 -days 3650 -key ca.key -out ca.crt
```

**📍 서버 인증서 생성**
