# Docker 镜像操作
---

镜像操作主要包括:

1. 查看镜像
2. 搜索镜像
3. 下载镜像
4. 运行镜像
5. 删除镜像
6. 保存镜像
7. 加载镜像


示例命令:

```python
# 1. 查看镜像
docker images             # 查看所有镜像
docker images -q          # 只查看镜像的ID
docker images --no-trunc  # 显示镜像完整信息

# 2. 搜索镜像
docker search 镜像名

# 3. 下载镜像
docker pull 镜像名:版本     # 不指定 TAG, 则默认使用 latest

# 4. 运行镜像
docker run -it 镜像名:版本 程序                # 交互式运行容器
docker run -it --name=标签名 镜像名:版本 程序   # 容器指定名字
docker run -itd 镜像名:版本 程序               # 后台运行容器

# 5. 删除镜像
docker rmi -f 镜像名                   # 删除指定镜像
docker rmi -f 镜像ID                   # 删除指定镜像
docker rmi -f $(docker images -qa)    # 删除所有镜像

# 6. 保存镜像
docker save 镜像名:版本 -o xxx.tar

"""
[root@bogon ~]# docker save alpine:latest -o myalpine.tar
[root@bogon ~]# ls
anaconda-ks.cfg  myalpine.tar
"""

# 7. 加载镜像
docker load -i xxx.tar

"""
[root@bogon ~]# docker load -i myalpine.tar
24302eb7d908: Loading layer [==================================================>]  5.811MB/5.811MB

REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
alpine       latest    e66264b98777   3 weeks ago    5.53MB
centos       latest    5d0da3dc9764   9 months ago   231MB
"""
```