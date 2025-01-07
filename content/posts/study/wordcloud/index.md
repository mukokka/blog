---
title: "嘟文词云生成指北 （co author: ChatGPT）"
date: 2025-01-07
draft: false
toc: true
categories: ["공부 해야돼"]
lightgallery: true

---

让chatgpt大师写了利用嘟文存档生成词云的代码，让我们一起说谢谢gpt大师

## 安装python

### 方案一：pycharm或jupyter

[PyTorch深度学习快速入门教程 P2](https://www.bilibili.com/video/BV1hE411t7RN)

嘟主跟着这位up主装了pycharm和jupyter，但好像jupyter创建虚拟环境比较麻烦于是选用了pycharm。

### 方案二：在vs code中配置python

这个方法是码农朋友教嘟主并在嘟主电脑上设置的，因为没有参考过网上的教程所以只能提一个方案。~~毕竟嘟主只会用vs code写博客嘟主怎么会知道这些。~~

## 安装toolbox

其实嘟主也不记得哪些包是python自带的哪些是需要再次安装的，代码复制放到pycharm中遇到红色波浪线再安装就好。pycharm记得解释器选择对应虚拟环境的。~~其实不小心安装到base也没什么大影响啦。~~

然后把嘟文导出文件中的`outbox.json`和`停用词文件`（例如[这个](https://github.com/goto456/stopwords)）和下述代码放到一个文件夹里就可以运行。

为了一目了然，写了注释的部分是需要/可以自定义修改的。

```
import json
import re
from bs4 import BeautifulSoup
import jieba
from wordcloud import WordCloud
import matplotlib.pyplot as plt
from datetime import datetime, timedelta
from PIL import Image
import numpy as np

def load_stopwords(file_path):
    with open(file_path, 'r', encoding='utf-8') as f:
        return set(f.read().splitlines())

with open('outbox.json', 'r', encoding='utf-8') as f:
    data = json.load(f)

# 停用词列表
stopwords = load_stopwords('cn_stopwords.txt')  # 假设停用词文件是 cn_stopwords.txt

posts = []

# 获取过去 372 天和 7 天的日期范围
start_date = datetime.now() - timedelta(days=372)
end_date = datetime.now() - timedelta(days=7)

# 提取并清理内容
for item in data.get('orderedItems', []):
    if 'object' in item and 'content' in item['object'] and 'published' in item:
        content = item['object']['content']
        published_date = item['published'] 

        try:
            post_date = datetime.strptime(published_date, '%Y-%m-%dT%H:%M:%SZ')
        except ValueError:
            continue

        # 如果帖子在过去时间范围内，才进行处理
        if start_date <= post_date <= end_date:
            clean_content = BeautifulSoup(content, 'html.parser').get_text()
            clean_content = re.sub(r'[^\w\s]', '', clean_content) 
            clean_content = re.sub(r'\s+', ' ', clean_content) 
            posts.append(clean_content)
text = ' '.join(posts)

words = jieba.cut(text)
filtered_words = [word for word in words if word not in stopwords and len(word) > 1] 

cut_words = ' '.join(filtered_words)

# 读取轮廓图像（mask）
mask = np.array(Image.open('mask.png'))  # 替换为你的遮罩图像路径

# 创建一个 WordCloud 对象并生成文字云
wordcloud = WordCloud(
    font_path='C:\Windows\Fonts\msyh.ttc',
    width=1600,
    height=800,
    background_color='white',
    #mask=mask,
    #contour_color='orange', # 设置边框颜色
    #contour_width=3, # 设置边框宽度
    max_words=150, # 限制文字云中显示的最大词汇数
    colormap='inferno' # 文字云中的配色，https://www.kaggle.com/code/niteshhalai/wordcloud-colormap
).generate(cut_words)

# 显示生成的文字云
plt.figure(figsize=(10, 5))
plt.imshow(wordcloud, interpolation='bilinear')
plt.axis('off') 
plt.show()
```

## 大功告成

![打码了嘟友id](images/wordcloud.png " ")

如果想要使用遮罩将遮罩图像路径也放在同一文件夹中并取消掉注释即可

![打码了嘟友id(2)](images/wordcloud_mask.png " ")

虽然感觉词云里只有一些语气词;;难道这表明只要学会了语气词就能听懂大部分外语…？
