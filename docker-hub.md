# Docker hub

[TOC]

## 注册Docker Hub账号

构建系统很重要的一环就是共享和发布，因此可以先建立一个Docker Hub账号，然后就可以使用Docker login命令登录了。

个人的信息都会保存在~/.dockercfg文件中。

## 本机登录

在 Docker Hub 网站可以登录，为了将本地自己构建的镜像推送到 Docker hub，需要在本地登录。

```
docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: yaowenguo
Password:
Login Succeeded
```

个人信息会保存到 `$HOME/.dockercfg` 文件中。

## 将本地镜像推送到 Hub 仓库

docker tag local-image:tagname new-repo:tagname
docker push new-repo:tagname

```
sudo docker push <docker_ic/image_name>
```
docker_ic 即 docker hub 用户 id，是必须添加的，否则 docker 会将镜像推雄到顶级的 root 仓库，而 root 仓库是由 Docker 公司团队管理的，因此推送无法成功。

image_name 即在 DockerHub 创建一个 repository 本地镜像名必须和仓库名相同。如果不同可以使用 tag 命令修改

```
docker tag webrtc:0.1 albertguo88/webrtc_image:latest
```
然后即可推送
```
docker  push albertguo88/webrtc_image:latest # 如果 tag 是 latest 可以省略
```




## 自动构建

除了从命令构建，然后推送到 Docker Hub。 Docker hub 还允许自定义构建。只需要将包含 dockerfile 文件的 GitHub 或 BitBucket 仓库连接到 Docker hub 即可。然后在向 GitHub 或 BitBucket 推送代码时，就会触发 Docker hub 的一次镜像构建，创建一个新的镜像。

## 删除镜像
本地而已
```
docker rm <repository_name/image_name> [....]
```
