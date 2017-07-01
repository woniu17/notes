### 安装内核模块
```
insmod some.ko # 临时安装，重启失效
lsmod | grep some # 查看是否安装成功

cp some.ko /lib/modules/`uname -r`/updates/
vi /etc/sysconfig/modules/some.modules
chmod 755 /etc/sysconfig/modules/some.modules
depmod
modprobe some
reboot
```
some.modules文件内容
```
modprobe some.ko
```
### SUID
>当s出现在文件拥有者的x权限上时,如`/usr/bin/passwd`这个文件的权限是`-rwsr-xr-x`,此时就被称为SET UID简称SUID.

- **SUID权限仅对二进制可执行文件有效**
- 执行者对于该文件具有x的权限
- 本权限仅在执行该文件的过程中有效
- 执行者将具有该文件拥有者的权限

```
chmod 4755 /path/to/bin/file
chmod u+s /path/to/bin/file
```
参考：http://www.cnblogs.com/javaee6/p/4026108.html
