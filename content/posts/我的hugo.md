---
title: "用Hugo构建我的Blog"
date: 2025-03-04T11:30:03+00:00
# weight: 1
tags: ["other"]
author: "Lance"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false
comments: false
description: "使用Hugo搭建自己Blog的笔记"
canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/weilanjin/weilanjin.github.io/content"
    Text: "编辑" # edit text
    appendFilePath: true # to append file path to Edit link
---
### 1.环境准备

```sh
# 安装 hugo
brew install hugo
# 查看版本
hugo version
# 创建仓库
cd Desktop
hugo new site blog --format=yaml # 配置文件使用 yaml 格式
```

### 2.引用主题

```sh
cd blog
git clone https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod --depth=1
```

### 3.启动服务

```sh
cd Desktop/blog
hugo server
```

### 4.新建文章

```sh
hugo new posts/my-first-post.md
```

### 5.github 创建仓库

 创建一个名为 weilanjin.github.io 的仓库

#### 5.1 创建自动化脚本

```yml
name: Deploy Hugo site to GitHub Pages  # 工作流名称

on:
  push:
    branches:
      - main  # 监听 main 分支上的推送操作

jobs:
  deploy:
    runs-on: ubuntu-latest  # 使用最新的 Ubuntu 作为运行环境
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v3  # 拉取代码，包括子模块

      - name: Install Hugo
        uses: peaceiris/actions-hugo@v2  # 安装 Hugo
        with:
          hugo-version: 'latest'  # 使用最新版本的 Hugo

      - name: Build Hugo Site
        run: hugo  # 运行 hugo 生成静态站点

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3  # 使用 gh-pages 部署
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}  # GitHub 提供的安全令牌
          publish_dir: ./public  # Hugo 生成的静态站点目录
          publish_branch: gh-pages  # 部署到 gh-pages 分支
```

#### 5.2 启用 GitHub Pages 的 Actions 写入权限

1. 进入你的 GitHub 仓库
2. 打开 Settings → Actions → General
3. 在 Workflow permissions 里选择：

✅ Read and write permissions（⚠ 默认是 Read-only）

#### 5.3 GitHub Pages 选择 gh-pages 分支

1. 进入你的 GitHub 仓库
2. 打开 Settings → GitHub Pages -> Page
3. 在 Source 里选择： gh-pages 分支

#### 5.4 提交代码

```sh
### 推送 本地的到远程仓库
git init
git remote add origin https://github.com/weilanjin/weilanjin.github.io.git
git branch -M main

git add .
git commit -m "Add GitHub Actions for deployment"
git push -u origin main
```

### hugo 配置

```yaml
# module:
#   imports:
#   - path: github.com/adityatelange/hugo-PaperMod

baseURL: "https://weilanjin.github.io/" 
languageCode: zh-cn
title: Lance blog
pagination:
  pagerSize: 5
theme: PaperMod

menu:
  main:
    - identifier: go
      name: Go
      url: /go/
      weight: 10
    - identifier: microservice
      name: Microservice
      url: /microservice/
      weight: 20
    - identifier: other
      name: Other
      url: /other/
      weight: 30
    - identifier: archives
      name: Archives
      url: /archives/
      weight: 40

params:
  env: production # to enable google analytics, opengraph, twitter-cards and schema.
  title: Lance blog
  description: "关于一个Go开发者的博客"
  keywords: [Go, Microservice, Distribute]
  author: Lance
  # author: ["Me", "You"] # multiple authors
  images: ["<link or path of image for opengraph, twitter-cards>"]
  DateFormat: "2006-01-02"
  defaultTheme: auto # dark, light
  disableThemeToggle: false # 是否显示主题切换按钮

  ShowReadingTime: true # 显示阅读时间
  ShowShareButtons: false # 显示分享按钮
  ShowPostNavLinks: false # 显示上下篇
  ShowBreadCrumbs: false  # 显示面包屑
  ShowCodeCopyButtons: true # 显示代码复制按钮
  ShowWordCount: true # 显示代码行数
  ShowRssButtonInSectionTermList: false  # 显示rss按钮
  UseHugoToc: true # 显示目录
  disableSpecial1stPost: false # 首页不显示文章
  disableScrollToTop: true # 显示回到顶部按钮
  comments: true # 评论
  hidemeta: false # 隐藏元数据
  hideSummary: false # 隐藏摘要
  showtoc: false # 隐藏目录
  tocopen: false # 目录默认打开

  assets:
    disableHLJS: false # 禁用 highlight.js 如果禁用 则不会加载 highlight.js 默认使用 Chrome 的代码高亮
    favicon: "<link / abs url>"
    favicon16x16: "<link / abs url>"
    favicon32x32: "<link / abs url>"
    apple_touch_icon: "<link / abs url>"
    safari_pinned_tab: "<link / abs url>"

  label:
    text: "Home"
    icon: /apple-touch-icon.png
    iconHeight: 35

  # profile-mode 个人简介
  profileMode:
    enabled: false # 是否替换首页显示
    title: Lance Blog
    subtitle: "一名Go开发者"
    imageUrl: "<img location>"
    imageWidth: 120
    imageHeight: 120
    imageTitle: my image
    buttons:
      - name: Posts
        url: posts
      - name: Tags
        url: tags

  # home-info mode
  # homeInfoParams:
  #   Title: "Hi there \U0001F44B"
  #   Content: Welcome to my blog

  socialIcons:
    # - name: x
    #   url: "https://x.com/"
    # - name: stackoverflow
    #   url: "https://stackoverflow.com"
    - name: github
      url: "https://github.com/weilanjin"

  analytics:
    google:
      SiteVerificationTag: "XYZabc"
    bing:
      SiteVerificationTag: "XYZabc"
    yandex:
      SiteVerificationTag: "XYZabc"

  cover:
    hidden: true # hide everywhere but not in structured data
    hiddenInList: true # hide on list pages and home
    hiddenInSingle: true # hide on single page

  editPost:
    URL: "https://github.com/weilanjin/weilanjin.github.io/edit/main/content/{{ .File.Path }}"
    Text: "edit" # edit text
    appendFilePath: true # to append file path to Edit link

  # for search
  # https://fusejs.io/api/options.html
  # 本地搜索功能
  fuseOpts:
    isCaseSensitive: false  # 大小写敏感
    shouldSort: true        # 按匹配度排序
    location: 0
    distance: 1000
    threshold: 0.4
    minMatchCharLength: 0
    limit: 10 # 最多返回 10 个匹配结果
    keys: ["title", "permalink", "summary", "content"] # 搜索范围（标题、链接、摘要、正文）

# Read: https://github.com/adityatelange/hugo-PaperMod/wiki/FAQs#using-hugos-syntax-highlighter-chroma
pygmentsUseClasses: true
markup:
  highlight:
    noClasses: false
    anchorLineNos: false
    codeFences: true
    guessSyntax: true
    lineNos: true
    style: github

# Hugo 相关配置
enableRobotsTXT: true
buildDrafts: false
buildFuture: false
buildExpired: false

# Hugo 站点的 HTML、CSS、JS 代码压缩，减少文件体积，提高网站加载速度。
minify:
  disableXML: true 
  minifyOutput: true
```