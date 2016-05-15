# rootfs

##硬件平台：at91sam9260ex

##TFTP
###sudo apt-get install tftpd-hpa.
###TFTP配置：vim /etc/default/tftpd-hpa

~~~

# /etc/default/tftpd-hpa

TFTP_USERNAME="tftp"
TFTP_DIRECTORY="/home/qianfan/tftpboot"
TFTP_ADDRESS="[::]:69"
TFTP_OPTIONS="-l -c -s"
~~~
###sudo /etc/init.d/tftpd-hpa restart
