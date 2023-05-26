# Docker 常用命令

___

## Docker 服务

```{shell}
sudo systemctl start docker
```

```{shell}
sudo systemctl stop docker
```

```{shell}
sudo systemctl restart docker
```

```{shell}
sudo systemctl status docker
```

```{shell}
sudo systemctl enable docker
```

___

## Docker 镜像

hub.docker.com

```{shell}
sudo docker images
```

```{shell}
sudo docker images -q # ID
```

```{shell}
sudo docker search IMAGE
```

```{shell}
sudo docker pull IMAGE:VERSION
```

```{shell}
sudo docker rmi IMAGE:VERSION
```

```{shell}
sudo docker rmi IMAGE
```

___

## Docker 容器

查看容器

```{shell}
sudo docker ps
```

```{shell}
sudo docker ps -a
```


运行容器(create + start)

```{shell}
sudo docker run [OPTIONS] IMAGE [COMMAND] [ARG...] # 常用
```

```{shell}
sudo docker start CONTAINER
```

```{shell}
sudo docker stop CONTAINER
```

进入容器

```{shell}
sudo docker exec [OPTIONS] CONTAINER [COMMAND] [ARG...]
```

- `-i` : 保持容器运行，通常和`-t`同时使用，加入这两个参数后，容器创建后自动进入容器中，退出容器后，容器自动关闭
- `-t` : 为容器重新分配一个伪输入终端，通常和`-i`一起使用
- `-d` : 以守护（后台）模式运行容器。创建一个容器在后台运行，使用`docker exec` 进入容器。 退出后，容器不会关闭。
- `-it` : 交互式容器
- `-id` : 守护式容器
- `--name` : 为创建的容器命名
- `exit` : 退出容器

___

## 数据卷

共享数据 -v

```{shell}
sudo docker run [OPTIONS] -v /home/data:/root/data IMAGE [COMMAND] [ARG...]
```

数据卷容器

```{shell}
sudo docker run [OPTIONS] -v /volume IMAGE [COMMAND] [ARG...]
```

```{shell}
sudo docker run [OPTIONS] --volumes-from  IMAGE [COMMAND] [ARG...]
```

数据copy

```{shell}
docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-
```

___

## 应用部署

端口映射 -p

```{shell}
sudo docker run [OPTIONS] -p 8080:8080  IMAGE [COMMAND] [ARG...]
```

___

## Dockerfile

### Docker镜像原理

文件系统：bootfs，rootfs

- Docker镜像的本质
> 一个分层的文件系统
- Docker中一个centos镜像为什么只有200MB，而一个centos操作系统的iso文件要几个G?
> Centos的iso镜像文件包含bootfs和rootfs，而docker的centos镜像复用了从啊哦做系统的bootfs，只有rootfs和其他镜像层
- Docker中一个tomcat镜像为什么有500MB,而一个tomcat安装包只有70多MB?
> 由于docker中镜像是分层的，tomcat虽然只有70多MB,但他需要依赖父镜像和基础镜像，所有对外暴露的tomcat镜像大小500多MB

1. Docker镜像是由特殊的文件系统叠加而成
2. 最底层是bootfs，并使用宿主机的bootfs
3. 第二层是root文件系统rootfs，称为base image
4. 然后网上可以叠加其他的镜像文件
5. 统一文件系统（Union File System）技术能够将不同的层整合成一个文件系统，为这些曾提供了一个统一的视角，这样就隐藏了多层的存在，在用户的角度来看，只存在一个文件系统。
6. 一个镜像可以放在另一个镜像的上面，位于下面的镜像称为父镜像，最底层的镜像称为基础镜像。
7. 当从一个镜像启动容器时，Docker会在最顶层加载一个读写文件系统作为容器。

### 镜像制作

#### 通过容器

```{shell}
sudo docker commit CONTAINER_ID IMAGE_NAME:TAG
```

```{shell}
docker save -o [FILENAME] *.tar [IMAGE]
```

```{shell}
docker load -i [FILENAME] *.tar
```

挂载的数据卷不会包含在镜像中

#### Dockerfile

##### Example

```{dockerfile}
FROM centos:7

LABEL name="changmeng"
LABEL email="houchangmeng@gmail.com"

RUN yum install -y vim
WORKDIR /usr

CMD ["/bin/bash"]
```

```{shell}
docker build -t -f [DOCKERFILE] [NAME]:[TAG] [.(CONTENT PATH)] 
```

___

## Docker和虚拟机

相同：
- 容器和虚拟机具有相似的资源隔离和分配优势

不同：
- 容器虚拟化的是操作系统，虚拟机虚拟化的是硬件
- 传统领虚拟机可以运行不同的操作系统，容器只能运行与宿主机同一类型的操作系统

___

## Docker Compose

使用多个Docker Container

