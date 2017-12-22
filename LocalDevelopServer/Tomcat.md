# Tomcat

## 简单介绍部署流程

1. 安装JDK并部署环境变量

2. 安装Tomcat并部署环境

注意tomcat官网会有版本选择说明，针对不同版本的JDK安装不同版本的tomcat

3. 设置端口，并部署web项目

可以在`Connector`中配置端口，默认8080

```
<Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```

在Host里面添加下面的`Context`用来映射项目的路径
```
<Context path="/" reloadable="true" docBase="E:\knowledge"/>
```

配置完成，本地可以通过`localhost:8080`访问项目