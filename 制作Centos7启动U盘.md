# 下载Centos-bin镜像文件(CentOS-7-x86_64-DVD-1708.iso)

http://mirrors.aliyun.com/centos/7/isos/x86_64/CentOS-7-x86_64-DVD-1708.iso

# 将bin镜像文件写入USB中
```
# /dev/sdb即U盘设备
dd if=CentOS-7-x86_64-DVD-1708.iso of=/dev/sdb
```
# 启动U盘制作完成，通过该U盘可以按照Centos

# 参考
https://wiki.centos.org/HowTos/InstallFromUSBkey
