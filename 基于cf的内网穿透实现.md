##面向水管较大且无法拿到公网ip的用户  
### 此方案具备以下优点；  
1.免费，白嫖一个公网ip  
2.绕美，可实现家里云建站免备案  
### 可能存在的问题？  
SSH穿透不出去，可能是我操作不当导致的，欢迎指正

## 操作演示  
>服务端下载以及授权
```sh
curl -L 'https://ghproxy.org/https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64' -o ./cloudflared
chmod +x ./cloudflared  
```  

![image](https://github.com/XuF163/Yunzai-ran-wrong-run/blob/main/QQ%E6%88%AA%E5%9B%BE20240223095046.png)
