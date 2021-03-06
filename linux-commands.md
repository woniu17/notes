### flock
#### crontab执行某一定时任务，如果上一周期触发的同一任务尚未执行完成，则本次不执行
```
*/1 * * * * root flock -xn /tmp/shell_01.lock -c '/path/to/task/shell_01.sh' > /dev/null 2>&1
```


### tc
#### 限制web(80端口)的写出速度[参考](https://www.cnblogs.com/Peiwen-on-the-way/articles/3566811.html)
```
tc qdisc add dev eth0 root handle 1:0 htb default 11
tc class add dev eth0 parent 1:0 classid 1:10 htb rate 512kbps ceil 640kbps prio 0
tc filter add dev eth0 parent 1:0 prio 0 protocol ip handle 10 fw flowid 1:10
iptables -A OUTPUT -t mangle -p tcp --sport 80 -j MARK --set-mark 10
```

### nethogs
#### 查看某个进程的TCP流量
```
nethogs
```

### svn
#### 将我的分支上的提交合并到trunk上
```
svn merge svn://url/of/my/branch trunk_working_directory
```
#### 将我的分支同步到与trunk完全一致 [参考](http://structure.usc.edu/svn/svn.ref.svn.c.merge.html)
```
svn merge svn://url/of/trunk@HEAD svn://url/of/my/branch@HEAD my_branch_working_directory
```
**说明**
```
# 将from_url@from_revision到to_url@to_revision的差异作用到working_directory上
svn merge from_url@from_revision to_url@to_revision working_directory
```

### filterdiff
#### 对比我的分支和trunk之间的内容差异（不包括文件属性差异）
```
svn diff svn://url/of/trunk svn://url/of/my/branch | filterdiff --clean
```
#### 对比我的分支和trunk之间的内容差异（排除某些文件）
```
svn diff svn://url/of/trunk svn://url/of/my/branch | filterdiff --clean --exclude='some_file_name'
```

### shopt
#### bash启用glob模式 [参考](https://stackoverflow.com/questions/9187566/globbing-with-ls-to-find-all-files-matching-a-certain-pattern)
```
shopt -s globstar
ls **/*.am
```

### iptables
#### 丢掉特定长度的UDP报文 [参考](https://serverfault.com/questions/523965/blocking-udp-packets-with-a-length-of-4)
```
# 丢掉目的端口为8888，UDP数据长度为4的UDP报文
# IP头部(20) + UDP头部(8) + UDP数据(4) = 32
iptables -t raw -A PREROUTING -p udp --dport 8888 -m length --length 32 -j DROP
# 丢掉目的端口为8888，UDP数据长度小于12的UDP报文
iptables -t raw -A PREROUTING -p udp --dport 8888 -m length --length 0:40 -j DROP
```

#### 丢掉特定内容的UDP报文 [参考一](https://stackoverflow.com/questions/825481/iptable-rule-to-drop-packet-with-a-specific-substring-in-payload) 
```
iptables -A INPUT -m string --algo bm --string "test" -j DROP
```
#### 丢掉特定内容(hex)的UDP报文 [参考二](https://serverfault.com/questions/404081/iptables-drop-packet-by-hex-string-match)
```
iptables --append INPUT --match string --algo kmp --hex-string '|f4 6d 04 25 b2 02 00 0a|' --jump DROP
```

### ethtool
```
ethtool -S eth0
```

### top
```
# 查看软中断
```

### stdbuf
```
# 同时在屏幕上和文件中输出日志，bin_cmd为输出日志的程序
stdbuf -o 0 bin_cmd | tee file.log
```

### awk
### 调用shell命令
```
ls -l | grep ^d | awk '{cmd=sprintf("find %s -name %s.*", $9, $9); system(cmd)}'
```

#### 统计TCP连接状态 [参考](https://gist.github.com/ameizi/cb126be7383fb463eae8)
```
netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'
```

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
# 程序运行时，需要链接所需的动态链接库，使用该命令可以查看系统从从哪些目录加载可用动态链接库
# 如果新增了动态链接库，可通过使用下列命令使系统重新读取动态链接库并加入缓存
# 默认搜寻/lilb和/usr/lib，以及配置文件/etc/ld.so.conf内所列的目录下的库文件
ldconfig -v
```
http://man.linuxde.net/ldconfig

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

### nc
```
# 查看TCP端口是否开启
nc -z 127.0.0.1 8008
# 查看UDP端口是否开启
nc -u -z 127.0.0.1 8888
```
