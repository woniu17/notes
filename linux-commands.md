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
