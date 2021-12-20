# Docker 学习笔记

[toc]

## 镜像 Images

- 获取镜像：`docker pull hello-world`

- 列出镜像：`docker images`

- 删除镜像：`docker rmi hello-world`

### 镜像标签（？）

```shell
docker tag 860c279d2fec runoob/centos:dev
docker tag 镜像ID 用户名称/镜像源名(repository name):新标签名(tag)
```

### 从容器创建镜像

对容器更改之后，commit 来提交这个容器的副本作为镜像：

```shell
docker commit -m="has update" -a="runoob" e218edb10161 runoob/ubuntu:v2
```

- `-m`：提交的描述信息。
- `-a`：指定镜像作者。
- `e218edb10161`：容器 ID。
- `runoob/ubuntu:v2`：指定要创建的目标镜像名。

### 从 Dockerfile 创建镜像

```shell
docker build -t runoob/centos:6.7 .
```

- `-t`：指定要创建的目标镜像名。
- `.`：Dockerfile 文件所在目录，也是上下文路径，可以指定 Dockerfile 的绝对路径。

## 容器 Conatiner

- 容器：镜像的实例化。
- 每个容器都有一个唯一的 ID，是一长串字符。
- 查看当前所有容器：`docker ps -a`。
  - 查询最后一次创建的容器：`docker ps -l`。

- 删除容器：`docker rm <ID/name>`。
  - 先 stop 了才能删除。


### 启动容器 run

```shell
docker run ubuntu:15.10 /bin/echo "Hello world"
```

在 `ubuntu:15.10` 容器内运行命令 `/bin/echo "Hello world"`

- `-t` 在新容器内指定一个伪终端或终端。
- `-i` 允许你对容器内的标准输入（stdin）进行交互。
  - 以上这两个参数 `-it` 可以进入容器内部的终端。

- `-d` 后台运行。
- `--name` 指定容器的命名标识。

### 运行控制

- 启动、停止和重启：`docker start/stop/restart`

- 进入容器：

  - attach：`docker attach <ID/name> `，如果退出会导致容器停止。

  - exec：`docker exec -it <ID/name> /bin/bash`，如果退出容器不会停止。

- 查看日志：`docker logs -f <ID/name>`，查看容器内部的标准输出。
- 查看进程：`docker top <ID/name>`，查看容器内部运行的进程。
- 查看 Docker 的配置：`docker inspect`。（？）

### 容器快照

- 导出容器为快照：`docker export 1e560fca3906 > ubuntu.tar`
- 导入快照为镜像：`cat ubuntu.tar | docker import - test/ubuntu:v1`（？）
  - 通过 URL 导入：`docker import http://example.com/exampleimage.tgz example/imagerepo`

### 网络映射

- `-P` 随机端口映射。使用 `docker ps` 可以看到实际的端口映射。
- `-p` 指定端口映射。`-p 8000:80` 表示将容器内的 80 端口映射到外部的 8000 端口。
  - 也可以使用地址映射（？）。
- 绑定 udp 端口可以在端口号后加上 `/udp`：`8000/udp`。默认绑定的是 tcp 端口。
- 查看端口绑定情况：`docker port <ID/name> 5000`

### 容器互联

- 新建网络：`docker network create -d bridge test-net`
  - 有 `bridge/overlay` 两种类型（？）。
- 容器连接：`docker run -itd --name test1 --network test-net ubuntu /bin/bash` 加入 `test-net` 网络。
  - 在该网络中的容器内部 `ping test1` 可以成功。

#### 配置 DNS

- 设置容器全局 DNS：在 `/etc/docker/daemon.json` 中配置如下内容，设置所有容器的 DNS：

  ```
  {
    "dns" : [
      "114.114.114.114",
      "8.8.8.8"
    ]
  }
  ```

  重启 docker 生效。

- 查看容器中 DNS：`docker run -it --rm  ubuntu  cat etc/resolv.conf`
- 手动指定容器 DNS：`docker run -it --rm -h host_ubuntu --dns=114.114.114.114 --dns-search=test.com ubuntu`
  - `--rm`：容器退出时自动清理容器内部的文件系统。
  - `-h HOSTNAME` 或 `--hostname=HOSTNAME`：设定容器的主机名，它会被写到容器内的 `/etc/hostname` 和 `/etc/hosts`。
  - `--dns=IP_ADDRESS`：添加 DNS 服务器到容器的 `/etc/resolv.conf` 中，让容器用这个服务器来解析所有不在 `/etc/hosts` 中的主机名。
  - `--dns-search=DOMAIN`：设定容器的搜索域，当设定搜索域为 `.example.com` 时，在搜索一个名为 `host` 的主机时，DNS 不仅搜索 `host`，还会搜索 `host.example.com`。

## 仓库 Registry/Repository

Docker Hub。

## Dockerfile

- Dockerfile 是一个用来构建镜像的文本文件，文本内容包含了一条条构建镜像所需的指令和说明。

### 指令和语法

- `FROM`：指定基于的镜像。

- `RUN`：在 `docker build` 时执行命令：

  - shell 格式：`RUN <命令>` 相当于在 shell 中运行命令。
  - exec 格式：`RUN ["可执行文件", "参数1", "参数2"]` 等价于 `RUN 可执行文件 参数1 参数2`。
  - 命令简化：Dockerfile 的指令每执行一次都会在 docker 上新建一层，过多无意义的层，会造成镜像膨胀过大。可以使用 `&&` 符号合并多行命令。

- `COPY`：从上下文目录中复制文件或者目录到容器里指定路径。

  ```shell
  COPY [--chown=<user>:<group>] <源路径1>...  <目标路径>
  COPY [--chown=<user>:<group>] ["<源路径1>",...  "<目标路径>"]
  ```

  - 源路径以上下文路径为根，目标路径是容器内的路径。

- `ADD`：复制文件，和 `COPY` 功能类似。但是更推荐 `COPY`。

  - 区别是文件为 tar 压缩文件，压缩格式为 gzip，bzip2 或 xz 情况下，会自动复制并解压到目标路径。

- `CMD`：在 `docker run` 时执行命令，用于指定容器启动后默认运行的程序。

  - 语法和 `RUN` 类似。区别是可以直接 `CMD ["参数1", "参数2"]`，此时相当于给 entrypoint 指定的程序传参。
  - 如果 Dockerfile 中如果存在多个 `CMD` 指令，仅最后一个生效。
  - 设定项可以被 `docker run` 时传入的 `--entrypoint` 参数覆盖。

- `ENTRYPOINT`：类似 `CMD`，但是不会被运行时参数覆盖。

  - 如果 Dockerfile 中如果存在多个 `ENTRYPOINT` 指令，仅最后一个生效。
  - `CMD` 和 `ENTRYPOINT` 组合使用，可以实现指定变参和定参。

- `ENV`：指定环境变量，只在 `docker build` 中有效。

  ```shell
  ENV <key> <value>
  ENV <key1>=<value1> <key2>=<value2>...
  ```

- `ARG`：指定环境变量，全程有效。语法和 `ENV` 一样。

- `VOLUME`：定义匿名数据卷。在启动容器时忘记挂载数据卷，会自动挂载到匿名卷。

  ```shell
  VOLUME ["<路径1>", "<路径2>"...]
  VOLUME <路径>
  ```

  - `docker run` 时可以通过 `-v` 参数修改挂载点。

- `EXPOSE`：声明端口。声明的端口会被 `-P` 映射。

- `WORKDIR`：

- `USER`：

- `HEALTHCHECK`：

- `ONBUILD`：

- `LABEL`：给镜像添加一些元数据（metadata）：`LABEL <key>=<value> <key>=<value> <key>=<value> ...`。

### 上下文路径

- 在 build 时指定的路径，至少存放 Dockerfile 文件。
- 该路径下的文件会被打包发送给 Docker 引擎供容器调用。

## Compose

