### 基于nginx的QQBot负载均衡   
适配器配置
```
permission: master
webhook:
  port: 8091  //其它崽节点 ..换端口
  path: /webhook
  signature: true
  ssl:
    key: ""
    cert: ""
    ca: ""
  ws: {}
toQRCode: true
toCallback: true
toBotUpload: true
hideGuildRecall: false
toQQUin: false
toImg: true
callStats: false
userStats: false
markdown:
  template: abcdefghij
sendButton: true
customMD: {}
mdSuffix: {}
btnSuffix: {}
filterLog: {}
simplifiedSdkLog: false
markdownImgScale: 1
sep: ""
bot:
  sandbox: false
  maxRetry: .inf
  timeout: 30000
token:
  - 28524356:10235467:xxxxxxx:xxxxxxxxx:1:0

```  
nginx配置  
```
# 重定向 HTTP 到 HTTPS
server {
    listen 80;
    listen [::]:80;
    server_name //你的域名

    return 301 https://$host$request_uri;
}

# HTTPS 虚拟主机配置
server {
    listen 443 ssl;
    listen [::]:443 ssl;
    server_name webhook.great429.dpdns.org;

    # Certbot 生成的证书路径，请确保路径正确
    ssl_certificate /etc/letsencrypt/live/xx/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/xx/privkey.pem;
    # ssl_trusted_certificate /etc/letsencrypt/live/xx/chain.pem; # 可选

    # 安全 SSL 配置
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers 'EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH';
    ssl_prefer_server_ciphers on;
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 10m;
    ssl_session_tickets off;
    ssl_stapling on;
    ssl_stapling_verify on;
    resolver 8.8.8.8 8.8.4.4 valid=300s;
    resolver_timeout 5s;
    # add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload" always;

    # 根目录（通常用不上）
    root /var/www/html;
    index index.html index.htm;

    # 反向代理 webhook 路径到应用程序监听的 8443 端口
    location /webhook {
    	   mirror /_mirror_webhook_8091;
        proxy_pass http://127.0.0.1:8090/webhook; 
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

     # 内部 location 用于镜像到 8091 端口
    location /_mirror_webhook_8091 {
        internal; # 关键：使其成为内部 location，不能从外部访问

        proxy_pass http://127.0.0.1:8091/webhook; # 转发到 8091
        # 确保镜像请求也包含必要的头信息
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        # 可以添加一个特殊的头，让后端知道这是一个镜像请求（可选）
        proxy_set_header X-Mirrored-Request "true";
        # proxy_request_buffering off; # 考虑是否需要
        # proxy_http_version 1.1;
        # proxy_set_header Connection "";
    }


    # 阻止访问隐藏文件
    location ~ /\. {
        deny all;
    }

    # 日志文件 (可选)
    # access_log /var/log/nginx/webhook.access.log;
    # error_log /var/log/nginx/webhook.error.log;
}
```
