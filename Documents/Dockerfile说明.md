# 基本结构
+ 基础镜像信息
+ 维护者信息
+ 镜像操作指令
+ 容器启动时执行指令

## 指令
分类|指令|说明
---|---|---
配置指令|ARG|定义创建镜像过程中使用的变量
配置指令|FROM|指定所创建镜像的基础镜像
配置指令|LABEL|为生成的镜像添加元数据标签信息
配置指令|EXPOSE|声明镜像内服务监听的端口
配置指令|ENY|指定环境变量
配置指令|ENTRYPOINT|指定镜像的默认入口命令
配置指令|VOLUME|创建一个数据挂载点
配置指令|USER|指定运行容器时用户名或UID
配置指令|WORKDIR|配置工作目录
配置指令|ONBUILD|创建子镜像时指定自动执行的操命令
配置指令|STOPSIGNAL|指定退出的信号值
配置指令|HEALTHCHECK|配置所启动容器如何进行健康检查
配置指令|SHELL|指定默认shell类型
操作指令|RUN|运行指定命令
操作指令|CMD|启动容器时指定默认执行的命令
操作指令|ADD|添加内容到镜像
操作指令|COPY|复制内容到镜像
## 指令详解
+ ARG
    + 定义创建镜像过程中使用的变量
    + 格式：ARG<name\>[=<default value\>]
    + 说明：在执行docker build 时，可以通过-build-arg[=]来为变量赋值。当镜像编译成功后，ARG指定的变量将不再存在。
    + 内置变量：HTTP_PROXY,HTTPS_PROXY,FTP_PROXY,NO_PROXY
+ FROM
    + 指定所创建镜像的基础镜像
    + 格式：
        + FROM<image\>[AS<name>]
        + FROM<image\>:<tag\>[AS<name>]
        + FROM<image\>@<digest\>[AS<name>]
    + 说明：任何Dockefile中第一条指令必须为FROM指令。并且，如果在同一个Dockerfile中创建多个镜像时，可以使用多个FROM指令。
    + 示例：
    ```dockerfile
    ARG VERSION=9.3
    FROM debian:${VERSION}
    ```
+ LABEL
    + 为生成的镜像添加元数据信息
    + 格式：LABEL<key\>=<value\> <key\>=<value\>   
    + 示例：
    ```dockerfile
    LABEL version="1.0.0-rc3"
    LABEL author="jianglei1987@github" date="2020-01-01"
    LABLE description="docker 笔记"
    ```
+ EXPOSE
    + 声明镜像内服务监听的端口
    + 格式：EXPOSE<port\>[<port\>/<protocol\>]
    + 说明：该指令只是起到声明作用，并不会自动完成端口映射。
    + 示例
    ```dockerfile
    EXPOSE 22 80 8443
    ```
+ ENV
    + 指定环境变量
    + 格式： 
        + ENV<key\><value>
        + ENV<key>=<value>
    + 示例：
    ```dockerfile
    ENV APP_VERSION=1.0.0
    ENV APP_HOME=/usr/local/app
    ENV PATH $PATH:/usr/local/bin
    ```
    + 说明：指令指定的环境变量在运行时可以被覆盖掉，如docker run --env <key\>=<value\> built_imagee。
+ ENTRYPOINT
    + 指定镜像的默认入口命令，该入口命令会在启动容器时作为根命令执行，所有传入值作为该命令的参数。
    + 格式：
        + ENTRYPOINT["executable","param1","param2"]:exec 调用执行。
        + ENTRYPOINT command pram1 param2 :shell中执行。
    + 说明：
        + 每个Dockerfile中只能有一个ENTRYPOINT，当指定多个时，只有最后一个起效。
        + 在运行时，可以被--entrypoint参数覆盖掉，如docker run --entrypoint。
+ VOLUME
    + 创建一个数据挂载点。
    + 格式：VOLUME["/data"]
    + 说明：运行容器时可以从本地主机或其他容器挂载数据卷，一般用来存放数据库和需要保持的数据等。
+ USER
    + 指定运行容器时的用户名或UID,后续的Run等指令也会使用指定的用户身份。
    + 格式：USER daemon
    + 说明：当服务不需要管理员权限时，可以通过该命令指定运行用户，并且可以在Dockerfile中创建所需要的用户。
+ WORKDIR
    + 为后续的RUN、CMD、ENTRYPOINT指令配置工作目录。
    + 格式为：WORKERDIR /path/to/workdir
    + 可以使用多个WORKDIR指令，后续命令如果参数是相对路径，则会基于之前命令指定的路径。
+ ONBUILD
    + 指定当基于所生成镜像创建子镜像时，自动执行的操作命令。
+ STOPSIGNAL
    + 指定所创建镜像启动的容器接收退出的信号值。
+ HEALTHCHECK
    + 配置所启动容器如何进行健康检查。
    + 格式：
        + HEALTHCHECK [OPTIONS] CMD command:根据所执行命令返回值是否为0来判断。
        + HEALTHCHECK NONE:禁止基础镜像中的健康检查。
        + OPTIONS支持如下参数：
            + .-interval=DURATION(default:30s)：过多久检查一次。
            + .-timeout=DURATION(default:30s)：每次检查等待结果的超时时间。
            + .-retries=N(default:3):如果失败了，重试几次才最终确定失败。
+ SHELL
    + 指定其他命令使用shell时的默认shell类型：默认值为["/bin/sh","-c"]
+ RUN 
    + 运行指定命令
    + 格式：
        + RUN <command/> 默认在shell终端中运行命令。
        + RUN["executable","param1","param2"] 使用exec执行，不会启动shell环境。
    + 示例：
    ```dockerfile
    RUN apt-get update \
        && apt-get install -y libsnappy-dev zlib1g-dev libbz2-dev\
        && rm -rf /var/cache/apt \
        && rm -rf /var/lib/apt/lists/*
    ```
+ CMD
    + CMD指令用来指定启动容器时默认执行的命令
    + 格式：
        + CMD ["executable","param1","param2"]：相当于执行 executable param1 param2。
        + CMD command param1 param2 ：在默认的Shell中执行，提供给需要交互的应用。
        + CMD ["param1","param2"]：提供给ENTRYPOINT的默认参数。
+ ADD
    + 添加到内容到镜像
    + 格式：ADD <src\> <dest\>
    + 将复制指定的<src\>路径到内容到容器的<dest\>路径下。
        + <src\> 可以是Dockerfile所在目录的一个相对路径（文件或目录）；也可以是一个URL;还可以是一个tar文件（自动解压为目录）。
        + <dest> 可以是镜像内绝对路径，或者相对工作目录（WORKDIR）的相对路径。
    + 示例：
    ```dockerfile
    ADD *.C/code/
    ```
+ COPY
    + 复制内容到镜像。
    + 格式: COPY <src\> <dest\>
    + 复制本地主机的<src\>(为Dockerfile 所在目录的相对路径，文件或目录)下内容到镜像中的<dest\>。目标不存在时，会自动创建。