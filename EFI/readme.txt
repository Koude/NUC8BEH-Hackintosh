- Version：200416
- Maintainer：weachy（维奇）
- 采菊东篱下，悠然见南山

如果你想了解更多关于 NUC8ixBE 豆子峡谷黑苹果相关知识，请查阅我的文章 http://u.nu/nuc8
If you want to learn more about hackintosh with Intel NUC 'Bean Canyon'. Please visit: https://u.nu/nuc8


- OpenCore 实现双系统引导的说明：
1、Windows 建议用 UEFI 模式安装（无需 Bootcamp 安装，⚠️不支持 MBR 传统引导模式）。
2、内置了四套 plist 配置文件，区别如下：
 单系统：
  config.plist：默认配置文件，适配 4k 分辨率的显示器
  config-1080p.plist：默认配置文件，适配低于 4k 分辨率的显示器
 双系统：
  config-双系统.plist：适配 4k 分辨率的显示器
  config-双系统-1080p.plist：适配低于 4k 分辨率的显示器
请选择适合自己的 plist，重命名为 config.plist 并覆盖 /EFI/OC/config.plist 文件
3、准备工作：
（1）单盘双系统用户需将 Windows 引导的 /EFI/Microsoft 文件夹备份，然后清空 EFI 分区，放入 EFI-OC-XXXXXX.zip 解压得到的 EFI 文件夹，将备份的 /EFI/Microsoft 文件夹与 OC 引导合并（即 EFI 下有 Boot、OC、Microsoft 三个文件夹）；
（2）双盘双系统的用户，无需改动两个 EFI 分区；

⚠️注意：以上配置均默认不显示 OpenCore 引导菜单，希望开机出现菜单的朋友，只需要开机时连续按 ESC 或 Option 键（Windows键盘的 Alt 键），即可出现引导菜单。另外，双系统用户也可在 MacOS “系统偏好设置”中的“启动磁盘”切换系统，Windows 下则需要安装苹果官方提供的 BootCamp 程序，可使用软件从 Windows 重启至 MacOS。


- Clover 迁移 OpenCore 方法：
1、备份出 EFI 分区中的文件
2、EFI 分区内的文件全部删除（双系统的保留 /EFI/Microsoft 文件夹），将解压出的 EFI 文件夹放入 EFI 分区
3、迁移三码。请参考“手动迁移三码方法”或直接使用豪客开发的“三码迁移工具”。
4、如果重启提示未找到启动项，请使用 PE 系统，修复 OpenCore 引导（可搜索“修复 Windows 系统引导”的教程作为参考）。


- 手动迁移三码方法：（建议缺乏动手能力的小白用户，直接使用豪客制作的“三码迁移工具”，可到群共享下载）
三码的配置名在 Clover 和 OpenCore 略有区别，对应关系如下：
	BoardSerialNumber 或 MLB	-> MLB
			    ROM	-> ROM
	  	   SerialNumber -> SystemSerialNumber
    		     CustomUUID	-> SystemUUID
使用文本编辑器分别打开上一步中备份的 Clover 的 config.plist，以及 OpenCore 的 config.plist 配置文件。将以上四个值复制到对应字段的下一行，注意复制时要包含“值类型定义”一起复制替换（“<>”的部分），例如 Clover 中如下配置：
<key>ROM</key>
<data>ExcM+qxi</data>
需要复制 <data>ExcM+qxi</data> 替换到 OC 配置文件中，不能只复制 ExcM+qxi。


- Intel 板载蓝牙驱动
我打包了豆子峡谷专用的 intel 板载蓝牙驱动（兼容 10.13 以上系统），可同时兼容 OpenCore 与 Clover，请到群共享中获取“intel板载蓝牙驱动_for_OC&Clover”


- Changelog：

2020-04-16
1、早先升级 WhateverGreen.kext1.3.8 版本，通过启动参数（igfxfw=2）解锁核显性能，但经过几天群友使用反馈，兼容性略差（部分群友反馈 DP 黑屏），因此替换为魔改版（感谢 NUC①群的 @Forever 提供建议），可以完全释放核显性能。
2、精简了 OpenCore 部分配置参数（感谢 NUC③群的 @针针）。
3、针对使用板载蓝牙以及 USB 无线网卡的用户，简化配置难度。
4、去除 HibernationFixup，稳定起见，还是建议睡眠模式（hibernatemode）为 0。

2020-04-08
1、更新 OpenCore 0.5.7 正式版，优化 GUI 界面元素。更接近白果：
（1）默认直接进系统，开机后连续点按 ESC/Option 键开启引导 GUI 界面（⚠️注意是连续点按，不是长按！！）。
（2）GUI 界面下按空格出现附加项（Recovery、Reset NVRAM 等）。
（3）加入 UEFI Shell，一些场景下可免去 PE 完成引导救急（对小白不太友好），退出输入“exit”并回车。
2、例行升级 kext 版本（Lilu、AppleALC、WhateverGreen、VirtualSMC、NVMeFix）。
3、引入新 kext：HibernationFixup。

2020-03-08
1、开启 GUI 引导菜单页面，仿白果风格，支持空格键呼出 Recovery 和 Reset NVRAM（第一个预览版本的 GUI，目前还略显简陋，并且未对 HiDPI 优化，期待一下后续版本）。
2、完善 OpenCore 双系统相关配置以及说明（感谢 @123、@1 0、@Feeling、@传说 等NUC4号群的小伙伴们的配合测试及意见反馈）。

2020-03-04
1、更新 OpenCore 0.5.6 正式版，优化开机快捷键的支持，开机后按 ESC/Option 键开启引导菜单（⚠️注意是连续按，不是长按！！不按默认直接进系统）
2、例行升级 kext 版本（Lilu、AppleALC、WhateverGreen）

2020-02-04
1、更新 OpenCore 0.5.5 正式版
2、修正白果原生开机快捷键支持
3、例行升级 kext 版本（NVMeFix、AppleALC、VirtualSMC）
4、Intel 板载蓝牙向下兼容 10.14.6（⚠️注意：不支持低于 10.14.6。默认为关闭状态，需自行开启。详见上述《Intel 板载蓝牙驱动使用说明》）

2020-02-02
1、开启原生 NVRAM（支持使用 OC 启动菜单进入 Recovery 系统以及清除 NVRAM，方便双系统用户使用系统偏好设置中的“启动磁盘”切换系统）（感谢 @HCLOK168 制作 SSDT）
2、加入 NVMeFix 补丁，可降低 NVME SSD 的待机功耗和温度（对于原本 NVMe SSD 温度就不高的用户可能不明显，我这里测试降低 5～10 度）
3、加入 Intel 板载蓝牙支持（默认关闭，需自行开启，详见上述《Intel 板载蓝牙驱动使用说明》）

2020-01-15
1、更新 OpenCore 0.5.4 正式版（OC 选择系统页面现已支持上下方向键、以及通过 Ctrl+Enter 键设置默认启动盘）
2、更新 acidanthera 全家桶（Lilu、WhateverGreen、AppleALC 等）
3、加入内核补丁禁止 rtc 定时唤醒，修复 10.15.2 不能自动睡眠的 bug。
4、声卡layout-id改为14（0E），解决音量过小的问题。

2019-12-16
1、采用与豆子峡谷同款 CPU 的 Macbook Pro 13' 2018 款的 CPU 变频策略，从测试结果来看，调度更优秀，低频更低、待机更省电（感谢 @HCLOK168）
2、去除“关于本机”中 2048 MB 显存的修改，恢复成默认（1536 MB）

2019-12-08
1、修复 Mac mini 2018 机型在“关于本机”不显示内存页的问题
2、大幅精简了整个引导的体积（感谢 @豪客）

2019-12-04
1、更新 OpenCore 0.5.3正式版
2、更新 acidanthera 全家桶（Lilu、WhateverGreen、AppleALC等）

