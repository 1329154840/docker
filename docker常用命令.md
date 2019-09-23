# docker内部常用命令
## 查看容器进程：
```
docker ps		     运行中的进程
docker ps -a                 所有进程
docker ps -f status=exited   暂停的进行,status有up、exited
docker ps -l                 最后一次运行的进程
```
## 运行: 
`docker run [容器id(前4位就够了) 或者 name]`
## 暂停: 
`docker stop [容器id(前4位就够了) 或者 name]`
## 查看容器日志: 
 `docker docker logs [容器id(前4位就够了) 或者 name]`
## 删除对应进程中的容器: 
`先stop 然后 rm [容器id(前4位就够了) 或者 name]`   
  注意这里只是删除进程不是移除对应镜像,rm后ps -a就没了
## 查看镜像
`docker images`
## 删除镜像： 
`docker rmi [镜像 或者 仓库名]`
## 搜索远端的镜像(私有还是公共看配置)
`docker search`
## 进入对应容器里的bash: 
`docker exec -it [容器id(前4位就够了) 或者 name] /bin/bash`   
  注意这里有4种进入方式,这是守护式进入
## 运行新的images: 
`docker run -i -d -p -v -t -e --name [容器id]`
```
-i       运行容器 
-d       后台运行并启用守护 
-p       端口映射[当前pc端口]:[容器内部端口] 
-v       目录挂载映射(多级路径权限报错加上 --privileged=true) 
-t       启动后附加的执行命令 
-e       环境变量配置 
--name   是进程容器名称，具体还有别的参数,i和d可以一块写
```
## 容器文件拷贝
`docker cp [本机路径] [容器id:对弈路径]`  
  本机文件发送到容器中
`docker cp [容器id:对弈路径] [本机路径]`  
  接收容器文件到本机
## 容器或镜像元数据(可查ip）
`docker inspect [容器id(前4位就够了) 或者 name]`  
  --format 输出对应json value，例如 --format='{{NetworkSetting。IPAddress}}'
## 镜像拉取
`docker pull [镜像id 或者 仓库名]`
## 将自己当前有的容器制作为镜像
`docker commit [容器id(前4位就够了) 或者 name] [要生成的镜像名称]`
# 对docker外部的命令
```
systemctl start docker		启动 docker
systemctl status docker		查看 docker 状态
systemctl stop docker		停止 docker
systemctl enable docker		开机自启
docker info 			查看 docker 概要信息
docker --help			查看 docker 帮助文档
```
