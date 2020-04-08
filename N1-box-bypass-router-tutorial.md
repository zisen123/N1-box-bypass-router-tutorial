[1]: <https://www.youtube.com/watch?v=kUb1C_g18yU>
[2]: <https://wxf2088.xyz/archives/321>
[3]: <https://www.right.com.cn/forum/?285101>
[4]: <https://www.right.com.cn/forum/>
[5]: <https://www.balena.io/etcher/>
[6]: <https://drive.google.com/open?id=1qrAh0RXZoCHy8OL3WG5FoIrVYsEBlWMY>
[7]: <https://drive.google.com/open?id=1fgb8f7epe-s4kJzKyQCa2Oes4vYkB1el>
# N1盒子旁路由教程面向小白啰嗦版
>[参考](#参考)  
[主要目的](#主要目的)  
[一些需要用到的东西](#一些需要用到的东西)  
[对N1盒子进行降级以及激活U盘启动](#对N1盒子进行降级以及激活U盘启动)  
[将N1固件烧录至U盘并使用U盘启动openwrt](#将N1固件烧录至U盘并使用U盘启动openwrt)  
[将固件刷入N1盒子的emmc并更改N1盒子的内网IP](#将固件刷入N1盒子的emmc并更改N1盒子的内网IP)  
[增加WAN接口以及更改LAN接口配置](#增加WAN接口以及更改LAN接口配置)  
[对于需要N1旁路由转发流量代理的设备进行设置](#对于需要N1旁路由转发流量代理的设备进行设置)  
[总结](#总结)  
[转载说明](#转载说明)

## 参考
>[王晓峰录制的单臂路由教程视频][1]  
>[王晓峰部落阁上的单臂路由文字教程][2]  
>[恩山论坛巨佬Flippy][3]  
>[恩山无线论坛][4]

## 主要目的
在拥有一个**主路由**的情况下将**N1盒子**作为**旁路由**, 再在手机电脑上设置网关让流量经过N1盒子转发,达到透明代理的效果.   
经过测试,N1盒子可以运行v2ray转发500M宽带的流量, 更大的带宽尚未测试. *也可能是我不知道*  
此方案不需要对主路由器做任何改动, 不会影响到共用路由器的他人上网体验, 适合多人共用路由器时个人有代理转发需求.

## 一些需要用到的东西
1. 一台Windows电脑 *Mac用户可以使用虚拟机*
2. 一个大于或等于2G的U盘 
3. 一个N1盒子 *在拼多多可以买到二手货*
4. 一个正常运行的主路由器 *对配置没有要求,能满足当前带宽即可*
5. 一根网线 *用于连接主路由和N1盒子*
6. ~~双公头数据线~~ *Flippy最新的固件可以通过ssh连接后使用指令刷入emmc*
7. 显示器 *电视也可以*
8. HDMI视频数据线
9. 键盘和鼠标 *没有键盘也行*
10. [Flippy制作的N1盒子openwrt固件][6]
11. [N1盒子降级工具以及激活U盘启动工具][7]
12. U盘烧录工具 *推荐使用[balenaEtcher][5]*

## 对N1盒子进行降级以及激活U盘启动
大部分拿到手的N1盒子除非商家特别说明一般都只装了原版系统, 但是要激活从U盘启动的话就必须要把N1盒子降级到对应的版本.  
下面是使用降级工具降级的操作步骤: *本步骤图片截图自王晓峰的YouTube视频*
1. 用HDMI视频数据线将N1盒子和显示器连接起来, 并使用鼠标和键盘将N1盒子连接到主路由的`WiFi`, 同时记下上面显示的IP地址.
   ![N1盒子默认显示界面](https://pic.downk.cc/item/5e8d4637504f4bcb04dc0baf.png)
2. 点击四下固件版本, 屏幕上提示打开ADB  
   ![打开ADB](https://pic.downk.cc/item/5e8d46d1504f4bcb04dc8c59.png)
3. 打开电脑,运行降级工具包里的`run.bat`, 输入数字<kbd>2</kbd>并<kbd>Enter</kbd>,随后输入刚刚记下的IP地址,最后按任意键开始降级 *注意此时电脑要保持联网状态,因为降级工具需要联网获取文件* .
4. 降级成功后运行激活U盘启动工具包里的`N1盒子激活U盘启动.bat`,并输入刚刚记下的IP地址即可激活U盘启动.

## 将N1固件烧录至U盘并使用U盘启动openwrt
这里推荐使用[balenaEtcher][5]来进行烧录: 将U盘插入电脑, 选择下载好的固件和U盘, 最后`Flash!`就完事了.
![](https://pic.downk.cc/item/5e8d4c4e504f4bcb04e0d59b.png)  
烧录完成之后将U盘插入N1盒子, 接上电源即可在U盘启动openwrt系统

## 将固件刷入N1盒子的emmc并更改N1盒子的内网IP
1. 将固件刷入N1盒子的emmc  
   我们想要N1盒子长久运行的话总不能一直插着个U盘吧, 既不安全也不稳定, 没准哪天U盘就崩掉了, 所以我们需要把固件刷到N1自带的8G存储里面, 感谢Flippy大神的固件让我们可以一键刷入而不像以前一样需要用到双公头数据线来刷机.  
   方法如下:
   >N1盒子插上U盘, 接上显示器, 插上键盘后插电开机, 看见以下画面之后分行输入  
   >```
   >cd /root
   >./inst-to-emmc.sh
   >```
   >![](https://pic.downk.cc/item/5e8d55ad504f4bcb04e78743.png)  
   等待刷机完成即可进行下一步
   
1. 更改N1盒子的内网IP  
   这里需要注意两个事情:  
- 如果你的光猫或者主路由的IP是`192.168.1.1`, 那么需要更改N1盒子的IP或者更改光猫或主路由的IP以避免冲突.  
- 如果你的主路由器的IP**不是**`192.168.1.x`, 那么你需要更改N1盒子的IP至主路由器的同一个网段. 例如你的主路由器的IP为`192.168.0.1`, 那么N1盒子的IP就应该是`192.168.0.x`并且`x!=1`.
>如何确认我的光猫或者主路由的IP?  
>只需要在电脑浏览器的地址栏输入`192.168.1.1`, 如果出现了登陆界面,那么说明你需要更改IP.

有两种更改N1盒子IP的方法: *务必都看一看*
- **直接连接键盘修改openwrt的网络配置文件** *该方法需要自备键盘* *为了便于显示清晰我截的是`PowerShell`的图*
   >键盘连接N1, N1接上显示器, 插上已经刷入固件的U盘, 接上电源启动N1, 显示内容如图 *正常情况下会出现图中的LOGO*
   ![](https://pic.downk.cc/item/5e8d55ad504f4bcb04e78743.png)  
   键盘输入`vi /etc/config/network`后<kbd>Enter</kbd>, 如下图
   ![](https://pic.downk.cc/item/5e8d5849504f4bcb04e9af22.png)  
   将光标移动到`option ipaddr '192.168.1.1'`, 按<kbd>i</kbd>进入编辑模式, 将`192.168.1.1`更改为`192.168.x1.x2`, `x1`取决于你的主路由的IP, 假设主路由IP为`192.168.1.x`, 那么`x1=1`, `x2`可以选择`254`这个数字来避免大部分的冲突. *前提是你没有把其他设备IP设置为`192.168.x1.254`*  
   简单点说就是N1的IP必须要和主路由同一个网段并且与其他设备的IP不同.  
   ![](https://pic.downk.cc/item/5e8d5b40504f4bcb04ec927d.png)  
   *图中显示为`192.168.0.254`是我修改后的结果,正常情况下会显示为`192.168.1.1`*  
   修改好之后按<kbd>Esc</kbd>退出编辑模式, 最后输入`:wq`来保存并退出  
   这样就修改好了N1盒子的IP地址
   
- **将N1盒子通过网线或者WiFi直连电脑进行更改**
  >N1盒子从U盘启动之后一般会自动开启一个名为`OpenWrt`的开放式WiFi, 电脑断开其他网络连接之后连接到这个WiFi, 没有WiFi的电脑可以用网线直连N1盒子和电脑.  
  连接上之后按下<kbd>Win</kbd>+<kbd>x</kbd>, 选择`Windows PowerShell`, 输入`ssh root@192.168.1.1`后<kbd>Enter</kbd>, 如图
  ![](https://pic.downk.cc/item/5e8d5db2504f4bcb04ee79e5.png)
  接下来的步骤与上一个方法相同, 不再赘述.

  >如果电脑没有自带ssh的话可以用浏览器访问`192.168.1.1`, 出现如下登陆界面, 输入默认密码`password`  
  ![](https://pic.downk.cc/item/5e8d611a504f4bcb04f15d93.png)  
  登录后在左侧菜单栏里面找到`网络-接口`, 对LAN进行编辑  
  ![](https://pic.downk.cc/item/5e8d6153504f4bcb04f19f11.png)  
  将红框内的IP改为`192.168.0.254` *根据你的实际情况更改*
  ![](https://pic.downk.cc/item/5e8d61c8504f4bcb04f20f45.png)


## 增加WAN接口以及更改LAN接口配置
更改N1盒子的内网IP成功之后, 将N1盒子用网线连接到主路由, 浏览器访问`192.168.0.254`, 输入默认密码`password`后即可登录, 接下来就是增加WAN接口和修改LAN接口
1. 增加WAN接口
   >首先来到`网络-接口`,点击`添加新接口`
   >![](https://pic.downk.cc/item/5e8d70fc504f4bcb04009fda.png)
   >如图进行设置
   >![](https://pic.downk.cc/item/5e8d70fc504f4bcb04009fe1.png)
2. 修改LAN接口
   >如图进行设置即可
   >![](https://pic.downk.cc/item/5e8d730f504f4bcb0402af03.png)
   >![](https://pic.downk.cc/item/5e8d730f504f4bcb0402af08.png)
   >![](https://pic.downk.cc/item/5e8d730f504f4bcb0402af13.png)
   >![](https://pic.downk.cc/item/5e8d730f504f4bcb0402af18.png)
   >最后记得点击`保存并应用`

## 对于需要N1旁路由转发流量代理的设备进行设置
- 电脑
  >首先连接到主路由的网络, 打开`Control Panel\Network and Internet\Network Connections`, 如图进行操作  
  >![](https://pic.downk.cc/item/5e8d7a6f504f4bcb04096bb1.png)  
  >![](https://pic.downk.cc/item/5e8d7a95504f4bcb04098eae.png)  
  记住下面这张图里的`IPv4 Address`  
  >![](https://pic.downk.cc/item/5e8d7ae1504f4bcb0409da2a.png)  
  >![](https://pic.downk.cc/item/5e8d7a95504f4bcb04098eb2.png)  
  这里`IP Address`里面填刚刚记下的`IPv4 Address`  
  >![](https://pic.downk.cc/item/5e8d7a95504f4bcb04098eb5.png)  
  最后`OK`即可生效
- 手机 *这里我以我自己的vivo手机为例, 其他手机包括iPhone设置大同小异*
  >点击进入WiFi详情页进行设置  
  >![](https://pic.downk.cc/item/5e8d7caa504f4bcb040b6a3a.jpg)  
  和电脑上类似,先记下原来的`IP地址`, 然后开启`静态IP`, 将`网关`改为`192.168.0.254`  
  >![](https://pic.downk.cc/item/5e8d7d1a504f4bcb040bcab4.jpg)

## 总结
本文从拿到N1开始进行降级, 激活U盘启动, 刷入emmc, 到更改N1盒子网络设置, 成功地实现了把设备流量转发给N1盒子处理, 再配合固件里面自带的插件即可实现透明代理上网, 如果出现了意料之外的问题, 欢迎提出issue讨论.

## 转载说明
此文为Zisen原创, 虽有许多参考之处但都获得了原文作者的许可, 如需转载请注明出处和作者, 本文地址为https://github.com/zisen123/N1-box-bypass-router-tutorial/blob/master/N1-box-bypass-router-tutorial.md