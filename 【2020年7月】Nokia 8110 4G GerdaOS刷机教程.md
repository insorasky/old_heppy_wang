之前那个教程过于久远，于是决定重新编写新的教程。
考虑到之后上了大学以后就很少再玩KaiOS了，这可能将是我倒数第二篇关于KaiOS的文章。
最后一篇是关于KaiOS手机如何和谐上网。

#1.导读
[GerdaOS][1]是一个第三方kaiOS ROM，这个ROM支持截屏、文件管理、安装第三方软件、开发者选项、IMEI修改、TTL修改、LTE频段解锁等，并且加入了广告屏蔽等功能。
他们说：“Install what you want, not they.”意味着你可以最大限度地定制你的香蕉机，而不是受HMD，kaiOS甚至阿三的jio摆布。
目前这个系统处在alpha阶段，可能会有未知的风险。刷机有风险，操作需谨慎。请务必备份好您的所有数据。这会删除您的所有数据。这一操作将导致您失去来自HMD官方的OTA更新。本人不对本内容产生的问题承担任何责任。
#2.系统要求
 - 一部正品Nokia 8110 4G香蕉机。华强北勿扰。
 - 一台电脑。最好运行的是Mac或者Linux系统。
 - 手机固件需要先升级到V12以上。
 - 一张microSD卡。可用空间需至少5GB。
#3.下载所需文件：
 - **v13recovery_gerdaos.img** Recovery镜像文件，重命名为recovery.img并存放在存储卡根目录下。
 - **adb工具包** [请移步此处获得安装教程。][2]
 - **Wallace** 临时root工具。请[点此][3]下载并放在SD卡根目录。
 - **Dumpall.zip** 备份工具，存放在存储卡根目录下。
 - **gerda-install-221edb8.zip** GerdaOS刷机包。存放在存储卡根目录下。
 - **驱动** mac和linux及原版Win10无需安装驱动，[Windows请参考这里][4]。
 - **OneKeyOmniSD** OmniSD一键安装器。[请点此下载。][5]
所有文件已经以百度网盘分享链接的形式在文章末尾贴出。
#4.重置提权
请参考本站文章 https://www.heppy.wang/archives/94/ 。
#5.临时root
 - 打开刚刚安装的OmniSD，选择Wallace Toolbox安装包安装。
 - 安装完成后，启动Wallace Toolbox。
 - 按1进行临时root，完成后关闭Wallace Toolbox。
 - 如果使用Wallace Toolbox无法正常root，请尝试使用Wallace（不带后缀）。
#6.降级Recovery
此步骤执行之前，请确保recovery镜像已经存放在存储卡根目录下并重命名为recovery.img。
在终端/命令提示符中逐行复制：
```shell
adb shell
busybox dd if=/dev/block/bootdevice/by-name/recovery of=/sdcard/recovery-backup.img bs=2048
busybox dd if=/sdcard/recovery.img of=/dev/block/bootdevice/by-name/recovery
mount -o remount,rw /system
echo '#!/system/bin/sh' > /system/bin/install-recovery.sh
echo 'exit 0' >> /system/bin/install-recovery.sh
chown root:root /system/bin/install-recovery.sh
chmod 750 /system/bin/install-recovery.sh
sync
mount -o remount,ro /system
exit
reboot
```
此时手机重启后正常进入系统，recovery降级完成。您可以重启至recovery（命令adb reboot recovery或者关机状态下同时按住Power+上键），如果首行显示“Gerda Recovery”即为安装成功。
#7.备份原有操作系统
 - 重启进入recovery（方法上面已经提到，这里不再赘述）
 - 选择Mount /system（上下键选择，返回键确认）
 - 选择Apply update from SD card
 - 选择dumpall.zip
 - 稍等一会
 - 在recovery重新弹出菜单后选择reboot system now重启至系统。
至此，你的当前系统已经备份到存储卡的DUMPS文件夹下。
#8.刷入刷机包
 - 重启进入recovery
 - 选择Mount /system
 - 选择Apply update from SD card
 - 选择gerda-install-702d409.zip
 - 稍等一会，等待recovery菜单重新弹出
 - 选择Wipe data/factory reset双清
 - 选Yes
 - 选择reboot system now重启至系统。
至此，如果开机界面变为一只Linux企鹅和GerdaOS图标，即为刷机成功。您可以体验您的新系统了。您可以通过自带的文件管理器安装第三方软件了。
#9.文件下载
 - gerda-install-221edb8.zip 链接：https://pan.baidu.com/s/1wAE8IxtC63qQOujLVmFFWw 提取码：hcmu
 - 其他所需文件：https://www.lanzous.com/b623501/ 密码：2xra


  [1]: https://gerda.tech/
  [2]: https://www.coolapk.com/feed/16691768?shareKey=ODAzYjU2ZmQxOWJlNWU3Mzk4MzI~&shareUid=1998781&shareFrom=com.coolapk.app_2.5
  [3]: https://www.heppy.wang/archives/94/
  [4]: https://www.jb51.net/softs/668696.html
  [5]: https://www.heppy.wang/archives/94/
