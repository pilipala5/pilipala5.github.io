---
title: "Hugo+PaperMod+Github创建自己的博客网站"
date: 2025-02-25T00:00:00+00:00
draft: false
toc: true
tags: ["Odds and Ends"]  # Add a list of tags
---

# 0. 友情提示

​	以上内容是我根据我的博客构建内容中总结，并不是在构建过程中记录的，或许有错误之处，若存在问题，欢迎大家指出！

# 1. 引言

> Lilian Weng's Log:  https://lilianweng.github.io/

​	在学习的过程中，偶然看见了Lilian Weng的个人博客网站，感觉像她那样记录自己的思想很有意义，因此学习她搭建了这个网页。希望以后也能分享自己的一些学习记录与思考。

​	在搭建这个网页的时候也走了一些弯路，因此，特意写一篇博客在这里，记录一下我搭建这个网页的过程，供大家参考。

# 2. 页面构建

## 2.1 Hugo下载

​	首先整个页面是基于Hugo搭建的，因此我们需要在电脑上下载Hugo。我的电脑是windows系统，直接在网上找到对应版本下载即可。下载之后解压文件夹，结构如下：

![文件结构](/images/post1/img1.png)

​	接下来，运行 **hugo.exe** 文件，运行后关闭，在该文件根目录，打开终端，输入

```
hugo version
```

​	输出版本号，则说明hugo安装成功，

![文件结构](/images/post1/img2.png)

​	然后将 **hugo.exe** 文件夹所在根目录加入”**系统环境变量-系统变量-Path**“中。添加完成后，随意在其他目录下打开终端，运行 hugo version，若能显示版本号，则说明环境变量配置成功。

## 2.2 构建页面

> 如下内容参考官方链接（20250225）： https://github.com/adityatelange/hugo-PaperMod/wiki/Installation

​	然后找到你想要构建页面的目录，打开终端，输入如下命令：

```
hugo new site MyFreshWebsite --format yaml
# replace MyFreshWebsite with name of your website
```

​	这里 MyFreshWebsite 会生成一个名叫 MyFreshWebsite 的文件夹，然后不要关闭终端，接着输入（这里的MyFreshWebsite 可以自定义，但是后面需要对应）：

```
cd MyFreshWebsite 
```

## 2.3 主题安装

​	这样我们就成功的构建了一个网页。接下来我们需要安装 PaperMod 主题，这里我们按照官方页面上推荐的方式。这里需要提前安装好 Git。没有安装的同学，我给大家找了一个教程，大家参考着自己安装（[Git安装教程](https://blog.csdn.net/weixin_42242910/article/details/136297201)）。然后我们在 MyFreshWebsite 目录下，输入如下命令：

```
git init
```

```
git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
git submodule update --init --recursive # needed when you reclone your repo (submodules may not get cloned automatically)
```

```
git submodule update --remote --merge
```

​	这样我们就成功下载了 PaperMod 主题了。MyFreshWebsite 的文件结构应当如下，这个图片我给的是我最终的版本，有些许不一样应该不重要，重要是关于hugo.yaml文件：

![image-20250225124919488](/images/post1/img3.png)

## 2.4 页面配置

​	打开我们的 **hugo.yaml** 文件，里面有些配置我们需要修改，这里我实际也不是很懂，但是先给大家一个简单案例吧，这些功能也是大家基本上都会用到的，所以这一段可以先无脑copy😀。

```yaml
baseURL: "https://pilipala5.github.io/"
languageCode: "en-us"
title: "YH's Log"
theme: ["PaperMod"]

menu:
  main:
    - identifier: Posts
      name: Posts
      url: /posts/
      weight: 10
    - identifier: Tags
      name: Tags
      url: /tags/
      weight: 20
    - identifier: Search
      name: Search
      url: /search/
      weight: 30
    - identifier: Archives
      name: Archives
      url: /archives/
      weight: 40

params:
  assets:
    css:
      - "css/custom.css"
  title: "pilipala5's Blog"
  description: "My personal blog using Hugo and PaperMod"
  author: "pilipala5"
  profileMode:
    enabled: false
  showTags: true
  showToc: true
  showReadingTime: true
  showPostNavLinks: true
  homeInfoParams:
    Title: "🐕Welcome to Yuhan's Log"
    Content: |
      Hi, I am a student from the School of Remote Sensing and Information Engineering, Wuhan University.I will share some of my learning experiences and thoughts here. Hope you like it!😊
  socialIcons:
    - name: github
      url: "https://github.com/pilipala5"
    - name: csdn
      url: "https://blog.csdn.net/m0_62716099?spm=1000.2115.3001.5343"
  defaultTheme: "dark"
  mainSections:
    - posts
  fuseOpts:
    isCaseSensitive: false
    shouldSort: true
    location: 0
    distance: 1000
    threshold: 0.4
    minMatchCharLength: 0
    # limit: 10 # refer: https://www.fusejs.io/api/methods.html#search
    keys: ["title", "permalink", "summary", "content"]

outputs:
  home:
    - HTML
    - RSS
    - JSON # necessary for search
```

​	提醒几个重要的点：

1. **baseurl** 是部署后你的网页所在的链接，需要把 "pilipala5" 修改成自己的Github用户名
2. **menu main**下的这四个对应的是四个功能：博客、标签、搜索、归档。这里还需要在其他地方配置一些文件，**但是这个 Yaml 文件就是最终版，不需要再进行任何的修改**。
3. **homeInfoParams** 下对应的是进入页面时的欢迎介绍，随意修改。
4. **socialIcons** 下对应的是一些其他的链接，随意修改

### 2.4.1 发表博客

​	现在我们需要发表第一个博客，这样才能检验其他功能是否正确。在 **MyFreshWebsite** 下打开终端，输入如下命令：

```
hugo new posts/my-first-blog.md
```

​	接下来我们就能发现生成了 **content/posts/my-first-blog.md** 文件，这个文件就是我们的博客。在博客的最开始，加入如下源代码：

```
---
title: "Hugo+PaperMod+Github创建自己的博客网站"	# 标题
date: 2025-02-25T00:00:00+00:00	# 只需要修改前面的日期
draft: false	# 是否为草稿，是草稿在hugo server时不会显示
toc: true	# 是否显示大纲
tags: ["Odds and Ends"]  # 用于显示标签
---
```

​	然后发布自己的博客内容。这里面 **tags 就是与 Yaml 文件中的 /tags/ 页面对应的**。然后在 **MyFreshWebsite** 下打开终端，本地运行服务，输入如下命令：

```
hugo server
```

​	本地部署成功后会显示一个链接，打开链接就是自己的页面，若能在 Posts 导航栏以及首页看见自己的博客，则发布成功。

### 2.4.2 归档页面

​	归档页面对应的是 Yaml 文件中的 Archives部分，这里只需要**在 content/ 下创建一个 archives.md文档**即可，在文档中插入如下内容：

```
---
title: "Archives"
layout: "archives"
url: "/archives/"
summary: archives
---
```

### 2.4.3 搜索功能

​	搜索功能同理，**在 content/ 下创建一个 search.md文档**,文档中插入如下内容：

```
---
title: "Search" # in any language you want
layout: "search" # necessary for search
# url: "/archive"
# description: "Description for Search"
summary: "search"
placeholder: "placeholder text in search input box"
---
```

​	这样，我们就成功构建了我们的页面了。

​	最后在 **MyFreshWebsite** 下打开终端，输入命令行（我不确定是否必须）构建静态页面：

```
hugo --minify
```

# 3. 页面推送

> 官方教程： https://gohugo.io/hosting-and-deployment/hosting-on-github/

​	接下来，我们就要利用 Github.io 推送我们的页面了。首先我们需要新建一个Github仓库，命名为 "**用户名.github.io**"。然后链接仓库与本地文件链接。我是这么做的，我不是很懂 Git，仅参考。

```
git branch -M main 
git remote add origin https://github.com/pilipala5/pilipala5.github.io.git 
```

​	查看Github本地仓库，到 Settings-Pages 页面中，将 Build and deployment 中 Source 修改为 Github Actions。

![image-20250225124919488](/images/post1/img4.png)

​	然后 .github/ 下创建一个 workflows 文件夹, 在 workflows下创建hugo.yaml文件。**.github/workflows/hugo.yaml 文件内容如下**：

```yaml
# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: Deploy Hugo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches:
      - main

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

# Default to bash
defaults:
  run:
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.141.0
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb
      - name: Install Dart Sass
        run: sudo snap install dart-sass
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5
      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
      - name: Build with Hugo
        env:
          HUGO_CACHEDIR: ${{ runner.temp }}/hugo_cache
          HUGO_ENVIRONMENT: production
          TZ: America/Los_Angeles
        run: |
          hugo \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

​	然后推送到仓库：

```
git add -A
git commit -m "Create hugo.yaml"
git push
```

​	在 Github 仓库主页查看 Actions，发现成功构建，即可进入 用户名.github.io 网页查看自己的个人博客了✌️！

![image-20250225124919488](/images/post1/img5.png)