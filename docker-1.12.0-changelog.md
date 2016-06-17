# Docker 1.12.0 变更日志

Docker 1.12.0将在DockerCon 2016前后发布。

## 镜像构建器
+ 在Dockerfile的定义规范中添加了新的指令“HEALTHCHECK”，用以支持用户定义的健康检查
+ 在Dockerfile的定义规范中添加了新的指令“SHELL”，用以实现在Dockerfile中执行某个命令时，采用的具体SHELL类型
+ 在Dockerfile的定义规范中添加了新的指令`#escape=`，支持与平台架构相关的文件路径解析
+ 在".dockerignore"中添加对注释的支持
+ 在Dockerfile中添加对UTF-8格式的支持
+ 在Dockerfile和.dockerignore中选择忽略UTF-8 BOM字节的内容，如果存在的话。值得一提的是：该BUG的修复，源于DaoCloud公有云用户在使用镜像构建服务时，代码仓库中的Dockerfile属于使用Windows平台上的notepad软件编辑，从而产生UTF-8 BOM, DaoCloud的工程师第一时间将该隐藏BUG汇报Docker社区并解决。

## Contrib
+ 

## 镜像仓库 Distribution
+ 添加了两个Docker Daemon的运行参数`--max-concurrent-downloads`和`--max-concurrent-uploads`，使得下载镜像时如果网络不支持多任务下载/上传的场景。
+ 目前针对Docker Registry的所有操作都支持`ALL_PROXY`这个环境变量
+ 在用户执行`docker load`时，Docker Daemon提供更多信息

## 日志
+ Syslog日志驱动开始支持`DGRAM`套接字
+ 在`docker logs`命令中添加`--details`选项，帮助用户获取日志标签
+ 当创建容器时，继承Docker Daemon默认的日志参数
+ 从日志消息的标签中删除前缀`/docker`，而是采用`{{.DaemonName}}`，这样的话可以保证让用户有权限来修改该前缀

## 网络
+ 在`docker network ls`命令中添加针对`driver`的过滤
+ 在`docker ps --filter`命令中添加针对`network`的过滤
+ 修复了当使用生成的名字来重命名容器时存在的DNS问题
+ 同时允许`docker network inspect -f {{.Id}}`和`docker network inspect -f {{.ID}}`来说明输出内容的不一致性
+ 为`create`,`run`,`network connect`命令添加`--link-local-ip`flag参数，来指定一个容器的本地连接地址

## 试验性质的功能（experimental）
+ 实现新的`plugin`命令来管理各种插件，同时实现了改命令下的子命令`install`,`enbale`,`disable`,`rm`,`inspect`,`set`

## Docker Engine API(v1.24)
+ 为`docker images --filter`命令添加两种类型的过滤筛选`before`和`after`
+ 为`docker search`命令添加了新的参数`--limit`
+ 为`docker search`命令添加了新的参数`--filter`
+ 目前API调用时，若Docker Daemon内部发生一个错误，则实现API返回一个JSON对象，使得API的实现更加一致
+ 避免了`docker run -i --restart`命令在exit时候经常会夯住的问题

## Docker Engine运行时
+ 为Docker Daemon添加了一个`--live-restore`的参数，使得当Docker Daemon停止运行时，容器仍然可以保持运行，另外在Docker Daemon重新之后，又获取这部分容器的控制权
+ 添加了OCI兼容的容器运行时实现方案（通过Docker Daemon的--add-runtime参数来实现），同时可以在`create`和`run`命令中通过`--runtime`参数选择其中之一
+ 实现了新的镜像存储驱动`overlay2`，该特性专门为Linux 4.0+的环境服务，可以提供多种更为底层的目录实现
+ 实现新的load和save的镜像事件
+ 开始支持通过systemd来热加载Docker Daemon的配置参数
+ 为btrfs类型的存储驱动添加磁盘限额
+ 为ZFS类型的存储驱动添加磁盘限额
+ 支持在`docker run`命令中指定容器复用其他容器的PID namespace：`docker run --pid=container:<id>`
+ 对齐默认seccomp文件中的被选择的能力（Capabilities）
+ 当Docker Daemon发生配置热更新时，实现事件`daemon reload`
+ 在Docker Daemon运行过程中，在pprof中实现trace能力，用以显示进程的执行现状
+ 添加支持了`detach`事件
+ 在`run`,`build`,`create`,`update`命令时，重新启动`-c`参数作为`--cpu-shares`的缩称
+ 在一次eCryptfs的挂载时，阻止使用aufs和overlay的存储驱动
+ 修复了tmpfs的挂载次序问题
+ 状态为`Created`的容器不再出现在`docker ps -a -f exited=0`命令的显示结果中
+ 修复了容器会始终处于`Removal In Progress`状态的问题
+ 当在run/create命令中没有指定命令时，HTTP返回码改为500，而不是400
+ 修复了使用`--detach-keys`参数时的BUG，在这里当输入时detach key的一个前缀时，这种情况不会被保留
+ SELinux标记功能会被自动禁用，加入容器开启了Privileged模式时
+ 如果有Volume挂载进入一个容器，那么`/etc/hosts`,`/etc/resolv.conf`,`/etc/hostname`不再是SELinux标记的
+ 修复了`--tmpfs`和挂载参数之间的不一致
+ 修复了Docker Daemon在启动时有可能存在的夯住的问题
+ 在某些场景下，忽略SIGPIPE时间，从而防止journald重启时有可能造成的daemon崩溃

## Swarm 模式
+ 实现了一个新的`swarm`命令来管理swarm，其中管理的子命令有`init`,`join`,`leave`和`update`
+ 实现了新的`service`命令来管理与swarm相关的服务，其中实现的子命令有`create`,`inspect`,`update`,`remove`和`tasks`
+ 实现了新的`node`命令来支持管理集群中的Docker Engine，其中实现的字命令有`accept`,`promote`,`demote`,`inspect`,`update`,`tasks`,`ls`和`rm`
+ 实现了试验性的两个命令`stack`和`deploy`,用以管理部署多服务融合的分布式应用

## Volume 存储卷
+ 为存储卷Volume添加了范围属性`local`和`global`，和网络的范围属性类似
+ Mount/Unmount操作会收到一个不透明的ID，来允许存储卷驱动曲风两个不同的调用者
+ 修复了在某种稀有场景下阻止删除存储卷volume的问题

## 功能弃用
Docker Daemon的发展历程中，对于新功能的加入，和旧功能的删除都有着严谨的态度。对于即将弃用的功能，Docker首先会将其设置为`deprecated`，随后在过去的若干时间之内再将其移除，给予用户充足缓冲区的同时，也保证了代码的整洁程度。

+ 环境变量 `DOCKER_CONTENT_TRUST_OFFLINE_PASSPHRASE` 和 `DOCKER_CONTENT_TRUST_TAGGING_PASSPHRASE` 已经被重命名为 `DOCKER_CONTENT_TRUST_ROOT_PASSPHRASE`和`DOCKER_CONTENT_TRUST_REPOSITORY_PASSPHRASE`
+ 删除了日志选项`syslog-tag`,`gelf-tag`和`fluentd-tag`，从而支持更通用的`tag`日志选项
+ 删除了被弃用的功能, 也就是在API端，容器启动时传递HostConfig参数
+ 删除了`docker tag`命令中的`-f/--force`参数
+ 删除了被遗弃的`/containers/<id|name>/copy`的URL
+ 弃用了`docker import`命令中老的三个参数的形式。


