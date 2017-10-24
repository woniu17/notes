### touch
```
# 将mtime设置为当前时间
touch file

# 将mtime设置为一天以前
touch -d '-1 day' file

#将mtime设置为2小时以前
touch -d '-2 hour' file

# 将mtime往后1天
touch -r file -d '+1 day' file

# 将mtime向前2小时
touch -r file -d '-2 hour' file
```
### lsattr
```shell
lsattr /etc/resolv.conf
# ----i--------e- /etc/resolv.conf
# i：设定文件不能被删除、改名、设定链接关系，同时不能写入或新增内容
# 因此当发现文件具有可读写权限，但仍无法删除时，可使用以下命令
chattr -i /etc/resolv.conf 
```

### date
```shell
 # 转成时间戳
date -d "2017/03/07 10:03:44" +%s
```
时间戳转换为时间格式：
```
# 时间戳转成特定格式
date -d @1494385820
date -d @1494385820 +"%Y-%m-%d %H:%M:%S"
```

### sed
```
# 将当前目录底下（递归）所有的c文件中的字符串xxx替换成yyy
sed -i s/xxx/yyy/g `grep xxx -rl --include="*.c" --include="*.h" ./`
```

### cp/scp
```
# 复制，不改变文件的修改时间等文件属性
cp -p from_file to_file
scp -p from_file to_file
```

### printf
```
# 将ip转换成相应的整型数
printf "%02x%02x%02x%02x\n" 52 80 12 199
34500cc7
printf "%u\n" 0x34500cc7
877661383
```

### ldconfig
```
默认搜寻/lilb和/usr/lib，以及配置文件/etc/ld.so.conf内所列的目录下的库文件。
```

### sar
```
# 查看网卡流量
sar -n DEV 1
# 查看TCP连接数量
sar -n TCP 1
```

### ps
```
# 查看某进程的所有线程各自绑定的CPU及CPU使用率
ps -L -o pid,tid,psr,pcpu,comm -p <pid>
```

### socat
```
# 向UDP端口127.0.0.1:5555发送包含hello的UDP数据包
echo hello | socat - udp4-datagram:127.0.0.1:5555
```

### dig
https://www.madboa.com/geek/dig/
```
# 查询IP（域名解析）
dig +short m.linqingxiang.com

# 查域名（域名反向解析）
dig +short -x 8.8.8.8

# 向特定的DNS服务器查询IP
dig +short @114.114.114.114 emo.linqingxiang.com
```

### host
```
# 查询域名
host emo.linqingxiang.com
```

### tcpdump
```
# 查看端口号为8080的http流
tcpdump -A tcp port 8080
# 查看lo网卡中端口号为8080的http流
tcpdump -i -A tcp port 8080
```
