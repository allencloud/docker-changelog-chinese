# Docker 17.04.0-ce 变更日志

## 镜像构建器

* 在构建镜像时，取消容器产生的日志
* 修复 `.dockerignore`中的`**/`的使用

## Docker Client 客户端

* 为`docker stack ls`的输出结果排序
* 添加指定挂载点一致性的标记符
* docker CLI `--help`命令的输出内容，目前已经支持和当前终端的宽度所适配
* 在`docker ps`命令中压缩镜像digest
* 隐藏和Windows相关的命令选项
* 修复`docker plugin install`的命令行提示，使得回车键默认为No
* 为Go模版支持`truncate`函数
* 为`docker stack deploy`命令添加port参数扩展功能
* 为`docker stack deploy`命令添加mount参数的扩展功能
* 为`docker build`命令添加参数`--add-host`的支持
* 为`docker network ls --format`添加`.CreatedAt`的显示
* 更新`--secret-rm`和`--secret-add`的顺序
* 为`docker plugin ls`添加`--filter enable=true`的过滤支持
* 为`docker service ls`添加`--format`的支持
* 为`docker ps --filter`添加`publish`和`expose`的过滤支持
* 为`docker service ps`添加多个service ID的支持
* 允许swarm使用参数`--availability=drain`来完成join
* `docker inspect`现在显示"docker-default"，当AppArmor被启动，并且其他的profile没有被指定时

## 日志

* 为容器日志实现可选的ring buffer
* 为AWS（CloudWatch）添加日志组的支持，通过`--log-opt awslogs-create-group=<true|false>`来实现
* 修复了使用静态二进制文件时，使用gcplogs驱动时可能存在的segfault

## 网络

* 在`docker network connect`命令运行时检查参数`--ip`,`--ip6`和`--link-local-ip`
* 添加`dns-search`的支持
* 添加`--verbose`选项使得`docker network inspect`显示所有Swarm 节点上的任务细节
* 当节点加入集群后，清理老旧的路径加密状态
* 保障Iptables初始化，只运行一次
* 修复错误的iptables过滤规则
* 在可附加的网络上，添加异步容器的别名到服务记录上
* 添加驱动标签`com.docker.network.container_interface_prefix`
* 提高`docker network ls`的性能，通过疏略一些没有用到的细节信息

## Docker Runtime 运行时

* 在`live-restore`没有指定时，处理重新加载时暂停着的容器
* 在Dockerfile中的Healthcheck检查中，禁止亚秒的支持（由DaoCloud Allen Sun 完成）
* 在`secret update`的API实现中，支持secret的名称以及ID前缀（由DaoCloud Allen Sun支持）
* 使用二进制frame实现websocket附加
* 修复Linux挂载调用时不应用传播类型变更的问题
* 修复失败的`exec -i`时ExecID的泄漏问题
* 清理有名字但是没有被标记的镜像，当`danglingOnly=true`时
* 对没有赋予特权功能的容器，添加一个Daemon端的参数`no_new_priv`, 作为这些容器的默认值
* 添加Daemon端的选项`--default-shm-size`
* 在Docker Daemon Reload功能中添加`registry-mirror`的支持（由DaoCloud Allen Sun支持）
* 当构建镜像时，忽略daemon的日志配置
* 将secret名字以及ID前缀的解析从客户端迁移后daemon端
* 在容器创建运行时，为`cgroup devices.allow`添加策略
* 当运行`systemd daemon-reload`时，修复`cpu.cfs_quota_us`被重置的问题

## Docker 编排 Swarm Mode

* Docker可以实现拓扑感知的调度
* 实现自动化的更新失败回滚
* 如果worker和manager在同一个节点上的话，通过Unix Socket连接
* 优化raft传输的package
* 在降级以及删除过程中，不采用自动化的manager shutdown操作
* 使用`TransferLeadership`使得leader的降级更为安全
* 减少默认的监控周期
* 添加服务日志的格式化实现
* 修复服务的日志API使其可以指定流式
* 在`service update`和`service create`命令中添加`--stop-signal`参数的支持
* 在`service update`和`service create`命令中添加`--read-only`的参数支持
* 在与镜像仓库通信之后重新初始化context
* （试验功能）为`docker service logs`添加`--tail`和`--since`参数的支持
* （试验功能）为`docker service logs`添加`--no-task-ids`和`--no-trunc`的参数支持

## Windows

* 在非Window机器上的docker engine下，阻止Docker Daemon下载Windows操作系统的镜像
