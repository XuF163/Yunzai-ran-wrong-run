### 基于nginx的QQBot负载均衡   
 
#### 适配器配置  
##### TRSS-小叶版适配器
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
##### 喵崽实现  
  总体思路：依赖切换到凉菜1.0.4+版本的qq-official-bot记得配置文件读写自行修改  
  关于消息冲突推送问题请参阅[链接](https://github.com/zhinjs/qq-official-bot/pull/77) ，截至25-4-22 建议  
  ```  
  /Miao-Yunzai/plugins/Lain-plugin/node_modules/qq-official-bot/lib/receivers下function webhookHandler的case constans_1.OpCode.DISPATCH:
  ```
  修改为
  ```  
case constans_1.OpCode.DISPATCH:
             this.emit('packet', data);
             return res.writeHead(200).end();
``` 
#### nginx配置  
    理由：
      1.便于异构适配器接入，提升负载均衡、容灾能力.  
      2.避免直接使用某适配器的证书不兼容问题.
      3.还没想好.  
   示例配置文件： 
   [![](https://mermaid.ink/img/pako:eNqdlG1P01AUx7_KzSUkI5bSlXVb-4I3ajQxSJM1wZi9qetla2At3nWKEhKYgo88LEwTDUiMW8QHJCQGlYL7Muvt9i28bfeEmwnzvtq95_875396z-4iTJkaghLMobt5ZKTQFV1NYzWbNABd8yq29JQ-rxoWUGSg5oDiaehOnlOtGRP30d28dssT3kzrxgII3Ud3MqY5y6YxUq0IL7IsO9LLTKq6EedEzgOdtVX3rFj7adefF9zCLxDyAv0YHWMT02C4i7I3uqjwP6mrC5YPlV93Q0kj0A8PAzmj5hAIS-C6QvueDpqggUCgyKMTE5dooxKQpxIKGGt2CUKePNEsS-OerONTAo1XO86jzfrhD3JUCMQMcE4L5KDSj6EuB0Can1ACNfuDu7V2nnAPnjm_V5tESzpKuVG_C79LZ3vdOSmBEGLTLAN4jgNTN0YAGOrcjrOzTw7KjbdbzummUz4i3-2OBS-XIv-ViuxW6k8-A_8Ggb-G_PnoTcR481Wr7pKXK6S6TA9qdoVsVEjpGEwnei6Gl7w7SZipWWQBZ7PYWF5pX067O_pRPEOOfeJ-eTFWO6u6pX2aDNSr72hm0F7dDa6tk6_vAwO-o-6DnvR-9qA4TUuOn5KVw0A0BNyPduNNGWhITVn6PdVCbbDVTGcuBnHaGfqLeO0uMaDbNnreL53Jge16_7YLuw0K_I9ZSra89hMpcitW__bJLe05xTOnuE929uiEuo-PXXub7O5BBqaxrkHJwnnEwCzCWdXbwkUPTkIrg7IoCSX6U0Mzan7OSsKksUQx-rrcNs1si8RmPp2B0ow6l6O7_LxGLTSf1vYpRoaG8GUzb1hQ4iOinwRKi3ABSoLAhuOxWETg-PFxISqEGfgAShGRFWJRLsqJghDlRS62xMCHflWOFcPhcTEqxjk-LkQjEYGBSNMtE08G77v_zC_9AWPmVrc?type=png)](https://mermaid-live.nodejs.cn/edit#pako:eNqdlG1P01AUx7_KzSUkI5bSlXVb-4I3ajQxSJM1wZi9qetla2At3nWKEhKYgo88LEwTDUiMW8QHJCQGlYL7Muvt9i28bfeEmwnzvtq95_875396z-4iTJkaghLMobt5ZKTQFV1NYzWbNABd8yq29JQ-rxoWUGSg5oDiaehOnlOtGRP30d28dssT3kzrxgII3Ud3MqY5y6YxUq0IL7IsO9LLTKq6EedEzgOdtVX3rFj7adefF9zCLxDyAv0YHWMT02C4i7I3uqjwP6mrC5YPlV93Q0kj0A8PAzmj5hAIS-C6QvueDpqggUCgyKMTE5dooxKQpxIKGGt2CUKePNEsS-OerONTAo1XO86jzfrhD3JUCMQMcE4L5KDSj6EuB0Can1ACNfuDu7V2nnAPnjm_V5tESzpKuVG_C79LZ3vdOSmBEGLTLAN4jgNTN0YAGOrcjrOzTw7KjbdbzummUz4i3-2OBS-XIv-ViuxW6k8-A_8Ggb-G_PnoTcR481Wr7pKXK6S6TA9qdoVsVEjpGEwnei6Gl7w7SZipWWQBZ7PYWF5pX067O_pRPEOOfeJ-eTFWO6u6pX2aDNSr72hm0F7dDa6tk6_vAwO-o-6DnvR-9qA4TUuOn5KVw0A0BNyPduNNGWhITVn6PdVCbbDVTGcuBnHaGfqLeO0uMaDbNnreL53Jge16_7YLuw0K_I9ZSra89hMpcitW__bJLe05xTOnuE929uiEuo-PXXub7O5BBqaxrkHJwnnEwCzCWdXbwkUPTkIrg7IoCSX6U0Mzan7OSsKksUQx-rrcNs1si8RmPp2B0ow6l6O7_LxGLTSf1vYpRoaG8GUzb1hQ4iOinwRKi3ABSoLAhuOxWETg-PFxISqEGfgAShGRFWJRLsqJghDlRS62xMCHflWOFcPhcTEqxjk-LkQjEYGBSNMtE08G77v_zC_9AWPmVrc) 
```
#  放配置文件的地方  
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
