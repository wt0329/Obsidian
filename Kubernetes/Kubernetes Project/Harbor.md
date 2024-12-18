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

  <font color="#000000">1. </font>개인 키 생성
```css
openssl genrsa -out yourdomain.com.key 4096
```

  <font color="#000000">2.</font> 인증서 서명 요청(CSR) 생성
```CSS
  openssl req -sha512 -new -key harbor.key -out harbor.csr
```

  <font color="#000000">3.</font> x509 v3 확장 파일 생성
```css
root@master:/etc/containerd/certs.d# cat v3.ext 
[ req ]
distinguished_name = req_distinguished_name
req_extensions = req_ext
prompt = no

[ req_distinguished_name ]
C = KR
ST = Seoul
L = Seoul
O = My Company
OU = My Division
CN = harbor.ateam.kr

[ req_ext ]
subjectAltName = @alt_names

[ alt_names ]
DNS.1 = harbor.ateam.kr
IP.1 = 10.10.250.80
```

  <font color="#000000">4. </font>인증서(CRT) 생성
  ```CSS
  openssl x509 -extfile v3.ext -req -sha512 -days 3650 -CA ca.crt -CAkey ca.key -CAcreateserial -in harbor.csr -out harbor.crt
```

  <font color="#000000">5.</font> DOCKER에서는 CERT 파일을 사용하기 때문에 CERT 파일 생성
```CSS
openssl x509 -inform PEM -in harbor.crt -out harbor.cert
```

  <font color="#000000">6.</font> 폴더 만든 후 인증서 복사
  ```CSS
cp ca.crt /etc/containerd/certs.d/harbor.ateam.kr/
cp harbor.cert /etc/containerd/certs.d/harbor.ateam.kr/
cp harbor.key /etc/containerd/certs.d/harbor.ateam.kr/
```

  <font color="#000000">7.</font> DOCKER 재시작
  ```CSS
sudo systemctl restart docker
```

## ✅ Harbor Helm Repo 다운로드


## ✅ Helm을 이용한 Harbor 배포
```css
helm install harbor -n ci-cd -f values.yaml .
```


---
**📃 참고 자료 

https://m.post.naver.com/viewer/postView.naver?volumeNo=35878696&memberNo=5733062&searchKeyword=json&searchRank=339

https://kschoi728.tistory.com/65

https://goharbor.io/docs/2.10.0/install-config/



