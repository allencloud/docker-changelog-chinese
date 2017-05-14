# Docker 17.05.0-ce 变更日志

英文链接地址：[docker-17.05.0-ce CHANGELOG.md](https://github.com/moby/moby/blob/v17.05.0-ce/CHANGELOG.md)。

## 镜像构建器

* 添加多环节构建支持
* 允许在`FROM`命令中使用构建时间参数（`ARG`）
* 添加一个新的参数，用以支持制定构建目标
* 支持从`stdin`中接受`-f -`参数，但是使用本地的上下文完成构建
* 默认构建时间参数（比如 HTTP_PROXY）的值，不再显示在镜像的历史信息中，除非一个相应的`ARG`指令被写入Dockerfile
* 当一个自定义的shell被一个父镜像使用，其中存在的设置命令的bug
* 修复了在`docker build --label`中，当标签label中包含单引号以及空格时存在的bug

# Docker 客户端

* 为命令`docker run`和`docker create`添加参数`--mout`
* 为命令`docker inspect`添加过滤参数支持`--type=secret`
* 为命令`docker secret ls`添加`--format`参数支持
* 为命令`docker secret ls`添加`--filter`参数支持（此功能由DaoCloud Allen Sun完成）
* 为命令`docker network ls`添加`--filter scope=<swarm|local>`的支持
* 为命令`docker update`添加`--cpus`的支持
* 为命令`docker system prune`以及其他的`prune`命令添加标签过滤支持
* 现在`docker stack rm`支持接受多个stack输入的支持
* 改善`docker version --format`选项的输出，当客户端的版本信息低于Docker Engine 的API版本信息时
* 当使用一个加密过的Docker客户端访问Docker Daemon时迅速提示用户
* 在通过`docker build`成功构建镜像时，显示被创建的tag信息
* 清理compose转换时可能存在的错误信息

# Contrib

* 添加对 Ubuntu 17.04 Zesty在 AMD 处理器上的docker包构建方式

# Docker Daemon

* 如果没有设置`--api-enable-cors`时，修复了`--api-enable-cors`被忽略的bug
* 在Docker启动的时候，清理Docker的临时目录
* 废弃`--graph`参数，反而使用参数`--data-root `

# 日志

* 添加日志插件的支持
* 在`docker service logs`命令以及API接口`/task/{id}/logs`支持针对单个任务的日志显示
* 添加`--log-opt env-regex`选项来使用正则表达式匹配环境变量

# 网络

* 允许用户替换和自定义ingress网络
* 在容器重启之后，修复了容器中UDP流量不工作的Bug
* 如果不同的`data-root`被设置时，修复了`/var/lib/docker`可以被写的Bug

# Docker Daemon运行时

* 保障了健康检查的探针在一个容器退出时，被Docker Daemon自动停止

# Swarm Mode

* 为服务service添加update/rollback的支持（`--update-order `和`--rollback-order`）
* 添加同步的`docker service create`和`docker service update`支持
* 在健康检查中添加“grace period”（启动耐心等待时间段）的支持，支持方式为`HEALTHCHECK --start-period`，以及在`docker service create`,`docker service update`，`docker create`和`docker run`四个命令中添加参数`--health-start-period`，从而支持容器拥有一个起始的启动时间
* 命令`docker service create `现在忽略那些不被用户指定的选项。这将允许很多的默认值都由swarm manager来指定。
* 命令`docker service inspect`现在显示那些没有被用户指定的默认值
* 现在命令`docker service logs`已经不属于试验版(experimental)功能
* 在服务的API中添加Credential Spec 和 SELinux 的支持
* 在命令`docker service create`和`docker service update`中添加`--entrypoint`的支持
* 在`docker service update`命令中添加参数`--network-add`和`--network-rm`
* 在`docker service create`和`docker service update`命令中添加参数`--credential-spec`
* 为命令`docker service ls`添加参数`--filter mode=<global|replicated>`的支持
* 在客户端解析网络ID，而不是在daemon端创建服务时解析
* 在命令`docker node ls`中添加`--format`的支持
* 在命令`docker stack deploy`中添加`--prune`参数，以删除那些从来没有在docker-compose文件中被定义的服务
* 当使用 ingress 模式的时候，为命令`docker service ls`添加显示列`--PORT`
* 当环境变量被使用时，修复了不必要的任务重新部署问题
* 当从docker-compose文件来部署内容时，修复了不支持`endpoint_mode`的问题。(此功能由DaoCloud Allen Sun完成）
* 当集群组件不能被创建时，重新处理启动环节，从而允许从一个已经损坏的Swarm环境中恢复

# 安全

* 当使用`--ipc=container:`或者`--ipc=host`时，允许设置SELinux的类型活着MCS的标签