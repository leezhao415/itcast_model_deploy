# Docker 快速入手
---

在这一节，我们通过一个小实验来快速入门 Docker 的使用方法。大致的步骤如下:

0. 查看 Docker 服务运行状态;
1. 查看本地镜像;
2. 从 Docker Hub 拉取基础镜像, 我们此处选择 ubuntu:18.04 镜像;
3. 再次查看本地镜像;
4. 使用 ubuntu:18.04 镜像构建容器，并交互式运行容器；
5. 在容器内部执行 LS 命令;
6. 退出容器;
7. 查看本地容器实例;
8. 再次启动停止的容器;
9. 退出并停止容器.


执行命令如下:

```python
# 0. 查看 Docker 服务运行状态;
systemctl status docker

# 1. 查看本地镜像;
docker images
"""
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
"""

# 2. 从 Docker Hub 拉取基础镜像, 我们此处选择 ubuntu 镜像;
docker search ubuntu
docker search ubuntu --no-trunc
docker pull ubuntu
"""
Using default tag: latest
latest: Pulling from library/ubuntu
405f018f9d1d: Pull complete
Digest: sha256:b6b83d3c331794420340093eb706a6f152d9c1fa51b262d9bf34594887c2c7ac
Status: Downloaded newer image for ubuntu:latest
docker.io/library/ubuntu:latest
"""

# 3. 再次查看本地镜像;
docker images
docker image ls
"""
REPOSITORY   TAG       IMAGE ID       CREATED      SIZE
ubuntu       latest    27941809078c   9 days ago   77.8MB
"""

# 4. 使用 ubuntu 镜像构建容器，并交互式运行容器，并在容器中执行 LS 命令；
docker run -it ubuntu:latest /bin/bash
"""
root@abcced6d5ee8:/# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
"""

# 5. 退出容器;
exit
"""
exit
[root@bogon docker]#
"""

# 6. 查看本地容器实例;
docker ps
docker ps -a
"""
CONTAINER ID   IMAGE           COMMAND       CREATED              STATUS                      PORTS     NAMES
abcced6d5ee8   ubuntu:latest   "/bin/bash"   About a minute ago   Exited (0) 35 seconds ago             sad_montalcini
"""

# 7. 再次启动停止的容器;
docker start 容器ID
"""
[root@bogon docker]# docker start abcced6d5ee8
abcced6d5ee8
"""

# 8. 再次进入容器
docker exec -it abcced6d5ee8 /bin/bash
"""
root@abcced6d5ee8:/# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
"""

# 9. 退出容器, 并停止容器
exit
docker stop 容器ID
"""
[root@bogon docker]# docker ps
CONTAINER ID   IMAGE           COMMAND       CREATED         STATUS         PORTS     NAMES
abcced6d5ee8   ubuntu:latest   "/bin/bash"   9 minutes ago   Up 6 minutes             sad_montalcini
[root@bogon docker]# docker stop abcced6d5ee8
abcced6d5ee8
[root@bogon docker]# docker exec -it abcced6d5ee8 /bin/bash
Error response from daemon: Container abcced6d5ee8e6980f8271d3e78d18d0dff708e377c22c7798006a133ac73559 is not running
"""
```



