---
title: 如何使用 Mkdocs
authors: Qinghan Yu
---
# 如何使用Mkdocs

## MKDocs Manual
For full documentation visit [mkdocs.org](https://www.mkdocs.org).
<br>Get started visit [getting-started](https://squidfunk.github.io/mkdocs-material/getting-started/)
<br>[中文文档](https://mkdoc-material.llango.com/getting-started/)

## Commands

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.

## 美化和多功能
<br>代码高亮的方法: [快来美化你的MKDocs吧](https://juejin.cn/post/7066641709198737416)
<br>修改字体: [尝试修改 mkdocs-material 网页的字体的过程记录](https://ronaldln.github.io/MyPamphlet-Blog/2023/10/23/mkdocs-material/?h=mkdoc)
<br>优美的参考: [鹤翔万里的笔记本](https://note.tonycrane.cc/)
<br>优美的参考: [RonaldLN的笔记本](https://ronaldln.github.io/MyPamphlet-Blog/category/configure-debug/),在Configure & Debug有一些关于mkdocs的内容
<br>对LaTeX的支持: [MathJax and KaTeX for MKDocs](https://squidfunk.github.io/mkdocs-material/reference/math/#mathjax-mkdocsyml)
<br>左侧导航栏设置：[导航设置](https://afeiroom.github.io/work/notes/mkdocs/navigation/)
<br>
<br>提示块实现
```markdown
!!! tldr "写在前面"
    感谢xxx中提供的安装方法，本文主要是根据他的博客实现的。
```
!!! tldr "写在前面"
    感谢xxx中提供的安装方法，本文主要是根据他的博客实现的。
!!! tip "Use `tip` for this."
    This is a tip.
!!! info "Use `info` for this."
    This is a piece of information, or you can use todo.
!!! question "Use `question` for this."
    This is a question.
!!! warning "Use `warning` for this."
    This is a warning
!!! danger "Use `danger` for this."
    This alerts danger!
!!! success "Use `success` for this."
    This alerts success

<br>引用标注的实现
```markdown
> TLDR means too long, didn't read
```
> TLDR means too long, didn't read

## markdown 基本语法
不同等级标题，斜体，加粗，删除线，链接，图片，表格

## 流程图
基于mermaid方法的流程图

## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.
