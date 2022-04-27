# Redis 安装



```shell
#下载对应包
wget http://download.redis.io/releases/redis-4.0.9.tar.gz
#解压对应包
tar xzf redis-4.0.9.tar.gz 
#移动，放到usr/local⽬录下
sudo mv ./redis-4.0.9 /usr/local/redis/
#进⼊redis⽬录
cd /usr/local/redis/
#生成
sudo make
#测试,NG 报错 You need tcl 8.5 or newer in order to run the Redis test.
sudo make test
#我这里缺少tcl所以安装下  tcl
wget http://downloads.sourceforge.net/tcl/tcl8.6.1-src.tar.gz  
tar xzvf tcl8.6.1-src.tar.gz
cd  ./tcl8.6.1/unix/
sudo ./configure  
sudo make  
sudo make install
#重新测试OK
sudo make test
\o/ All tests passed without errors!

Cleanup: may take some time... OK
#再重新测试OK
sudo make test
#安装完成后，我们进入目录/usr/local/bin中查看
andre@ubuntu18:/usr/local/redis$ ls -alt /usr/local/bin  
total 24492
drwxr-xr-x  2 root root    4096 Apr 27 20:11 .
lrwxrwxrwx  1 root root      12 Apr 27 20:11 redis-sentinel -> redis-server
-rwxr-xr-x  1 root root 6395936 Apr 27 20:11 redis-check-aof
-rwxr-xr-x  1 root root 6395936 Apr 27 20:11 redis-check-rdb
-rwxr-xr-x  1 root root 3001200 Apr 27 20:11 redis-cli
-rwxr-xr-x  1 root root 2790144 Apr 27 20:11 redis-benchmark
-rwxr-xr-x  1 root root 6395936 Apr 27 20:11 redis-server
-rwxr-xr-x  1 root root    8488 Apr 27 20:06 tclsh8.6
drwxr-xr-x 11 root root    4096 Apr 27 19:48 ..
-rwxr-xr-x  1 root root     426 Apr 26 21:55 virtualenv-clone
-rwxr-xr-x  1 root root     214 Apr 26 21:55 pbr
-rwxr-xr-x  1 root root     266 Apr 26 21:54 django-admin
-rwxr-xr-x  1 root root     124 Apr 26 21:54 django-admin.py
-rw-r--r--  1 root root     306 Apr 26 21:53 django-admin.pyc
-rwxr-xr-x  1 root root    2210 Feb 10  2019 virtualenvwrapper_lazy.sh
-rwxr-xr-x  1 root root   41703 Feb 10  2019 virtualenvwrapper.sh

#redis-server redis服务器
#redis-cli redis命令行客户端
#redis-benchmark redis性能测试工具
#redis-check-aof AOF文件修复工具
#redis-check-rdb RDB文件检索工具

#配置⽂件，移动到/etc/⽬录下
andre@ubuntu18:/usr/local/redis$ sudo mkdir /etc/redis
andre@ubuntu18:/usr/local/redis$ sudo cp /usr/local/redis/redis.conf /etc/redis/

#客户端运行
andre@ubuntu18:/usr/local/redis$ redis-cli
Could not connect to Redis at 127.0.0.1:6379: Connection refused
Could not connect to Redis at 127.0.0.1:6379: Connection refused
not connected> 
not connected> 

#启动redis
andre@ubuntu18:/usr/local/redis$ redis-server
```

### 修改配置文件

```shell
sudo vi /etc/redis/redis.conf
```

>绑定ip：如果需要远程访问，可将此⾏注释，或绑定⼀个真实ip
>bind 127.0.0.1

> 端⼝，默认为6379
> port 6379

> 是否以守护进程运⾏
> 如果以守护进程运⾏，则不会在命令⾏阻塞，类似于服务
> 如果以⾮守护进程运⾏，则当前终端被阻塞
> 设置为yes表示守护进程，设置为no表示⾮守护进程
> 推荐设置为yes
> daemonize yes

> 数据⽂件
> dbfilename dump.rdb

> 数据⽂件存储路径
> dir /var/lib/redis

> ⽇志⽂件
> logfile "/var/log/redis/redis-server.log"

> 数据库，默认有16个
> database 16

> 主从复制，类似于双机备份。
> slaveof

### 客户端

- 客户端的命令为redis-cli

- 可以使⽤help查看帮助⽂档

  > redis-cli --help

- 连接redis

  > redis-cli

- 运⾏测试命令

  > ping

- 切换数据库

- 数据库没有名称，默认有16个，通过0-15来标识，连接redis默认选择第一个数据库

  > select 10