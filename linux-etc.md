### ycm如何include项目的头文件
1. 在项目最上层目录添加文件`.ycm_extra_conf.py`
1. 在该文件里面添加头文件所在目录的相对路径
```
'-I',
'moduleA/include',
'-I',
'moduleB/include',
```

### ycm跳转到定义/声明
```
nnoremap <leader>d :YcmCompleter GoToDefinition<CR>
nnoremap <leader>l :YcmCompleter GoToDeclaration<CR>
nnoremap <leader><Space> :YcmCompleter GoToDefinitionElseDeclaration<CR>
```

### ctrlp过滤搜索文件/目录
```
set wildignore+=*/build/*,*/tmp/*,*.so,*.swp,*.zip
let g:ctrlp_custom_ignore = {
    \ 'dir':  '\v[\/]\.(git|hg|svn)$',
    \ 'file': '\v\.(exe|so|dll|lo|o)$',
    \ }
```

### vim的键盘符号详细说明
```
:help key-notation
```

### vim小技巧[参考](http://roclinux.cn/?p=1621)
1. A: 在本行行尾插入
1. I: 在本行行尾插入
1. J：可以去除本行和下一行之间的换行符，也就是将下一行续接到本行尾部
1. ~：光标所在处的字符进行大小写互换
1. zz： 将当前行放置于页面中间，利于阅读
1. zt：将当前行放置于页面的最顶端，一般阅读函数定义时，非常非常有用
1. zb：将当前行放置于页面的最末端
1. ctrl-a：可以将光标所在处的数字加1，负数和多位数都在支持范围内哦。可以用这个快捷键配合宏来干很多事情喽。
1. ctrl-x：有加1就会有减1，聪明！

### root的crontab表
```
/var/spool/cron/root
```

### LDADD和LIBADD的区别、指定libtool(foo.la)库和非libtool库的区别 [参考](https://stackoverflow.com/questions/23685981/what-is-the-difference-between-ldadd-and-libadd)
1. LDADD用于指定可执行文件的依赖库
    ```
    myprog_LDADD = ./foo/libfoo.la -L /usr/lib -lpthread
    ```
1. LIBADD用于指定库文件的依赖库
    ```
    libfoo_la_LIBADD = ../bar/libbar.la -L/opt/local/lib -lm
    ```

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

### iptalbes无法正常启动 [参考](http://blog.csdn.net/u012700515/article/details/52127501)
```
# 缺少iptables文件
touch /etc/sysconfig/iptables
/etc/init.d/iptables start
```


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


### awk处理Windows文件
1. Linux换行符
```
This is a line!\n
```
1. Windows换行符
```
This is a line!\r\n
```
1. Mac换行符
```
This is a line!\r
```
如果awk处理的是windows文件，那么会出现“意想不到”的结果，文件内容如下：
```
This is a line!\r\n
```

使用以下命令处理该文件:
```
cat file.txt | awk '{printf("content #%s#", $0);}'
```

 预期输出的效果是：
 ```
 content #This is a line!#
 ```

 实际输出的效果是：
 ```
 #ontent #This is a line!
 ```
 行末的'#'跑到了行首，并替换了行首的字母'c'

 原因：
 在Linux中，\n才是换行符的标志，因此awk输出的内容是：
 ```
 content #This is a line!\r#
 ```

 其中\r是特殊控制字符，意为回到行首，\r将输出光标移到行首后，接着输出字符'#'

 ### linux如何在没有视频输出设备的情况下，开启图形化软件
```
# Xvfb实现兼容X11显示协议的虚拟设备
yum install xorg-x11-server-Xvfb
# 虚拟出了一个分辨率为1024x768，24色，设备编号为":1"的显示设备
Xvfb :1 -screen 0 1024x768x24
# 设置终端的环境变量，将显示输出指向虚拟设备":1"
export DISPLAY=:1
```

### 如何查看当前的shell是sh，还是bash，或是ksh
```
echo $0
```

### 如何使用Slimerjs在无界面的服务器上抓取Flash视频 [参考](https://blog.goquxiao.com/posts/2016/04/06/flash-crawl-flash)
```
# 安装flash插件
yum -y install http://linuxdownload.adobe.com/linux/x86_64/adobe-release-x86_64-1.0-1.noarch.rpm
yum repolist | grep -i adobe
yum install flash-plugin
# 安装虚拟视频设备
yum install xorg-x11-server-Xvfbu
# 启动虚拟视频设备
Xvfb :1 -screen 0 1024x768x24
export DISPLAY=:1
```

### Linux是否支持不同的进程/线程监听同一个IP-Port [参考一](https://lwn.net/Articles/542629/) [参考二](http://www.cnblogs.com/Anker/p/7076537.html)
1. 子进程继承父进程监听的TCP/UDP端口
1. UDP通过设置SO\_REUSEADDR达到可以绑定同一个IP-Port
    > Tom noted that the traditional SO\_REUSEADDR socket option already allows multiple UDP sockets to be bound to, and accept datagrams on, the same UDP port. However, by contrast with SO_REUSEPORT, SO_REUSEADDR does not prevent port hijacking and does not distribute datagrams evenly across the receiving threads.
1. linux内核3.9版本支持通过设置SO\_REUSEPORT支持多个监听socket绑定同一个IP-port，为防止端口劫持，后绑定的进程的用户ID必须和先绑定的进程的用户ID相同
   > One of the features merged in the 3.9 development cycle was TCP and UDP support for the SO\_REUSEPORT socket option; that support was implemented in a series of patches by Tom Herbert. The new socket option allows multiple sockets on the same host to bind to the same port, and is intended to improve the performance of multithreaded network server applications running on top of multicore systems.
   > To prevent unwanted processes from hijacking a port that has already been bound by a server using SO\_REUSEPORT, all of the servers that later bind to that port must have an effective user ID that matches the effective user ID used to perform the first bind on the socket.

### Linux环境变量与shell变量 [参考](https://www.cnblogs.com/disanyuzhou/p/6825919.html)
1. 环境变量
环境变量，又称用户变量，与shell无关
```
# 显示当前环境变量
env
```
1. shell变量
Shell变量，又称本地变量，包括私有变量以及用户变量
```
# 显示当前shell本地变量
set
# 将shell本地变量导出成环境变量
export LUA_PATH
# 查看转换成环境变量的shell本地变量
```

### 进程关闭后，内核会自动关闭属于该进程的TCP连接
```
假设进程A开启一条TCP连接，在运行过程中，进程意外关闭或崩溃，那么内核会自动的关闭该TCP连接，即给对端发送Fin包。
```

### TCP连接在内核存在对端发送过来的数据时，调用close，则内核会发送Reset包给对端
```
当对一个TCP套接字调用close进行关闭时，一般情况下会发送Fin包，但如果对端发送的数据还保存在内核中，进程没有使用recv将数据读到应用层，则此时调用close，内核将发送Reset包给对端。
```
