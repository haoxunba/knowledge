# Nginx for Window

## Nginx简介

nginx是一款开源的高性能的http服务器和反向代理服务器，也就是说Nginx本身就可以托管网站（类似于Tomcat一样），进行Http服务处理，也可以作为反向代理服务器使用，从而达到网站的负载均衡。


阿里技术团队http://tengine.taobao.org/book

`nginx -s stop` 	fast shutdown

`nginx -s quit` 	graceful shutdown

`nginx -s reload` 	changing configuration, starting new worker processes with a new configuration, graceful shutdown of old worker processes

`nginx -s reopen` 	re-opening log files

## 我目前的配置

1. 打开`conf/nginx.conf`

2. 自定义server下面的listen端口号

3. 更改root D:source为自己的项目路径，路径要配到html文件的上一级

4. 打开命令行，同时将路径切换到当前nginx所在文件

4. 命令行检测nginx.conf配置文件是否合法`nginx -t -c conf/nginx.conf`

5. 命令行`start nginx`

## Nginx配置反向代理服务器 


使用Nginx和Tomcat来搭建高性能负载均衡集群，即使用Nginx的反向代理功能来实现请求的分发

下面的案例是使用tomcat来模拟目标主机，通过nginx搭建反向代理服务器，从而转发请求到真实的tomcat服务器

```
    upstream tomcatserver1 {  
      server 192.168.72.49:8081;  
    }  
    upstream tomcatserver2 {  
      server 192.168.72.49:8082;  
    }  
    server {  
      listen       80;  
      server_name  localhost;  

      #charset koi8-r;  

      #access_log  logs/host.access.log  main;  

      location / {  
        proxy_pass   http://tomcatserver1;  
        index  index.html index.htm;  
      }       
    }  
    server {  
      listen       81;  
      server_name  localhost;  

      #charset koi8-r;  

      #access_log  logs/host.access.log  main;  

      location / {  
        proxy_pass   http://tomcatserver2;  
        index  index.html index.htm;  
      }          
    }  
```

`upstream tomcatserver1` 配置服务器集群， 可以配置多个服务器，通过weight设置不同服务器的权重
`proxy_pass   http://tomcatserver1` 代理服务器转发请求，后面的就是集群的名字

https://www.cnblogs.com/xingzc/p/5753030.html（里面还详细介绍反向代理服务器的优点）


## FQA

1. 什么情况下access.log才会有内容

在浏览器中输入连接向服务器发送请求的时候才会有内容

2. 假如我将listen的端口从9001改为9002时，为什么9001还是可以正常起作用

因为是在任务管理器中开了多个nginx.exe进程，需要将原来的进程关闭才能避免上面的情况出现

> 通过命令行关闭所有相同进程的命令
1. win+r 输入powershell(注意不是cmd命令)
2. 输入ps命令，查看当前所有进程
3. 结束单个进程命令： kill + 进程名称前面的id
4. 结束多个相同进程命令： `taskkill /F /IM nginx.exe`(注意空格位置)

3. nginx默认端口80会隐藏

默认端口打开文件时`localhost:80/index.html`，实际的浏览器会将80隐藏`localhost/index.html`

4. 端口被占用 

情形1： 设置8080端口（或其他端口时）会跳转到别的页面，而不跳转到nginx配置的页面，并且nginx没有任何反应，log文件没有任何信息响应，也不会报错；

情形2： 设置设置8080端口（或其他端口时）访问不到页面，打开error.log可以看到报错错误信息是`bind() to 0.0.0.0:80 failed (10013: An attempt was made to access a socket in a way forbidden by its access permissions)`

上面两种情形可以理解端口被占用，解决办法

运行–cmd

```
C:\>netstat -aon|findstr "8080" 
TCP 127.0.0.1:80 0.0.0.0:0 LISTENING 2448
```

端口被进程号为2448的进程占用，继续执行下面命令：
```
C:\>tasklist|findstr "2448" 
```
```
thread.exe 2016 Console 0 16,064 K
```
很清楚，thread占用了你的端口,Kill it
如果第二步查不到，那就开任务管理器，进程—查看—选择列—pid（进程位标识符）打个勾就可以了
看哪个进程是2448，然后杀之即可。

另外,强制终止进程： CMD命令: `taskkill /F /pid 1408`;  powershell命令 `kill 1480`

其实上面我都还没解决问题 最后发现有个http.d 这个是apache的进程 结束了这个进程nginx才启动了

如果朋友们使用的phpstudy这个集成软件，那么你在使用它的nginx的时候就要注意了，如果你的listen端口不是80，但是还是出现了上述的错误，那么你要去看看 include vhost.conf里的配置

https://www.cnblogs.com/YangJieCheng/p/5843660.html




