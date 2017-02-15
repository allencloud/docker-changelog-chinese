# Docker 1.13.0 发布，编排领域迈向成熟期

## 镜像构建器

* 在镜像构建时，允许用户指定镜像，将该镜像作为构建的缓存源。这些镜像不需要拥有本地的父子链关系，同事可以通过从其他惊喜那个仓库中下啦
* 在镜像构建成功之后，可以添加一个参数来压缩镜像层中From的基础镜像（试验版功能）
* 修复了Dockerfile解析器的Bug，该bug在程序退出时存在空行的时候出现
* 在`docker build`命令中添加了构建步数
* 镜像构建时，添加压缩构建上下文的支持
* 在`docker build`命令中添加`--network`参数，使镜像构建时允许指定具体网络
* 修复构建镜像时的`--label`和 `docker run`时`--label`参数的不一致性
* 修复了使用overlay存储驱动时，镜像层可能存在的不一致性
* 之前没有使用的`build-args`目前已经支持，届时会显示一条警告日志，而不是使整个构建流程报错退出
* 修复了Windows系统上构建缓存的问题
* 允许在Windows系统构建时支持User参数
* 在Windows系统上修复了环境变量大小写的问题

## 发行版

* 为 PPC64LE 上的 Ubuntu 16.04 Xenial 支持构建docker包
* 为 s390x 的 Ubuntu 16.04 Xenial 支持构建docker包
* 为 PPC64LE 的 Ubuntu 16.10 Yakkety Yak 支持构建docker包
* 为 VMWare Photon OS 支持docker的RPM构建
* 为tgz添加shell补全功能
* 为Docker的安装脚本添加中国的镜像源
* 为 Ubuntu 16.10 Yakkety Yak 系统添加DEB包的构建
* 为 Fedora 25 添加RPM包de构建
* 为 aarch64 添加 `make deb` 的支持

## 镜像仓库 Distribution

* 更新 notary 依赖到0.4.2版本
  - 支持Windows系统上的编译
  - 优化客户验证错误
  - 支持寻找在 `~/.docker/trust/private` 下任意位置的 key, 不仅仅是在目录 `~/.docker/trust/private/root_keys` 或者 `~/.docker/trust/private/tuf_keys`
  - 之前notary的实现，对于任何的更新错误，客户端都会在缓存上失效。现在，我们只有在网络错误，或者服务器不响应服务，或者修饰TUF数据时返回错误。无效的TUF会导致更新失败，例如用户发起了一个无效的root滚动更新。
  - 改进root验证以及yubikey debug 日志
  - 当root验证或者快过期时发出警告
  - 当角色元数据临近过期时发出警告
  - 修复修复密码检索尝试计数和终端检测存在的问题
* 当不同的用户上传同样的镜像层到受验证的镜像仓库时, 避免重复上传不必要的块
* 允许镜像仓库认证信息的外部存储

## 日志

* 在所有的日志驱动中标准化默认的日志标签值；
* 当使用长日志行数时，提高性能与内存利用率
* 支持Windows系统上的syslog驱动
* 添加Logentries驱动
* 更新AWS日志驱动以支持日志标签
* 为fluentd日志驱动添加unix socket的支持
* 在Windows系统上支持fluentd
* 当docker的标签被用作journald的名称时，审查这些labels标签
* 修复了 `docker logs --tail` 显示的日志行数比实际少的错误
* 为Splunk的日志驱动提高性能和可靠性
* 为Splunk的日志驱动格式化配置项，同时跳过验证连接

## 网络

* 为每个网络添加`--attachable`参数，使得支持容器在运行时加入swarm mode的overlay网络中
* 在`docker service create`中使用`--publish`选项时，添加了任务单主机端口暴露的功能
* 为Windows 10添加overlay网络驱动的支持
* 在IPTables中修改默认的 `FORWARD` 链为`DROP`
* 在windows机器上，添加新的功能，使得允许用户指定特定的IP地址到预留的网络上
* 修复inspect 网络时，网关地址包含掩码的错误
* 修复了一个错误，出现该错误时，在一个网桥中多个网络地址会导致 `--fixed-cidr` 丢失准确的地址
* 在 `docker network inspect`的结果中显示网络创建的时间戳
* 在 `docker network inspect`的结果中，为Swarm的overlay网络显示peer节点
* 允许服务对虚拟IP（VIP）的ping请求

## 插件

* 将插件功能移出实验版功能
* 为`docker plugin remove`添加`--force`参数
* 支持动态重新加载认证插件
* 为`docker plugin ls` 命令添加描述
* 为`docker plugin inspect`命令添加`-f/--format`的选项
* 添加`docker plugin create`命令
* 发送请求TLS的peer验证到验证插件
* 在Swarm Mode中支持全局时间的网络以及IPAM插件
* 将`docker plugin install`命令分解为两个API调用`/privileges`和`/pull`

## API (v1.25)和客户端变化

* 支持从一个compose文件来执行`docker stack deploy`命令
* 实现运行中的容器checkpoint和restore的功能，不过为试验版功能
* 为`docker info`添加`--format`的命令行参数
* 从`docker volume create`命令中删除`--name`参数
* 添加`docker stack ls`
* 为`docker ps`命令添加一个`is-task`的过滤参数
* 为`docker service create`添加一个`--env-file`的命令行参数
* 为`docker stats`命令添加一个`--format`命令行参数
* 在Swarm Mode中，将`docker node ps`默认指向自身这个节点 （由本人Allen贡献）
* 在`docker service create`中添加命令行参数`--group`
* 为 service/node/stack ps的输出添加`--no-trunc`命令行参数
* 添加日志到`ContainerAttachOptions`, 如此一来Go语言的客户端有能力发起获取容器日志的请求，作为attach进程的部分输出
* 允许客户端与低版本的docker引擎通信
* 当容器的状态是Removal In Progress时，docker引擎将此状态通知客户端
* 为`/info`API节点添加`isolation`的字段
* 为`/info`API节点添加`userns`的字段
* 不允许在service API的访问请求中指定同时指定两个Service Mode（由本人Allen贡献）
* 在API访问节点`containers/create`中添加使用更常规以及安全的方式指定挂载的功能
* 允许顶级的命令`docker inspect`来获取所有类型的docker资源，如容器，网络，存储卷，镜像等
* 为`docker run`和`docker create`命令添加`--cpus`参数以控制CPU计算资源，同时在`HostConfig`中添加`NanoCPUs`
* 允许用户在`docker run`和`docker create`的时候，取消`--entrypoint`的原有配置
* 为了满足更好的一致性，重构了CLI命令行，添加了命令`docker container`和`docker image`
* 从`docker service ls`的显示结果中删除了`COMMAND`列
* 为`docker events`命令添加了`--format`参数
* 允许`docker node ps`命令指定多个不同的节点
* 限制`docker images`命令输出结果精确到小数点后两位
* 为`docker run`命令添加`--dns-option`命令行参数
* 在镜像commit事件中添加镜像ID
* 在`docker info`中添加外部二进制文件的信息
* 在`docker info`中添加`Manager Addresses`的信息
* 为`docker images`命令添加一个信息的索引过滤

## Docker引擎运行时

* 添加一个`--experimental`的daemon命令行参数来支持试验版本的功能，而是将这些功能通过一个独立的构建版本来提供
* 添加一个`--shutdown-timeout`的daemon命令行参数来指定默认的超时时间（秒），从在dameon退出前可以优雅的停止容器
* 添加一个`--shutdown-timeout`的参数来为单个容器指定停止命令的超时时间（秒）
* 为Docker 引擎添加一个新的命令行参数`--userland-proxy-path`,允许配置userland proxy,而不是使用硬编码的docker-proxy
* 为dockerd添加一个daemon端的命令行参数`--init`，另外在`docker run`的时候，使用tini这一可以清理僵尸进程的init进程作为pid为1的进程
* 为docker引擎添加一个命令行参数`--init-path`来允许配置`docker-init`二进制文件的地址
* 支持允许热加载`insecure-registry`（由本人Allen贡献）
* 为Windows上的docker引擎支持选项`storage-size`
* 提高`docker run --rm`命令的可靠性，实现方式为将`--rm`的实现从客户端移至daemon端
* 支持参数 `--cpu-rt-period` 和 `--cpu-rt-runtime`, 允许当内核中`CONFIG_RT_GROUP_SCHED`被启动时，容器有能力运行实时线程
* 允许并行的容器停止，暂停，解除暂停
* 为overlay2存储驱动实现XFS限额
* 为service、task的过滤，修复部分/完整的问题
* 允许docker引擎运行在一个特定的用户名空间内 
* 修复了一个资源竞争问题，当使用device mapper存储驱动时，设备延迟删除与恢复设备
* 在Windows平台上支持命令`docker stats`
* 当`--userns=host`时，允许使用`--pid=host`和`--net=host`
* （试验版本）添加 Prometheus metrics 输出，输出的维度有容器，镜像和daemon操作
* 修复了`NetworkDisabled=true`时, `docker stats`会存在的错误
* 在Windows平台上支持`docker top`命令
* 记录exec进程的PID
* 添加通过`getent`寻找user/groups的支持
* 添加新的`docker system`命令，包含 `df`和`prune`子命令，可以实现系统的资源管理，同时也包括`docker {container, image, volume, network} prune`子命令 
* 修复了一个bug，容器不能被停止或者杀死，当使用devicemapper时，设置了xfs的max_retries 从 0 到 ENOSPC
* 修复了在CentOS上使用devicemapper时`docker cp`命令拷贝到容器存储卷时会出现的错误
* 提高overlay(2)存储驱动的优先级
* 添加`--seccomp-profile`的docker daemon命令行参数，来指定一个seccomp的路径来覆盖原先的默认路径
* 修复了当 `--default-ulimit`被设置到daemon时，docker inspect不会先ulimit的错误
* 在`docker exec -t`命令中添加对环境变量 `TERM`的支持
* 在`docker kill`命令上记录容器`--stop-signal`的设置

## Swarm Mode 编排

* 添加secret管理
* 添加对于服务选项（hostname, mounts和环境变量）的支持
* 在`docker service inspect --pretty`中显示服务模式（由本人Allen贡献）
* 通过显示较短的task ID来使得`docker service ps`的输出更能令人接受
* 将 `docker node ps`默认指向当前节点（由本人Allen贡献）
* 为服务创建`docker create`添加`--dns`,`--dns-opt`和`--dns-search`
* 为`docker service update`添加 `--force`参数
* 在命令`docker service create`和`docker service update`中添加`--health-*`和`--no-healthcheck`命令行参数
* 为`docker service ps`命令添加`-q`参数
* 在`docker service ls`的显示结果中显示全局服务的数量
* 从`docker service update`命令中删除命令行参数`--name`。这个命令行参数只适用于命令`docker service create`
* 修复了因为短暂的网络问题，工作节点有可能存在的失效bug
* 添加支持健康感知的负载均衡和DNS记录
* 为`docker service create`命令添加`--hostname`参数
* 为`docker service create`命令添加`--host`命令，为`docker service update`命令添加`--host-add`和`--host-rm`参数
* 为`docker service create/update`命令添加`--tty`命令行参数
* 自动监测、存储、暴露节点的IP地址，一旦被管理者节点发现
* 加密余下的管理者节点的密钥和raft数据
* 为`docker service update`命令添加参数`--update-max-failure-ratio`,`--update-monitor`和`--rollback`
* 修复了在容器内部运行`docker swarm init`命令时的网络地址自动发现
* （试验版功能）添加命令`docker service logs`来支持查看一个服务的日志
* 通过digest来ping镜像，当`docker service create`和`docker service update`
* 为raft的快照功能，添加了自定义配置的选项`--max-snapshots`和`--snapshot-interval`
* 不要尝试重新下载镜像，当镜像已经通过digest被pin之后
* 在Windows平台上支持Swarm Mode
* 允许hostname在`docker service update`时被更新
* 支持v2版本的插件机制
* 为服务添加内容验证安全机制

## 存储卷

* 为存储卷volumes添加标签支持
* 支持通过标签label来过滤存储卷
* 在`docker volume rm`命令中添加一个`--force`参数来强制删除已经被删除的存储卷中的数据
* 创建数据卷时，扩展`docker volume inspect`命令，使其显示所有的选项
* 为本地的NFS存储卷，添加解析主机名的功能

## 安全

* 修复容器内共享存储卷的SELinux标记
* 禁止 `/sys/firmware/**` 被apparmor访问

## 功能废弃
　
* 将`docker daemon`标记为废弃状态（deprecated）, daemon 命令已经被单独移至一个全新的二进制文件dockerd
* 废弃了没有使用版本号的API
* 将 Ubuntu 15.10 (Wily Werewolf) 移出了支持平台的行列。Ubuntu 15.10 已经停止更新，因此Docker对其的支持也不再接受任何更新
* 将 Fedora 22 移出了支持平台的行列，Fedora 22 已经停止更新，因此Docker对其的支持也不再接受任何更新
* 将 Fedora 23 移出了支持平台的行列，Fedora 23 已经停止更新，因此Docker对其的支持也不再接受任何更新
* 在命令`docker pull`中废弃镜像语法`repo:shortid`
* 为overlay和overlay2存储驱动，废弃缺少`d_type`的文件系统支持
* 在Dockerfile中废弃`MAINTAINER`命令
* 为API访问点`/images/json`废弃`filter`参数
* 废弃为Docker引擎设置重复的标签
* 在`NetworkSettings`中，废弃`top-level`的网络信息
