---
title: Mac MotoX
date: 2017-05-15 05:18:19
tags: [mac, motox, life]
---
用Mac给MotoX刷机

### 解锁BL
1. 安装fastboot
```
brew cask install android-sdk
```
2. 下载并安装moto USB驱动  
http://www.motorola.com/getmdmmac
3. 进入手机设置中的开发者模式，勾选“允许OEM解锁”
4. 关机，同时按音量向下和开机3秒，松开，进能进入fastboot
5. 连接手机和电脑，从命令行输入
```
fastboot oem get_unlock_data
```
6. 复制解锁数据
7. 进入moto解锁页面并登录  
https://motorola-global-portal.custhelp.com/app/standalone/bootloader/unlock-your-device-b
8. 输入解锁数据，并按Request Unlock Key
9. 进入登录moto的邮箱，复制unlock key(e.g. 123456)
10. 在电脑的命令行输入, 替换123456为自己的unlock key
```
fastboot oem unlock 123456
```
11. 重启
```
fastboot reboot
```

参考 http://bbs.ihei5.com/thread-385849-1-1.html

### 安装TWPR
1. 下载MotoX对应的TWPR  
https://dl.twrp.me/victara/
2. 进去fastboot模式，并连接上电脑
3. 打开命令行并进去下载twrp到目录，刷入TWPR，替换twrp-3.1.0-0-victara.img为对应下载的twrp
```
fastboot flash recovery twrp-3.1.0-0-victara.img
```
4. 遇到粉红色警告dismatch partition size(recovery)，忽视之

### 刷入lineageOS
1. 下载支持国行xt1085的包，支持电信4g  
下载地址参考 http://bbs.ihei5.com/thread-743540-1-1.html
2. 进入fastboot，选择recovery
3. 选择wipe - advanced wipe - 全部打勾 -swipe to wipe. 
*由于没有外接otg，otg会显示error，忽视之.*
*此步骤会把原有系统都清除掉，重启会不能进入手机，只能进入fastboot，谨慎对待*
4. copy rom包到手机, 用adb push命令
*Mac下，无法识别android，无法直接copy.*
*手机进入fastboot，然后连接电脑，命令行输入adb devices，无输出；然后选择recovery，再从命令行输入adb devices，有输出recovery*
```
adb push lineage-14.1-20170430-UNOFFICIAL-victara.zip /sdcard/.
```
5. 从recovery页面选择install ,然后点击刚刚复制进去的rom包安装即可
6. 等待开机即可，由于是第一次开机所以会比较慢一点。
*默认关闭root,开启root方法为在开发者选项里的“Root授权”，选择仅限于adb即可*

### 全局root
1. 下载root卡刷包  
http://www.lineageosrom.com/2017/01/download-supersuzp-and-su-removalzip.html， 选择addonsu-arm-signed.zip
2. 进入recovery并刷入， 就可以在开发者选项修改了

### 去除网络的感叹号
```
adb shell "settings put global captive_portal_https_url https://captive.v2ex.co/generate_204";
```
*也可以下载JuiceSSH，连接本地设备，输入su， 然后再输入命令“settings put global captive_portal_https_url https://captive.v2ex.co/generate_204”*
