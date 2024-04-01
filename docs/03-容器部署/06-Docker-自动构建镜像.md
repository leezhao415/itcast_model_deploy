# Docker 自动构建镜像
---

我们也可以使用 Dockerfile 构建镜像，Dockerfile 其实就是把我们前面的一系列安装、配置命令写到一个文件中，通过 docker build 命令，一键完成镜像的构建。接下来，我们以 python3.7.5 作为基础镜像，来构建我们自己的镜像。


## 1. Dockfile 编写

```python
# 继承的基础镜像
FROM python:3.7.5
MAINTAINER "wechat:chinesecpp, email:chinacpp@hotmail.com"

# 安装 app 需要的 Python 包
RUN pip install pandas flask scikit-learn jieba zhconv -i https://pypi.tuna.tsinghua.edu.cn/simple

# 设置工作目录
WORKDIR /root/app

# COPY 命令使用的是相对路径
COPY app/ /root/app

# 显式声明容器服务监听的端口
EXPOSE 5000

# 当启动容器时默认执行的命令
CMD ["python", "app.py"]
```

接下来，使用下面命令构建 Docker 镜像：

```python
docker build -t spam:1.0 .
```


## 2. 镜像使用


镜像构建完成之后，启动镜像创建容器实例：

```python
docker run -d -p 8000:5000 spam:1.0
```

持久化本地存储镜像:

```python
docker save spam:1.0 -o spam.tar
```
