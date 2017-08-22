# Docker 17.07.0-ce 变更日志(2017-08-XX)

英文链接地址：[docker-17.07.0-ce CHANGELOG.md](https://github.com/docker/docker-ce/blob/v17.07.0-ce/CHANGELOG.md)。

以`DEPRECATE`开头的内容是非常重要的废弃通知。关于管理废弃命令行标签以及API，更多的信息可以查询 https://docs.docker.com/engine/deprecated/ 。此链接中会记录相应废弃项的具体日期。

## API 和 客户端

*  在config.json文件中支持代理配置[docker/cli#93](https://github.com/docker/cli/pull/93)
*  默认开启 Docker Engine的pprof/debug [moby/moby#32453](https://github.com/moby/moby/pull/32453)
*  目前在`docker login`命令中，可以使用`--password-stdin`作为命令行的标准输入，来传入用户所需的密码 [docker/cli#271](https://github.com/docker/cli/pull/271)
*  为`docker scale`命令添加`--detach`命令行参数 [docker/cli#243](https://github.com/docker/cli/pull/243)
*  防止容器不存在而导致的`docker logs --no-stream`命令的夯死 [moby/moby#34004](https://github.com/moby/moby/pull/34004)
*  修复了`docker stack ps`命令输入错误时，往标准输出中打，而不往标准错误中打 [docker/cli#298](https://github.com/docker/cli/pull/298)
*  修复了当部署时出错情况下，`docker service create`命令的进度条会被卡住的问题 [docker/cli#259](https://github.com/docker/cli/pull/259)
*  完善了在用户与容器的交互模式下，进度条的展示效果 [docker/cli#260](https://github.com/docker/cli/pull/260) [docker/cli#237](https://github.com/docker/cli/pull/237)
*  在使用 `docker login --password` 命令时打印一条警告日志，同时建议用户使用 `--password-stdin`命令行参数 [docker/cli#270](https://github.com/docker/cli/pull/270)
*  使 API 版本号协商的鲁棒性更高 [moby/moby#33827](https://github.com/moby/moby/pull/33827)
*  当连接到老于 Docker 17.05 版本的 Docker 引擎时，隐藏 `--detach` 参数 [docker/cli#219](https://github.com/docker/cli/pull/219)
*  为API `GET /networks/(id or name)` 添加过滤参数 `scope` [moby/moby#33630](https://github.com/moby/moby/pull/33630)

## 镜像构建器

* 实现长任务的交付session，同时允许用户增量的发送构建上下文信息 [moby/moby#32677](https://github.com/moby/moby/pull/32677) [docker/cli#231](https://github.com/docker/cli/pull/231) [moby/moby#33859](https://github.com/moby/moby/pull/33859)
* 解析连续的空行时，抛出警告 [moby/moby#33719](https://github.com/moby/moby/pull/33719)
* 修复了 `.dockerignore` 中以 `/` 起头但是不匹配任何内容时发生的错误 [moby/moby#32088](https://github.com/moby/moby/pull/32088)

## 日志

* 修复了rotate日志文件时错误的文件mode [moby/moby#33926](https://github.com/moby/moby/pull/33926)
* 修复了journald 和 syslog 的标准错误日志 [moby/moby#33832](https://github.com/moby/moby/pull/33832)

## Docker 运行时

* 允许停止被暂停的容器 [moby/moby#34027](https://github.com/moby/moby/pull/34027)
* 为overlay2的存储驱动支持 限额 支持 [moby/moby#32977](https://github.com/moby/moby/pull/32977)
* 删除了`docker ps`存在的容器锁，此修复修复了大部分情况下`docker ps`命令夯死的情况 [moby/moby#31273](https://github.com/moby/moby/pull/31273)
* 在memdb中存储容器的名字 [moby/moby#33886](https://github.com/moby/moby/pull/33886)
* 修复了 `docker exec` 和 `docker pause`命令中可能存在的资源竞争 [moby/moby#32881](https://github.com/moby/moby/pull/32881)
* DeviceMapper: 重做了日志以及添加了日志参数 `--storage-opt dm.libdm_log_level` [moby/moby#33845](https://github.com/moby/moby/pull/33845)
* DeviceMapper: 防止`device in use`错误的产生，一旦 deferred 的删除启动了，而不是 deferred 的删除 ???? [moby/moby#33877](https://github.com/moby/moby/pull/33877) 
* DeviceMapper: 使用 KeepAlive 来防止正在使用的任务被 GC 掉 [moby/moby#33376](https://github.com/moby/moby/pull/33376)
* 当prune操作被中止时，立即返回当前的prune数据 [moby/moby#33979](https://github.com/moby/moby/pull/33979)
* 修复了并发运行 `docker rename <container-id> new_name`时，可能存在的容器多名字的问题 [moby/moby#33940](https://github.com/moby/moby/pull/33940)
* 修复文件描述符的泄漏以及错误的处理 [moby/moby#33713](https://github.com/moby/moby/pull/33713)
* 修复了当运行容器时的SIGSEGV信号的处理问题 [docker/cli#303](https://github.com/docker/cli/pull/303)
* 当健康检查停止之后，防止协程goroutine的泄漏 [moby/moby#33781](https://github.com/moby/moby/pull/33781)
* 镜像: 改善镜像存储的锁 [moby/moby#33755](https://github.com/moby/moby/pull/33755)
* 修复了当容器被销毁时，Btrfs 的限额组没有被删除的错误 [moby/moby#29427](https://github.com/moby/moby/pull/29427)
* libcontainerd：修复了已故contained进程不被正确清理的问题 [moby/moby#33419](https://github.com/moby/moby/pull/33419)
* 为Windows平台上的Linux容器做好准备工作：
	* LCOW: 为服务VM的工具箱添加一个专属的空间来使用scratch [moby/moby#33809](https://github.com/moby/moby/pull/33809)
	* LCOW: 支持大部分操作，不包括远程文件系统 [moby/moby#33826](https://github.com/moby/moby/pull/33826) [moby/moby#33241](https://github.com/moby/moby/pull/33241)
	* LCOW: 修改文件目录 lcow 到 "Linux Containers" [moby/moby#33835](https://github.com/moby/moby/pull/33835) 
	* LCOW: 传入命令行参数的时候，无需添加额外的引号 [moby/moby#33815](https://github.com/moby/moby/pull/33815)
	* LCOW: 当系统平台schema变更时，更新必要的内容 [moby/moby#33785](https://github.com/moby/moby/pull/33785)


## Swarm 模式

* 为插件secret后端添加初始的支持 [moby/moby#34157](https://github.com/moby/moby/pull/34157) [moby/moby#34123](https://github.com/moby/moby/pull/34123)
* 使用自然排序排序Swarm的stacks和节点 [docker/cli#315](https://github.com/docker/cli/pull/315)
* 使得docker engine支持 配置 的事件 [moby/moby#34032](https://github.com/moby/moby/pull/34032)
* 当一个节点处于加入一个集群的过程中，只允许传入一个加入地址 [moby/moby#33361](https://github.com/moby/moby/pull/33361)
* 当一个网络的名字既有local和swarm类型的网络时，修复了服务创建的错误 [moby/moby#33575](https://github.com/docker/cli/pull/184)
* (实验版本) 支持在swarm集群上添加插件的支持 [moby/moby#33575](https://github.com/moby/moby/pull/33575)