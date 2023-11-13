## 과정
### 1. PEM key를 통한 연결
```bash
cd ~/[PEM key를 저장한 경로]
```
	[[windows에서 ec2 ssh접속]]
```bash
ssh -i [ec2 -> 인스턴스 -> 연결 복사]
```
### nvm ,nodejs 설치 (React)
>[nvm링크](https://github.com/nvm-sh/nvm)
>[nvm, nodejs 설치 설명](https://iter.kr/%EC%9A%B0%EB%B6%84%ED%88%AC-nvm-node-js-%EC%84%A4%EC%B9%98-%EC%84%A4%EC%A0%95/)

#### nvm 설치
```bash
sudo apt update
sudo apt upgrade

sudo apt install -y build-essentail libssl-dev
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
```
#### node & npm 설치
```bash
nvm intall [node-version]
node -v
npm -v
```
#### node defualt 변경 방법
```bash
 nvm alias default [node-version]
```
### Python
``` bash
sudo apt-get install python3-pip
pip3 --version
```
### nginx
```bash
sudo apt install nginx
service nginx status
```


### 보안 그룹 설정
> 80(http) 8080(springboot) 22(ssh 연결) 443(https)

### 배포할 프로젝트 clone
```bash
git clone [배포할 repo]
```
>frontend(reanct)에 필요한 패키지 설지 & build
```bash
 cd frontend
 npm install
 npm run build
```
### 서빙 준비하기



nginx port = 80
