### 字符串完整输出
```
set print elements 0
```

### gdb输出不分页
```
set pagination off
```

### 字符串输出
```
x/s char_array
x/10s char_array
```



### 打印函数参数
```
info args
```

### 打印函数局部变量
```
info locals
```

http://www.cnblogs.com/chengliangsheng/p/3597010.html

### gdb输出到文件中
```
set logging file <file name>
set logging on
print my_array
set logging off
```

### 打印结构体
```
set print pretty on/off
```

### 打印数组
```
set print array on/off
```

### 打印所有线程的堆栈
```
thread apply all bt
```

### 执行命令并退出（适用于线上排查问题）
```
db -ex "set pagination 0" -ex "thread apply all bt" -batch -p $pid
```
