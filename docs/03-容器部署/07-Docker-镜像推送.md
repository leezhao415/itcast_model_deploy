# Docker 镜像推送

## 将Docker镜像推送到Docker Hub

推送 Docker 镜像到 Docker Hub 需要以下几步：

**创建 Docker Hub 账户**：访问 [Docker Hub](https://hub.docker.com/)，然后按照提示创建一个新账户。如果你已经有一个账户，可以跳过这一步。

**在 Docker Hub 上创建新的仓库**：登录你的 Docker Hub 账户，然后在 Dashboard 中选择 "Create Repository"。在创建仓库的界面中，输入你的仓库名（例如 "my-application"），然后选择 "Create"。

**在本地登录 Docker**：在你的命令行中输入 `docker login`，然后按照提示输入你的 Docker Hub 的用户名和密码。

**标记你的 Docker 镜像**：在你推送镜像之前，你需要给你的镜像添加一个标签，这个标签应该与你的 Docker Hub 仓库的地址相匹配。你可以使用 `docker tag` 命令来给你的镜像添加标签。例如，如果你的 Docker Hub 用户名是 "myusername"，你的仓库名是 "my-application"，你的镜像名是 "my-application"，你可以运行以下命令来添加标签：

```docker
docker tag my-application myusername/my-application
```

在这个命令中，`my-application` 是你本地的镜像名，`myusername/my-application` 是你的 Docker Hub 仓库地址。

**推送你的 Docker 镜像**：最后，你可以使用 `docker push` 命令来推送你的镜像到 Docker Hub。例如：

```text
docker push myusername/my-application
```

这个命令会将你的 "my-application" 镜像推送到你的 Docker Hub 仓库。

完成这些步骤后，你就可以在 Docker Hub 上看到你的 Docker 镜像了，你也可以在任何有 Docker 的机器上使用 `docker pull` 命令来下载和运行你的镜像。