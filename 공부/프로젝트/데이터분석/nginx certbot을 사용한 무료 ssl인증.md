# nginx + certbot
## Let's Encrypt 클라이언트 다운로드
```bash
apt-get update 
sudo apt-get install certbot 
apt-get install python3-certbot-nginx
```
## NGINX SSL 설정
```nginx
server {
				listen 80;
                server_name btrip.store www.btrip.store;

                location / {
                        # First attempt to serve request as file, then
                        # as directory, then fall back to displaying a 404.
                        root /home/ubuntu/B-Trip/React/b-trip/build;
                        index index.html index.htm;
                        try_files $uri $uri/ /index.html;
                }

                location /api/ {
                        proxy_pass http://13.209.157.25:8080;
                }
	    }
```
## SSL/TLS 인증서 받기
```bash
sudo certbot --nginx -d example.com -d www.example.com
```

## 참조
[# Let’s Encrypt 인증서로 NGINX SSL 설정하기](https://nginxstore.com/blog/nginx/lets-encrypt-%EC%9D%B8%EC%A6%9D%EC%84%9C%EB%A1%9C-nginx-ssl-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0/)

