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
