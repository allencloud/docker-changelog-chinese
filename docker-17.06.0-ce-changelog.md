# Docker 17.06.0-ce 变更日志(2017-06-19)

英文链接地址：[docker-17.06.0-ce CHANGELOG.md](https://github.com/docker/docker-ce/blob/v17.06.0-ce/CHANGELOG.md)。

注意：默认情况下，Docker 17.06 禁用了与 Docker Registry v1 的通信。如果你需要和还没有迁移到v2的镜像仓库通信时，请设置Docker Daemon的参数`--disable-legacy-registry=false`。与 v1 版本的镜像仓库的通信功能，会在 Docker 17.12 中被移除。

## 镜像构建器

* 为 `docker build` 命令添加 `--iidfile` 选项。这将允许用户指定一个地址来存储最终的镜像ID
* 允许用户在`git checkout URLs`指定任意远程的链接 

## Docker 客户端

* 为 `docker stack ls` 命令添加一个 `--format` 参数
* 在 compose 的初始化构建中，允许为镜像添加标签
* 为 `docker history` 命令添加 `--format`参数的支持
* 为 `docker system df` 命令添加 `--format` 参数的支持
* 允许用户在stack文件中指定域名服务器以及域名的搜索服务器
* 在 `docker stack deploy` 命令中，对服务添加`read-only`的支持
* 显示Swarm集群以及节点TLS的信息
* 为 `docker stack deploy`命令添加节点亲和的支持
* 为 `docker swarm` 命令添加一个新的子命令`ca`，来允许管理Swarm集群的CA
* 为 compose 添加 crendential 的配置信息
* 为 `--network`和`--network-add`选项添加csv的支持
* 在Windows平台上修复stack compose bind－mount 存储卷的问题
* 使得 Docker 客户端有能力应对与一个没有registry信息的Docker Daemon通信
* 当使用 `--rollback` 参数的时候，允许使用 `--detach`和`--quiet`标志符
* 从`docker login`命令中删除遗弃的参数`--email`
* 调整了`docker stats`命令中的内存输出显示

## Distribution

* 当用户通过docker daemon启动一个镜像下载时，倘若镜像的Digest和Tag都提供，选择Digest而不是Tag

## 日志

* 为 GCP 日志驱动添加资源监控元数据；
* 为 AWS CloudWatch 日志驱动添加多线处理的功能。

## 网络

* 为Docker Swarm模式下的服务，添加本地网络的支持，比如 macvlan，ipvlan，bridge，以及 host
* 在服务创建过程中，允许用户传入网络驱动相关的驱动选项；
* 允许用户使用参数`--data-path-addr`来隔离 Swarm 控制层的数据流程与应用传输层的数据流量
* 对Docker Daemon自带的服务发现功能，进行了一系列的提高。

## Docker 运行时

* 在 prometheus 的监控数据中，添加镜像构建数据以及Docker引擎的信息；
* 为devicemapper添加选项，使得可以自动配置 blkdev；
* 为 `docker info` 添加日志驱动列表；
* 添加 API 接口允许用户获取一个镜像的manifest；
* 在 `forceremove` 下，Docker Daemon 出错时不要从内存中删除容器；
* 为 metric 插件添加支持
* 当无效的过滤参数被传入`prune`参数，docker daemon抛出一个错误；
* 添加Docker Daemon的参数允许推送外部镜像层；
* 当containerd退出之后，修复了一个bug，防止containerd被重启；
* 为Docker Daemon 添加集群方面的事件支持，比如service的事件，secret的事件，node的事件；
* 在Windows平台上添加 DNS搜索域的支持；
* 为Docker的编译语言Golang进行升级，目前升级后的Golang版本为1.8.3；
* 当journald重启之后，修复了containerd崩溃的bug；
* 修复了由于无效的环境变量问题而导致的健康检查出错问题；
* 当Docker Daemon关闭时，不去创建相应的源目录；
* 如果一个容器的停止信号是 `SIGKILL` 的话，当容器停止时阻止它重启；
* 确保日志驱动在`StartLogging`和`StopLogging`接口中被传入了相同的文件名参数；
* 删除了在信号 `SIGUSR1` Docker Daemon的数据结果导出功能，这会避免程序的恐慌退出。

## 安全

* 在 默认的 seccomp 文件中允许使用 UNAME26 bit set 的personality.

## Docker Swarm 集群功能

* 添加一个参数，从而允许用户为数据层网络指定一个不同的网络接口，这样数据层数据与控制层数据的流量能隔离；
* 在容器内允许指定secret的路径；
* 在Windows平台上支持secret功能；
* 在Swarm Info和 Node Info API接口中添加TLS的信息；
* 为Swarm下的服务添加各种形式的配置config；
* 添加API接口，允许用户动态更新Swarm的CA认证信息；
* 服务认证信息的pin功能目前已经从daemon端移到了客户端；
* 服务的节点放置，目前已经将平台因素考虑在内；
* 当加入Swarm集群失败（join失败）时，修复了有可能出现的夯死；
* 修复了防止外部CA被接受的问题；
* 在混淆的不同版本docker daemon的集群中，修复了可能出现的编排程序恐慌；
* 在Swarm Mode初始化的过程中，避免分配重复的IP地址。

## 废弃

* 默认情况下，Docker 17.06 禁用了与 Docker Registry v1 的通信。如果你需要和还没有迁移到v2的镜像仓库通信时，请设置Docker Daemon的参数`--disable-legacy-registry=false`。与 v1 版本的镜像仓库的通信功能，会在 Docker 17.12 中被移除。