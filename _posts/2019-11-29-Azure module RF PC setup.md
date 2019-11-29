---
layout: post
title: Azure module RF PC setup
categories: AzureSphere
description: some word here
keywords: keyword1, keyword2
---

**MT3620 RF 测试电脑环境搭建**  

&emsp;&emsp;全新安装操作系统,windows 10 X64 中文专业版,操作系统版本为1607或更新，将电脑的防病毒威胁的实时保护关闭以激活系统.如果需要切换为英文系统且手头上只有中文系统安装盘时,安装后设定语言为英文即可.  
1.默认安装RF测试软件,解压IQfact+\_MTK\_3620\_MS\_WiFi\_USB\_3.3.0.Eng3\_Lock.zip  
2.安装[vs_community.exe](https://visualstudio.microsoft.com/zh-hans/vs/community/)，安装好后需要更新Visual Studio，在Visual Studio 2017菜单栏点帮助---检查更新，然后更新到15.7以后的版本  
3.保持联网状态下安装Azure\_Sphere\_SDK\_Preview\_for\_Visual\_Studio.exe,目前RF测试电脑SDK版本是19.05,时间稍长，耐心等待.注意装SDK FOR WINDOWS的版本时RF测试第1,2项无法通过.  
[**SDK for visual studio下载地址**](https://aka.ms/AzureSphereSDKDownload)  
[**SDK for windows下载地址**](https://aka.ms/AzureSphereSDK/Windows)  
[**备用地址**](https://docs.microsoft.com/en-us/azure-sphere/install/install-sdk#azure-sphere-sdk-preview-for-visual-studio)  
以上都不能下载请自行到微软[**官网**](https://www.microsoft.com/zh-cn/)搜索  
4.拷贝Manufacturing\_Drop\_19.05目录下整个RF Testing Tools目录到C盘根目录  
5.解压IQpathloss.zip到C:\LitePoint\IQfact_plus，最终目录树C:\LitePoint\IQfact_plus\IQpathloss\1.8.1，可创建IQpathloss 1.8.1.exe快捷方式到桌面  
6.拷贝更新的IQ\_Setup\_MT3620.ini到\BIN覆盖默认的  
7.拷贝更新的Serial_Mac.txt到\BIN覆盖默认的,缺少的话,RF测试结束时要求输入MAC的对话框不会弹出  
以上两个更新的文件可以在files\_in\_bin\_for\_4\_RF\_Ports\_05252019\_update\_region_US.zip中或其它压缩包中都可以找到.  
8.拷贝LITEPOINT提供的仪器LICENSE文件TesterId.DLL放到\BIN  
```
如果是临时版本：仪器厂商会给出XXX.LIC文件  
仪器默认是静态IP：192.168.100.254.可从仪器网口连接网线到PC或笔记本，在笔记本上配静态192.168.100.2.  
在浏览器上打开192.168.100.254页面，点击Admin,选择Admin tool,点Network。如果没有打开，则是flash插件没打开，点击后选always。  
将仪器IP设为DHCP，保存退出。断开笔记本上的网线并接到路由器。  
然后在路由器里查看仪器获取的实际IP。  
然后打开一个网页浏览器，输入新获取的IP后会进入仪器面板，如果没有打开，则是flash插件没打开，点击后选always.  
点击Admin,选择Admin tool,点license info  
点击Browse,然后选择加载由仪器商发过来的LIC文件  
点open,然后点upgrade按提示操作。  
升级后会自动重启，然后刷新页面即可看到升级后的license信息。  
```   
9.拷贝RF测试文件到\BIN,包含4个BAT脚本,4个TXT和1个RF测试主脚本  
10.可先在IQSTUDIO里用mt3620_check_test_setup.txt检查RF环境是否OK.如确保以上都成功完成,则RF测试系统搭建完毕,测试时需要关闭屏蔽箱.  
11.修改APP_ID，如果是RF1口在测试，这里的值改为1。如果是RF2口则改为2，但并不代表RF1就对应1，只是为了同其他端口不同ID即可，为了避免多端口同时测试RF产生冲突.改完后点界面上的保存按钮  
12.运行RF主测试脚本,程序会自动加载RF测试界面，找到CONNECT_IQ_TESTER,将仪器的IP地址改为192.168.1.109,这个地址是在路由器里看到的，如果变了，则改成实际获得的IP。注意格式是192.168.1.109:A  

**RF线损测量**  

&emsp;&emsp;线损测量工具，可直接将RF线连接到RF1和RF2形成loopback,按教程测量后会产生一个ref.csv.里面测得的值不是最终的实际线损值，因为这个测量工具默认是按-10dBm发的，所以要用-10减去这个值，比如ref.csv里显示量测到2412MHZ的值是-11.82，则真实线损为-10-（11.82）=1.82.其它类似都应这样计算。把更新了真实线损值的ref.csv拷贝到\BIN下，并重命名为path_loss.csv,这个文件的B,C,D,E四列分别对应RF1,RF2,RF3,RF4口,四列都需要填充.  

  

**常用命令**  

azsphere show-version  
azsphere login  
azsphere logout  
azsphere dev recover      //在线更新模块OS  
azsphere dev recover -i C:\test\images  //假定images目录在C:\test  
azsphere dev show-attached  
azsphere dev show-ota-status  //必须在联网状态下执行,离线会报错  
azsphere dev show-os-version  //必须在联网状态下执行,离线会报错  
azsphere dev cap show-attached  
azsphere dev wifi scan  
azsphere dev wifi show-status  
azsphere dev manufacturing-state show  

cd C:\RF Testing Tools\RfToolCli  
RfSettingsTool.exe show  
RfSettingsTool.exe show -hexdump  

**备注:**  
&emsp;&emsp;如果发现RF测试FAIL时MAC地址也能写入，则很有可能是IQFACTSTUDIO里设定问题，定位到FINALIZE EEPROM这项，右键选择RUN MODE，选择SKIP ON FAIL.  
&emsp;&emsp;测试出现error:could not delete component 'cbcf6b97-cdda-4cb9-a2b8-24a9a35ba408': Operation not permitted; application development capability is required.........时是因为模块19.05这版OS的BUG,其它版本无此问题.  
 
