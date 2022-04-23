# 定义
定义和运行多个Docker容器的应用
# docker-compose.yml
定义一组相关联的应用容器为一个服务栈。
序号|命令|功能
---|---|---
1|build|指定Dockerfile所在文件夹的路径
2|cap_add,cap_drop|指定容器的内核能力分配
3|command|覆盖容器启动后默认执行的命令
4|cgroup_parent|指定父egroup组，意味着将继承该组的资源限制。
5|container_name|指定容器名称
6|cevices|指定设备映射关系
7|depends on|指定多个服务之间的依赖关系
8|dns|自定义DNS服务器
9|dns_search|配置DNS搜索域
10|dockerfile|指定额外的编译镜像的Dockerfile文件
11|entrypont|覆盖容器中默认的入口命令
12|env_file|从文件中获取环境变量
13|environment|设置环境变量
14|expose|暴露端口，但不映射到宿主机，只被连接的服务访问
15|extend|基于其他模板文件进行扩展
16|external_links|链接到docker-compose.yml外部的容器
17|extra_hosts|
18|healthcheck|
19|image|
20||
21|labels|
22|links|
23|logging|
24|network_mode|
25|networks|
26|pid|
27|ports|
28|secrets|
29|security_opt|
30|stop_grace_period|
31|stop_signal|
32|sysctls|
33|ulimits|
34|userns_mode|
35|volumes|
36|restart|
37|deploy|


# 重要概念
+ 任务(task)：一个容器被称为一个任务。任务拥有独一无二的ID，在同一个服务中的多个任务序号依次递增。
+ 服务(service)：某个相同应用镜像的容器副本集合，一个服务可以横向扩展多个容器实例。
+ 服务栈(stack)：由多个服务组成，相互配合完成特定业务，如Web应用服务、数据库服务共同构成Web服务栈，一般由一个docker-compose.yml文件定义。
# 安装卸载
+ pip安装
+ 二进制包
+ 容器中执行