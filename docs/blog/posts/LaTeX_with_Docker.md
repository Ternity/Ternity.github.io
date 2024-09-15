---
title: Use LaTeX by docker
comments: true
draft: true 
date: 2024-09-14 
categories:
  - LaTeX
  - Docker
tags:
  - LaTeX
---

# Use LaTeX by docker
In this post, I will show you how to use LaTeX by docker.<br>
!!! tldr "写在前面"
    感谢刘云霈博士在[个人博客](https://cloudac7.github.io/p/containerize-your-life-%E5%AE%B9%E5%99%A8%E5%8C%96latex%E7%8E%AF%E5%A2%83%E5%8A%A9%E5%8A%9B%E8%AE%BA%E6%96%87%E5%86%99%E4%BD%9C/)中提供的安装方法，本文主要是根据他的博客实现的。

## 预先安装

安装docker，vscode，基于code的 Dev Container 插件 和 LaTeX Workshop 插件

随后创建/进入LaTeX目录

按`ctrl`+`,`进入设置，如果你不希望改变全局设置，请选择 `Workspace` 选项卡，则以下配置仅对当前工作区生效。

搜索 `Docker`，在左侧目录中找到 LaTeX 分类下的两个选项：`latex-workshop.docker.enabled` 和 `latex-workshop.docker.image.latex`。

根据设置的描述，这两个选项分别对应于是否启用 Docker 环境编译、选择哪个镜像导入。

于是我们勾选 `latex-workshop.docker.enabled`，启用，然后在 `latex-workshop.docker.image.latex` 的文本框中填入：

```
1 ghcr.io/xu-cheng/texlive-full 
```
## 拉取LaTeX容器镜像

```
1 docker pull ghcr.io/xu-cheng/texlive-full 
```

## 自动编译tex文件

编辑并保存tex文件，如果一切正常没有报错，那么LaTeXWorkshop将会自动保存并编译得到PDF。也可预览PDF。

## 使用GitHub Action实现github上的自动编译

更多方法可见刘云霈博士的博客[One More Trick: Github Action](https://cloudac7.github.io/p/containerize-your-life-%E5%AE%B9%E5%99%A8%E5%8C%96latex%E7%8E%AF%E5%A2%83%E5%8A%A9%E5%8A%9B%E8%AE%BA%E6%96%87%E5%86%99%E4%BD%9C/)



<!-- more -->