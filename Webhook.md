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
  关于消息冲突推送问题请参阅[链接](https://github.com/zhinjs/qq-official-bot/pull/77) ，截至25-4-22 建议  /Miao-Yunzai/plugins/Lain-plugin/node_modules/qq-official-bot/lib/receivers下的
  ```  
  function webhookHandler的case constans_1.OpCode.DISPATCH:
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
### Nginx自动重启+钉钉定时报告-尝试解决家里云跳ip   
```
#!/bin/bash

# 如果任何命令执行失败（返回非零状态），则立即退出脚本
set -e

# --- 配置 ---
 DINGTALK_WEBHOOK_URL="" # 示例旧地址
#DINGTALK_WEBHOOK_URL="" # <-- **您的钉钉 Webhook 地址**

# 需要检查的 URL 列表
URLS=(
    "http://127.0.0.1:8090/webhook"
    "http://127.0.0.1:8091/webhook"
    "http://ddns.042999.example:8090/webhook"
    "https://bot.429.expample/webhook"
)

# 需要解析 IP 的域名
DOMAIN_TO_RESOLVE="ddns.429.homo"

# 初始化失败计数器
NGINX_FAILURE=0
URL_FAILURE_COUNT=0
DNS_FAILURE=0

# --- 函数 ---

# Function to send message to DingTalk in Markdown format
send_dingtalk_markdown() {
    local title="$1"    # Markdown message title
    local text="$2"     # Markdown message content
    # Note: Markdown content might need careful escaping depending on content complexity

    # Construct the JSON payload for Markdown message
    # Using printf to handle potential special characters better than simple string concatenation
    # Escape double quotes within text for JSON
    local escaped_text=$(echo "$text" | sed 's/"/\\"/g')

    local payload=$(printf '{"msgtype": "markdown", "markdown": {"title": "%s", "text": "%s"}}' "$title" "$escaped_text")

    # Use curl to send the POST request to DingTalk webhook
    # -s Silent mode
    # -H Set Content-Type header
    # -X POST Specify POST method
    # -d Send data
    # > /dev/null Discard curl output
    curl -s -H "Content-Type: application/json" \
         -X POST \
         -d "$payload" \
         "$DINGTALK_WEBHOOK_URL" > /dev/null
}

# --- Main Script Logic ---

# Script log file path
LOG_FILE="/var/log/daily_check_script.log"
# Redirect stdout and stderr to the log file, and also to console if interactive
exec > >(tee -a "$LOG_FILE") 2>&1

echo "--- 脚本开始执行于 $(date) ---"

# Initialize report title and message body for Markdown
REPORT_DATE=$(date +"%Y-%m-%d %H:%M")
REPORT_TITLE="每日服务检查报告 - ${REPORT_DATE}"
REPORT_BODY="" # Initialize an empty string for the markdown body

# 1. Resolve Domain IP
echo "正在解析域名 ${DOMAIN_TO_RESOLVE} 的 IP 地址..."
REPORT_BODY+="## 域名解析检查\n\n" # Markdown level 2 heading

# Use dig +short to get A records, handle potential errors
RESOLVED_IPS=$(dig +short A ${DOMAIN_TO_RESOLVE} 2>/dev/null)
DIG_EXIT_CODE=$? # Capture dig exit code

if [ $DIG_EXIT_CODE -eq 0 ] && [ -n "$RESOLVED_IPS" ]; then # Check dig exit code and if output is not empty
    echo "${DOMAIN_TO_RESOLVE} 解析到的 IP 地址:\n${RESOLVED_IPS}"
    REPORT_BODY+="- **✅** 域名 \`${DOMAIN_TO_RESOLVE}\` 解析成功.\n"
    REPORT_BODY+="  解析到的 IP 地址:\n" # Indent for a sub-list or block
    # Add each resolved IP as a list item
    while IFS= read -r ip; do
        REPORT_BODY+="  - \`${ip}\`\n" # Markdown sub-list item
    done <<< "$RESOLVED_IPS"
else
    echo "警告: 无法解析域名 ${DOMAIN_TO_RESOLVE} 的 IP 地址。 Dig Exit Code: ${DIG_EXIT_CODE}"
    REPORT_BODY+="- **❌** 无法解析域名 \`${DOMAIN_TO_RESOLVE}\` 的 IP 地址，请检查 DNS 设置.\n"
    REPORT_BODY+="  Dig Exit Code: \`${DIG_EXIT_CODE}\`\n"
    DNS_FAILURE=1
fi

REPORT_BODY+="\n" # Add newline for separation


# 2. Restart Nginx
echo "正在重启 Nginx 服务..."
REPORT_BODY+="## Nginx 服务检查\n\n" # Markdown level 2 heading
sudo systemctl restart nginx
NGINX_RESTART_EXIT_CODE=$? # Capture restart command exit code

# Check Nginx restart status and add to report body using Markdown
if [ $NGINX_RESTART_EXIT_CODE -eq 0 ]; then # Check if restart command itself succeeded
    echo "Nginx 重启命令执行成功。"
    # Give Nginx a moment to start
    sleep 5
    if sudo systemctl is-active nginx; then # Check if service is active after restart
        echo "Nginx 服务处于活跃状态。"
        REPORT_BODY+="- **✅ Nginx 服务重启成功并运行中.**\n" # Markdown list item with bold text
    else
        echo "Nginx 重启命令执行成功，但服务未处于活跃状态。"
        REPORT_BODY+="- **❌ Nginx 服务重启成功但未运行起来，请检查！**\n" # Markdown list item with bold text
        # Capture more details if service is not active
        NGINX_STATUS_DETAIL=$(sudo systemctl status nginx --no-pager 2>&1 | head -n 10) # Capture first 10 lines of status
        REPORT_BODY+="  \`\`\`\n${NGINX_STATUS_DETAIL}\n\`\`\`\n" # Add status detail in a code block
        NGINX_FAILURE=1
    fi
else # Nginx restart command itself failed
    echo "Nginx 重启命令执行失败！Exit Code: ${NGINX_RESTART_EXIT_CODE}"
    REPORT_BODY+="- **❌ Nginx 服务重启命令失败，请检查！**\n" # Markdown list item with bold text
    REPORT_BODY+="  Exit Code: \`${NGINX_RESTART_EXIT_CODE}\`\n"
    # Capture more details even if command failed
    NGINX_STATUS_DETAIL=$(sudo systemctl status nginx --no-pager 2>&1 | head -n 10) # Capture first 10 lines of status
    REPORT_BODY+="  \`\`\`\n${NGINX_STATUS_DETAIL}\n\`\`\`\n" # Add status detail in a code block
    NGINX_FAILURE=1
fi

REPORT_BODY+="\n" # Add newline for separation

# 3. Get Server Resource Usage
echo "正在获取服务器资源使用情况 (内存/CPU)..."
REPORT_BODY+="## 服务器资源使用情况\n\n" # Markdown level 2 heading

# Get Memory Usage (Used/Total in MB)
# free -m output:
#               total        used        free      shared  buff/cache   available
# Mem:           7804        1234        5670          23         900        6100
# Swap:          1023           0        1023
MEM_INFO=$(free -m | awk 'NR==2{print $2,$3}') # total used
TOTAL_MEM=$(echo $MEM_INFO | awk '{print $1}')
USED_MEM=$(echo $MEM_INFO | awk '{print $2}')

if [ -n "$TOTAL_MEM" ] && [ -n "$USED_MEM" ]; then
    echo "内存使用情况: ${USED_MEM}MB / ${TOTAL_MEM}MB"
    REPORT_BODY+="- 内存使用: \`${USED_MEM}MB\` / \`${TOTAL_MEM}MB\`\n"
else
    echo "警告: 无法获取内存使用情况."
    REPORT_BODY+="- 内存使用: **❌ 无法获取**\n"
fi

# Get CPU Usage (%)
# Using top -bn1, get the line with %Cpu(s), then extract the idle percentage (typically 4th field for us, sy, ni, id...)
# Calculate CPU usage as 100 - idle
# Example output of top -bn1 | grep '%Cpu(s):'
# %Cpu(s):  0.3 us,  0.2 sy,  0.0 ni, 99.5 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
CPU_IDLE=$(top -bn1 | grep '%Cpu(s):' | awk '{print $4}' | sed 's/,//') # Extract 4th field, remove comma
CPU_USAGE="无法获取" # Default value

if [ -n "$CPU_IDLE" ]; then
    # Ensure CPU_IDLE is a number before calculating
    # Use printf to format the output to one decimal place
    if [[ "$CPU_IDLE" =~ ^[0-9]*(\.)?[0-9]+$ ]]; then
         CPU_USAGE=$(awk "BEGIN { printf \"%.1f\", 100 - $CPU_IDLE }") # Calculate and format to 1 decimal place
        echo "CPU 空闲率: ${CPU_IDLE}%, CPU 使用率: ${CPU_USAGE}%"
        REPORT_BODY+="- CPU 使用率: \`${CPU_USAGE}%\`\n"
    else
        echo "警告: 无法解析 CPU 空闲率: ${CPU_IDLE}."
        REPORT_BODY+="- CPU 使用率: **❌ 无法解析 (\`${CPU_IDLE}\`)**\n"
    fi
else
    echo "警告: 无法获取 CPU 使用情况."
    REPORT_BODY+="- CPU 使用率: **❌ 无法获取**\n"
fi


REPORT_BODY+="\n" # Add newline for separation
# 4. Check URLs
echo "正在检查 URL 可用性 (发送 POST 请求)..." # Update log message
REPORT_BODY+="## URL 可用性检查 (POST)\n\n" # Update Markdown heading

for url in "${URLS[@]}"; do
    echo "正在检查 URL: $url (发送 POST)" # Update log message
    # Use curl to check the URL by sending a POST request
    # -s Silent mode
    # -X POST Specify POST method
    # -H "Content-Type: application/json" Set Content-Type header (common for APIs)
    # -d "{}" Send a minimal empty JSON body (adjust if API requires specific data)
    # -o /dev/null discards the response body
    # -w "%{http_code}" prints the HTTP status code after the request
    # --connect-timeout 5 Connection timeout
    # --max-time 10 Total operation timeout
    HTTP_STATUS=$(curl -s -X POST -H "Content-Type: application/json" -d '{}' -o /dev/null -w "%{http_code}" --connect-timeout 5 --max-time 10 "$url" 2>/dev/null)
    CURL_EXIT_CODE=$? # Capture curl exit code

    # Check curl exit code AND if HTTP status code starts with 2 (2xx means success)
    # HTTP status 000 usually indicates connection error before getting any response
    if [ "$CURL_EXIT_CODE" -eq 0 ] && [[ "$HTTP_STATUS" =~ ^2 ]]; then
        echo "$url is reachable with status $HTTP_STATUS."
        REPORT_BODY+="- **✅** \`${url}\` - 访问成功 (HTTP Status: \`${HTTP_STATUS}\`).\n" # Use inline code for status
    else
        echo "警告: $url 不可访问. Curl Exit Code: $CURL_EXIT_CODE, HTTP status: ${HTTP_STATUS:-N/A}."
        REPORT_BODY+="- **❌** \`${url}\` - 访问失败. Curl Exit Code: \`${CURL_EXIT_CODE}\`, HTTP Status: \`${HTTP_STATUS:-N/A}\`.\n" # Show status even if 000 or empty
        URL_FAILURE_COUNT=$((URL_FAILURE_COUNT + 1)) # Increment failure counter
    fi
done

REPORT_BODY+="\n" # Add newline for separation

# 5. Add Summary
REPORT_BODY+="## 检查总结\n\n" # Markdown level 2 heading
TOTAL_URLS=${#URLS[@]} # Get total number of URLs

REPORT_BODY+="- 域名解析 (\`${DOMAIN_TO_RESOLVE}\`): "
if [ $DNS_FAILURE -eq 0 ]; then
    REPORT_BODY+="**✅ 成功**\n"
else
    REPORT_BODY+="**❌ 失败**\n"
fi


REPORT_BODY+="- Nginx 服务重启: "
if [ $NGINX_FAILURE -eq 0 ]; then
    REPORT_BODY+="**✅ 成功并运行**\n"
else
    REPORT_BODY+="**❌ 失败**\n"
fi


REPORT_BODY+="- URL 检查: **${URL_FAILURE_COUNT}** 个失败 (共 ${TOTAL_URLS} 个URL)\n"

# Add a concluding remark
REPORT_BODY+="\n检查完成，请根据报告内容及时处理异常。"

# 6. Send Report to DingTalk
echo "正在发送报告到钉钉 (Markdown 格式)..."

# If there are any failures, potentially change the title or add more emphasis (optional)
if [ $NGINX_FAILURE -ne 0 ] || [ $URL_FAILURE_COUNT -ne 0 ] || [ $DNS_FAILURE -ne 0 ]; then
    REPORT_TITLE="⚠️ 异常告警: 每日服务检查报告 - ${REPORT_DATE}" # Add warning icon to title
    # You could also add @ mentions here if needed, requires modifying send_dingtalk_markdown and payload
fi


send_dingtalk_markdown "$REPORT_TITLE" "$REPORT_BODY"
echo "报告已发送。"

echo "--- Script finished at $(date) ---"
```
