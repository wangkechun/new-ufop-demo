
# 七牛 newDora 使用实例
使用ubuntu14.04来示例
```
root@iZj6ciwrphxsxe9miuva60Z:~# lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 14.04.4 LTS
Release:	14.04
Codename:	trusty
```
首先安装docker

```
root@iZj6ciwrphxsxe9miuva60Z:~# docker -v
Docker version 1.12.2, build bb80604
```

然后下载ufop demo

```
mkdir new-ufop-demo
cd new-ufop-demo/
wget http://q.hi-hi.cn/ufop-demo-2016102020
chmod +x ufop-demo-2016102020
```

跑起来看下

```
./ufop-demo-2016102020
```

开个新窗口

```
curl -v 'http://127.0.0.1:9100/handler?cmd=new-ufop-demo&url=http://baidu.com'
curl -v -X POST -T /etc/hosts http://127.0.0.1:9100/handler
curl -v http://127.0.0.1:9100/health
```

前两个请求返回的body，里面包含了 

```
req body:
-------------
指定url的数据或者POST过去的数据
time: 2016-10-20 20:22:45.417314444 +0800 CST
```

这个就是一个简单的ufop，作用就是在文件前面添加了`req body:`然后返回

至于第三个api，是健康检查API，只要这个API可以正常访问，系统就认为这个实例是存活的，可以处理请求。

我们实现的时候返回一个字符串就好。



然后我们开始把这个ufop构建成ufop镜像

```
docker build -t ufop-demo:v1 .
docker run -it -p 9100:9100 --rm ufop-demo:v1
```

然后开个新窗口测试一下镜像是否正常

```
curl -v http://127.0.0.1:9100/health
```

不出意外的话是正常的



然后下载我们的 qdoractl 命令行工具

```
wget http://q.hi-hi.cn/qdoractl_linux_0.3
```





