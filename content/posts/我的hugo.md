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
    URL: "https://github.com/weilanjin/blog"
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
#### 创建自动化脚本
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

```sh
### 推送 本地的到远程仓库
git remote add origin https://github.com/weilanjin/blog.git
git branch -M main

git add .
git commit -m "Add GitHub Actions for deployment"
git push -u origin main
```