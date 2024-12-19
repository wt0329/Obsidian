## 💡 목표
- 공식 문서를 참조하여 호스트 4번에 Rancher 설치 및 kubespray를 이용하여 설치한 cluster 등록
- Rancher 설치 후 kubespary cluster에 helm을 이용하여 Prometheus, Grafana 설치

## ✔ 조건
- Rancher version : 2.8.4
	- 공식 이미지를 사용하거나 사내 harbor 내부에 docker를 설치한 커스텀 이미지 사용
	
## 💬 준비사항
- Docker 설치

---

## ✅ Docker를 이용한 Rancher 설치

📍 **Docker 설치**
```linux
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

  <font color="#000000">1.</font> Set up Docker's `apt` repository

  <font color="#000000">2.</font> Install the Docker packages
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

📍 **Rancher 설치**
```
docker run -d --restart=unless-stopped \
  -p 80:80 -p 443:443 \
  --privileged \
  rancher/rancher:v2.8.4
```

**📍비밀번호 확인**
```
docker logs  28e4aad32d46  2>&1 | grep "Bootstrap Password:"
```

## ✅ 설치 후 Cluster 1을 import
![[Untitled.png]]

![[Untitled 1.png]]
  💥 2번째 명령어 사용

---

**📃 참고 자료**

https://docs.docker.com/engine/install/ubuntu/

https://ranchermanager.docs.rancher.com/getting-started/installation-and-upgrade/other-installation-methods/rancher-on-a-single-node-with-docker


