# 在CentOS上搭建Shadowsocks服务器

## 1.安装pip

```shell
  > curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
  > python get-pip.py
```  

## 2.安装shadowsocks

```shell
  > pip install shadowsocks
```
  
## 3.配置shadowsocks

```shell
  > vi /etc/shadowsocks.json
```
编辑

```shell
  {
    "server":"0.0.0.0",
    "server_port":443,
    "password":"1234567890",
    "timeout":600,
    "method":"aes-256-cfb"
  }
```
需要修改的是server(机器ip),server_port(ss服务指定的端口),password(密码)

## 4.启动Shadowsocks服务器

做完上面的步骤就可以启动ss服务了

```shell
> ssserver -c /etc/shadowsocks/config.json #直接启动
> ssserver -c /etc/shadowsocks/config.json -d start #后台运行
> ssserver -c /etc/shadowsocks/config.json -d stop #停止服务
> tail -f /var/log/shadowsocks.log #查看ss日志
```

## 5.配置Shadowsocks作为系统服务开机自动启动

首先创建脚本
```shell
 > sudo vim /etc/init.d/shadowsocks
```

编辑脚本

```shell
  #!/bin/sh
  #
  # shadowsocks:    shadowsocks Daemon
  #
  # chkconfig:    - 90 26
  # description:  MemCached Daemon
  # Source function library.
  # Author: mike
  start(){
          ssserver -c /etc/shadowsocks.json -d start
  }

  stop(){
          ssserver -c /etc/shadowsocks.json -d stop
  }

  case "$1" in
  start)
          start
          ;;
  stop)
          stop
          ;;
  reload)
          stop
          start
          ;;
  *)
          echo "Usage: $0 {start|reload|stop}"
          exit 1
          ;;
  esac
  exit 0
```

然后添加为系统服务和开机自启

```shell
  chkconfig --add shadowsocks #添加系统服务
  chkconfig shadowsocks on #配置系统服务开机默认启动
```
启动shadowsocks

```shell
  service shadowsocks start #启动shadowsocks服务
```

## 6.参考

[在CENTOS 7上搭建Shadowsocks图文教程](https://www.4spaces.org/install-shadowsocks-on-centos-7/)

[github官方Shadowsocks 使用说明](https://github.com/shadowsocks/shadowsocks/wiki/Shadowsocks-%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E)

[设置 shadowsocks server 开机启动](https://my.oschina.net/oncereply/blog/467349)

[shadowsocks安装配置及系统服务脚本](https://blog.51cto.com/dyc2005/1942307)

[chkconfig命令](https://www.cnblogs.com/qmfsun/p/3847459.html)


## 结束!
