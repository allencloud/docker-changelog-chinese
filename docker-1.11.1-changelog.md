# Docker 1.11.1 变更日志

英文链接地址：[docker-1.11.1 CHANGELOG.md](https://github.com/moby/moby/blob/v1.11.1/CHANGELOG.md)。

Docker 1.11.0 发布至今不到两周的时间里，正如大家谈及最多的那样，Docker在架构上产生了巨大的变化，由原来单一的二进制文件docker，拆分为4个不同的二进制文件构成：docker、containerd、docker-containerd-shim和docker-runc。

一般而言，大版本的发布都会伴随着软件架构的变动、功能特性的增强。然而从软件发展的角度而言，这样的变动似乎总是意味着不稳定性，或者是Bug的存在。因此，对于0版本号的docker，用户往往都表现得非常谨慎，追踪功能特性、预先体验稳定性、排查基本功能是否受影响，会是比较切实际的做法。而对于在开发、测试、生产等多种环境中的版本更新，用户往往会采取0之后的版本，比如1.11.1。反其道思考，docker 1.11.1的更新日志，也会最先来披露1.11.0版本中存在的Bug。

就在过去的4月26日，Docker官方发布了1.11.1版本。也许不会如0版本那么具有轰动性，然后对于真正想使用1.11.x版本的用户，其变更日志绝对是评估是否采纳的最好依据。

以下是docker 1.11.1的变更日志：

## docker 1.11.1 (2016-04-26)

### Distribution

- 修复了schema2 manifest文件的媒体类型（media type），使其成为`application/vnd.docker.container.image.v1+json`([#21949](https://github.com/docker/docker/pull/21949))

### Docker文档

- 为Docker 1.11.0版本中发生变的API变化，添加了相应了API说明文档([#22048](https://github.com/docker/docker/pull/22048))

### 镜像构建器（Builder）

* 实现了在`docker build`中添加的labels参数不会覆盖`Dockerfile`中原有的Label属性([#22184](https://github.com/docker/docker/pull/22184))

### 网络

- 修复了在转发DNS查询请求时可能发生的程序恐慌([#22261](https://github.com/docker/docker/pull/22261))
- 修复了当使用用户定义的网络时，操作系统线程有可能在一个不正确的网络命名空间中自动终止的Bug ([#22261](https://github.com/docker/docker/pull/22261))

### Docker Daemon运行时

- 修复了，通过配置文件（如/etc/docker/daemon.json）动态加载Docker Daemon的标签（labels）时失效的Bug([#22299](https://github.com/docker/docker/pull/22299))
- 修复了容器挂载`/var/run`时会组织其他容器被删除的问题，该问题在1.11.0版本前已经被修复，而1.11.0版本又引入了改Bug([#22256](https://github.com/docker/docker/pull/22256))
- 修复了不能同时更新容器`memory-swap`和`memory`配置的问题([#22255](https://github.com/docker/docker/pull/22255))
- 修复了1.11.0版本中当不提供`serveraddress`属性时，`/auth`接口不初始化该属性的Bug ([#22254](https://github.com/docker/docker/pull/22254))
- 在取消一个容器重启时，添加为容器添加有可能遗失的临时文件 ([#22237](https://github.com/docker/docker/pull/22237))
- 删除当没有指定重启策略时，会出现的离奇错误消息([#21993](https://github.com/docker/docker/pull/21993))
- 修复了当插件通过json spec形式被激活时可能出现的程序恐慌([#22191](https://github.com/docker/docker/pull/22191))
- 如果一个容器的重启超过了10秒钟，重新设置Docker重启管理器的超时属性，该功能回退到docker 1.10的状态 ([#22125](https://github.com/docker/docker/pull/22125))
- 删除了容器重启操作被取消之后会显示的错误信息([#22123](https://github.com/docker/docker/pull/22123))
- 修复了在`docker exec`之后不能通过`docker rm`正常删除容器的问题([#22121](https://github.com/docker/docker/pull/22121))
- 修复了当服务于多个并发的`docker stats`命令时，Docker Daemon可能存在的程序恐慌 ([#22120](https://github.com/docker/docker/pull/22120))`
- 重新废弃一个功能，该功能在宿主机目录不存在时会自动为容器创建([#22065](https://github.com/docker/docker/pull/22065))
- 在Docker Daemon关闭时，隐藏有可能出现的rpc通信错误([#22058](https://github.com/docker/docker/pull/22058))