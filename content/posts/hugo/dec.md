---
title: "博客装修清单"
date: 2022-11-21
showtoc: true
ShowWordCount: true
comment:
  enable: true
categories: ["建站笔记"]
lastmod: 2023-01-02
---

列一个清单，记录一下几乎每次换主题都需要重新配置的部分。方便下次换主题（你

## 装修清单

- [X] 文章meta信息修改
  - 一般在`layouts\post\single.html`
  - FixIt主题在`layouts\_default\summary.html`还需要再改一次
  - 在config文件中加上`hasCJKLanguage = true`
- [X] 页脚站点运行时间
- 短代码
  - [X] [图片轮播](https://guanqr.com/tech/website/a-way-to-realize-carousel-in-meme/)
  - [X] [豆瓣卡片](https://www.sulvblog.cn/posts/blog/)
  - [X] [折叠](https://github.com/Ice-Hazymoon/hugo-theme-luna/blob/master/layouts/shortcodes/accordion.html)
- [X] 友链卡片
  1. 导入友链scss文件和短代码
  2. `@import`
- [X] favicon
- [X] 评论
  - 崩溃中，我是看到FixIt主题支持twikoo所以决定换这个的，因为even主题使用twikoo总是怪怪的，但是在FixIt中尝试了很久都没有成功。直到检索到[《主题文档 - 基本概念》](https://pre.fixit.lruihao.cn/zh-cn/theme-documentation-basics/)，在评论中看到：
  - **本地开发环境不启用评论，使用生产环境即可`hugo server -e production`**
  - 啊啊啊啊我哭！
- [X] 手机端回顶部
- [X] 全站字体
  - `$global-font-family: 'Noto Serif SC','Noto Serif KR', serif;`
  - 在head.html写
    ```
      <link rel="preconnect" href="https://fonts.googleapis.com">
      <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
      <link href="https://fonts.googleapis.com/css2?family=Noto+Serif+KR&family=Noto+Serif+SC&display=swap" rel="stylesheet">
    ```
  - 在_custom.scss写
    ```
      @font-face {
        font-family: 'Noto Serif SC','Noto Serif KR', serif;
        src: url('https://fonts.googleapis.com/css2?family=Noto+Serif+KR&family=Noto+Serif+SC&display=swap');
      }
    ```
  - 其实不这么麻烦也行，SC的韩文很违和，KR的中文丑的很可爱
- [ ] 在归档页加上分类
