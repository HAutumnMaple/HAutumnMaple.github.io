+++
title = "学习工作常用命令"
date = "2026-02-06T18:48:11+08:00"

categories = ["学习"]
tags = ["工作", "常用命令"]

+++

## Hugo博客常用命令

```hugo
hugo help								# help栏
hugo server	                			# 无草稿本地预览
hugo server -D							# 全部内容本地预览
hugo new posts/xxx.md					# 在post中创建一篇新博客
rm ./content/posts/my-first-post.md		# 删除一篇博客
hugo new xxx.md							# 创建一个新页面
```

配备多台电脑的同步命令：

```git
git clone https://github.com/HAutumnMaple/HAutumnMaple.github.io.git
cd HAutumnMaple.github.io
git submodule update --init --recursive
hugo server -D
```

## Git常用命令

```git
git config --global user.name "xxx" 	# 设置用户名
git config --global user.email "xxx"	# 设置邮箱
git config --list						# 查看配置
git clone xxx							# 拉取项目到本地
git status								# 查看仓库状态
git add xxx								# 添加文件到暂存区，. 即是全部文件
git commit -m "xxx"						# 提交本次提交描述信息
git remote -v							# 查看远程仓库
git remote add origin xxx				# 添加远程仓库
git pull origin xxx						# 拉取远程更新
git push origin xxx						# 推送本地提交
git log									# 查看提交历史，加上--oneline就是简洁显示，--graph就是图形化显示
git diff								# 查看工作区和暂存区的差别，加上--stage就是看和最后一次提交的差别
git branch								# 查看分支，加上-a就是加上远程分支
```

## Linux常用命令

```linux
htop									# 看内存和进程相关信息
watch -n 1 --color gpustat --color 		# 监视GPU运行情况
nvidia-smi								# 看GPU情况
ps aux									# 查看现有进程
kill pid								# 温和杀进程
kill -9 pid								# 强制杀进程
ls -l xxx								# 查看详细权限
chmod xxx xxx.txt						# 修改文件权限，常用权限为777（全局开放读写执行）
chmod -R xxx xxx						# 递归修改目录权限
chown user xxx.txt						# 修改文件所有者
df -h									# 看全局存储
du -sh xxx								# 看某个文件的占用存储
iostat -dx 1							# 看磁盘整体IO
iotop -o								# 实时显示哪个进程在做IO
```

## Docker常用命令

```docker
docker build -t xxx:tag .				# 根据当下文件夹的Dockerfile文件构建镜像
docker images							# 查看docker镜像
docker ps								# 查看现存容器，加上-a可以看关闭的容器
docker exec -it xxx bash				# 使用bash的方式打开容器
docker start xxx						# 启动停止的某个容器
docker stop xxx							# 停止某个启动的容器
docker restart xxx						# 重启某个容器
docker rm xxx							# 删除某个已经停止的容器
docker rmi xxx:tag						# 删除某个镜像
docker run -d \							# 创建并运行某个容器
  --name <container_name> \
  -p <host_port>:<container_port> \
  -v <host_path>:<container_path> \
  -e KEY=VALUE \
  <image>:<tag>
docker logs xxx							# 查看某个容器的日志
docker inspect xxx						# 查看某个容器的详细信息
docker push xxx:tag						# 推送镜像
docker pull xxx:tag						# 拉取镜像
docker save -o xxx.tag xxx:tag			# 导出镜像
```

