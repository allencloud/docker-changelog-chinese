# Docker 1.10.0 Changelog

英文链接地址：[docker-1.10.0 CHANGELOG.md](https://github.com/moby/moby/blob/v1.10.0/CHANGELOG.md)。

Docker粉们，是否还记得上一个Docker大版本的发布是什么时候？如果已经模糊的话，小编告诉你，是4个月前的10月14号。如今，就在中国的春节即将到来之际，Docker官方发布了跨时代的Docker 1.10.0版本，着实为猴年的到来献上了最大的一份礼。

如果说Docker 1.9.0的overlay网络意味着Docker集群能力方面质的飞跃，那么这次发布的Docker 1.10.0更是多个方面实现重大突破。总体而言，小编认为，意义非凡的变化与演进，以及看点，主要有以下这几方面：

* Docker Engine支持配置热更新，容器与Docker Daemon的耦合性大大降低
* Docker API的完善程度稳步上升，容器隔离维度更细致，更全面
* Docker安全划时代的支持了User Namespace与seccomp，安全程度直逼虚拟机
* 镜像格式首推“内容寻址方式”，可以做到Docker镜像的全球唯一性，镜像的安全与存储都有大的改观
* 网络能力，无论单机，还是跨主机，功能逐渐完善，成熟度稳步提升，发展之迅速不得不令人佩服
* 稍显惋惜的是，传统的容器管理工具LXC即将退出Docker的舞台，伴随Docker接近三年的时间，终于被更完善的容器管理方案取代，同时Docker的飞速发展以及追求卓越也一览无余

看来Docker 1.10.0的主要变化，我们来看一下这一大版本更为细节的变化，相信Docker Engine、安全、Distribution、网络、日志、存储卷Volume等多方面，总有一款适合您。详情请见下文：


## 1.Docker Engine(Runtime)

* 开发了新命令`docker update`,允许用户更新运行中容器的资源限制（对于资源的重分配意义很大，后期可以考虑集成）
* 为`docker run`命令添加flag参数`--tmpfs`，用于为容器挂载一个内存文件系统，便于文件的快速读写
* 为`docker images`命令添加flag参数`--format`
* 允许用户在文件中配置Docker Daemon的运行参数，并通过SIGHUP信号对其进行热加载，换言之，Docker开始支持动态加载配置参数
* 更新`docker events`命令的内容输出，使其包含更多的元数据以及事件类型；但是，需要注意的是，此改动完全兼容Docker的API，但是在Docker CLI中仍未支持
* 为`docker run`命令添加flag参数`--blkio-weight-device`， 为`docker run`命令添加flag参数`--blkio-weight-device`
* 为`docker run`命令添加flag参数`--device-read-bps`和`--device-write-bps`，用于磁盘I/O的读写BPS
* 为`docker run`命令添加flag参数`--device-read-iops`和`--device-write-iops`,用于磁盘I/O的iops
* 为`docker run`命令添加flag参数`--oom-score-adj`，用于容器进程发生oom现象时，如何选择kill进程的评分依据
* 为命令`run`,`attach`,`start`以及`exec`添加flag参数`--detach-keys`,以此来覆盖从容器退出的的默认键盘键，比如使用Docker客户端自定义的ctrl+a来作为这些命令的ESC键
* 为`docker run`,`docker create`以及`docker build`命令添加`--shm-size`，用于动态内存文件系统的大小（/dev/shm等同于tmpfs）
* 丰富`docker info`命令的输出内容，为其添加运行容器的数目，停止容器的数目以及挂起容器的数目
* 丰富`docker info`命令的输出内容，为其添加操作系统类型以及机器结构
* 为`docker daemon`命令添加flag参数`--cgroup-parent`,从而为所有容器设置父cgroup，即挂载于制定的cgroup路径下
* 为`docker cp`命令添加flag参数`-L`，使得`cp`命令不是仅仅拷贝符号链接，而是拷贝符号链接真实指向的内容
* 新增过滤规则`status=dead`,使命令`docker ps`的输出更符合用户需求
* 更改`docker run`命令的返回码(exit code)，从而区分在命令返回的原因究竟是Docker Engine的错误，还是容器内部应用自身的错误
* 扩展`docker events`命令中添加`--since`和`--util`参数后的内容输出，使其支持时区以及纳秒
* 为`docker stats`命令添加flag参数`-a/--all`，用于显示运行中容器以及停止容器的资源使用
* 将默认的cgroup-driver设置为`cgroupfs`
* 当使用`docker build -t`命令为某一镜像设置标签时，Docker Daemon触发一个tag事件；换言之，Docker Daemon新支持一种tag事件
* 重新启动Docker Daemon时，尽最大的努力来合理安排link容器的启动顺序
* 允许`docker build`命令为镜像设置多个标签（tag）
* 对任意url请求添加OPTIONS请求，而不是修改跨域存在的问题
* 修复`docker build`命令的flag参数`--quiet`,使其真正意义上实现quiet，即不输出构建过程的内容
* 修复命令`docker images --filter dangling=false`,现在真正意义上显示的是所有的non-dangling镜像
* 修复命令`docker volumes ls --filter dangling=false`,现在真正意义上显示的是所有的non-dangling的存储卷
* 修复重启容器时因为容器内部进程长时间处理SIGTERM信号未果儿存在的资源竞争BUG
* Docker Daemon中加入了IBM公司的的共享文件系统GPFS
* 修复Bug，使得volume driver不允许被容器化
* 删除容器过程中，不删除有命名的挂载点
* 修复阻止容器启动的某些不知名Bug
* 禁止在重启过程中对容器执行`docker exec`操作
* devicemapper方面，当Docker Daemon重启过程中`--storage-opt dm.basesize`参数的增长会导致基础设备存储空间的增长


##2.安全
* 为`Docker Daemon`命令添加flag参数`--userns-remap`，用以支持Linux Namespace中的用户命名空间（User Namespace），之前Docker对于用户命名空间的支持仅仅存在于Docker试验版。
* 在`docker run`命令的flag参数`--security-opt`中，添加对自定义seccomp文档的支持，主要用户对容器应用在系统调用方面实现沙箱化
* 为Docker添加默认的seccomp文档，在容器的系统调用方案实现更为细粒度的隔离
* 为`docker daemon`命令添加flag参数`--authorization-plugin`，用以支持自定义的访问控制列表（ACL）
* 当用户使用BTRFS时，允许用户在容器中运行SELinux

##Distribution
* 为AuthConfig数据结构添加一个名为`registryToken`的属性。该Token属性允许API客户端从一个镜像仓库中获取用户的认证token，然后将token直接发给远程API
* 对镜像和镜像层实现内容寻址的存储方式。内容寻址指的是：通过镜像层的内容来唯一的决定一个镜像的ID，从而保证镜像的全局唯一性。需要注意的是：当Docker第一次运行的时候，会有一个对原有镜像进行格式转换的过程。此过程很可能会占用很长时间，主要取决于当前Docker Daemon所管理镜像以及容器的数目大小。从此，镜像中不再有`parent image`的概念，而是指向一系列的镜像层引用。同时`docker load/docker save`的压缩包现在会包含内容寻址镜像的配置文件。
* 支持新的menifest格式("schema2")
* 对镜像下载和镜像上传做了众多改善，比如：性能的提高，下载失败时的重试机制，客户端失联的取消操作等
* 对镜像仓库v1协议的回调限制
* 新版的Notary支持，其中包含了客户端的pkcs11。ps：notary是Docker一款官方推出的在客户端和服务端通过受信集合来交互的工具
* 修复Bug，此Bug在先前的版本中出现时会导致Docker Daemon因为等待一个并不存在的进程去下拉镜像而僵死

##3.网络
* 对于容器而言，使用基于DNS的发现机制，取代原先的/etc/hosts方式
* 对`docker network connect`命令中添加网络方面的别名，如`--net-alias`,以及`docker run`命令时的`--alias`
* 为`docker run`以及`docker network connect`命令添加参数`--ip`和`--ip6`,用以在一个网络中为容器自定义IP
* 为`docker network create`命令添加参数`--ipam-opt`，用于传入自动的IP地址管理（IPAM）的选项
* 在`--cluster-store-opt`参数中添加`kv.path`
* 在`--cluster-store-opt`参数中添加`discovery.heartbeat`和`discovery.ttl`参数，用于配置服务发现的TTL以及心跳周期
* 为`docker network inspect`命令添加`--format`参数
* 为`docker network connect`命令添加`--link`参数，用以提供容器级别的别名
* 通过远程IP地址管理器（IPAM）插件的形式，支持？？？？？？？
* 为`docker network disconnect`命令添加`--force`参数，用以强制容器从某个network中断开连接
* 在Linux内核版本3.10+上，使用内嵌的overlay驱动来完成跨主机的网络互联
* 在用户定义的network中，支持在`docker run`命令中使用`--link`参数
* 扩展`docker network rm`命令，使其有能力一次删除多个network
* 在`docker inspect`命令的输出内容中添加所有容器的名称
* 在`docker network inspect`命令中，加入为用户自定义网络自动生成的子网地址
* 为`docker network ls`命令添加flag参数`--filter`，用以隐藏预先定义的networks
* 支持已停止容器的 network 连接/断开功能
* 在`docker inspect`命令的返回结果中添加network id
* 修复MTU的bug，之前版本Docker在有两条或者两条以上路由记录时启动就会失效
* 为容器修复重复IP地址的bug
* 修复了有时Docker Daemon在创建默认网络时存在的bug
* 在使用`--net=host`参数时，不为容器替换127.0.0.1的域名服务器地址

##4.日志
* 新增日志驱动Splunk
* 在TCP＋TLS的基础上，完善对Syslog的支持
* 扩展`docker logs --since`和`--until`命令，用以支持纳秒
* 扩展日志驱动`awslogs`，使其当Docker Daemon在AWS EC2上运行时，可以自动监测正确的Amazon CloudWatch的日志时区

##5.存储卷Volumes
* 为Docker的存储卷添加设置mount propagation mode的支持
* 为存储卷插件的API添加`ls`和`inspect`接口
* 修复了不能从命名存储卷中拷贝数据的bug
* 允许外部存储卷驱动管理异步存储卷

#6.镜像构建器（builder）
* 在`.dockerignore`文件中添加对`**`的支持，实现通配多级目录
* 修复Dockerfile中对UTF-8编码字符的处理
* 修复从标准输入读取内容时的权限问题

#7.客户端
* 通过使用`DOCKER_API_VERSION`环境变量来支持API版本覆盖的支持

#8.MISC
* systemd：在systemd服务的配置文件中，为`LimitNPROC`添加`TaskMax`参数。

#9.遗弃
* 从Docker Daemon移除对LXC的支持，在docker 1.8版本时，Docker用来管理容器的驱动之一——LXC驱动就已经被标记为deprecated，现在此驱动已经被移除
* 在`docker daemon`命令中移除flag参数`--exec-driver`，因为此驱动已经不再会被使用
* 移除部分Docker CLI方面的参数，比如（-rm，选择用--rm来替代）
* 在容器启动的API中遗弃数据结构HostConfig
* 由于某些特定Linux版本发行版项目的终止，从而遗弃这些项目下docker软件包的支持；比如 Fedora 21和Ubuntu 15.04（vivid）
* 遗弃`docker tag`命令的`-f`参数
