# Docker 1.11.0 变更日志

春节过后，时隔两个月，Docker的又一个大版本1.11.0发布，除了功能的不断完善之外，该版本最大的看点无疑在于Docker Daemon的架构由原来一个模块，现在拆分为4个独立的模块组成，

* docker
* containerd
* docker-containerd-shim
* docker-runc

关于这部分的实现与价值，DaoCloud的Docker工程师将会在后续深层次披露。

作为在生产环境中深度使用Docker的科技型公司，DaoCloud同时也踊跃参与Docker软件的开源社区，为Docker贡献代码，其中本次docker 1.11.0版本的变更日志中，有关info信息的现实，配置文件加载的验证，请求返回码的更正等多个pr，均由DaoCloud的工程师孙宏亮完成实现并合并。

以下是 Docker 1.11.0版本的完整变更日志。

## 重要提示
使用Docker 1.11.0时，和往常有很大不同的是，现在Linux平台上Docker的安装，包括4个不同的二进制文件，它们分别是：docker, docker-containerd, docker-containerd-shim 以及 docker-runc。如果在你的环境中，有一些脚本是强依赖于单独一个的docker二进制文件，使用前需要确认更新这些脚本。和Docker Daemon的交互依然和以前保持不变，而其他部分的二进制文件的使用则可以认为对用户透明。另外，在Windows平台上，二进制文件依然以单独的docker.exe形式存在。


## 镜像构建模块 （Builder）

* 修复了当处理WORKDIR命令时，Docker不能使用uid/gid的Bug
* 修复了通过用户命名空间（userns）不能使用正确的uid/gid的Bug

## Docker客户端（Client）

* 安全参数的划分符，由之前的`:`改为`=`
* 在`pull`,`push`,`build`,`login`和`search`操作中，可以加入用户代理，以传输至Docker Registry
* 允许通过API分别设定容器的域名名称（DOMAIN_NAME）和主机名
* `docker info`命令的执行过程中，如果内核与操作系统信息没有找到，提示警告信息
* 修复了`docker stats --no-stream`命令输出有可能都是0的Bug
* 修复了有些新启动的容器不会出现在`docker stats`命令中的Bug
* 在Linux-cgo终端中不再支持后处理（Post Processing）
* 如果--hostname参数不符合RFC1123的话，该参数会被拒绝传入
* Docker开始支持通过SOCKS代理来转发客户端请求
* Docker目前支持外部的认证存储
* `docker ps`命令现在支持显示被挂载到容器内部的存储卷列表
* `docker info`现在支持显示Docker Daemon的根目录地址
* Docker现在不允许用户登录时输入一个空的用户名，因为空格会被移除
* Docker的运行事件属性会通过key来排序
* `docker ps`命令不再显示已经停止容器的暴露端口
* Docker现在会自动还原现场，如果一个`docker save`和`docker export`操作失败的话
* `docker load`命令现在支持显示一个进度条

## Distribution

* 修复了当下载没有layer的镜像时会发现的恐慌错误
* 修复了使用一个错误配置的token服务来向Docker Registry推送镜像时的恐慌
* 当进行一个受信的镜像推送时，目前需要完成一次全面的授权工作
* 已经支持Registry的OAuth
* 目前`docker login`使用docker/distribution中的实现来处理token信息
* `docker login`不再提示输入邮箱地址
* Docker如果没有basic auth的信息时，会自动转向docker regsitry v1
* 在网络错误或者超时的情况下，Docker会重新恢复对一个镜像layer的下载
* 修复了推送cross-repository时生成的manifest的媒介类型
* 当Docker Content Trust开启时，当下载一个镜像时，修复了Docker需要请求额外的push认证信息

## 日志模块（logging）

* 修复了jorunald日志驱动的数据临界区冲突
* 当发送日志时，Docker的syslog驱动现在使用RFC-5424的格式
* Docker的GELF日志驱动现在允许通过`gelf-compression-type`和`gelf-compression-level`参数来制定压缩算法以及压缩级别
* Docker Daemon开始使用`--law-logs`参数来保证输入没有着色的日志
* 在微软平台上的Docker，目前包括了ETW (Event Tracing in Windows)的日志驱动，命名为`etwlogs`
* Journald日志驱动开始有能力处理标签
* Fluent日志驱动开始支持以下参数，`fluentd-address`, `fluentd-buffer-limit`, `fluentd-retry-wait`, `fluentd-max-retries` 和 `fluentd-async-connect`
* Docker开始支持向Google Cloud发送日志，主要通过日志驱动`gcplogs`


## 各式更新（Misc）
* 当通过`docker save`命令保存链接关系的镜像时，最终`docker load`命令会以父子的关系，重新保存这些镜像
* 目前Docker Cli的编译工作已经支持OpenBSD
* 标签目前已经可以被添加在网络、存储卷和镜像的创建过程中
* `dockremap`目前会被作为默认用户创建
* 修复一部分的响应请求内存泄漏
* 当作为systemd的一项服务时，Docker会管理它的进程cgroup
* `docker info`会回报cgroup内核内存的大小，或者在不支持该参数时发送一个告警
* `docker info`目前可以支持cgroup驱动的显示
* Docker命令的自动补全，已经在PowerShell中得到支持
* 在Docker的世界中，不再存在dockerinit
* 支持在arm64的设备上构建Docker
* 在一个原生的Windows安装过程中，实现构建docker.exe的试验版支持

## 网络
* 修复了一个节点被从一个集群中强制删除时可能出现的恐慌错误
* 修复了在Swarm集群中启动一个容器时，会报“error creating vxlan interface”的错误
* 不管是否用拥有一个运行的容器，`docker network inspect`会显示所有的endpoint
* 对MacVlan和IPVlan的支持，已经被加入至Docker的试验版本
* `docker network ls`的输出现在会以网络名进行排序
* 修复了Docker会允许保留的default名来创建网络的Bug
* `docker network inspect`目前会显示一个网络是否是内部的
* 当创建一个网络时，通过清晰的参数来实现控制IPv6（docker network create --ipv6），这会在`docker network inspect`中以一个新的域`EnableIPv6`来实现
* 在内嵌的DNS服务中支持AAAA记录，也就是IPv6的服务发现
* 修复了Bug，从而不向外部主机转发docker域内IPv6查询请求
* 为轮询的DNS策略，从嵌入的DNS服务器中支持多种A/AAAA纪录
* 在非正常的docker重启之后，修复了endpoint的数量不一致
* 将暴露端口和端口映射关系的所属权，从Endpoint移至Sandbox
* 当主机的配置有ipv6.disable=1时，修复了不能docker reload的Bug
* 在IPAM网络地址管理驱动中添加了nil的内置实现
* 修复了iptables.Exists()逻辑中的Bug
* 修复使用overlay网络时，veth 网络接口的内存泄漏问题
* 修复了在关闭过程中一个网络删除后，阻止docker reload操作的Bug
* 确保iptables链在firewalld重新加载过程中被重新创建
* 在配置重新加载的过程中允许传入全部的数据存储仓库地址
* 对于匿名的容器，对IP与名称的映射使用别名，比如：DNS PTR record
* 修复从/etc/hosts文件删除一项时有可能产生的程序恐慌
* 从容器的网络命名空间中找到被转发的DNS查询
* 在docker daemon重新加载配置时，为网桥网络获取网络的内部网络模式配置
* 在docker daemon重新加载配置时，获取原先的IPAM网络地址管理驱动

## 插件
* 修复了每次罗列插件时都会出现的文件描述符泄漏
* 修复了面对大量数据时，authz插件有可能会使payload body崩溃的Bug

## Docker Daemon 运行时
* 修复了通过无效的参数启动一个容器之后还原现场可能存在的程序恐慌
* 修复了事件计时器提前停止可能出现临界区冲突
* 修复了layer存储中存在的数据临界区问题，该问题有可能导致map对象的失效以及docker daemon进程的崩溃
* 在mount时，对于宿主机文件的自动创建功能由重新恢复，此前在Docker 1.9版本时，该功能已经被弃用
* 当用户命名空间启动时，现在可以让容器间共享NET和IPC命名空间
* `docker inspect <image-id>`命令目前已经可以支持显示rootfs的layer
* Windows上的Docker现在已经支持了一个top命令的最简单实现
* 当一个容器不能通过Docker的命令启动时，Docker将会报告这个错误
* 如果`udev sync`不可用时，Docker将拒绝运行device mapper
* 当配置被重新加载时，修复了不验证配置有效性的Bug
* 修复了出现夯机的现象，当初始启动失败后的attach操作
* 修复了Docker配置文件中的Registry服务参数没有被正常考虑的问题
* 修复了exec和resize操作有可能造成的临界区数据访问冲突问题
* 修复了在docker事件中，没有正确考虑的纳秒计算问题
* 修复了当传入一个64位ID时，Docker命令的处理问题
* Docker会返回一个204 status code，当删除网络成功的时候
* 修复了杀死进程时该进程已经自己退出时，Docker Daemon有可能无限期等待的Bug
* devicemapper驱动开始支持参数`dm.min_free_space`,如果映射的设备剩余空间到达了设定值，那么新设备的创建也会被禁止
* Docker现在支持通过参数`--security-opt=no-new-privileges`,禁止容器内的进程获取新的privilege的权限
* 带有参数`--device`启动一个容器时，现在将会重新解析符号链接
* Docker现在开始通过containerd和runc来创建新的容器
* 修复了Docker配置重新加载只检索配置文件中选项的问题
* 当网络参数`--net=host`时，Docker现在允许为容器再设定一个hostname
* 当`--privileged`和新的`--userns=host`参数呗指定时，Docker目前允许运行privileged容器时使用参数`--userns-remap`
* 当容器崩溃后重启时，修复了该情况下Docker不会清洗现场的Bug
* 当重新加载配置文件，读到一个没有定义的配置项时，Docker将会报告这个错误，
* 修复了Docker Daemon重启时，容器加载过程中依赖与插件的错误
* `docker update`操作支持更新一个容器的重启策略
* `docker inspect`现在返回一个新的`State`状态，包含了可读的容器状态。
* Docker开始支持限制容器的运行进程个数，一旦容器的PID Cgroups被启动的话，可以通过参数`pids-limit`来实现，当然这需要Linux内核版本的支持
* `docker load`命令新添加了一个`--quiet`参数，来使得缓解运行输出
* 修复了IPv6点对点通信时的邻点发现问题
* 修复了如果容器通过无效参数启动时，清理过程中有可能出现的运行恐慌
* 修复了当运行终端呗关闭之后，容器不能被停止的Bug

## 安全

* 拥有类型`pcp_pmcd_t`的对象，目前已经授予了访问`/var/lib/docker`的管理权限
* `restart_syscall`,`copy_file_range`和`mlock2`已经作为容器允许的系统调用，被加入至默认的seccomp文档中
* `send`,`recv`和`x32`已经加入至容器允许的系统调用，从而使得容器可以安装32位的软件包
* Docker Content Trust目前可以向服务端发送请求，以实现签名快照
* 将对于YubiKeys的支持移出了试验部分（experimental）

## 存储卷

* 命令`docker volume ls`的输出目前已经按照存储卷的名称来排序
* 本地的存储卷驱动（local driver）目前可以接受与mount命令类似的参数
* 修复了一个Bug，使得只有一个字母的文件夹可以被用来作为存储卷的源地址
* 在`docker run -v`命令中，新添加一个新的flag，名为`nocopy`。这个标识符告知Docker Daemon不要拷贝容器中的内容到存储卷之中（原先这是一个默认的行为）