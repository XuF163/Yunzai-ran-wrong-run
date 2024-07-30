# Bot挂机指北  :从入门到跑路  
![image](https://socialify.git.ci/XuF163/Yunzai-ran-wrong-run/image?description=1&font=Inter&issues=1&language=1&name=1&owner=1&pattern=Signal&pulls=1&stargazers=1&theme=Light)

> 这是一个硬件向的bot挂机教程（家里云内容居多），供新手阅读以及参考。
> 适用于适合Yunzai及其衍生项目挂机场景
> 转载请注明出处并并保留此仓库链接即可      
>**404**:如果所在地网络不佳，可尝试访问的gitee[镜像站仓库](https://gitee.com/xyzqwefd/Yunzai-ran-wrong-run)
----------------------------------------------------------------------------------------------------
> 交流、建议与吹水:没什么好交流的
----------------------------------------------------------------------------------------------------  
### 目录 
[硬件参考指标](#硬件参考指标)  
[软件运行环境](#软件运行环境)  
[购买建议](#购买建议)  
[初始化与自动维护配置](#初始化与自动维护配置)  
[网络与防火墙](#网络与防火墙)  
[多开、托管与代挂](#多开、托管与代挂)  
[集群化与批量部署](集群部署实现.md)  
[数据安全与代码审查](安全策略与内容审查.md)  
[关联阅读](#关联阅读)
## 服务器获取
### 硬件参考指标
> 挂机器人不怎么挑设备，以下是本人建议的硬件性能指标，仅供参考

CPU:核心数量>=2 ，建议主频>=1.6Ghz  
内存RAM:2G即可，如果需要同时运行nonebot则建议4G起步  
存储：预留30G左右即可满足目前全部插件安装需求  
网络:大厂1M的公网专线和多数自营小IDC提供的共享10M带宽速度基本一致，目前市面上的服务器和家用宽带都能满足  

### 软件运行环境
云服务器优先推荐Ubuntu20或更高版本，家庭及宿舍场景为保证维护性建议win server 2019或更高版本  
**注意:** 部分云厂商可能会对win系统收取额外费用（华为云的找客服可以免掉）,同时系统负载也会更大,在价格高昂的云服务器上浪费大量算力运行微软的杀毒进程未免过于浪费  


### 购买建议  
#### 云服务厂商  
更新：阿里99元/年的鸡，可以按照99元一年的价格续费到2026年
已知问题：不能叠加300元/年的带学生优惠
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

## 初始化与自动维护配置  
### 平台与群组  
  
如果你仍然试图部署QQ/微信群bot，请确保你的账号安全并尽量**不要让陌生人偷你的机器人**。  
**请勿相信售卖QQBot开发权限的行为，谨防此类诈骗行为**
### 自动化维护  
结合崽目前可能存在的一些不稳定性以及部分私有云管理员面临定期遭遇恶意断电的场景（如学校宿舍等）常见问题，可参考以下教程优化bot维护和使用体验。  
#### winserver平台  
2024-5更新 ：WebStorm真好用啊，内存够的话下面的内容就不要看了，直接用WebStorm即可
![img.png](img.png)
此外，也可以通过创建bat批处理文件搭配**开机启动项**和**自动任务**处理  
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
这玩意稳得很，一般不需要单独的维护操作。**后面有空再补一个定时脚本介绍吧**  
对Linux服务器而言SSH客户端关闭后，崽就会自动关闭，这里推荐使用screen 和tmux 进行进程保活。

（如果你使用手机部署，请勿参考）

screen

创建新终端并启动
```
scren -S  bot#bot可替换为任何你觉得好记的名字
cd到云崽目录，启用redis持久化...
node app
```
到这里就可以安心退出SSH客户端了，除非卡死或者风控，你的崽会继续运行

回到进程
```
screen -r bot #bot可替换为任何你觉得好记的名字  
如果失败了，则可能是由于你的本地终端异常关闭导致的
screen -ls #查看进程 id
screen -r -d +id号即可返回崽窗口
```
tmux

安装命令
```
git clone https://github.com/tmux/tmux.git
cd tmux
sh autogen.sh
./configure && make
```
或者
```
# Ubuntu 或 Debian
$ sudo apt-get install tmux

# CentOS 或 Fedora
$ sudo yum install tmux
```
启动

tmux

退出
```
exit
```
创建新终端并启动
```
tmux new -s bot#bot可替换为任何你觉得好记的名字 
cd到云崽目录，启用redis持久化...
node app
```
返回
```
tmux attach -t bot#bot可替换为任何你觉得好记的名字
```
shell脚本编写

点击此[链接](zhuanlan.zhihu.com/p/264346586)查看
### docker  
推荐使用[trss-scripts](trss.me)进行管理，请确保你有足够的资源冗余来运行trss。  
如果你尝试使用dockerfile构建，请注意权限问题。 
推荐拉现成的docker，但请**谨慎使用来历不明的镜像**   

此外，使用dockerfile构建可能产生权限问题。

## 网络与防火墙  
>如果你有使用锅巴面板、GPT网页控制台或者MCSM等web服务进行操作，那么请看这里。
### 公有云  
跟随厂商指引即可  
### 私有云  
如果你初次配置家里云并想要远程管理，那么请部署内网穿透以实现公网对服务器的访问，详情请阅读以下内容。  
#### frp  
frp是最通用实现内网穿透策略，容易上手，但是存在着**流量价格昂贵**、**带宽小**等弊端，当然，如果你之手偶尔需要，**樱花frp**等就非常适合你（相关服务商请自行百度，此处仅为举例，不构成购买建议）。  
#### 云+本地组网  
建议使用zerotier，[文字教程](https://zhuanlan.zhihu.com/p/422171986)  
参考视频：（请勿相信任何广告）
[基于云服务器的frp搭建](https://www.bilibili.com/video/BV1dr4y147aq/?share_source=copy_web&vd_source=e85480d7b3bbd3f29920de3475ea939f)
#### DDNS  
>如果你有公网ip(v4均可v6)并且构建了家里云，那么可以通过以下教程实现对服务器的远程访问
>当然嘞，你需要先**了解你的ip**
***参考链接***：
##### [怎么知道自己有没有ipv4公网地址](https://www.zhihu.com/question/288053819)  
##### [怎么知道家里有没有ipv6](https://www.test-ipv6.com/index.html.zh_CN)  


1.获取域名  
常见的域名服务商包括花生壳、腾讯、阿里等云服务厂商，通常你可以用极低的价格购买到好记有趣的域名，当然，一些路由器厂商可能也提供了免费的二级域名供DDNS使用  
以TP-LInk为例，登录路由器后台管理界面，在**应用管理** -**DDNS**选项中可以快速自定义一个二级域名  
![image](https://github.com/XuF163/Yunzai-ran-wrong-run/assets/121853480/8c6dd2c4-516e-4713-b727-2a4d89321918)
![image](https://github.com/XuF163/Yunzai-ran-wrong-run/assets/121853480/1a942045-68a3-405f-abd9-21ad003ea86e)  
如果你的供应商没有提供免费的域名，则可以前往[花生壳域名]（https://domain.oray.com/）购买，国内多数路由器厂商都提供了对花生壳域名的支持。  
当然嘞，如果您已经在其它平台购买了域名，则可以尝试使用ikuai或opnewrt等路由系统实现动态解析  

2.解析与绑定  
无论您使用了何种网络，一般而言，我们都建议开启**网络桥接**，这会使得操作更加得心应手。  
##### ipv4  
1.绑定内网ip  
请确认您的服务器在内网中已经开启桥接，如果开启了DHCP服务，则需设置一个固定的ip （推荐使用ip-MAC地址映射而**不推荐**使用静态路由表） 。
![image](https://github.com/XuF163/Yunzai-ran-wrong-run/assets/121853480/4a68d7fe-de9c-49e4-8140-c8bc71a31d9a)
![image](https://github.com/XuF163/Yunzai-ran-wrong-run/assets/121853480/94d0c942-3b98-4cfd-89d9-aae6020d0f6f)
以下是tplink对此功能的一些描述  
![image](https://github.com/XuF163/Yunzai-ran-wrong-run/assets/121853480/5f51e458-8138-46f0-889c-0a6cc8bf47a9)
2构建虚拟服务器与端口映射  
![image](https://github.com/XuF163/Yunzai-ran-wrong-run/assets/121853480/28f365a9-c45e-4fb6-8cd7-879ee6860e9d)
（此图在tplink的应用管理-虚拟服务器中可以找到）  
将你的家里云的内网ip和端口映射到公网的一个接口，酱紫就实现了外网访问的功能。  

**exp**
你的家里云
```
192.168.0.109:50831#经典的锅巴端口
```
从50831映射到5050
而你的域名  
```
crzryThrusdayVivo50.kfcddns.vip
````
已经设置好了映射  
则你在运营商流量环境或连接外界宽带访问锅巴时所用的地址：  
```
crzryThrusdayVivo50.kfcddns.vip:5050
```
就可以实现类似在家中访问的效果  
**PS**:家宽的上传带宽普遍能达到30Mbs的高速，因此往往能获得良好的使用体验  
       但是，**一些特殊端口**（如80 443）无法使用  
###### 安全第一 !   
使用ipv4做映射时请尽量只用数值较高的端口号（不超过65535即可）和**高强度的密码**确保设备安全  
一些《回本业务》存在着一些风险，请珍惜你的v4公网ip，请勿用于商用场景，否则可能有被运营商封禁的风险并可能承担其它连带责任   
##### ipv6 
>一些常识：
>ipv6的[简单介绍](https://zhuanlan.zhihu.com/p/382612295)

如果你购买了域名，请参考以下教程部署：  
[白嫖阿里云DNS构建内网穿透](https://www.zhihu.com/tardis/zm/art/555478866?source_id=1005)  


如果你不想购买域名，则可[通过脚本实时获取你的v6公网Ip](https://www.jianshu.com/p/afe8c2b476ff)   
PS:**访问ipv6地址需要加个中括号，例如：  
```
[2001:0db8:3c4d:0015:0000:0000:1a2f:1a2b]:2526 #这只是一个栗子，并不是一个有效地址
```

**如果以上内容对你而言仍然过于繁琐，请使用Win server家里云版本并安装向日葵，todesk等自带frp的桌面控制软件**  
**体验可能不佳，但白嫖绝对快乐**  

# 多开、托管与代挂  
>注意到Karin/yunzai的lain-plugin/喵版yunzai的trss分支提供了多适配器选项，在节约性能的前提下达到了既定目的     

以下是一个早期的、基于文件实现数据隔离的多开教程
如果你需要多开云崽，请先按需创建Yunzai（名称可能因分支而异）文件夹的副本来到以下目录：
```
Yunzai-Bot\config\config
```

找到redis.yaml，里面应该有以下内容：
```
# redis地址
host: 127.0.0.1
# redis端口
port: 6379 
# redis密码，没有密码可以为空    
password: 
# redis数据库    
db: 0
```
在云崽的第一个副本中db：0应该改为db:1只要不冲突即可，单一redis最多支持16个数据库，也就是说，通过db号的修改（从0到15）你最多可以在一个完整的数据库中创建16个崽





### 关联阅读   
[Yunzai](https://gitee.com/yhArcadia/Yunzai-Bot-plugins-index)  
[Karin](https://karinjs.github.io/Karin)






