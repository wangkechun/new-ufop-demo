
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
chmod +x qdoractl_linux_0.3
mv qdoractl_linux_0.3 qdoractl
./qdoractl
```

应该就能正常运行了



先登陆

```
./qdoractl login
```

依次输入你的七牛账户的ak sk，可以从这里获得  https://portal.qiniu.com/user/key  



然后看一下现有的ufop

```
./qdoractl list
```

不出意外是空的



我们来注册一个

```
./qdoractl register ufop-demo-20161020
```

注意注册的时候要换个名字，这个肯定被别人注册过了哦。

我们再list看下

```
./qdoractl list
```

这个时候应该就有了



然后是上传镜像

```
docker images
```

可以看到我们之后构建的镜像名字叫做 `ufop-demo:v1` ，注意要带tag哦



然后上传镜像

```
./qdoractl push ufop-demo:v1
```

稍等几分钟，耐心等待镜像上传完毕。

然后 

```
./qdoractl image
```

就可以看到刚刚的镜像了



然后我们来创建一个版本，运行

```
./qdoractl release --mkconfig .
```

会在当前文件夹生成一个`dora.yml`

然后开始编辑，ufopname、image这几个字段肯定是要修改的，改完大概这样纸

```
---
ufopname: ufop-demo-20161020 # ufop名称，使用qdoractl list命令获取可用image列表
verstr: v1 # 当前版本名称
image: ufop-demo:v1 # 使用的image，需要带上tag,使用qdoractl image命令获取可用image列表
desc:  "hello world" # 描述信息，可选
flavor: M0C1 # 运行实例的机器配置，不同的配置单实例价格不一样，使用qdoractl flavor命令获取可用的配置列表
health_check:
  path: "/health" # 健康检查的url相对路径，平台会试图通过访问这个url来判断某个实例是否存活，返回200即可
  timeout: 3 # 调用监控检查API的时间间隔，默认3s
log_file_paths: [] # OPTIONAL 用户日志路径，会采集该路径下的用户日志，系统也会对该目录下的已采集日志进行自动回收。

```

改完就可以构建版本了

```
./qdoractl release --config .
```

```
root@iZj6ciwrphxsxe9miuva60Z:~/new-ufop-demo# ./qdoractl release --config .
Build success
Ufop:      ufop-demo-20161020
           Version:      v1
           Image:        ufop-demo:v1
           Flavor:       M0C1
           Desc:         hello world
You can also see the release via run `./qdoractl release ufop-demo-20161020 -d`
```

来看看我们刚刚构建的版本

```
./qdoractl release ufop-demo-20161020 -d
```

```
root@iZj6ciwrphxsxe9miuva60Z:~/new-ufop-demo# ./qdoractl release ufop-demo-20161020 -d
Mode:             PRIVATE
ReviewState:      PASSED

Releases:
Version      Image             Flavor      Desc             Health Check Path      Health Check Timeout      LogFilePaths
v1           ufop-demo:v1      M0C1        hello world      /health                3                         []

Deploys:
Version      Region      Expect      Actual
```

目前还没有实例在运行，我们来部署几个实例

```
./qdoractl deploy ufop-demo-20161020 v1 --region xs --expect 1
```

```
root@iZj6ciwrphxsxe9miuva60Z:~/new-ufop-demo# ./qdoractl deploy ufop-demo-20161020 v1 --region xs --expect 1
Deploy success
Please run `./qdoractl deploy ufop-demo-20161020 --id deploy-6ed8c584-96c5-11e6-8541-6c92bf2f06d8-1476968539472398952` to view you deploy
Please run `./qdoractl release ufop-demo-20161020 v1 -d` to view you release
```

然后按照提示查看构建状态

```
./qdoractl deploy ufop-demo-20161020 --id <xxxxxxx>
```

大概等一分钟就可以看到状态变成DONE了

```
root@iZj6ciwrphxsxe9miuva60Z:~/new-ufop-demo# ./qdoractl deploy ufop-demo-20161020 --id deploy-98b44a2b-96c5-11e6-8541-6c92bf2f06d8-1476968609619617820
Version      Expect      Region      Created                  Id                                                                   Status      Message
v1           1           xs          2016-10-20 21:03:29      deploy-98b44a2b-96c5-11e6-8541-6c92bf2f06d8-1476968609619617820      DONE
```

我们来看看部署的成果

```
./qdoractl release ufop-demo-20161020 v1 -d
```

```
root@iZj6ciwrphxsxe9miuva60Z:~/new-ufop-demo# ./qdoractl release ufop-demo-20161020 v1 -d
Version:                   v1
Image:                     ufop-demo:v1
Flavor:                    M0C1
Desc:                      hello world
Health Check Path:         /health
Health Check Timeout:      3
LogFilePaths:              []

Deploys:
Region      Expect      Actual
xs          1           1
```

现在实例就已经跑起来了，这个时候我们访问这个账号的某个对象存储bucket中的一个文件，

url 后面加上`\?ufop-demo-20161020` ，就可以看到你的ufop处理之后的结果。

（目前还未上线，暂不支持查看）

比如：

```
curl -v http://q.hi-hi.cn/hello.txt\?ufop-demo-20161020
```





我们的教程暂时就到这里结束了。

上面使用的ufop源代码在这里

https://github.com/wangkechun/new-ufop-demo/blob/master/server.go

qdoractl的文档在这里 https://github.com/wangkechun/new-ufop-demo/blob/master/help.md 



