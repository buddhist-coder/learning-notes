# 用法:	docker [OPTIONS] COMMAND

为容器提供一个自包含的运行环境!

## Options:
```
    --config string                                     客户端配置文件的位置（默认为“ /root/.docker”）
    -c, --context string                                用于连接到守护程序的上下文名称（覆盖DOCKER_HOST env var和使用“ docker context use”设置的默认上下文）
    -D, --debug                                         启用调试模式
    -H, --host lis                                      守护的socket连接到
    -l, --log-level string                              设置日志记录级别（“调试” |“信息” |“警告” |“错误”|“致命”）（默认为“信息”）
    --tls                                               使用TLS; --tlsverify暗示
    --tlscacert string                                  仅由该CA签名的信任证书（默认为“ /root/.docker/ca.pem”）
    --tlscert string                                    TLS证书文件的路径（默认为“ /root/.docker/cert.pem”）
    --tlskey string                                     TLS密钥文件的路径（默认为“ /root/.docker/key.pem”）
    --tlsverify                                         使用TLS并验证远程
    -v, --version                                       打印版本信息并退出
```

## 管理 命令:
### builder 管理构建
用法:	docker builder COMMAND
```
Commands:
	build
		用法:   docker builder build [OPTIONS] PATH | URL | -
		从Dockerfile构建镜像
		Options:
			--add-host list                             添加自定义主机到IP的映射（host：ip）
			--build-arg list                            设置构建时变量
			--cache-from strings                        视为缓存源的镜像
			--cgroup-parent string                      容器的可选父cgroup
			--compress                                  使用gzip压缩构建上下文
			--cpu-period int                            限制CPU CFS（完全公平调度程序）期限
			--cpu-quota int                             限制CPU CFS（完全公平的调度程序）配额
			-c, --cpu-shares int                        PU份额（相对重量）
			--cpuset-cpus string                        允许执行的CPU（0-3，0,1）
			--cpuset-mems string                        允许执行的MEM（0-3，0,1）
			--disable-content-trust                     跳过镜像验证（默认为true）
			-f, --file string                           Dockerfile的名称（默认为“ PATH /Dockerfile”）
			--force-rm                                  始终删除中间容器
			--iidfile string                            将镜像ID写入文件
			--isolation string                          容器隔离技术
			--label list                                设置镜像的元数据
			-m, --memory bytes                          内存限制
			--memory-swap bytes                         交换限制等于内存加交换：“-1”以启用无限交换
			--network string                            设置构建期间RUN指令的联网模式（默认为“默认”）
			--no-cache                                  构建镜像时不要使用缓存
			--pull                                      始终尝试提取镜像的较新版本
			-q, --quiet                                 禁止生成输出并成功打印镜像ID
			--rm                                        成功构建后删除中间容器（默认为true）
			--security-opt strings                      安全选项
			--shm-size bytes                            /dev/shm的大小
			-t, --tag list                              名称以及“ name：tag”格式的标签（可选）
			--target string                             设置要构建的目标构建阶段
			--ulimit ulimit                             Ulimit选项（默认[]）
	prune       删除构建缓存
运行'docker builder COMMAND --help'以获取有关命令的更多信息。
```

###   config      管理Docker配置
用法:	docker config COMMAND
```
Commands:
	create  从文件或STDIN创建配置
		用法:	docker config create [OPTIONS] CONFIG file|-
		Options:
			-l, --label list                            配置标签
			--template-driver string                    模板驱动

	inspect 显示一个或多个配置的详细信息
		用法:	docker config inspect [OPTIONS] CONFIG [CONFIG...]
		Options:
			-f, --format string                         使用给定的Go模板格式化输出
			--pretty                                    以人类友好的格式打印信息

	ls  列出配置
		用法:	docker config ls [OPTIONS]
		Aliases:
			ls, list
		Options:
			-f, --filter filter                         根据提供的条件过滤输出
			--format string                             使用Go模板的漂亮打印配置
			-q, --quiet                                 仅显示ID

	rm  删除一个或多个配置
		用法:	docker config rm CONFIG [CONFIG...]
		Aliases:
			rm, remove
运行'docker config COMMAND --help'以获取有关命令的更多信息。
```

###   container   管理容器
用法:	docker container COMMAND
```
Commands:
	attach 将本地标准输入，输出和错误流附加到正在运行的容器
		用法:	docker container attach [OPTIONS] CONTAINER
		Options:
    		--detach-keys string                        覆盖分离容器的键序列
    		--no-stdin                                  不要附上STDIN
    		--sig-proxy                                 将所有收到的信号代理到进程（默认为true）

	commit 根据容器的更改创建新镜像
		用法:	docker container commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
		Options:
			-a, --author string                         作者（例如，“约翰·汉尼拔·史密斯<hannibal@a-team.com>”）
			-c, --change list                           将Dockerfile指令应用于创建的镜像
			-m, --message string                        提交讯息
			-p, --pause                                 提交期间暂停容器（默认为true）

	cp 在容器和本地文件系统之间复制文件/文件夹
		用法:	docker container cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-
						docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH
		使用“-”作为源以从stdin中读取tar归档文件并将其提取到容器中的目录目的地。
		使用“-”作为目标以流式传输tar的tar存档容器源到标准输出。
		Options:
			-a, --archive                               存档模式（复制所有uid / gid信息）
			-L, --follow-link                           始终遵循SRC_PATH中的符号链接

	create 创建一个新容器
		用法:	docker container create [OPTIONS] IMAGE [COMMAND] [ARG...]
		Options:
			--add-host list                             添加自定义主机到IP的映射（host：ip）
			-a, --attach list                           附加到STDIN，STDOUT或STDERR
			--blkio-weight uint16                       块IO（相对权重），介于10到1000之间，或者为0禁用（默认为0）
			--blkio-weight-device list                  块IO权重（相对设备权重）（默认为[]）
			--cap-add list                              添加Linux功能
			--cap-drop list                             放弃Linux功能
			--cgroup-parent string                      容器的可选父cgroup
			--cidfile string                            将容器ID写入文件
			--cpu-period int                            限制CPU CFS（完全公平的调度程序）期限
			--cpu-quota int                             限制CPU CFS（完全公平的调度程序）配额
			--cpu-rt-period int                         限制CPU实时时间（以微秒为单位）
			--cpu-rt-runtime int                        以微秒为单位限制CPU实时运行时间
			-c, --cpu-shares int                        CPU份额（相对重量）
			--cpus decimal                              CPU数量
			--cpuset-cpus string                        允许执行的CPU（0-3，0,1）
			--cpuset-mems string                        允许执行的MEM（0-3，0,1）
			--device list                               将主机设备添加到容器
			--device-cgroup-rule list                   将规则添加到cgroup允许的设备列表中
			--device-read-bps list                      限制从设备读取的速率（每秒字节数）（默认为[]）
			--device-read-iops list                     限制从设备读取的速率（每秒IO）（默认[]）
			--device-write-bps list                     将写入速率（每秒的字节数）限制为设备（默认为[]）
			--device-write-iops list                    将写入速率（每秒的IO）限制为设备（默认为[]）
			--disable-content-trust                     跳过镜像验证（默认为true）
			--dns list                       　         设置自定义DNS服务器
			--dns-option list                           设定DNS选项
			--dns-search list                           设置自定义DNS搜索域
			--domainname string                         容器NIS域名
			--entrypoint string                         覆盖镜像的默认ENTRYPOINT
			-e, --env list                              设置环境变量
			--env-file list                 　　        读入环境变量文件
			--expose list                    　         公开一个或多个端口
			--gpus gpu-request                          要添加到容器中的GPU设备（'all' 通过所有GPU）
			--group-add list                            添加其他群组即可加入
			--health-cmd string                         运行命令以检查运行状况
			--health-interval duration                  运行检查之间的时间（ms | s | m | h）（默认为0s）
			--health-retries int                        需要连续报告不健康状况
			--health-start-period duration              开始运行状况重试倒计时之前，容器初始化的开始时间（ms | s | m | h）（默认为0s）
			--health-timeout duration                   允许执行一次检查的最长时间（ms | s | m | h）（默认为0s）
			--help                                      打印用法
			-h, --hostname string                       容器主机名
			--init                     　　　　         在容器内运行一个初始化程序，以转发信号并获取进程
			-i, --interactive                           即使未连接STDIN也保持打开状态
			--ip string                      　         IPv4地址（例如172.30.100.104）
			--ip6 string                                IPv6地址（例如2001：db8 :: 33）
			--ipc string                                使用的IPC模式
			--isolation string                          容器隔离技术
			--kernel-memory bytes                       内核内存限制
			-l, --label list                            在容器上设置元数据
			--label-file list                           读入行分隔的标签文件
			--link list                     　          将链接添加到另一个容器
			--link-local-ip list                        容器IPv4 / IPv6链接本地地址
			--log-driver string                         容器的日志记录驱动程序
			--log-opt list                              日志驱动程序选项
			--mac-address string                        容器MAC地址（例如92：d0：c6：0a：29：33）
			-m, --memory bytes                          内存限制
			--memory-reservation bytes                  内存软限制
			--memory-swap bytes                         交换限制等于内存加交换：“-1”以启用无限交换
			--memory-swappiness int                     调优容器内存交换（0到100）（默认-1）
			--mount mount                               将文件系统挂载附加到容器
			--name string                               为容器分配一个名称
			--network network                           将容器连接到网络
			--network-alias list                        为容器添加网络范围的别名
			--no-healthcheck                            禁用任何容器指定的健康检查
			--oom-kill-disable                          禁用OOM杀手
			--oom-score-adj int                         调整主机的OOM首选项（-1000至1000）
			--pid string                                使用的PID名称空间
			--pids-limit int                            调整容器pids限制（将-1设置为无限制）
			--privileged                     　　　     赋予此容器扩展的特权
			-p, --publish list                   　     将容器的端口发布到主机
			-P, --publish-all                           将所有公开的端口发布到随机端口
			--read-only                                 将容器的根文件系统挂载为只读
			--restart string                            重启策略以在容器退出时应用（默认为“no”）
			--rm                                        退出时自动删除容器
			--runtime string                            用于此容器的运行时
			--security-opt list                         安全选项
			--shm-size bytes                            / dev / shm的大小
			--stop-signal string                        停止容器的信号（默认为“ SIGTERM”）
			--stop-timeout int                          停止容器的超时时间（以秒为单位）
			--storage-opt list                          容器的存储驱动程序选项
			--sysctl map                                Sysctl选项（默认map[]）
			--tmpfs list                                挂载tmpfs目录
			-t, --tty                                   分配伪TTY
			--ulimit ulimit                             Ulimit选项（默认[]）
			-u, --user string                           用户名或UID（格式：<名称| uid> [：<group | gid>]）
			--userns string                             要使用的用户名称空间
			--uts string                                使用的UTS名称空间
			-v, --volume list                           绑定挂载卷
			--volume-driver string                      容器的可选券驱动器
			--volumes-from list                         从指定的容器挂载卷
			-w, --workdir string                        容器内的工作目录

	diff   检查容器文件系统上文件或目录的更改
		用法:	docker container diff CONTAINER

	exec   在正在运行的容器中运行命令
		用法:	docker container exec [OPTIONS] CONTAINER COMMAND [ARG...]
		Options:
			-d, --detach              　　　　          分离模式：在后台运行命令
			--detach-keys string   　                   覆盖分离容器的键序列
			-e, --env list           　　　　　         设置环境变量
			-i, --interactive         　　　　          即使未连接STDIN也保持打开状态
			--privileged        　　　                  赋予命令扩展权限
			-t, --tty                  　　　　　       分配伪TTY
			-u, --user string          　　　           用户名或UID（格式：<名称| uid> [：<group | gid>]）
			-w, --workdir string       　               容器内的工作目录

	export 将容器的文件系统导出为tar存档
		用法:	docker container export [OPTIONS] CONTAINER
		Options:
			-o, --output string                         写入文件，而不是STDOUT

	inspect 显示一个或多个容器的详细信息
		用法:	docker container inspect [OPTIONS] CONTAINER [CONTAINER...]
		Options:
			-f, --format string   　                    使用给定的Go模板格式化输出
			-s, --size          　　　                  显示文件总大小

	kill   杀死一个或多个正在运行的容器
		用法:	docker container kill [OPTIONS] CONTAINER [CONTAINER...]
		Options:
			-s, --signal string                         发送到容器的信号（默认为“ KILL”）

	logs   提取容器的日志
		用法:	docker container logs [OPTIONS] CONTAINER
		Options:
			--details                                   显示提供给日志的其他详细信息
			-f, --follow                                跟踪日志输出
			--since string                              显示自时间戳记以来的日志（例如2013-01-02T13：23：37）或相对时间（例如42m，持续42分钟）
			--tail string                               从日志末尾开始显示的行数（默认为“all”）
			-t, --timestamps                            显示时间戳
			--until string                              在时间戳（例如2013-01-02T13：23：37）或相对（例如42m持续42分钟）之前显示日志

	ls  列出容器
		用法:	docker container ls [OPTIONS]
		Aliases:
			ls, ps, list
		Options:
			-a, --all                                   显示所有容器（默认显示为正在运行）
			-f, --filter filter                         根据提供的条件过滤输出
			-format string                              使用Go模板打印漂亮的容器
			-n, --last int                              显示最后创建的n个容器（包括所有状态）（默认为-1）
			-l, --latest                                显示最新创建的容器（包括所有状态）
			--no-trunc                                  不要截断输出
			-q, --quiet                                 仅显示数字ID
			-s, --size                                  显示文件总大小

	pause   暂停一个或多个容器中的所有进程
		用法:	docker container pause CONTAINER [CONTAINER...]

	port    列出端口映射或容器的特定映射
		用法:	docker container port CONTAINER [PRIVATE_PORT[/PROTO]]

	prune   删除所有停止的容器
		Options:
			--filter filter   　                        提供过滤器值（例如'until = <timestamp>'）
			-f, --force                                 不提示确认

	rename  重命名容器
		用法:	docker container rename CONTAINER NEW_NAME

	restart 重新启动一个或多个容器
		用法:	docker container restart [OPTIONS] CONTAINER [CONTAINER...]
		Options:
			-t, --time int                              在杀死容器之前等待停止的秒数（默认为10）

	rm  删除一个或多个容器
		Usage:	docker container rm [OPTIONS] CONTAINER [CONTAINER...]
		用法:
			-f, --force                                 强制删除正在运行的容器（使用SIGKILL）
			-l, --link                                  删除指定的链接
			-v, --volumes                               删除与容器关联的卷

	run 在新容器中运行命令
		用法:	docker container run [OPTIONS] IMAGE [COMMAND] [ARG...]
		Options:
			--add-host list                  　         添加自定义主机到IP的映射（host：ip）
		    -a, --attach list                           附加到STDIN，STDOUT或STDERR
			--blkio-weight uint16                       块IO（相对权重），介于10到1000之间，或者为0禁用（默认为0）
			--blkio-weight-device list                  块IO权重（相对设备权重）（默认为[]）
			--cap-add list                              添加Linux功能
			--cap-drop list                             放弃Linux功能
			--cgroup-parent string                      容器的可选父cgroup
			--cidfile string                            将容器ID写入文件
			--cpu-period int                            限制CPU CFS（完全公平的调度程序）期限
			--cpu-quota int                             限制CPU CFS（完全公平的调度程序）配额
			--cpu-rt-period int                         限制CPU实时时间（以微秒为单位）
			--cpu-rt-runtime int                        以微秒为单位限制CPU实时运行时间
		    -c, --cpu-shares int                        CPU份额（相对重量）
    		--cpus decimal                              CPU数量
			--cpuset-cpus string                        允许执行的CPU（0-3，0,1）
			--cpuset-mems string                        允许执行的MEM（0-3，0,1）
		    -d, --detach                                在后台运行容器并打印容器ID
			--detach-keys string                        覆盖分离容器的键序列
			--device list                               将主机设备添加到容器
			--device-cgroup-rule list                   将规则添加到cgroup允许的设备列表中
			--device-read-bps list                      限制从设备读取的速率（每秒字节数）（默认为[]）
			--device-read-iops list                     限制从设备读取的速率（每秒IO）（默认[]）
			--device-write-bps list                     将写入速率（每秒的字节数）限制为设备（默认为[]）
			--device-write-iops list                    将写入速率（每秒的IO）限制为设备（默认为[]）
			--disable-content-trust                     跳过镜像验证（默认为true）
			--dns list                                  设置自定义DNS服务器
			--dns-option list                           设定DNS选项
			--dns-search list                　         设置自定义DNS搜索域
			--domainname string                         容器NIS域名
			--entrypoint string                         覆盖镜像的默认ENTRYPOINT
		    -e, --env list                       　   　设置环境变量
			--env-file list                             读入环境变量文件
			--expose list                               公开一个或多个端口
			--gpus gpu-request                          要添加到容器中的GPU设备（“all”通过所有GPU）
			--group-add list                            添加其他群组即可加入
			--health-cmd string                         运行命令以检查运行状况
			--health-interval duration                  运行检查之间的时间（ms | s | m | h）（默认为0s）
			--health-retries int                        需要连续报告不健康状况
			--health-start-period duration              开始运行状况重试倒计时之前，容器初始化的开始时间（ms | s | m | h）（默认为0s）
			--health-timeout duration                   允许执行一次检查的最长时间（ms | s | m | h）（默认为0s）
			--help                                      打印用法
		    -h, --hostname string                       容器主机名
			--init                           　　　     在容器内运行一个初始化程序，以转发信号并获取进程
		    -i, --interactive                    　　   即使未连接STDIN也保持打开状态
			--ip string                                 IPv4地址（例如172.30.100.104）
			--ip6 string                                IPv6地址（例如2001：db8 :: 33）
			--ipc string                                使用的IPC模式
			--isolation string                          容器隔离技术
			--kernel-memory bytes                       内核内存限制
		    -l, --label list                            在容器上设置元数据
			--label-file list                           读入行分隔的标签文件
			--link list                      　　　     将链接添加到另一个容器
			--link-local-ip list                        容器IPv4 / IPv6链接本地地址
			--log-driver string                         容器的日志记录驱动程序
			--log-opt list                              日志驱动程序选项
			--mac-address string                        容器MAC地址（例如92：d0：c6：0a：29：33）
		    -m, --memory bytes                          内存限制
			--memory-reservation bytes                  内存软限制
			--memory-swap bytes                         交换限制等于内存加交换：“-1”以启用无限交换
			--memory-swappiness int                     调优容器内存交换（0到100）（默认-1）
			--mount mount                               将文件系统挂载附加到容器
			--name string                               为容器分配一个名称
			--network network                           将容器连接到网络
			--network-alias list                        为容器添加网络范围的别名
			--no-healthcheck                            禁用任何容器指定的健康检查
			--oom-kill-disable                          禁用OOM杀手
			--oom-score-adj int                         调至主机的OOM首选项（-1000至1000）
			--pid string                     　　　     使用的PID名称空间
			--pids-limit int                　　　      调整容器pids限制（将-1设置为无限制）
			--privileged                                赋予此容器扩展的特权
		    -p, --publish list                          将容器的端口发布到主机
		    -P, --publish-all                           将所有公开的端口发布到随机端口
			--read-only                                 将容器的根文件系统挂载为只读
			--restart string                            重启策略以在容器退出时应用（默认为“no”）
			--rm                                        退出时自动删除容器
			--runtime string                            用于此容器的运行时
			--security-opt list                         安全选项
			--shm-size bytes                            / dev / shm的大小
			--sig-proxy                                 代理接收到该进程的信号（默认为true）
			--stop-signal string                        停止容器的信号（默认为“ SIGTERM”）
			--stop-timeout int                          停止容器的超时时间（以秒为单位）
			--storage-opt list                          容器的存储驱动程序选项
			--sysctl map                                Sysctl选项（默认map[]）
			--tmpfs list                                挂载tmpfs目录
		    -t, --tty                                   分配伪TTY
			--ulimit ulimit                             Ulimit选项（默认[]）
		    -u, --user string                           用户名或UID（格式：<名称| uid> [：<group | gid>]）
			--userns string                             要使用的用户名称空间
			--uts string                                使用的UTS名称空间
		    -v, --volume list                           绑定挂载卷
			--volume-driver string                      容器的可选音量驱动器
			--volumes-from list                         从指定的容器挂载卷
		    -w, --workdir string                        容器内的工作目录

	start   启动一个或多个已停止的容器
		用法:	docker container start [OPTIONS] CONTAINER [CONTAINER...]
		Options:
			-a, --attach               　　　　　　　　 连接STDOUT / STDERR和转发信号
			--detach-keys string   　　　　             覆盖分离容器的键序列
			-i, --interactive                           附加容器的STDIN

	stats   显示实时的容器资源使用情况统计流
		用法:	docker container stats [OPTIONS] [CONTAINER...]
		Options:
			-a, --all                                   显示所有容器（默认显示为正在运行）
			--format string                             使用Go模板打印漂亮的镜像
			--no-stream                                 禁用流统计信息，仅获取第一个结果
		    --no-trunc                                  不要截断输出

	stop   停止一个或多个运行中的容器
		用法:	docker container stop [OPTIONS] CONTAINER [CONTAINER...]
		Options:
			-t, --time int                              等待停止之前等待的秒数（默认值为10）

	top     显示容器的运行过程
		用法:	docker container top CONTAINER [ps OPTIONS]

	unpause    取消暂停一个或多个容器中的所有进程
		用法:	docker container unpause CONTAINER [CONTAINER...]

	update  更新一个或多个容器的配置
		用法:	docker container update [OPTIONS] CONTAINER [CONTAINER...]
		Options:
			--blkio-weight uint16                       块IO（相对权重），介于10到1000之间，或者为0禁用（默认为0）
			--cpu-period int                            限制CPU CFS（完全公平的调度程序）期限
			--cpu-quota int                             限制CPU CFS（完全公平的调度程序）配额
			--cpu-rt-period int                         将CPU实时时间限制为微秒
			--cpu-rt-runtime int                        将CPU实时运行时间限制为微秒
		    -c, --cpu-shares int                        CPU份额（相对重量）
		    --cpus decimal                              CPU数量
		    --cpuset-cpus string                        允许执行的CPU（0-3，0,1）
		    --cpuset-mems string                        允许执行的MEM（0-3，0,1）
		    --kernel-memory bytes                       内核内存限制
		    -m, --memory bytes                          内存限制
			--memory-reservation bytes                  内存软限制
			--memory-swap bytes                         交换限制等于内存加交换：“-1”以启用无限交换
			--pids-limit int                            调整容器pids限制（将-1设置为无限制）
			--restart string                            容器退出时重新启动策略以应用

	wait        阻塞直到一个或多个容器停止，然后打印其退出代码
		用法:	docker container wait CONTAINER [CONTAINER...]
```
###   context     管理上下文
用法:	docker context COMMAND
```
Commands:
	create  创建上下文
		Usage:	docker context create [OPTIONS] CONTEXT
		Docker endpoint config:
		NAME                                            描述
		from                                            复制命名上下文的Docker端点配置
		host                                            用于连接的Docker端点
		ca                                              仅由该CA签名的信任证书
		cert                                            TLS证书文件的路径
		key                                             TLS密钥文件的路径
		skip-tls-verify                                 跳过TLS证书验证
		Kubernetes endpoint config:
		NAME                                            描述
		from                                            复制命名上下文的Kubernetes端点配置
		config-file                                     Kubernetes配置文件的路径
		context-override                                覆盖kubernetes配置文件中设置的上下文
		namespace-override                              覆盖kubernetes配置文件中设置的名称空间
		案例:
		$ docker context create my-context --description "some description" --docker "host=tcp://myserver:2376,ca=~/ca-file,cert=~/cert-file,key=~/key-file"
		Options:
			--default-stack-orchestrator string   　　　用于此上下文的堆栈操作的默认协调器（swarm | kubernetes | all）
			--description string                 　　　 上下文描述
			--docker stringToString                     设置docker端点（默认[]）
			--from string                               从命名上下文创建上下文
			--kubernetes stringToString                 设置kubernetes端点（默认[]）

	export  将上下文导出到tar或kubeconfig文件
		用法:	docker context export [OPTIONS] CONTEXT [FILE|-]
		Options:
			--kubeconfig                                导出为kubeconfig文件

	import  从tar或zip文件导入上下文
		用法:	docker context import CONTEXT FILE|-
		Import a context from a tar or zip file

	inspect 显示有关一个或多个上下文的详细信息
		用法:	docker context inspect [OPTIONS] [CONTEXT] [CONTEXT...]
		Options:
			-f, --format string                         使用给定的Go模板格式化输出

	ls  列出上下文
		用法:	docker context ls [OPTIONS]
		Aliases:
			ls, list
		Options:
			--format string                             使用Go模板打印漂亮的上下文
			-q, --quiet                                 仅显示上下文名称

	rm  删除一个或多个上下文
		用法:	docker context rm CONTEXT [CONTEXT...]
		Aliases:
			rm, remove
		Options:
			-f, --force                                 强制删除使用中的上下文

	update  更新上下文
		用法:	docker context update [OPTIONS] CONTEXT
		Docker endpoint config:
		NAME                                            描述
		from                                            复制命名上下文的Docker端点配置
		host                                            用于连接的Docker端点
		ca                                              仅由该CA签名的信任证书
		cert                                            TLS证书文件的路径
		key                                             TLS密钥文件的路径
		skip-tls-verify                                 Skip TLS certificate validation
		Kubernetes endpoint config:
		NAME                                            描述
		from                                            复制命名上下文的Kubernetes端点配置
		config-file                                     Kubernetes配置文件的路径
		context-override                                覆盖kubernetes配置文件中设置的上下文
		namespace-override                              覆盖kubernetes配置文件中设置的名称空间
		案例:
		$ docker context update my-context --description "some description" --docker "host=tcp://myserver:2376,ca=~/ca-file,cert=~/cert-file,key=~/key-file"
		Options:
			--default-stack-orchestrator string   　　　用于此上下文的堆栈操作的默认协调器（swarm | kubernetes | all）
			--description string                  　　　上下文描述
			--docker stringToString                     设置docker端点（默认[]）
			--kubernetes stringToString                 设置kubernetes端点（默认[]）

	use    设置当前docker上下文
	用法:	docker context use CONTEXT

Run 'docker context COMMAND --help' for more information on a command.
```
###   engine      管理Docker引擎
用法:	docker engine COMMAND
```
Commands:
	activate   激活企业版
		用法:	docker engine activate [OPTIONS]
		使用此命令，您可以应用现有的Docker企业许可证，或者从Docker交互式下载一个。 在互动交流中，您可以注册新试用版，或下载现有许可证。 如果你是当前正在运行Community Edition引擎，该守护程序将更新为企业版Docker引擎，具有更多功能和更长的时间长期支持。
		有关不同Docker Enterprise许可证类型的更多信息，请访问https://www.docker.com/licenses
		对于非交互式可脚本部署，请从以下位置下载许可证
		https://hub.docker.com/ then specify the file with the '--license' flag.
		Options:
			--containerd string                         覆盖容器端点的默认位置
			--display-only                              仅显示许可证信息并退出
			--engine-image string                       指定引擎镜像
			--format string           　　　　　　　    使用Go模板的精美印刷许可证
			--license string           　　　　　　　   许可文件
			--quiet                    　　　　　　　　 仅按ID显示可用许可证
			--registry-prefix string                    覆盖提取引擎映像的默认位置（默认为“ docker.io/store/docker”）
			--version string                            指定引擎版本（默认为使用当前运行的版本）

	check   检查可用的引擎更新
		用法:	docker engine check [OPTIONS]
		Options:
			--containerd string                         覆盖容器端点的默认位置
			--downgrades                                报告降级（默认省略旧版本）
			--engine-image string                       指定引擎镜像（默认使用与当前运行相同的镜像）
			--format string                             使用Go模板进行漂亮的打印更新
			--pre-releases                              包括预发行版本
			-q, --quiet                                 仅显示可用版本
			--registry-prefix string                    覆盖提取引擎映像的现有位置（默认为“ docker.io/store/docker”）
			--upgrades                                  报告可用的升级（默认为true）

	update  更新本地引擎
		用法:	docker engine update [OPTIONS]
		Options:
			--containerd string                         覆盖容器端点的默认位置
			--engine-image string                       指定引擎镜像（默认使用与当前运行相同的镜像）
			--registry-prefix string   　　　　　       覆盖提取引擎镜像的当前位置（默认为“ docker.io/store/docker”）
			--version string          　　　　　　      指定引擎版本

Run 'docker engine COMMAND --help' for more information on a command.
```

###   image      管理镜像
用法:	docker image COMMAND
```
Commands:
	build   从Dockerfile构建镜像
		用法:	docker image build [OPTIONS] PATH | URL | -
		Options:
			--add-host list           　　　　　　　　  添加自定义主机到IP的映射（host：ip）
			--build-arg list         　　　　　　　　   设置构建时变量
			--cache-from strings                        视为缓存源的镜像
			--cgroup-parent string    　　　　　        容器的可选父cgroup
			--compress                                  使用gzip压缩构建上下文
			--cpu-period int                            限制CPU CFS（完全公平调度程序）期限
			--cpu-quota int                             限制CPU CFS（完全公平的调度程序）配额
			-c, --cpu-shares int                        CPU份额（相对重量）
			--cpuset-cpus string                        允许执行的CPU（0-3，0,1）
			--cpuset-mems string                        允许执行的MEM（0-3，0,1）
			--disable-content-trust                     跳过镜像验证（默认为true）
			-f, --file string                           Dockerfile的名称（默认为“ PATH / Dockerfile”）
			--force-rm                                  始终删除中间容器
			--iidfile string          　　　　　　　　  将镜像ID写入文件
			--isolation string                          容器隔离技术
			--label list                                设置镜像的元数据
			-m, --memory bytes            　　　　　　  内存限制
			--memory-swap bytes                         交换限制等于内存加交换：“-1”以启用无限交换
			--network string                            设置构建期间RUN指令的联网模式（默认为“default”）
			--no-cache                                  构建镜像时不要使用缓存
			--pull                                      始终尝试提取镜像的较新版本
			-q, --quiet                                 禁止生成输出并成功打印镜像ID
			--rm                                        成功构建后删除中间容器（默认为true）
			--security-opt strings                      安全选项
			--shm-size bytes                            / dev / shm的大小
			-t, --tag list                              名称以及“ name：tag”格式的标签（可选）
			--target string                             设置要构建的目标构建阶段。
			--ulimit ulimit                             Ulimit选项（默认[]）

	history     显示镜像的历史记录
		用法:	docker image history [OPTIONS] IMAGE
		Options:
			--format string   　　　　　　　　　        使用Go模板打印漂亮的镜像
			-H, --human          　　　　　　　　　　   以人类可读的格式打印尺寸和日期（默认为true）
			--no-trunc                                  不要截断输出
			-q, --quiet                                 仅显示数字ID

	import      从tarball导入内容以创建文件系统镜像
		用法:	docker image import [OPTIONS] file|URL|- [REPOSITORY[:TAG]]
		Options:
			-c, --change list      　　　　　　　　　　 将Dockerfile指令应用于创建的映像
			-m, --message string  　　　　　　　　      设置导入镜像的提交消息

	inspect     在一张或多张镜像上显示详细信息
		用法:	docker image inspect [OPTIONS] IMAGE [IMAGE...]
		Options:
			-f, --format string  　　　　　　　　　　   使用给定的Go模板格式化输出

	load       从tar存档或STDIN加载镜像
		用法:	docker image load [OPTIONS]
		Options:
			-i, --input string                          从tar存档文件中读取，而不是从STDIN中读取
			-q, --quiet                                 抑制负载输出

	ls          列出镜像
		用法:	docker image ls [OPTIONS] [REPOSITORY[:TAG]]
		Aliases:
			ls, images, list
		Options:
			-a, --all             　　　　　　　　　　　显示所有镜像（默认隐藏中间镜像）
			--digests                                   显示摘要
			-f, --filter filter                         根据提供的条件过滤输出
			--format string                             使用Go模板打印漂亮的镜像
			--no-trunc        　　　　　　　　　        不要截断输出
			-q, --quiet                                 仅显示数字ID

	prune   删除未使用的镜像
		用法:	docker image prune [OPTIONS]
		Options:
			-a, --all             　　　　　　　　　　　删除所有未使用的镜像，而不仅仅是悬空的镜像
			--filter filter   　　　　　　　　　　      提供过滤器值（例如'until = <timestamp>'）
			-f, --force                                 不提示确认

	pull   从仓库中提取镜像或存储库
		用法:	docker image pull [OPTIONS] NAME[:TAG|@DIGEST]
		Options:
			-a, --all-tags                              下载存储库中所有标记的镜像
			--disable-content-trust                     跳过镜像验证（默认为true）
			-q, --quiet                                 禁止详细输出

	push   将镜像或存储库推送到仓库
		用法:	docker image push [OPTIONS] NAME[:TAG]
		Options:
			--disable-content-trust   　　　　          跳过镜像签名（默认为true）

	rm     删除一个或多个镜像
		用法:	docker image rm [OPTIONS] IMAGE [IMAGE...]
		Aliases:
			rm, rmi, remove
		Options:
			-f, --force      　　　　　　　　　　　     强制删除镜像
		    --no-prune   　　　　　　　　　　　         不要删除未加标签的父母

	save   将一个或多个镜像保存到tar存档（默认情况下流式传输到STDOUT）
		用法:	docker image save [OPTIONS] IMAGE [IMAGE...]
		Options:
			-o, --output string                         写入文件，而不是STDOUT

	tag    创建一个引用了SOURCE_IMAGE的标签TARGET_IMAGE
		用法:	docker image tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]

Run 'docker image COMMAND --help' for more information on a command.
```
###   network     管理网络
用法:	docker network COMMAND
```
Commands:
	connect     将容器连接到网络
		用法:	docker network connect [OPTIONS] NETWORK CONTAINER
		Options:
			--alias strings                             为容器添加网络范围的别名
			--driver-opt strings                        网络的驱动程序选项
			--ip string                                 IPv4地址（例如172.30.100.104）
			--ip6 string                                IPv6地址（例如2001：db8 :: 33）
			--link list                                 将链接添加到另一个容器
			--link-local-ip strings                     为容器添加本地链接地址

	create      创建一个网络
		用法:	docker network create [OPTIONS] NETWORK
		Options:
			--attachable                                启用手动容器附件
			--aux-address map                           网络驱动程序使用的辅助IPv4或IPv6地址（默认map[]）
			--config-from string                        从中复制配置的网络
			--config-only                               创建仅配置网络
			-d, --driver string                         驱动程序来管理网络（默认为“网桥”）
			--gateway strings                           主子网的IPv4或IPv6网关
			--ingress                                   创建群集路由网状网络
			--internal                                  限制外部访问网络
			--ip-range strings                          从子范围分配容器ip
			--ipam-driver string                        IP地址管理驱动程序（默认为“default”）
			--ipam-opt map                              设置IPAM驱动程序特定的选项（默认map[]）
			--ipv6                                      启用IPv6网络
			--label list                                在网络上设置元数据
			-o, --opt map                               设置驱动程序特定的选项（默认map[]）
			--scope string                              控制网络范围
			--subnet strings                            代表网段的CIDR格式的子网

	disconnect  断开容器与网络的连接
		用法:	docker network disconnect [OPTIONS] NETWORK CONTAINER
		Options:
			-f, --force                                 强制容器断开网络连接

	inspect     在一个或多个网络上显示详细信息
		用法:	docker network inspect [OPTIONS] NETWORK [NETWORK...]
		Options:
			-f, --format string                         使用给定的Go模板格式化输出
			-v, --verbose                               详细输出以进行诊断

	ls          列出网络
		用法:	docker network ls [OPTIONS]
		Aliases:
			ls, list
		Options:
			-f, --filter filter                         提供过滤器值（例如'driver = bridge'）
			--format string                             使用Go模板的精美印刷网络
			--no-trunc                                  不要截断输出
			-q, --quiet                                 仅显示网络ID

	prune       删除所有未使用的网络
		用法:	docker network prune [OPTIONS]
		Options:
			--filter filter                             提供过滤器值（例如'until = <timestamp>'）
			-f, --force                                 不提示确认

	rm          删除一个或多个网络
		用法:	docker network rm NETWORK [NETWORK...]
		Aliases:
			rm, remove

Run 'docker network COMMAND --help' for more information on a command.
```
###   node        管理Swarm节点
用法:	docker node COMMAND
```
Commands:
	demote                                              从群中的管理器降级一个或多个节点
		用法:	docker node demote NODE [NODE...]

	inspect     在一个或多个节点上显示详细信息
		用法:	docker node inspect [OPTIONS] self|NODE [NODE...]
		Options:
			-f, --format string                         使用给定的Go模板格式化输出
			--pretty                                    以人类友好的格式打印信息

	ls  列出群中的节点
		用法:	docker node ls [OPTIONS]
		Aliases:
			ls, list
		Options:
			-f, --filter filter                         根据提供的条件过滤输出
			--format string                             使用Go模板打印漂亮的节点
			-q, --quiet                                 仅显示ID

	promote    将一个或多个节点提升为集群中的管理器
		用法:	docker node promote NODE [NODE...]

	ps  列出在一个或多个节点上运行的任务，默认为当前节点
		用法:	docker node ps [OPTIONS] [NODE...]
		Options:
			-f, --filter filter                         根据提供的条件过滤输出
			--format string                             使用Go模板进行漂亮的打印任务
			--no-resolve                                不要将ID映射到名称
			--no-trunc                                  不要截断输出
			-q, --quiet                                 仅显示任务ID

	rm  从群中删除一个或多个节点
		用法:	docker node rm [OPTIONS] NODE [NODE...]
		Aliases:
			rm, remove
		Options:
			-f, --force                                 强制从群集中删除节点

	update  更新节点
		用法:	docker node update [OPTIONS] NODE
		Options:
			--availability string                       节点的可用性（“active” |“pause” |“drain”）
			--label-add list                            添加或更新节点标签(key=value)
			--label-rm list                             删除节点标签（如果存在）
			--role string                               节点的作用("worker"|"manager")

Run 'docker node COMMAND --help' for more information on a command.
```
###   plugin      管理插件
用法:	docker plugin COMMAND
```
Commands:
	create      从rootfs和配置创建一个插件。 插件数据目录必须包含config.json和rootfs目录。
		用法:	docker plugin create [OPTIONS] PLUGIN PLUGIN-DATA-DIR
		Options:
			--compress                                  使用gzip压缩上下文

	disable     禁用插件
		用法:	docker plugin disable [OPTIONS] PLUGIN
		Options:
			-f, --force                                 强制禁用活动插件

	enable      启用插件
		用法:	docker plugin enable [OPTIONS] PLUGIN
		Options:
			--timeout int                               HTTP客户端超时（以秒为单位）（默认为30）

	inspect     显示有关一个或多个插件的详细信息
		用法:	docker plugin inspect [OPTIONS] PLUGIN [PLUGIN...]
		Options:
			-f, --format string                         使用给定的Go模板格式化输出

	install     安装插件
		用法:	docker plugin install [OPTIONS] PLUGIN [KEY=VALUE...]
		Options:
			--alias string                              插件的本地名称
			--disable                                   不要在安装时启用插件
			--disable-content-trust                     跳过镜像验证（默认为true）
			--grant-all-permissions                     授予运行插件所需的所有权限

	ls         列出插件
		用法:	docker plugin ls [OPTIONS]
		Aliases:
			ls, list
		Options:
			-f, --filter filter                         提供过滤器值（例如'enabled = true'）
			--format string                             使用Go模板的精美打印插件
			--no-trunc                                  不要截断输出
			-q, --quiet                                 仅显示插件ID

	push        将插件推送到仓库
		用法:	docker plugin push [OPTIONS] PLUGIN[:TAG]
		Options:
			--disable-content-trust                     跳过镜像签名（默认为true）

	rm         删除一个或多个插件
		用法:	docker plugin rm [OPTIONS] PLUGIN [PLUGIN...]
		Aliases:
			rm, remove
		Options:
			-f, --force                                 强制删除活动插件

	set        更改插件的设置
		用法:	docker plugin set PLUGIN KEY=VALUE [KEY=VALUE...]

	upgrade     升级现有插件
		用法:	docker plugin upgrade [OPTIONS] PLUGIN [REMOTE]
		Options:
			--disable-content-trust                     跳过镜像验证（默认为true）
			--grant-all-permissions                     授予运行插件所需的所有权限
			--skip-remote-check                         不检查指定的远程插件是否与现有插件镜像匹配

Run 'docker plugin COMMAND --help' for more information on a command.
```

###   secret      管理Docker Secret
用法:	docker secret COMMAND
```
Commands:
	create      从文件或STDIN创建秘密作为内容
		用法:	docker secret create [OPTIONS] SECRET [file|-]
		Options:
			-d, --driver string            　　　　     Secret 驱动
			-l, --label list               　　　　　   Secret 标签
		    --template-driver string                    Template driver

	inspect     显示有关一个或多个秘密的详细信息
		用法:	docker secret inspect [OPTIONS] SECRET [SECRET...]
		Options:
			-f, --format string                         使用给定的Go模板格式化输出
			--pretty                                    以人类友好的格式打印信息

	ls          列出机密
		用法:	docker secret ls [OPTIONS]
		Aliases:
			ls, list
		Options:
			-f, --filter filter                         根据提供的条件过滤输出
			--format string                             使用Go模板打印漂亮的秘密
			-q, --quiet                                 仅显示ID

	rm          删除一个或多个秘密
		用法:	docker secret rm SECRET [SECRET...]
		Aliases:
			rm, remove

Run 'docker secret COMMAND --help' for more information on a command.
```

###   service    管理服务
用法:	docker service COMMAND
```
Commands:
	create      创建一个新服务
		用法:	docker service create [OPTIONS] IMAGE [COMMAND] [ARG...]
		Options:
			--config config                             指定配置以公开给服务
			--constraint list                           放置约束
			--container-label list                      容器标签
			--credential-spec credential-spec           托管服务帐户的凭据规范（仅Windows）
			-d, --detach                                立即退出，而不是等待服务融合
			--dns list                                  设置自定义DNS服务器
			--dns-option list                           设定DNS选项
			--dns-search list                           设置自定义DNS搜索域
			--endpoint-mode string                      Endpoint 模式（vip或dnsrr）（默认为“ vip”）
			--entrypoint command                        覆盖镜像的默认ENTRYPOINT
			-e, --env list                          　　设置环境变量
			--env-file list                      　　　 读入环境变量文件
			--generic-resource list                     用户定义的资源
			--group list                                为容器设置一个或多个补充用户组
			--health-cmd string                         运行命令以检查运行状况
			--health-interval duration                  运行检查之间的时间（ms | s | m | h）
			--health-retries int                        需要连续报告不健康状况
			--health-start-period duration              容器初始化之前的开始时间，然后计算重试到不稳定（ms | s | m | h）
			--health-timeout duration                   允许执行一次检查的最长时间（ms | s | m | h）
			--host list                                 设置一个或多个自定义主机到IP的映射（host：ip）
			--hostname string                           容器主机名
			--init                                      在每个服务容器中使用init来转发信号并获取进程
			--isolation string                          服务容器隔离模式
			-l, --label list                            服务标签
			--limit-cpu decimal                         限制CPU
			--limit-memory bytes                        限制内存
			--log-driver string                  　　　 记录驱动程序以进行服务
			--log-opt list                              记录驱动程序选项
			--mode string                               服务模式(replicated or global) (default "replicated")
			--mount mount                               将文件系统挂载附加到服务
			--name string                               服务名称
			--network network                           网络附件
			--no-healthcheck                            禁用任何容器指定的健康检查
			--no-resolve-image                          不要查询仓库来解析镜像摘要和支持的平台
			--placement-pref pref                　　　 添加展示位置偏好设置
			-p, --publish port                          将端口发布为节点端口
			-q, --quiet                                 禁止进度输出
			--read-only                                 将容器的根文件系统挂载为只读
			--replicas uint                             任务数
			--replicas-max-per-node uint                每个节点的最大任务数 (default 0 = unlimited)
			--reserve-cpu decimal                       备用CPU
			--reserve-memory bytes                      备用内存
			--restart-condition string           　　   满足条件时重启 ("none"|"on-failure"|"any") (default "any")
			--restart-delay duration                    重新启动尝试之间的延迟（ns | us | ms | s | m | h）(default 5s)
			--restart-max-attempts uint                 放弃之前的最大重新启动次数
			--restart-window duration                   用于评估重启策略的窗口 (ns|us|ms|s|m|h)
			--rollback-delay duration                   任务回滚之间的延迟 (ns|us|ms|s|m|h) (default 0s)
			--rollback-failure-action string            对回滚失败采取的措施 ("pause"|"continue") (default "pause")
			--rollback-max-failure-ratio float          回滚期间可以容忍的故障率 (default 0)
			--rollback-monitor duration                 每个任务回滚后监视失败的持续时间 (ns|us|ms|s|m|h) (default 5s)
			--rollback-order string                     回滚顺序("start-first"|"stop-first") (default "stop-first")
			--rollback-parallelism uint                 同时回滚的最大任务数 (0一次全部回滚) (default 1)
			--secret secret                             指定要公开给服务的机密
			--stop-grace-period duration                在强制杀死容器之前需要等待的时间 (ns|us|ms|s|m|h) (default 10s)
			--stop-signal string                        停止容器的信号
			--sysctl list                               Sysctl选项
			-t, --tty                                   分配伪TTY
			--update-delay duration                     更新之间的延迟（ns | us | ms | s | m | h）(default 0s)
			--update-failure-action string              对更新失败采取的措施 ("pause"|"continue"|"rollback") (default "pause")
			--update-max-failure-ratio float            更新期间容错率 (default 0)
			--update-monitor duration                   更新每个任务以监视失败后的持续时间 (ns|us|ms|s|m|h) (default 5s)
			--update-order string                       更新订单 ("start-first"|"stop-first") (default "stop-first")
			--update-parallelism uint                   同时更新的最大任务数 (0 to update all at once) (default 1)
			-u, --user string                           用户名或UID（格式：<名称| uid> [：<group | gid>]）
			--with-registry-auth                        将注册表身份验证详细信息发送给群集代理
			-w, --workdir string                        容器内的工作目录

	inspect     显示一项或多项服务的详细信息
		用法:	docker service inspect [OPTIONS] SERVICE [SERVICE...]
		Options:
			-f, --format string                         使用给定的Go模板格式化输出
			--pretty                                    以人类友好的格式打印信息

	logs        提取服务或任务的日志
		用法:	docker service logs [OPTIONS] SERVICE|TASK
		Options:
			--details                                   显示提供给日志的其他详细信息
			-f, --follow                                跟踪日志输出
			--no-resolve                                不要在输出中将ID映射到名称
			--no-task-ids                               在输出中不包括任务ID
			--no-trunc                                  不要截断输出
			--raw                                       不要整齐地格式化日志
			--since string                              显示自时间戳记以来的日志（例如2013-01-02T13：23：37）或相对时间（例如42m，持续42分钟）
			--tail string                               从日志末尾开始显示的行数 (default "all")
			-t, --timestamps                            显示时间戳

	ls          列出服务
		用法:	docker service ls [OPTIONS]
		Aliases:
			ls, list
		Options:
			-f, --filter filter                         根据提供的条件过滤输出
			--format string                             使用Go模板的精美打印服务
			-q, --quiet                                 仅显示ID

	ps          列出一项或多项服务的任务
		用法:	docker service ps [OPTIONS] SERVICE [SERVICE...]
		Options:
			-f, --filter filter                         根据提供的条件过滤输出
			--format string                             使用Go模板进行漂亮的打印任务
			--no-resolve                                不要将ID映射到名称
			--no-trunc                                  不要截断输出
			-q, --quiet                                 仅显示任务ID

	rm          删除一项或多项服务
		用法:	docker service rm SERVICE [SERVICE...]
		Aliases:
			rm, remove

	rollback    将更改还原到服务的配置
		用法:	docker service rollback [OPTIONS] SERVICE
		Options:
			-d, --detach                                立即退出，而不是等待服务融合
			-q, --quiet                                 禁止进度输出

	scale       扩展一个或多个复制服务
		用法:	docker service scale SERVICE=REPLICAS [SERVICE=REPLICAS...]
		Options:
			-d, --detach                                立即退出，而不是等待服务融合

	update     更新服务
	    用法:	docker service update [OPTIONS] SERVICE
        Options:
        	--args command                              服务命令参数
        	--config-add config                         在服务上添加或更新配置文件
        	--config-rm list                            删除配置文件
        	--constraint-add list                       添加或更新展示位置约束
        	--constraint-rm list                        删除约束
        	--container-label-add list                  添加或更新容器标签
        	--container-label-rm list                   通过其键删除容器标签
        	--credential-spec credential-spec           托管服务帐户的凭据规范 (Windows only)
        	-d, --detach                                立即退出，而不是等待服务融合
			--dns-add list                              设置自定义DNS服务器
			--dns-option-add list                       添加或更新DNS选项
			--dns-option-rm list                        删除DNS选项
			--dns-rm list                               删除自定义DNS服务器
			--dns-search-add list                       添加或更新自定义DNS搜索域
			--dns-search-rm list                        删除DNS搜索域
			--endpoint-mode string               　　　 Endpoint 模式 (vip or dnsrr)
			--entrypoint command                        覆盖镜像的默认ENTRYPOINT
			--env-add list                              添加或更新环境变量
			--env-rm list                               删除环境变量
			--force                                     即使没有要求也强制更新
			--generic-resource-add list                 添加通用资源
			--generic-resource-rm list                  删除通用资源
			--group-add list                            向容器添加其他补充用户组
			--group-rm list                             从容器中删除以前添加的补充用户组
			--health-cmd string                         运行命令以检查运行状况
			--health-interval duration                  运行检查之间的时间(ms|s|m|h)
			--health-retries int                        需要连续报告不健康状况
			--health-start-period duration              容器初始化的开始时间，然后计算对不稳定的重试次数 (ms|s|m|h)
			--health-timeout duration                   允许一次检查运行的最长时间 (ms|s|m|h)
			--host-add list                      　　　 添加自定义主机到IP的映射(host:ip)
			--host-rm list                              删除自定义主机到IP的映射(host:ip)
			--hostname string                           容器主机名
			--image string                              服务镜像标签
			--init                                      在每个服务容器中使用init来转发信号并获取进程
			--isolation string                          服务容器隔离模式
			--label-add list                            添加或更新服务标签
			--label-rm list                             通过其键删除标签
			--limit-cpu decimal                         限制CPU
			--limit-memory bytes                        限制内存
			--log-driver string                         记录驱动程序以进行服务
			--log-opt list                              记录驱动程序选项
			--mount-add mount                           添加或更新服务上的挂载
			--mount-rm list                             通过目标路径删除挂载
			--network-add network                       添加网络
			--network-rm list                           删除网络
			--no-healthcheck                            禁用任何容器指定的健康检查
			--no-resolve-image                          不要查询仓库来解析镜像摘要和支持的平台
			--placement-pref-add pref                   添加展示位置偏好设置
			--placement-pref-rm pref                    删除展示位置偏好设置
			--publish-add port                          添加或更新已发布的端口
			--publish-rm port                           通过目标端口删除已发布的端口
        	-q, --quiet                                 禁止进度输出
			--read-only                                 将容器的根文件系统挂载为只读
			--replicas uint                             任务数
			--replicas-max-per-node uint                每个节点的最大任务数(default 0 = unlimited)
			--reserve-cpu decimal                       备用CPU
			--reserve-memory bytes                      备用内存
			--restart-condition string                  满足条件时重启("none"|"on-failure"|"any")
			--restart-delay duration                    重新启动尝试之间的延迟 (ns|us|ms|s|m|h)
			--restart-max-attempts uint                 放弃之前的最大重新启动次数
			--restart-window duration                   用于评估重启策略的窗口 (ns|us|ms|s|m|h)
			--rollback                                  回滚到以前的规范
			--rollback-delay duration                   任务回滚之间的延迟 (ns|us|ms|s|m|h)
			--rollback-failure-action string            对回滚失败采取的措施("pause"|"continue")
			--rollback-max-failure-ratio float          回滚期间可以容忍的故障率
			--rollback-monitor duration                 每个任务回滚后监视失败的持续时间(ns|us|ms|s|m|h)
			--rollback-order string                     回滚顺序 ("start-first"|"stop-first")
			--rollback-parallelism uint                 同时回滚的最大任务数(0 to roll back all at once)
			--secret-add secret                         在服务上添加或更新机密
			--secret-rm list                            删除秘密
			--stop-grace-period duration                在强制杀死容器之前需要等待的时间(ns|us|ms|s|m|h)
			--stop-signal string                        停止容器的信号
			--sysctl-add list                           添加或更新Sysctl选项
			--sysctl-rm list                            删除Sysctl选项
        	-t, --tty                                   分配伪TTY
			--update-delay duration                     更新之间的延迟(ns|us|ms|s|m|h)
			--update-failure-action string              对更新失败采取的措施 ("pause"|"continue"|"rollback")
			--update-max-failure-ratio float            更新期间可容忍的故障率
			--update-monitor duration                   更新每个任务以监视失败后的持续时间 (ns|us|ms|s|m|h)
			--update-order string                       更新顺序("start-first"|"stop-first")
			--update-parallelism uint                   同时更新的最大任务数 (0 to update all at once)
        	-u, --user string                           用户名或UID（格式：<名称| uid> [：<group | gid>]）
			--with-registry-auth                        将注册身份验证详细信息发送给群集代理
        	-w, --workdir string                        容器内的工作目录

Run 'docker service COMMAND --help' for more information on a command.
```

###   stack       管理Docker堆栈
用法:	docker stack [OPTIONS] COMMAND
```
Options:
		  --orchestrator string                         协调器使用 (swarm|kubernetes|all)
Commands:
	deploy      部署新堆栈或更新现有堆栈
		用法:	docker stack deploy [OPTIONS] STACK
		Aliases:
			deploy, up
		Options:
			--bundle-file string                        分布式应用程序捆绑包文件的路径
			-c, --compose-file strings                  组成文件的路径，或从标准输入中读取的“-”
			--orchestrator string                       协调器使用 (swarm|kubernetes|all)
			--prune                                     不再引用的修剪服务
			--resolve-image string                      查询仓库以解决镜像摘要和支持的平台("always"|"changed"|"never") (default "always")
			--with-registry-auth                        将注册身份验证详细信息发送到Swarm代理

	ls          列出堆栈
		用法:	docker stack ls [OPTIONS]
		Aliases:
			ls, list
		Options:
			--format string                             使用Go模板的漂亮打印纸叠
			--orchestrator string                       协调器使用 (swarm|kubernetes|all)

	ps          列出堆栈中的任务
		用法:	docker stack ps [OPTIONS] STACK
		Options:
			-f, --filter filter                         根据提供的条件过滤输出
			--format string                             使用Go模板进行漂亮的打印任务
			--no-resolve                                不要将ID映射到名称
			--no-trunc                                  不要截断输出
			--orchestrator string                       协调器使用(swarm|kubernetes|all)
			-q, --quiet                                 仅显示任务ID

	rm          移除一个或多个堆栈
		用法:	docker stack rm [OPTIONS] STACK [STACK...]
		Aliases:
			rm, remove, down
		Options:
			--orchestrator string                       协调器使用 (swarm|kubernetes|all)

	services    列出堆栈中的服务
		用法:	docker stack services [OPTIONS] STACK
		Options:
			-f, --filter filter                         根据提供的条件过滤输出
			--format string                             使用Go模板的精美打印服务
			--orchestrator string                       协调器使用(swarm|kubernetes|all)
			-q, --quiet                                 仅显示任务ID

Run 'docker stack COMMAND --help' for more information on a command.
```

###   swarm       管理 Swarm
用法:	docker swarm COMMAND
```
Commands:
	ca          显示并旋转root CA
		用法:	docker swarm ca [OPTIONS]
		Options:
			--ca-cert pem-file                          用于新群集的PEM格式的根CA证书的路径
			--ca-key pem-file                           用于新集群的PEM格式的根CA密钥的路径
			--cert-expiry duration                      节点证书的有效期(ns|us|ms|s|m|h) (default 2160h0m0s)
			-d, --detach                                立即退出，而不是等待根旋转收敛
			--external-ca external-ca                   一个或多个证书签名端点的规范
			-q, --quiet                                 禁止进度输出
			--rotate                                    旋转群集CA-如果未提供证书或密钥，则将生成新的证书或密钥

	init        初始化 swarm
		用法:	docker swarm init [OPTIONS]
		Options:
		    --advertise-addr string                     广告地址 (format: <ip|interface>[:port])
		    --autolock                                  启用管理员自动锁定（需要解锁密钥才能启动停止的管理员）
		    --availability string                       节点的可用性 ("active"|"pause"|"drain") (default "active")
		    --cert-expiry duration                      节点证书的有效期 (ns|us|ms|s|m|h) (default 2160h0m0s)
		    --data-path-addr string                     用于数据路径通信的地址或接口 (format: <ip|interface>)
		    --data-path-port uint32                     用于数据路径通信的端口号（1024-49151）。 如果未设置任何值或将其设置为0，则使用默认端口（4789）。
		    --default-addr-pool ipNetSlice              CIDR格式的默认地址池(default [])
		    --default-addr-pool-mask-length uint32      默认地址池子网掩码长度 (default 24)
		    --dispatcher-heartbeat duration             调度程序心跳期 (ns|us|ms|s|m|h) (default 5s)
		    --external-ca external-ca                   一个或多个证书签名端点的规范
		    --force-new-cluster                         从当前状态强制创建新集群
		    --listen-addr node-addr                     监听地址 (format: <ip|interface>[:port]) (default 0.0.0.0:2377)
		    --max-snapshots uint                        要保留的其他Raft快照数量
		    --snapshot-interval uint                    Raft快照之间的日志条目数 (default 10000)
		    --task-history-limit int                    任务历史记录保留限制(default 5)

	join        加入swarm作为节点和/或管理者
		用法:	docker swarm join [OPTIONS] HOST:PORT
		Options:
			--advertise-addr string                     广告地址 (format: <ip|interface>[:port])
			--availability string                       节点的可用性("active"|"pause"|"drain") (default "active")
			--data-path-addr string                     用于数据路径通信的地址或接口 (format: <ip|interface>)
			--listen-addr node-addr                     监听地址 (format: <ip|interface>[:port]) (default 0.0.0.0:2377)
			--token string                              进入swarm的令牌

	join-token  管理加入令牌
		用法:	docker swarm join-token [OPTIONS] (worker|manager)
		Options:
			-q, --quiet                                 仅显示令牌
			--rotate                                    旋转加入令牌

	leave       离开swarm
		用法:	docker swarm leave [OPTIONS]
		Options:
			-f, --force                                 强制该节点离开群，忽略警告

	unlock      解锁 swarm
		用法:	docker swarm unlock

	unlock-key  管理解锁密钥
		用法:	docker swarm unlock-key [OPTIONS]
		Options:
			-q, --quiet                                 仅显示令牌键
			--rotate                                    旋转解锁键

	update     更新 swarm
		用法:	docker swarm update [OPTIONS]
		Options:
			--autolock                                  变更管理员自动锁定设定(true|false)
			--cert-expiry duration                      节点证书的有效期 (ns|us|ms|s|m|h) (default 2160h0m0s)
			--dispatcher-heartbeat duration             调度程序心跳期 (ns|us|ms|s|m|h) (default 5s)
			--external-ca external-ca                   一个或多个证书签名端点的规范
			--max-snapshots uint                        要保留的其他Raft快照数量
			--snapshot-interval uint                    Raft快照之间的日志条目数 (default 10000)
			--task-history-limit int                    任务历史记录保留限制(default 5)

Run 'docker swarm COMMAND --help' for more information on a command.
```

###   system      管理 Docker
用法:	docker system COMMAND
```
Commands:
	df                                                  显示Docker磁盘使用情况
		用法:	docker system df [OPTIONS]
		Options:
			--format string                             使用Go模板打印漂亮的图像
			-v, --verbose                               显示有关空间使用情况的详细信息

	events      从服务器获取实时事件
		用法:	docker system events [OPTIONS]
		Options:
			-f, --filter filter                         根据提供的条件过滤输出
			--format string                             使用给定的Go模板格式化输出
			--since string                              显示自时间戳记以来创建的所有事件
			--until string                              流事件直到该时间戳

	info        显示系统范围的信息
		用法:	docker system info [OPTIONS]
		Options:
			-f, --format string                         使用给定的Go模板格式化输出

	prune       删除未使用的数据
		用法:	docker system prune [OPTIONS]
		Options:
			-a, --all                                   删除所有未使用的镜像，而不仅仅是悬空的镜像
			--filter filter                             提供过滤器值（例如'label = <key> = <value>'）
			-f, --force                                 不提示确认
			--volumes                                   修剪卷

Run 'docker system COMMAND --help' for more information on a command.
```

###   trust       管理对Docker镜像的信任
用法:	docker trust COMMAND
```
Management Commands:
	key   管理用于签署Docker镜像的密钥
		用法:	docker trust key COMMAND
		Commands:
			generate    生成并加载签名密钥对
				用法:	docker trust key generate NAME
				Options:
					--dir string                        要生成键入内容的目录，默认为当前目录

			load    加载用于签名的私钥文件
				用法:	docker trust key load [OPTIONS] KEYFILE
				Options:
					--name string                       加载密钥的名称 (default "signer")

		Run 'docker trust key COMMAND --help' for more information on a command.

	signer    管理可以签署Docker镜像的实体
		用法:	docker trust signer COMMAND
		Commands:
			add                                         添加签名者
				用法:	docker trust signer add OPTIONS NAME REPOSITORY [REPOSITORY...] 
				Options:
					--key list                          签名者公钥文件的路径

			remove   删除签名者
				用法:	docker trust signer remove [OPTIONS] NAME REPOSITORY [REPOSITORY...]
				Options:
					-f, --force                         在删除最新签名者之前，请勿提示您进行确认

		Run 'docker trust signer COMMAND --help' for more information on a command.

Commands:
	inspect    返回有关密钥和签名的低级信息
		用法:	docker trust inspect IMAGE[:TAG] [IMAGE[:TAG]...]
		Options:
			--pretty                                    以人类友好的格式打印信息

	revoke    解除对镜像的信任
		用法:	docker trust revoke [OPTIONS] IMAGE[:TAG]
		Options:
			-y, --yes                                   不提示确认

	sign   签名镜像
		用法:	docker trust sign IMAGE:TAG
		Options:
					--local                             签署本地标记的镜像

Run 'docker trust COMMAND --help' for more information on a command.
```

###   volume     管理数据卷
用法:	docker volume COMMAND
```
Commands:
	create   创建一个数据卷
		用法:	docker volume create [OPTIONS] [VOLUME]
		Options:
			-d, --driver string                         指定数据卷驱动程序名称 (default "local")
			--label list                                设置数据卷的元数据
			-o, --opt map                               设置驱动程序特定选项(default map[])

	inspect     显示一个或多个卷上的详细信息
		用法:	docker volume inspect [OPTIONS] VOLUME [VOLUME...]
		Options:
			-f, --format string                         使用给定的Go模板格式化输出

	ls          列出数据卷
		用法:	docker volume ls [OPTIONS]
		Aliases:
			ls, list
		Options:
			-f, --filter filter                         提供过滤器值（例如'dangling = true'）
			--format string                             使用Go模板打印漂亮的卷
			-q, --quiet                                 仅显示数据卷名

	prune       删除所有未使用的本地数据卷
		用法:	docker volume prune [OPTIONS]
		Options:
			--filter filter                             提供过滤器值（例如'label = <label>'）
			-f, --force                                 不提示确认

	rm          删除一个或多个数据卷
		用法:	docker volume rm [OPTIONS] VOLUME [VOLUME...]
		删除一个或多个卷。 您不能删除容器正在使用的卷。
		Aliases:
			rm, remove
		Examples:
		$ docker volume rm hello
		hello
		Options:
			-f, --force                                 强制删除一个或多个卷

Run 'docker volume COMMAND --help' for more information on a command.
```