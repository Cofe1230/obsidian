# 리눅스 timezone 한국 시간 설정
## AWS EC2 ubuntu 환경
``` base
#현재 시간 확인
date

#어디에 시간이 맞췆 있는지 확인
sudo cat /etc/localtime

#localtime 삭제
sudo rm /etc/localtime

#timezone Aisa/Seoul 로 변경
sudo ln -s /usr/share/zoneinfo/Asia/Seoul /etc/localtime

#변경 확인
date
```