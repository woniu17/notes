### centos6搭建samba服务
1. 安装samba套件
    ```
    yum install samba samba-client samba-commo
    chkconfig smb on
    chkconfig nmb on
    ```
1. 添加配置项
    ```
    #/etc/samba/smb.conf
    [global]  
    workgroup = WORKGROUP  
    security = user 
    map to guest = bad user  
    valid users = root
    [my-131]  
    path = /root/code
    force user = root
    force group = root
    browsable =yes  
    writable = yes  
    guest ok = yes  
    read only = no
    ```
1. 给root添加密码
    ```
    smbpasswd -a root
    ```
1. 设置防火墙规则
    ```
    iptables -I INPUT -m state --state NEW -m udp -p udp --dport 137 -j ACCEPT  
    iptables -I INPUT -m state --state NEW -m udp -p udp --dport 138 -j ACCEPT
    iptables -I INPUT -m state --state NEW -m tcp -p tcp --dport 139 -j ACCEPT
    service iptables save  
    ```
1. 重启samba服务
    ```
    service smb restart
    service nmb restart
    ```
1. windows访问smaba服务
    ```
    # 映射网络磁盘，my-131是samba配置文件指定的
    net use Z: \\192.168.153.131\my-131
    # 关闭网络磁盘映射
    net use /delete Z: #关闭
    ```
