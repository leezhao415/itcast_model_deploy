# Gradio 应用分享

## 1. 互联网分享

如果运行环境能够连接互联网，在launch函数中设置share参数为True，那么运行程序后。Gradio的服务器会提供XXXXX.gradio.app地址。通过其他设备，比如手机或者笔记本电脑，都可以访问该应用。这种方式下该链接只是本地服务器的代理，不会存储通过本地应用程序发送的任何数据。这个链接在有效期内是免费的，好处就是不需要自己搭建服务器，坏处就是太慢了，毕竟数据经过别人的服务器。

```python
demo.launch(share=True)
```

## 2. Huggingface托管

为了便于向合作伙伴永久展示我们的模型App,可以将gradio的模型部署到 HuggingFace的 Space托管空间中，完全免费的哦。

方法如下：

1，注册huggingface账号：[https://huggingface.co/join](https://link.zhihu.com/?target=https%3A//huggingface.co/join)

2，在space空间中创建项目：[https://huggingface.co/spaces](https://link.zhihu.com/?target=https%3A//huggingface.co/spaces)

3，创建好的项目有一个Readme文档，可以根据说明操作，也可以手工编辑app.py和requirements.txt文件。

## 3. 局域网分享

通过设置server_name=‘0.0.0.0’（表示使用本机ip）,server_port（可不改，默认值是7860）。那么可以通过本机ip:端口号在局域网内分享应用。

```python
# show_error为True表示在控制台显示错误信息。
demo.launch(server_name='0.0.0.0', server_port=8080, show_error=True)
```

这里host地址可以自行在电脑查询，**C:\Windows\System32\drivers\etc\hosts** 修改一下即可 127.0.0.1再制定端口号

## 4. 密码验证

在首次打开网页前，可以设置账户密码。比如auth参数为（账户，密码）的元组数据。这种模式下不能够使用queue函数。

```python
demo.launch(auth=("admin", "pass1234"))
```

如果想设置更为复杂的账户密码和密码提示，可以通过函数设置校验规则。

```python
#账户和密码相同就可以通过
def same_auth(username, password):
    return username == password

demo.launch(auth=same_auth,auth_message="username and password must be the same")
```
