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

*
*
*
*
*
*
*
*
*
*

## Docker引擎运行时

## Swarm Mode 编排

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
