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


### 修改时区
```
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```


### ssh免密码登陆
```
ssh-keygen -t rsa -P '' # machina A
# machine A .ssh/id_rsa.pub
cat id_rsa.pub >> ~/.ssh/authorized_keys # machine B
chmod 600 ~/.ssh/authorized_keys # machine B
# A可免密码登陆B
```
**注意：拥有私钥的机器可登陆拥有公钥的机器**
