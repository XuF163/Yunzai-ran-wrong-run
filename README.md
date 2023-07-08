# Yunzai挂机指北  :从入门到跑路  

> 这是一个硬件向的bot挂机教程，适合新手阅读  
> 有时间就更新，可能会介绍到卸崽跑路之后的问题  
> 转载请注明出处并并保留此仓库链接即可      
>**404**:如果所在地网络不佳，可尝试访问本人的gitee[镜像站仓库](https://gitee.com/xyzqwefd/Yunzai-ran-wrong-run)
## sight to get服务器
### 硬件参考指标
> 挂机器人不怎么挑设备，以下是本人建议的硬件性能指标，仅供参考

CPU:核心数量>=2 ，建议主频>=1.6Ghz  
内存RAM:2G即可，如果需要同时运行nonebot则建议4G起步  
存储：预留30G左右即可满足目前全部插件安装需求  
网络:大厂1M的公网专线和多数自营小IDC提供的共享10M带宽速度基本一致，目前市面上的服务器和家用宽带都能满足  

### 软件运行环境
云服务器优先推荐Ubuntu20或更高版本，家庭及宿舍场景为保证维护性建议win server 2019或更高版本  
**注意:** 部分云厂商可能会对win系统收取额外费用,同时系统负载也会更大,在价格高昂的云服务器上浪费大量算力运行微软的杀毒进程未免过于浪费  


### 购买建议  
#### 云服务厂商  
优先白嫖大厂的试用产品，后续可购买新人特价机（以百元/年）为宜。  
常见可白嫖厂商：  
|厂商名称|时长|需要实名|需要绑卡|体验|  
|-----|-----|-----|-----|-----|  
|阿里云|3+7+n|✓||7+n学生机真香|  
|华为云|3|✓||win要加钱|  
|腾讯云|1|✓||销号后注册有惊喜|  
|AWS、azure国际版||✓|卡有一定门槛|  
**警告**：不建议使用任何所谓NAT服务器、挂机宝等产品，如果实在要购买建议按**最低支付周期**缴费并且 **保护好个人隐私信息**  
#### 物理机  
推荐使用软路由类**低功耗小主机**作为家用服务器按需购买即可。  
>如果你只是为了挂崽，海鲜市场100-200的二手产品（例如J1900的瘦客户端）就能很好满足你的需求
目前较受欢迎U本人建议认准N100即可，J4125、N5105在2023年6月后并不非常值得够购买（笔者就是这样的怨种）
**关于只因架服务器**:考虑到功耗问题，普通家庭不建议上此类大型服务器（家里有矿可以忽略这条）

## 日常运行  
### 平台与群组  
目前QQ频道、KOOK频道、米游社大别野以及web版云崽都是可以正常稳定运行的，可放心挂机。  
如果你仍然试图部署QQ/微信群bot，请确保你的账号安全并尽量**不要让陌生人偷你的机器人**。
### 自动化维护  
结合崽目前可能存在的一些不稳定性以及部分私有云管理员面临定期遭遇恶意断电的场景（如学校宿舍等）常见问题，可参考以下教程优化bot维护和使用体验。  
#### winserver平台
可以通过创建bat批处理文件搭配**开机启动项**和**自动任务**处理  
以下演示在winserver2019下进行
##### 添加开机启动项
按下win + r
![image](https://github.com/XuF163/Yunzai-ran-wrong-run/assets/121853480/d4b2f566-7f31-4a14-a33e-f295ccd30470)  
输入以下路径并点击确定按钮  
```
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp

```
在此目录下可以通过添加快捷方式实现开机自启动  
![image](https://github.com/XuF163/Yunzai-ran-wrong-run/assets/121853480/69ff1f6c-5015-40f7-808c-ba8926a95dcb)  
#### 添加计划任务  
打开服务器管理器  
![image](https://github.com/XuF163/Yunzai-ran-wrong-run/assets/121853480/014c1753-cddd-4eb3-a943-5b4de1b4c9ae)  
点击工具-任务计划程序  
![image](https://github.com/XuF163/Yunzai-ran-wrong-run/assets/121853480/937f5407-6075-4fe3-bab8-c0d6100e4dc6)
在此处可以自行配置任务  
![image](https://github.com/XuF163/Yunzai-ran-wrong-run/assets/121853480/9ef365c0-16e6-4850-87be-773cf0c4b046)  
**以下**是几个本人常用的bat源码，可以**照抄**直接用，只需要新建txt-复制内容-修改后缀为bat即可    
**开机自启动**  
```
start C:\Program" "Files\Git\git-bash.exe --cd=C:\Users\Administrator\Desktop\Miao-Yunzai -c "node app"
```
>如果你使用默认设置安装git，那么直接使用此指令即可，否则请替换对应的文件路径。

**git闪退自动处理**
```
:start
choice /t 300 /d y /n >nul  #300可修改为检测间隔，可任意修改
tasklist|find /i "git-bash.exe" 
if %errorlevel%==0 ( 
	echo "yes"
) else (
	echo "No" 
	start Yzstart.bat  #Yzstart对应上一个脚本的文件名称
)
goto start
```
**注意**：需要将你写的bat的**快捷方式**加入计划任务/开机启动项才能启用你的维护脚本。  
### Linux  
这玩意稳得很，一般不需要单独的维护操作。
### docker  
推荐使用[trss-scripts](trss.me)进行管理，请确保你有足够的资源冗余来运行trss。









