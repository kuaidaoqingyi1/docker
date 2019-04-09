### docker 常用命令

####docker 帮助命令

```shell
docker version 
docker info
docker --hepl
```

 #### docker 镜像命令

1. docker images  列出本地镜像

```shell
docker images 
               -a:列出本地所有镜像(含中间映象层)
               -q:只显示镜像ID
               --digests:显示镜像的摘要信息
               --no-trunc:显示完整的镜像信息
```

2.docker  search  <images Name> 

```shell
docker search [] <images name> 
			   --no-trunc:显示完整的镜像描述信息
			   -s:列出收藏数不小于指定值的镜像
			   --automated:只列出automated类型的镜像
```

3.docker pull  <images name>

```shell
docker pull <images name>
```

4.docker rmi <images id> 

```shell
docker rmi  []   <images id>
             -f 删除单个
             -f 镜像名1:TAG镜像名2  删除多个
             -f $(docker images -qa) 删除全部

```

#### docker 容器命令

1.docker run 新建并启动容器

```shell
docker run  []    <image >
            --name="容器新名字" 为容器指定一个新的名称
            -d 后台运行容器,并返回容器的id.也即启动守护式容器
            -i 已交互式运行容器,通常与-t同时使用
            -t 为容器重新分配一个伪输入终端,通常与-i同时使用
            -P 随机端口映射
            -p 指定端口映射 有以下四种格式
               IP:hostPort:containerPort
               IP:containerProt
               hostPort:containerPort
               containerPort
```

2.docker ps [OPTOPMS]  列出当前所有正在运行的容器

```shell
docker ps []
           -a 列出当前所有正在运行的容器+历史上运行过的容器
           -l 显示最近创建的容器
           -n 显示最近n个创建的容器
           -q 静默模式,只显示容器编号
           --no-trunc 不截断输出
```

3.退出容器

```shell
两种退出方式
exit 容器停止退出
ctrl+P+Q 容器不停止退出

docker attach  <id > 进入交互容器
```



4.启动容器

```shell
docker start  <id>
```

5.重启容器

```shell
docker restart <id>
```

6.停止容器

```shell
docker stop  <id>
```

7.强制停止容器

```shell
docker kill <id>
```

8.删除已停止的容器

```shell
单个
docker rm <id>  
全部
docker rm -f $(docker ps -a -q)
docker ps -a -q |xargs docker rm
```

9.重点

```shell
1.守护式启动
docker run -d 容器名或id
  docker ps -a 进行查看，会发现容器已经退出
  横重要一点说明的一点：docker容器后台运行，就必须有一个前台进程
  容器运行的命令如果不是那些一直挂起的命令(比如运行 top tail ),就是会自动退出
  这个是docker的机制问题，比如你的web容器，我们以NGINX为例，正常情况下我们配置启动服务只需要启动响应的service即可。例如 service NGINX start 但是 这样做NGINX为后天进程启动模式，就导致docker前台没有运行的应用
这样的容器后台启动后会，会立即自杀应为docker觉得NGINX没什么事干
所以解决方案是让你的程序已前台进程的方式启动

2.查看容器日志 
 docker logs -f -t --tail 容器ID
             -t 是加入的时间
             -f 跟随最新的日志打印
             --tail 数字 显示最后多少条

3.查看容器运行的进程
docker top 容器ID 

4.查看容器内部细节
docker insperct 容器ID

5.进入正在运行的容器并以命令行交互
   docker exec -it 容器id bashShell
   		docker exec -t 10b9a3588ab9 ls -l /tmp
   docker attach 容器ID
   上述两个的区别
   		attach  直接进入容器启动命令的终端，不会启动新的进程
   		exec    是在容器中打开新的终端，并且可以启动新的进程

6.从容器中拷贝文件到主机
	docker cp 容器id:容器类路径   目的主机路径
```







#### docker 导入删除容器

1.导出某个容器

导出某个容器，使用docker export命令，语法：

```shell
:  docker export $container_id > 容器快照名
```

![](https://ws1.sinaimg.cn/large/006tKfTcgy1g1m16a7odrj31d108rq3r.jpg)

2、导入容器**--docker import命令
有了容器快照之后，我们可以在想要的时候随时导入。导入快照使用docker import命令。

例如我们可以使用cat centos.tar | docker import - my/centos:v888 导入容器快照作为镜像



3.停止容器

```shell
docker stop $(docker ps -a -p)
```

4.查看本地用那些容器

```shell
docker images
```

4.删除images ，通过images的id来进行

```shell
docker rmi <images id>
```

删除全部

```shell
docker rmi $(docker images -q)
```

