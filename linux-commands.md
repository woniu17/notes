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

