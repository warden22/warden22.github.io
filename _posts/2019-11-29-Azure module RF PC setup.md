---
layout: post
title: Azure module RF PC setup
categories: AzureSphere
description: some word here
keywords: keyword1, keyword2
---

MT3620 RF 测试电脑环境搭建  

全新安装操作系统,windows 10 X64 中文专业版,操作系统版本为1607或更新，将电脑的防病毒威胁的实时保护关闭以激活系统  
如果需要切换为英文系统且手头上只有中文系统安装盘时,安装后设定语言为英文即可.  
默认安装RF测试软件,解压IQfact+\_MTK\_3620\_MS\_WiFi\_USB\_3.3.0.Eng3\_Lock.zip  
安装[vs_community.exe](https://visualstudio.microsoft.com/zh-hans/vs/community/)，安装好后需要更新Visual Studio，点帮助---检查更新，然后更新到15.7以后的版本  
保持联网状态下安装Azure\_Sphere\_SDK\_Preview\_for\_Visual\_Studio.exe,目前RF测试电脑SDK版本是19.05,时间稍长，耐心等待.注意装SDK FOR WINDOWS的版本时RF测试第1,2项无法通过.  
[SDK for visual studio下载地址](https://aka.ms/AzureSphereSDKDownload)  
[SDK for windows下载地址](https://aka.ms/AzureSphereSDK/Windows)  
[备用地址](https://docs.microsoft.com/en-us/azure-sphere/install/install-sdk#azure-sphere-sdk-preview-for-visual-studio)  
以上都不能下载请自行到微软官网搜索  
拷贝Manufacturing\_Drop\_19.05目录下整个RF Testing Tools目录到C盘根目录  
解压IQpathloss.zip到C:\LitePoint\IQfact_plus，最终目录树C:\LitePoint\IQfact_plus\IQpathloss\1.8.1，可创建IQpathloss 1.8.1.exe快捷方式到桌面  
拷贝更新的IQ\_Setup\_MT3620.ini到\BIN覆盖默认的  
拷贝更新的Serial_Mac.txt到\BIN覆盖默认的,缺少的话,RF测试结束时要求输入MAC的对话框不会弹出  
以上两个更新的文件可以在files\_in\_bin\_for\_4\_RF\_Ports\_05252019\_update\_region_US.zip中或其它压缩包中都可以找到,这个文件在服务器上有保存.  
拷贝LITEPOINT提供的仪器LICENSE文件TesterId.DLL放到\BIN  
拷贝RF测试文件到\BIN,包含4个BAT脚本,4个TXT和1个RF测试主脚本  
可先在IQSTUDIO里用mt3620_check_test_setup.txt检查RF环境是否OK.如确保以上都成功完成,则RF测试系统搭建完毕.  


在查看模块系统版本时必须联网,在离线状态下会报错,指令为  
azsphere dev show-ota-status  
新版SDK(如19.10版本)对应上述命令改为azsphere dev show-os-version  
