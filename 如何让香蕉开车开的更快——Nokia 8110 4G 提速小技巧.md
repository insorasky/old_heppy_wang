感谢Luxferre、speeduploop等大神提供的各种小技巧。在应用这些小技巧之前，请先root你的香蕉机，并安装好adb工具包和相关驱动。
###1、解除CPU限制
 - 运行 adb pull /data/opt/init （如果提示文件不存在，就手动在当前目录下创建名为init的文件【没有扩展名！】并输入首行 #!/system/bin/sh）
 - 使用notepad++等文本编辑器（建议不用记事本）将以下代码加入当前目录下的init文件的首行以后：
```
echo '96'      > /sys/devices/system/cpu/cpufreq/interactive/target_loads
echo '1094400' > /sys/devices/system/cpu/cpufreq/interactive/hispeed_freq
echo '24'      > /sys/devices/system/cpu/cpufreq/interactive/go_hispeed_load
echo '0'       > /sys/module/msm_thermal/core_control/enabled
```
 - 保存，运行：
```
adb push init /data/opt/init
adb shell
chmod +x /data/opt/init
exit
adb reboot
```
 - 等待手机重启即可。
2、启用双卡显示4G（实际4G+2G，硬件硬伤）：运行以下：
```
adb shell setprop persist.moz.ril.0.network_types gsm,wcdma,lte
adb shell setprop persist.moz.ril.1.network_types gsm,wcdma,lte
adb reboot
```
3、禁用SIM2：
```
#禁用SIM2
adb shell setprop persist.radio.multisim.config none
adb reboot

#恢复SIM2
adb shell setprop persist.radio.multisim.config dsds
adb shell setprop persist.moz.ril.0.network_types gsm,wcdma,lte
adb shell setprop persist.moz.ril.1.network_types gsm,wcdma,lte
adb reboot
```
4、禁用网络限制
```
adb shell setprop persist.net.tcp.buffersize.wifi 524288,1048576,2097152,524288,1048576,2097152
adb shell setprop persist.net.tcp.buffersize.umts 6144,87380,1048576,6144,87380,524288
adb shell setprop persist.net.tcp.buffersize.gprs 6144,87380,1048576,6144,87380,524288
adb shell setprop persist.net.tcp.buffersize.edge 6144,87380,524288,6144,16384,262144
adb shell setprop persist.net.tcp.buffersize.hspa 6144,87380,524288,6144,16384,262144
adb shell setprop persist.net.tcp.buffersize.lte 524288,1048576,2097152,524288,1048576,2097152
adb shell setprop persist.net.tcp.buffersize.evdo 6144,87380,1048576,6144,87380,1048576
adb reboot
```
5、启用虚拟内存
此操作经测试发现会拖慢手机速度（主要是存储器实在太烂了），请谨慎使用。
 - 创建虚拟内存文件（默认512M，即512M RAM+512M虚拟内存）
```
adb shell
mount -o remount,rw /system
mount -o remount,rw /data
busybox mkdir /data/opt
busybox dd if=/dev/zero of=/data/opt/swapfile bs=1024 count=524288
busybox mkswap /data/opt/swapfile
exit
```
 - 按1的方式导出init文件
 - 首行后加入以下两行：
```
swapon /data/opt/swapfile
swapoff /dev/block/zram0
```
 - 按1的方式导入init文件并重启。
