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
### Frontend build
>frontend(reanct)에 필요한 패키지 설지 & build
```bash
 cd frontend
 npm install
 npm run build
```
### 서빙 준비하기
#### nginx 상태 확인
```bash
sudo service nginx status
```
#### nginx conf 수정
```bash
server{
	listen 80;
	location / {
		root /home/ubuntu/B-Trip/React/b-trip/build;
		index index.html index.htm;
		try_files $uri $uri/ /index.html;
	}
	#backend CORS회피를 위한 proxy
	location /api/ {
		proxy_pass http://[backend주소];
	}
}
```
#### nginx 재시작
```bash
sudo service nginx restart
```

### Frontend 재배포
```bash
git pull origin master

cd frontend

npm run build

sudo service nginx restart
```
### SpringBoot Maven (Backend) 배포
#### Java 설치
``` bash
sudo apt-get install openjdk-11-jdk

java -version
```
#### Maven 설치
```bash
sudo apt-get install maven
```
> backend mvnw 있는 폴더에서

```bash
mvn -N io.takari:maven:wrapper

./mvnw clean package
```

#### jar 파일 실행
>target 디렉토리 안에 들어가서

```bash
nohup java -jar [생성된 jar 파일 이름] &
```
> - Linux, Unix에서 shell script 파일 (.sh)을 demon 형태로 실행시키는 프로그램
> - 명령어 뒤에 `&`를 붙이면 현재의 명령과 다른 명령을 분리한다는 의미!

```bash
lsof -i :8080
```
> - 작동 중인 포트 확인
### MySQL 설정
```bash
sudo apt update && sudo apt upgrade # apt 패키지 인스톨러 갱신

sudo apt install mysql-server

sudo apt install libmysqlclient-dev
```
> mysql 기본 세팅 및 접속권한 부여

```bash
mysql> CREATE USER 'root'@'[서버 주소]' IDENTIFIED BY '[root 계정 비밀번호]';

mysql> GRANT ALL PRIVILEGES ON . TO 'root'@'[서버 주소]' WITH GRANT OPTION;

mysql> FLUSH PRIVILEGES;
```
### Springboot 재배포
```bash
lsof -i :[port번호]

sudo kill -9 [위에서 확인한 pid번호]

git pull origin main

./mvnw clean package

java -jar [생성된 jar 파일 이름] &
```
## 문제 및 해결방법
### 메모리 부족
>프리티어 사용시 램이 1Gb 주어져 메모리 부족해 빌드시 멈춰버리는 현상이 일어난다
>추가로 서버가 불안정하게 종료된다고 한다
>이를 해결할 방법으로 메모리가 부족할때 HDD의 일정 공간을 RAM처럼 사용하여 회피하는 방법

```bash
sudo dd if=/dev/zero of=/swapfile bs=128MB coun=16

sudo chmod 600 /swapfile

sudo swapon /swapfile

sudo swapon -s

sudo vi /etc/fstab
#파일 끝에 추가
/swapfile swap swap defaults 0 0

#확인 방법
sudo free -m
```

