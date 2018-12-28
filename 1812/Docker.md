#### 分层概念

99% 的镜像都是由 base 镜像中安装和配置需要的软件所构建出来的

当容器启动时，一个可写的容器层会加载的镜像顶部

所有对容器的操作和改变，都只会发生在容器层

下面的镜像层是不变的，是只读的



创建文件时 新文件被加在容器中

读文件会从上到下找 一旦找到就复制到容器层

改文件也是复制在容器层

删除文件 会找到文件后在容器层里记录删除操作



所以镜像可以被许多容器共享

[来源](https://blog.csdn.net/CloudMan6/article/details/71159794)



#### 用 commit 创建镜像



docker run -it xxx  ( 运行 镜像 -it 是进入交互模式 )

然后 安装软件后 

docker commit 旧名字 新名字 ( 保存为新镜像 )



docker ps 查看运行中的

docker images 查看有的镜像



但不推荐这样玩 最好写 Dockerfile



#### Dockerfile



docker build -t xxx . 

​	-t 是重命名镜像

​	. 是默认从当前目录开始找 Dockerfile  （ 当前目录就是 build context ）

​	-f 可以指定 Dockerfile 位置

Dockerfile 中的 ADD、COPY 等命令可以将 build context 中的文件添加到镜像



#### 镜像缓存的特性



build 时加 --no-cache 可不缓存

否则会相同操作会缓存

构建命令的顺序有很大的影响，如果交换了顺序 上下层就不一样了，缓存就会失效





#### 常用指令





FROM
指定 base 镜像。

MAINTAINER
设置镜像的作者，可以是任意字符串。

COPY
将文件从 build context 复制到镜像。
COPY 支持两种形式：

COPY src dest

COPY ["src", "dest"]

注意：src 只能指定 build context 中的文件或目录。

ADD
与 COPY 类似，从 build context 复制文件到镜像。不同的是，如果 src 是归档文件（tar, zip, tgz, xz 等），文件会被自动解压到 dest。

ENV
设置环境变量，环境变量可被后面的指令使用。例如：

...

ENV MY_VERSION 1.3

RUN apt-get install -y mypackage=$MY_VERSION

...


EXPOSE
指定容器中的进程会监听某个端口，Docker 可以将该端口暴露出来。我们会在容器网络部分详细讨论。

VOLUME
将文件或目录声明为 volume。我们会在容器存储部分详细讨论。

WORKDIR
为后面的 RUN, CMD, ENTRYPOINT, ADD 或 COPY 指令设置镜像中的当前工作目录。

RUN
在容器中运行指定的命令。

CMD
容器启动时运行指定的命令。
Dockerfile 中可以有多个 CMD 指令，但只有最后一个生效。CMD 可以被 docker run 之后的参数替换。

ENTRYPOINT
设置容器启动时运行的命令。

Dockerfile 中可以有多个 ENTRYPOINT 指令，但只有最后一个生效。CMD 或 docker run 之后的参数会被当做参数传递给 ENTRYPOINT。

来源：CSDN 
原文：https://blog.csdn.net/CloudMan6/article/details/72353838 



2018年12月28日 23点46分







