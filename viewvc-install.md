### 安装viewvc
```
pip install svn
pip install pygments
wget http://viewvc.org/downloads/viewvc-1.1.26.tar.gz
tar xvf viewvc-1.1.26.tar.gz
./viewvc-install
```

### 配置viewvc.conf
```
# http://chschneider.eu/linux/server/apache2.shtml
root_parents = /data/svn : svn
default_root = svn
address = <a href="mailto:webmaster@server.example.com">Contact administrator.</a>
languages = de, en-us
root_as_url_component = 1
```

### 连接httpd

````
<Directory /usr/local/viewvc-1.0>
    Order allow,deny
    Allow from all
</Directory>
ScriptAlias /viewvc /usr/local/viewvc-1.0/bin/cgi/viewvc.cgi
ScriptAlias /query /usr/local/viewvc-1.0/bin/cgi/query.cgi
```
