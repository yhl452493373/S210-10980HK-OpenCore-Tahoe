# S210-10980HK(10880H)-OpenCore-Tahoe，支持Sequioa、Tahoe

**Tahoe是黑果的最后一场狂欢了，完善各个驱动，特别是博通无线网卡，只能慢慢等，因为OCLP的核心成员被苹果收编了，进度很慢。**



## SIP状态切换说明

**csr-active-config改为通过`ToggleSipEntry.efi`开关，关闭状态值改为`7F0A0000`，可以在`UEFI`-`Drivers`-`ToggleSipEntry.efi`的`Arguments`里面设置，目前是`0xA7F`，计算后就是`7F0A0000`**。

**如果要切换SIP的开关状态，则在选择启动项时，按空格键，然后移动到Toggle SIP上，按回车。`Toggle SIP (Disable)`是关闭SIP状态，`Toggle SIP (Enable)`是启用SIP状态**



## 目录说明

+ `BCM94360Z4`目录下是适用于`BCM94360Z4`网卡的EFI，这个网卡的蓝牙免驱，因此没有蓝牙相关驱动。
  + `Sequioa`下 WIFI 需要通过 `OCLP-Mod` 打补丁
  + `Tahoe`下目前 WIFI 无解
+ `Intel AX210`目录下是适用于`Intel AX210`网卡的EFI，应该也适用于`9260AC`、`AX201`等网卡，包含蓝牙、WIFI驱动。
  + `Sequioa`和`Tahoe`下蓝牙都正常工作
  + `Sequioa`下 WIFI 使用`AirportItlwm_Ventura`驱动，需通过`OCLP-Mod`打补丁，使用体验和博通无限网卡差不多，通过系统的WIFI直接进行管理
  + `Tahoe`下使用的`itlwm`驱动，其网络被识别为`有线连接`，因此**不能获取位置信息，不能进行地图定位**
  + `Tahoe`下 WIFI 无需打驱动，需要使用`HeliPort`来管理，`HeliPort`可以放到登录项中进行开机自启
  + Intel的 WIFI 连接速率都比较低，只是能用的状态（同一位置同热点，94360z4有1300Mbps的速率，AX210只有260Mbps）
+ `OCLP-Mod`下载地址：https://github.com/laobamac/OCLP-Mod/releases ，选第一个，自行解决出墙问题



## 更新日志

基于opencore1.0.5正式版

+ 2025.10.23 区分`BCM94360z4`和`Intel AX210`网卡，`Intel AX210`在`Tahoe`可用 WIFI；精简Kexts，`BCM94360z4`移除蓝牙相关驱动
+ 2025.06.26 机型换成MacBookPro16,2，如果你用了USBMap.kext或者USBPorts.kext，需要修改机型；支持安装Tahoe
+ 2023.12.25 使用`AppleIGB.kext`修复i211网卡在macOS Monterey下无法使用的问题
+ 2023.10.09 声卡id由11改为37，解决插入耳机后微信等软件声音小问题；增加进入OC选择界面时的DUANG的音效（DUANG的音效只能由3.5mm耳机孔输入，hdmi或dp无法输出）
+ 2023.10.07 更新AirportBrcpFixup、RestrictEvents到最新
+ 2023.09.13 更新oc到0.9.5正式版，更新kext到最新
+ 2023.08.08 更新oc到0.9.4正式版，更新kext到最新
+ 2023.08.01 增加AMFIPass.kext，启动参数增加-amfipassbeta，移除amfi0=0x80，解决VMWare Fusion以及注入后的PD虚拟机无法使用问题
+ 2023.07.27 csr-active-config改为通过`ToggleSipEntry.efi`开关，关闭状态值改为`7F0A0000`，可以在`UEFI`-`Drivers`-`ToggleSipEntry.efi`的`Arguments`里面设置，目前是`0xA7F`，计算后就是`7F0A0000`
+ 2023.07.26 更新oc到0.9.3正式版，更新kext到最新，支持安装Sonoma，通过OCLP驱动部分博通网卡
+ 2023.03.10 更新oc到0.9.0正式版，更新kext到最新
+ 2023.02.15 更新oc到0.8.9正式版，更新kext到最新
+ 2023.01.04 更新oc到0.8.8正式版，更新kext到最新
+ 2022.12.07 更新oc到0.8.7正式版，更新kext到最新
+ 2022.11.08 更新oc到0.8.6正式版，更新kext到最新
+ 2022.10.07 去除i211的AppleIGB驱动，在最新的Monterey、Ventrua下测试，为免驱
+ 2022.10.06 更新oc到0.8.5正式版，更新kext到最新
+ 2022.09.17 更新AppleIGB驱动以修复macOS Ventura下i211网卡无法上网问题；重新定制视屏输出端口
+ 2022.09.07 更新oc到0.8.4正式版，更新kext到正式版;重新定制USB端口
+ 2022.08.02 更新oc到0.8.3正式版，更新kext到正式版
+ 2022.07.26 解决macOS Monterey下，i211at网卡无法上网问题
+ 2022.07.11 更新oc到0.8.3开发版，更新部分kext到开发版（现在支持macOS Beta3）
+ 2022.07.07 更新oc到0.8.2正式版，更新kext到正式版（此版不支持安装macOS13 beta3）
+ 2022.06.12 更新oc到0.8.2开发版，更新kext到开发版
+ 2022.06.07 更新oc到0.8.1，更新kext到最新
+ 2022.03.08 更新oc到0.7.9，更新kext到最新
+ 2022.02.11 更新oc到0.7.8，更新kext到最新
+ 2022.01.11 更新oc到0.7.7，更新kext到最新
+ 2021.12.16 修复睡眠后每2小时被唤醒一次的问题
+ 2021.12.07 更新oc到0.7.6，更新kext到最新
+ 2021.11.10 更新oc到0.7.5，更新kext到最新
+ 2021.10.07 增加-revsbvmm参数，在任意SecureBootModel下都能检测到更新；关闭SecureBootModel
+ 2021.10.02 变更部分配置
+ 2021.09.30 改变PciRoot(0x0)/Pci(0x2,0x0)的AAPL,ig-platform-id为0900A53E ，同时重新定制显卡补丁，以解决Monterey Beta8下DP无输出问题（Beta7以及以下版本未测试）
+ 2021.09.24 更新oc到0.7.4开发版，据说解决了beta6升级到beta7时一直重启的问题，需要用OCC最新版并切换到开发版来编辑（也可以用OCAT最新版）
+ 2021.09.07 更新oc到0.7.3，更新kext到最新，支持macOS12的AirPlay to Mac (已测试）)
+ 2021.08.19 支持Monterey Beta版，可以正常安装使用，但部分软件和Monterey存在兼容性问题。
+ 2021.08.03 更新oc到0.7.2，更新kext到最新，支持macOS12的AirPlay to Mac（未测试）
+ 2021.08.02 增加HoRNDIS驱动，可以通过安卓的USB共享网络上网
+ 2021.07.23 修复重启问题
+ 2021.07.14 重新定制USBMap.kext;修复单显示器下DP音频或HDMI音频未识别的问题
+ 2021.07.06 更新oc到0.7.1，更新kext到最新
+ 2021.06.09 更新oc到0.7.0，更新kext到最新
+ 2021.05.06 更新oc到0.6.9，更新kext到最新
+ 2021.04.07 更新oc到0.6.8，更新kext到最新
+ 2021.03.02 更新oc到0.6.7，更新kext到最新
+ 2021.02.24 修复在macOS 11.2.1 (20D75)中，插入ipad设备充电后出现的频繁突然断电问题

---



## 安装说明



### 我的配置

机型S210，i9 10980hk，64G，4K显示器（mini dp）+4K显示（hdmi 2.0），声卡为alc235（老板说是alc 233，但声卡id0x10ec0235即实际alc 235），网卡为BCM94360z4



### 工作情况
以下均基于Sequioa测试

+ DP、HDMI2.0（单DP、单HDMI、DP+HDMI）

+ 睡眠唤醒

+ 原生电源管理

+ BCM94360Z4蓝牙免驱

+ BCM94360Z4无线网卡通过OCLP打补丁后正常驱动

+ I219V（背面靠近双USB3.0的那个）

+ I211AT（背面靠近电源接口的那个）

+ USB（3.0、2.0、type-c）

+ 随航、接力、隔空投送

+ 耳机、麦克风、DP/HDMI音频

  

### BIOS设置
Boot - CSM Configuration - CSM Support [ Disable ]



### 其他注意事项
+ 系统偏好设置 - 节能 唤醒以供网络访问，取消选中，否则睡眠后过一段时间会被唤醒
+ 系统偏好设置 - 节能 断点后自动启动，取消选中，这个在黑苹果上没啥用
+ 系统偏好设置 - 节能 中的启用电能小憩需要取消选中，否则长时间睡眠后可能崩溃
+ 启动时，如果出现苹果logo被压扁，尝试换一个显示器。我自己的XV273K暗影骑士，接HDMI口，启动一阶段就是扁的，进系统后正常。我的便携显示器，接的mini dp，启动过程logo都正常

**可以在windows中安装macos支持文件（启动转换助理的“操作”菜单中可以下载，进windows后，安装），实现类似白苹果的windows下选择启动系统功能。注意，安装后需要在c:\program files\apple software update下将bootcamp升级，否则bootcamp会错误识别所有mac磁盘都可作为macos启动。安装后，会隐藏一些磁盘，请参照[彻底解决win10开机后，D盘或者其他隐藏，每次要重新添加盘符的方法](http://www.purplestone.cn/share/2361.html)处理**

