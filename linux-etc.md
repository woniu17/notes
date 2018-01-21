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

### 缩小TCP写超时时间
```
sysctl -a | grep retries
net.ipv4.tcp_retries1 = 3
net.ipv4.tcp_retries2 = 3
```

### 本地TCP端口范围限制
```
net.ipv4.ip_local_port_range = 32768	61000
```

### iptalbes无法正常启动
```
# 缺少iptables文件
touch /etc/sysconfig/iptables
/etc/init.d/iptables start
```
http://blog.csdn.net/u012700515/article/details/52127501

### 多版本openssl，如何编译python
```
# 系统中存在多个版本的openssl
# openssl-devel-0.9.8e-34.el5_11
/lib64/libssl.so.09
/lib64/libcrypto.so.09

# openssl-devel-1.0.1e-16.14
/usr/lib64/libssl.so.10
/usr/lib64/libcrypto.so.10

# 将高版本的.so文件连接到/lib64底下
ln -s /usr/lib64/libssl.so.10 /lib64/libssl.so.10
ln -s /usr/lib64/libcrypto.so.10 /lib64/libcrypto.so.10

# 删除编译过的python源代码，重新解压源文件
./configure
make
make install
```

### SVN服务器搭建注意事项
```
# 不能再配置行末添加注释，如：
[general]
anon-access = none # hello
auth-access = write # 不能再行末添加注释
password-db = passwd
```

### mac os关闭HiDPI
```
# 在vmware上安装mac os后，安装完vmware tools后，需要关闭HiDPI才能正常全屏
sudo defaults write /Library/Preferences/com.apple.windowserver.plist DisplayResolutionEnabled -bool false
```
