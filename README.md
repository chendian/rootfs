# rootfs

##硬件平台：at91sam9260ex

##TFTP
- sudo apt-get install tftpd-hpa.
- TFTP配置：vim /etc/default/tftpd-hpa

~~~

# /etc/default/tftpd-hpa

TFTP_USERNAME="tftp"
TFTP_DIRECTORY="/home/qianfan/tftpboot"
TFTP_ADDRESS="[::]:69"
TFTP_OPTIONS="-l -c -s"
~~~
- sudo /etc/init.d/tftpd-hpa restart

##NFS
- sudo apt-get install nfs-kernel-server
- 配置文件：vim /etc/exports `</home/chendian/make_rootf/rootfs 192.168.1.10(rw,sync,no_root_squash)>`
- 重启生效： sudo /etc/init.d/nfs-kernel-server restart

##构建根文件系统
- 参考http://processors.wiki.ti.com/index.php/Creating_a_Root_File_System_for_Linux_on_OMAP35x
- lib库
~~~
cp /opt/FriendlyARM/toolschain/4.4.3/arm-none-linux-gnueabi/lib/libm.so.6 busybox/_install/lib
cp /opt/FriendlyARM/toolschain/4.4.3/arm-none-linux-gnueabi/lib/libc.so.6 busybox/_install/lib
cp /opt/FriendlyARM/toolschain/4.4.3/arm-none-linux-gnueabi/lib/ld-linux.so.3 busybox/_install/lib
~~~

##NFS挂载测试
- 重启NFS服务器。sudo /etc/init.d/nfs-kernel-server restart.
- 进入u-boot，设置启动参数：
~~~
setenv bootargs 'console=ttyS0,115200 root=/dev/nfs rw nfsroot=192.168.1.102:\
/home/chendian/rootf_make/rootfs ip=192.168.1.10:192.168.1.102:192.168.1.1:255.255.255.0\
::eth0:off init=/linuxrc noinitrd '
saveenv
~~~
~~~
《嵌入式linux基础教程第二版》，P190
ip=<client-ip>:<server-ip>:<gw-ip>:<netmask>:<hostname>:<device>:<autoconf>
~~~
此处ip第一个是板子的ip，第二个是服务器ip也就是你的电脑的ip,第三个是网关地址，一般是192.168.x.1或192.168.xx.254,不是广播地址。autoconf定义了获取初始IP参数的协议。比如BOOTP或者DHCP协议。如果不需要自动设置，可以设置为off。

##Logo
- sudo apt-get install figlet
- figlet 'Hello world' > logo
