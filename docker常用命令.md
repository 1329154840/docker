# docker内部常用命令
## 1.查看容器进程：
```
docker ps		     运行中的进程
docker ps -a                 所有进程
docker ps -f status=exited   暂停的进行,created|restarting|running|paused|exited
docker ps -l                 最后一次运行的进程
```
```
-a, --all       显示所有容器(默认显示正在运行)

-n              显示最后创建的n个容器(包括所有状态)(默认值-1)
                示例：docker ps -n2

-l,--latest     显示最新创建的容器(包括所有状态)

-q, --quiet     只显示数字id    

-s, --size      显示总文件大小

--no-trunc      不截断输出

-f, --filter    根据提供的条件过滤输出
                过滤条件如下：
                Filter | Description
                ---|---
                id      | 容器的ID
                name    | 容器的Name
                label   | 表示键或键值对的任意字符串。表示为<key>或<key>=<value>
                exited  | 表示容器退出代码的整数。只有对所有人有用。
                status  | created,restarting,running,removing,paused,exited,dead之一
                ancestor| 筛选指定镜像的容器，例如<image-name>[:<tag>],<image id>, or <image@digest>
                before or since | 筛选在给定容器ID或名称之前或之后创建的容器
                volume  | 运行已挂载给定卷或绑定挂载的容器的筛选器。
                network | 过滤器运行连接到给定网络的容器。
                publish or expose | 筛选发布或公开给定端口的容器,例如<port>[/<proto>] or <startport-endport>/[<proto>]
                health  | 根据容器的健康检查状态过滤容器，例如starting, healthy, unhealthy or none.
                isolation | 仅Windows守护进程，例如default, process, or hyperv.
                is-task | 筛选服务的“任务”容器。布尔选项（true or false）

                示例：
                docker ps -f name=^'modality'
                docker ps --filter name=nginx
                docker ps -a --filter exited=0
                docker ps --filter status=running
                docker ps --filter expose=3306
    --format    使用Go模板漂亮地打印容器
                过滤条件如下：
                Placeholder | Description
                ---|---
                .ID         | 容器的ID
                .Image      | 镜像的ID
                .Command    | 引用命令
                .CreatedAt  | 创建容器的时间
                .RunningFor | 自容器启动以来的运行时长
                .Ports      | 暴露的端口
                .Status     | 容器状态
                .Size       | 容器的磁盘大小
                .Names      | 容器的名称
                .Labels     | 分配给容器的所有标签
                .Label      | 此容器的特定标签的值,例如`{{.Label "com.docker.swarm.cpu"}}`
                .Mounts     | 容器挂载的卷
                .Networks   | 容器所用的网络名称

                示例：
                docker ps --format "{{.ID}}: {{.Names}}: {{.Command}}"
```
## 2.运行暂停的容器: 
`docker start [容器id(前4位就够了) 或者 name]`
## 3.暂停: 
`docker stop [容器id(前4位就够了) 或者 name]`
## 4.查看容器日志: 
 `docker docker logs [容器id(前4位就够了) 或者 name]`
## 5.删除对应进程中的容器: 
`先stop 然后 rm [容器id(前4位就够了) 或者 name]`   
  注意这里只是删除进程不是移除对应镜像,rm后ps -a就没了
## 6.查看镜像
`docker images`
## 7.删除镜像： 
`docker rmi [镜像 或者 仓库名]`
## 8.搜索远端的镜像(私有还是公共看配置)
`docker search`
## 9.进入对应容器里的bash: 
`docker exec -it [容器id(前4位就够了) 或者 name] /bin/bash`  
推荐使用，这是守护式进入，剩下3种，attach多窗口单个阻塞其他也被阻塞了，SSH不合适，nsenter通过PID访问
## 10.运行新的images: 
`docker run -i -d -p -v -t -e --name [容器id(前4位就够了) 或者 name]`
```
-i       运行容器 
-d       后台运行并启用守护 
-p       端口映射[当前pc端口]:[容器内部端口] 
-v       目录挂载映射(多级路径权限报错加上 --privileged=true) 
-t       启动后附加的执行命令 
-e       环境变量配置 
--name   是进程容器名称，具体还有别的参数,i和d可以一块写
```
## 11.容器文件拷贝
`docker cp [本机路径] [容器id:对弈路径]`  
  本机文件发送到容器中  
`docker cp [容器id:对弈路径] [本机路径]`  
  接收容器文件到本机
## 12.容器或镜像元数据(可查ip）
`docker inspect [容器id(前4位就够了) 或者 name]`  
  --format 输出对应json value并进行格式化输出，例如 --format='IP: {{.NetworkSettings.IPAddress}}'
 ```
 {{.NetworkSettings.IPAddress}}   获取容器的IP
 {{.Name}}                        获取容器Name
 {{.Config.Hostname}}             获取容器Hostname
 ``` 
## 13.镜像拉取
`docker pull [镜像id 或者 仓库名]`
## 14.将自己当前有的容器制作为镜像
`docker commit [容器id(前4位就够了) 或者 name] [要生成的镜像名称]`
## 15.将镜像存在保存本地为.tar
`docker save -o [本地路径以及文件名为.tar结尾] [镜像id 或者 仓库名] `
## 16.将保存本地为.tar加载进镜像
`docker load -i [本地路径以及文件名为.tar结尾]`
# 对docker外部的命令（systemctl 在Centos7自带）
```
systemctl start docker		启动 docker
systemctl status docker		查看 docker 状态
systemctl stop docker		停止 docker
systemctl enable docker		开机自启
docker info 			查看 docker 概要信息
docker --help			查看 docker 帮助文档
```
# 对docker容器时区修改
```
docker exec -i -t [CONTAINNER] /bin/bash                进入容器
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime 修改时间方法1
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime     修改时间方法2
date -R                                                 检查时间
```
