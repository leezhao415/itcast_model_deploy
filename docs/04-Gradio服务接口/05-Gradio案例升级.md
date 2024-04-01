# Gradio 案例升级

## 1. 文本分类—垃圾邮件分类

```python
import gradio as gr
from sklearn.feature_extraction.text import CountVectorizer
import re
import zhconv
import jieba.posseg as psg
import pickle
import joblib


def clean_data(email):
    # 1.去除非中文字符
    email = re.sub(r'[^\u4e00-\u9fa5]', '', email)
    # 2.繁体转简体
    email = zhconv.convert(email, 'zh-cn')
    # 3.邮件词性筛选
    email_pos = psg.cut(email)
    allow_pos = ['n', 'nr', 'ns', 'nt', 'v', 'a']
    email = []
    for word, pos in email_pos:
        if pos in allow_pos:
            email.append(word)

    # 4.转换成 str 类型
    email = ' '.join(email)
    return email


def email_handle(text):
    # 1.对数据进行清理
    content = clean_data(text)
    # 2.数据特征提取
    vocab = pickle.load(open('03-模型训练特征.pkl', 'rb'))
    transfer = CountVectorizer(vocabulary=vocab)
    content = transfer.transform([content])
    # 3.模型加载
    model = joblib.load('04-邮件分类模型.pth')
    output = model.predict(content)
    prediction = output[0]
    prediction = '垃圾邮件' if prediction == 'spam' else '正常邮件'
    return prediction


demo = gr.Interface(fn=email_handle, inputs="text", outputs="label")
gr.close_all()
demo.launch()
```

## 2. 图像分类

```python
from torchvision import transforms
import gradio as gr
import torch
import torchvision.models as models

model = torch.hub.load('pytorch/vision:v0.6.0', 'resnet18', pretrained=True).eval()

# 打开文本文件
file_path = 'labels.txt'  # 将文件路径替换为你实际的文本文件路径
with open(file_path, 'r') as file:
    # 读取文件内容
    labels = file.readlines()
# 去掉每个label的换行符
labels = [label.rstrip() for label in labels]


# 预测函数
def predict(inp):
    inp = transforms.ToTensor()(inp).unsqueeze(0)
    with torch.no_grad():
        prediction = torch.nn.functional.softmax(model(inp)[0], dim=0)
        confidences = {labels[i]: float(prediction[i]) for i in range(1000)}
    return confidences


# fn表示触发函数，当我们点击提交按钮时，触发predict函数进行推理
# inputs表示图像会被转换为PIL.Image格式
# outputs将以label的形式展示出来
# examples可以将图片作为示例展示出来，供用户使用
gr.Interface(fn=predict,
             inputs=gr.Image(type="pil"),
             outputs=gr.Label(num_top_classes=3),
             examples=["demo/cat.jpg", "demo/tiger.jpeg"]).launch()
```

## 3. 图片筛选器

尽管gradio的设计初衷是为了快速创建机器学习用户交互页面。但实际上，通过组合gradio的各种组件，用户可以很方便地实现非常实用的各种应用小工具。

例如: 数据分析展示dashboard, 数据标注工具, 制作一个小游戏界面等等。

本范例我们将应用 gradio来构建一个图片筛选器，从百度爬取的一堆猫咪表情包中刷选一些我们喜欢的出来。

```python
#!pip install -U torchkeras
import torchkeras
from torchkeras.data import download_baidu_pictures

# download_baidu_pictures('猫咪表情包', 100)

import gradio as gr
from PIL import Image
import time, os
from pathlib import Path

base_dir = '猫咪表情包'
selected_dir = 'selected'
files = [str(x) for x in
         Path(base_dir).rglob('*.jp*g')
         if 'checkpoint' not in str(x)]


def show_img(path):
    return Image.open(path)


def fn_next(done, todo):
    # 构建排序关键字
    # 根据已查看和未查看的数量进行排序
    sort_key = lambda file: (int(os.path.splitext(file)[0]) - done, -todo)  
    # 对数据集中的图像进行排序
    sorted_images = sorted(os.listdir(base_dir), key=sort_key)
    img_sort = sorted_images[done + 1]
    path = os.path.join(base_dir, img_sort)
    done += 1
    todo -= 1
    img = show_img(path)
    
    return done, todo, path, img, msg


def save_selected(img_path):
    img = Image.open(img_path)
    img = img.convert("RGB")
    img.save(os.path.join(selected_dir, img_path))
    msg = "Image saved successfully!"
    
    return msg


def get_default_msg():
    msg = "图像未筛选状态！"
    
    return msg


with gr.Blocks() as demo:
    with gr.Row():
        total = gr.Number(len(files), label='总数量')
        with gr.Row():
            done = gr.Number(0, label='已完成')
            todo = gr.Number(len(files), label='待完成')
            bn_next = gr.Button("下一张")
    path = gr.Text(files[0], lines=1, label='当前图片路径')
    feedback_button = gr.Button("选择图片", variant="primary")
    msg = gr.TextArea(value=get_default_msg(), lines=3, max_lines=5)
    img = gr.Image(value=show_img(files[0]), type='pil')

    bn_next.click(fn_next,
                  inputs=[done, todo],
                  outputs=[done, todo, path, img, msg])
    feedback_button.click(save_selected,
                          inputs=path,
                          outputs=msg)

demo.launch()
```

