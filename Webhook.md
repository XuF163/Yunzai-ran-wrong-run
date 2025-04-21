### 基于nginx的QQBot负载均衡   
[![](https://mermaid.ink/img/pako:eNqVk01v0zAYx7-K5VMm0pC0aVL7sAsgkNC2SMsBTbmYxk0j2qS4DhSqHlbUwkC8CcYJTrAL0pg4TLyU8WmapHwLnDSDAN0Bn_z4-f_8-O_HHsJm6FKIYZ_ejGjQpBd94jHSdQIgRo8w7jf9Hgk4sC1A-sDONCKyOoS3QrZCt3n5Wibc9PxgAKTb9Ho7DG8oHqOE61WkKMrav8wG8YOGitQMjKeT9OT5_PNs8XCcjr8AKUusYnzGQiaSWomaPSlR2pnUpQHPoXev0pMP8YNpcry75JxgidhWZX39nLCCgbW1bYPzhQ8gXbFta7vYWOQz2e-TYPBj_3V87-ni6FPycbwUyyD-Nk4OD1Yx4hz_gRSXhMF89jZ9Nv2TSA_34u-TgjiVVgRXyV1kIhC_eBx_fQkkqniKDKqqCrauSvPZJDl-tDjaXyuVyzjb-gtL3hws7r8HpX64lDS5f4twWmrHGSnhFcrQY74LMWcRlWGXsi7JQjjMGAfyNu1SB2IxdWmLRB3uQCcYCUz0bicMu6ckCyOvDXGLdPoiinquKFM83F-rjAYuZRfCKOAQa4aWbwLxEA4grtcVrWGahqoaSFcbSK_L8A7EOlJMHdV1s2Yiw6iptZEM7-ZlVQVpWg0ZqKoawiNCDRlS1-ch21h-n_wXjX4Ck2MwCg?type=png)](https://mermaid-live.nodejs.cn/edit#pako:eNqVk01v0zAYx7-K5VMm0pC0aVL7sAsgkNC2SMsBTbmYxk0j2qS4DhSqHlbUwkC8CcYJTrAL0pg4TLyU8WmapHwLnDSDAN0Bn_z4-f_8-O_HHsJm6FKIYZ_ejGjQpBd94jHSdQIgRo8w7jf9Hgk4sC1A-sDONCKyOoS3QrZCt3n5Wibc9PxgAKTb9Ho7DG8oHqOE61WkKMrav8wG8YOGitQMjKeT9OT5_PNs8XCcjr8AKUusYnzGQiaSWomaPSlR2pnUpQHPoXev0pMP8YNpcry75JxgidhWZX39nLCCgbW1bYPzhQ8gXbFta7vYWOQz2e-TYPBj_3V87-ni6FPycbwUyyD-Nk4OD1Yx4hz_gRSXhMF89jZ9Nv2TSA_34u-TgjiVVgRXyV1kIhC_eBx_fQkkqniKDKqqCrauSvPZJDl-tDjaXyuVyzjb-gtL3hws7r8HpX64lDS5f4twWmrHGSnhFcrQY74LMWcRlWGXsi7JQjjMGAfyNu1SB2IxdWmLRB3uQCcYCUz0bicMu6ckCyOvDXGLdPoiinquKFM83F-rjAYuZRfCKOAQa4aWbwLxEA4grtcVrWGahqoaSFcbSK_L8A7EOlJMHdV1s2Yiw6iptZEM7-ZlVQVpWg0ZqKoawiNCDRlS1-ch21h-n_wXjX4Ck2MwCg)  
tips:按照适配器项目说法，各个Yunzai节点实际上仍然独立向tx发送消息  
#### 适配器配置
```
//QQBot.yaml  
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
  - 284522356:10235467:xxxxxxx:xxxxxxxxx:1:0

```  
nginx配置  
```
#放配置文件的地方  
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
        mirror /_mirror_webhook_external_142;
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

    location /_mirror_webhook_external_142 {
        internal; # 关键：使其成为内部 location,虽然连接的是外网主机

        # 代理到外部 HTTP 服务
        proxy_pass http://ip:8090/webhook;

        # --- 重要: 为外部目标设置正确的 Host Header ---
        # $proxy_host 会自动使用 proxy_pass 中的主机名或 IP 和端口
        proxy_set_header Host $proxy_host;
        # 或者显式设置: proxy_set_header Host "ip:8090";

        # 传递其他必要的头信息
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        # 可以传递原始 Host (如果目标需要)
        # proxy_set_header X-Original-Host $host;
        proxy_set_header X-Mirrored-Request "true"; # 标记为镜像请求

        # 推荐与外部服务通信使用的设置
        proxy_http_version 1.1;
        proxy_set_header Connection ""; # 清除 Connection header，避免混淆
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
