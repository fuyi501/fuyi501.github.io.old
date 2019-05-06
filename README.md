---
title:  博客配置
key: sum-20180102
tags: 博客 
modify_date: 2018-01-13 00:00:00 +08:00
comment: true 
excerpt_type: html 
---

## 博客主题

博客使用 jekyll TeXt 主题，github 地址：`https://github.com/kitian616/jekyll-TeXt-theme`

## 安装主题

从 github 克隆主题代码 `git clone https://github.com/kitian616/jekyll-TeXt-theme.git`，或者下载压缩包

<!--more-->

## 本地预览

下载好代码后，确保自己已经安装完成 jekyll 和 bundle ，执行

```sh
bundle install # 安装依赖
bundle exec jekyll serve # 启动项目
```

然后就可以访问 `http://localhost:4000` 来预览博客网站了

## 配置 

[配置详情](https://tianqi.name/jekyll-TeXt-theme/docs/zh/configuration)

[参考项目1](https://github.com/kitian616/jekyll-TeXt-theme/blob/master/docs/_config.yml)

[参考项目2](https://github.com/kitian616/kitian616.github.io/blob/master/_posts/2015-10-14-about-this-blog.md)

### css 修改

修改代码位于 `/_includes/head.html`

### 代码高亮

`highlight_theme: "tomorrow-night-bright"`

### url

`https://fuwenwei.com` 和 `https://blog.fuwenwei.com`

### GitHub 源码仓库

`repository: fuyi501/fuyi501.github.io` 这个设置可以在文章详情页点击在github进行编辑

## 文章配置项

### 文章摘要

该主题的摘要有两种模式——TEXT 模式和 HTML 模式。 当 _config.yml 配置项 excerpt_type 的值为 text 时是 TEXT 模式，为 html 时是 HTML 模式，默认为 TEXT 模式。
设置为 html 时摘要为 HTML 文档，与文章内容一致，并且 默认展示整篇文章的内容。若想控制摘要内容，需要在文章中想要显示到的地方加上 <!--more-->。

### 在 github 修改

修改代码位于 `/_includes/article-header.html`

### gitalk 评论

### leancloud 统计文章点击量

[leancloud](https://leancloud.cn/)

### Google Analytics 站点统计

[Google Analytics](https://analytics.google.com/analytics/web/)

### logo

可以通过替换 _includes/svg/logo.svg 来设置你的 Logo

### Favicon

推荐使用 [RealFaviconGenerator](https://realfavicongenerator.net/) 来生成 Favicon

## 撰写博客

使用 markdown 开撰写博客文章，文章文件名遵循 `年-月-日-标题.md` 格式，文章开头需包含 YAML 头信息，如下所示：

```yaml
---
layout: article
title:  博客配置
key: sum-100018 # 页面的唯一标识符，供评论系统和点击量统计使用。
tags: 博客 
# date: 2018-01-12 00:00:00 +08:00
modify_date: 2018-01-12 00:00:00 +08:00
comment: true # 是否开启评论支持，默认开启，设置为 false 关闭。
excerpt_type: html # text (default), html
---
```

### markdown 增强

| 增强项 | 描述 |
| --------------- | ----------- |
| **Mathjax** | 在文章中方便的加入数学公式，使用 MathML、LaTeX 和 ASCIIMathML 语法 | [示例](https://tianqi.name/jekyll-TeXt-theme/post/2017/07/07/mathjax.html) |
| **Mermaid** | 在文章中方便的加入流程图 | [示例](https://tianqi.name/jekyll-TeXt-theme/post/2017/06/06/mermaid.html) |
| **Chart**   | 在文章中方便的加入可交互的图表 | [示例](https://tianqi.name/jekyll-TeXt-theme/post/2017/05/05/chart.html) |

### 附加样式

#### 提示

| 样式名称 |
| ---- |
| **success** |
| **info** |
| **warning** |
| **error** |

Success Text.
{:.success}

Info Text.
{:.info}

Warning Text.
{:.warning}

Error Text.
{:.error}

**markdown:**

    Success Text.
    {:.success}
^
    Info Text.
    {:.info}
^
    Warning Text.
    {:.warning}
^
    Error Text.
    {:.error}

#### 标签

| Class Names |
| ---- |
| **success** |
| **info** |
| **warning** |
| **error** |

`success`{:.success}

`info`{:.info}

`warning`{:.warning}

`error`{:.error}

**markdown:**

    `success`{:.success}
^
    `info`{:.info}
^
    `warning`{:.warning}
^
    `error`{:.error}