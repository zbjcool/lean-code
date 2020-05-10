[TOC]


### Docker 是什么？
Docker 将应用程序与该程序的依赖，打包在一个文件里面。运行这个文件，就会生成一个虚拟容器。程序在这个虚拟容器里运行，就好像在真实的物理机上运行一样。有了 Docker，就不用担心环境问题。


### Docker 的主要用途
Docker 的主要用途，目前有三大类。

（1）提供一次性的环境。比如，本地测试他人的软件、持续集成的时候提供单元测试和构建的环境。

（2）提供弹性的云服务。因为 Docker 容器可以随开随关，很适合动态扩容和缩容。

（3）组建微服务架构。通过多个容器，一台机器可以跑多个服务，因此在本机就可以模拟出微服务架构。

### Docker 的安装
安装后
```bash
$ docker version
# 或者
$ docker info
```
Docker 需要用户具有 sudo 权限，为了避免每次命令都输入sudo，可以把用户加入 Docker 用户组[官方文档](https://docs.docker.com/install/linux/linux-postinstall/#manage-docker-as-a-non-root-user)
1. Create the docker group.
```bash
sudo groupadd docker
```
2. Add your user to the docker group.

```bash
sudo usermod -aG docker $USER
```
3. 重新登录


Docker 是服务器----客户端架构。命令行运行docker命令的时候，需要本机有 Docker 服务。如果这项服务没有启动，可以用下面的命令启动[官方文档](https://docs.docker.com/config/daemon/systemd/)。
```bash

# service 命令的用法
$ sudo service docker start

# systemctl 命令的用法
$ sudo systemctl start docker
```

### image

Docker 把应用程序及其依赖，打包在 image 文件里面。只有通过这个文件，才能生成 Docker 容器。image 文件可以看作是容器的模板。Docker 根据 image 文件生成容器的实例。同一个 image 文件，可以生成多个同时运行的容器实例。

image 是二进制文件。实际开发中，一个 image 文件往往通过继承另一个 image 文件，加上一些个性化设置而生成。举例来说，你可以在 Ubuntu 的 image 基础上，往里面加入 Apache 服务器，形成你的 image。

```bash

# 列出本机的所有 image 文件。
$ docker image ls

# 删除 image 文件
$ docker image rm [imageName]
```


image 文件是通用的，一台机器的 image 文件拷贝到另一台机器，照样可以使用。一般来说，为了节省时间，我们应该尽量使用别人制作好的 image 文件，而不是自己制作。即使要定制，也应该基于别人的 image 文件进行加工，而不是从零开始制作。

为了方便共享，image 文件制作完成后，可以上传到网上的仓库。Docker 的官方仓库 Docker Hub 是最重要、最常用的 image 仓库。此外，出售自己制作的 image 文件也是可以的。

### 实例：hello world

需要说明的是，国内连接 Docker 的官方仓库很慢，还会断线，需要将默认仓库改成国内的镜像网站，具体的修改方法在下一篇文章的第一节。有需要的朋友，可以先看一下。

首先，运行下面的命令，将 image 文件从仓库抓取到本地。

```bash
$ docker image pull library/hello-world
```
上面代码中，docker image pull是抓取 image 文件的命令。library/hello-world是 image 文件在仓库里面的位置，其中library是 image 文件所在的组，hello-world是 image 文件的名字。

由于 Docker 官方提供的 image 文件，都放在library组里面，所以它的是默认组，可以省略。因此，上面的命令可以写成下面这样。

```bash
$ docker image pull hello-world
```
抓取成功以后，就可以在本机看到这个 image 文件了。

```bash
$ docker image ls
docker images
docker images hello-world
```
现在，运行这个 image 文件。

```bash
$ docker run hello-world
```
docker container run命令会从 image 文件，生成一个正在运行的容器实例。

注意，docker container run命令具有自动抓取 image 文件的功能。如果发现本地没有指定的 image 文件，就会从仓库自动抓取。因此，前面的docker image pull命令并不是必需的步骤。

-rm
在Docker容器退出时，默认容器内部的文件系统仍然被保留，以方便调试并保留用户数据。

但是，对于foreground容器，由于其只是在开发调试过程中短期运行，其用户数据并无保留的必要，因而可以在容器启动时设置--rm选项，这样在容器退出时就能够自动清理容器内部的文件系统。示例如下：

docker run --rm bba-208

等价于

docker run --rm=true bba-208


显然，--rm选项不能与-d同时使用，即只能自动清理foreground容器，不能自动清理detached容器
注意，--rm选项也会清理容器的匿名data volumes。

所以，执行docker run命令带--rm命令选项，等价于在容器退出后，执行docker rm -v。




如果运行成功，你会在屏幕上读到下面的输出。
```bash
$ docker container run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

... ...
```
输出这段提示以后，hello world就会停止运行，容器自动终止。

有些容器不会自动终止，因为提供的是服务。比如，安装运行 Ubuntu 的 image，就可以在命令行体验 Ubuntu 系统。

```bash
$ docker container run -it ubuntu bash
```
对于那些不会自动终止的容器，必须使用docker container kill 命令手动终止。

```bash
$ docker container kill [containID]
```


### 容器文件
image 文件生成的容器实例，本身也是一个文件，称为容器文件。也就是说，一旦容器生成，就会同时存在两个文件： image 文件和容器文件。而且关闭容器并不会删除容器文件，只是容器停止运行而已。


列出本机正在运行的容器
$ docker container ls

列出本机所有容器，包括终止运行的容器
$ docker container ls --all
上面命令的输出结果之中，包括容器的 ID。很多地方都需要提供这个 ID，比如上一节终止容器运行的docker container kill命令。

终止运行的容器文件，依然会占据硬盘空间，可以使用docker container rm命令删除。


$ docker container rm [containerID]
运行上面的命令之后，再使用docker container ls --all命令，就会发现被删除的容器文件已经消失了。

### 实例：制作自己的 Docker 容器

### 其他有用的命令

docker 的主要用法就是上面这些，此外还有几个命令，也非常有用。

（1）docker container start

前面的docker container run命令是新建容器，每运行一次，就会新建一个容器。同样的命令运行两次，就会生成两个一模一样的容器文件。如果希望重复使用容器，就要使用docker container start命令，它用来启动已经生成、已经停止运行的容器文件。


$ docker container start [containerID]
（2）docker container stop

前面的docker container kill命令终止容器运行，相当于向容器里面的主进程发出 SIGKILL 信号。而docker container stop命令也是用来终止容器运行，相当于向容器里面的主进程发出 SIGTERM 信号，然后过一段时间再发出 SIGKILL 信号。


$ bash container stop [containerID]
这两个信号的差别是，应用程序收到 SIGTERM 信号以后，可以自行进行收尾清理工作，但也可以不理会这个信号。如果收到 SIGKILL 信号，就会强行立即终止，那些正在进行中的操作会全部丢失。

（3）docker container logs

docker container logs命令用来查看 docker 容器的输出，即容器里面 Shell 的标准输出。如果docker run命令运行容器的时候，没有使用-it参数，就要用这个命令查看输出。


$ docker container logs [containerID]
（4）docker container exec

docker container exec命令用于进入一个正在运行的 docker 容器。如果docker run命令运行容器的时候，没有使用-it参数，就要用这个命令进入容器。一旦进入了容器，就可以在容器的 Shell 执行命令了。


$ docker container exec -it [containerID] /bin/bash
（5）docker container cp

docker container cp命令用于从正在运行的 Docker 容器里面，将文件拷贝到本机。下面是拷贝到当前目录的写法。


$ docker container cp [containID]:[/path/to/file] .

### 搜索某个image

docker search nginx



## docker安装nginx
docker search nginx

docker pull nginx

docker run --name runoob-nginx-test -p 8081:80 -d nginx

执行以上命令会生成一串字符串，类似 6dd4380ba70820bd2acc55ed2b326dd8c0ac7c93f68f0067daecad82aef5f938，这个表示容器的 ID，一般可作为日志的文件名。

我们可以使用 docker ps 命令查看容器是否有在运行：


docker run -d -p 8082:80 --name zbj-nginx -v ~/docker/nginx/www:/usr/share/nginx/html -v ~/docker/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v ~/docker/nginx/logs:/var/log/nginx nginx

