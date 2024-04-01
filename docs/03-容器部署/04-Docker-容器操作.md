# Docker 容器操作
---

容器操作主要包括:

1. 查看容器
2. 启动容器
3. 停止容器
4. 删除容器
5. 进入容器
6. 容器导出
7. 容器导入


```python
# 1. 查看容器
docker ps       # 查看正在运行的容器实例
docker ps -a    # 查看正在运行或者已停止的容器实例

# 2. 运行容器
docker start 容器ID     # 启动容器
docker restart 容器ID   # 重启容器

# 3. 停止容器
docker stop 容器ID

# 4. 删除容器
docker rm -f 容器ID              # 删除指定容器
docker rm -f $(docker ps -qa)   # 删除所有容器

# 5. 进入容器
# attach 退出终端会导致容器停止
# exec 不会导致容器停止
docker attach 容器ID
docker exec -it 容器ID /bin/bash

# 6. 容器导出
docker export 容器ID > xxx.tar

# 7. 容器导入
docker import xxx.tar xxx:tag
```