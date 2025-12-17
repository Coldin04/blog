---
title: Blog翻新，更优雅的界面，更舒适的工作方式
description: 更换Hugo来生成Blog，也能拥有更加一致的写作体验
slug: new_blog
date: 2024-04-29 09:00:00+0800
#image: 2.jpg
categories:
    - blog
    - hugo
tags:
    - blog
    - hugo
    - 博客
    - 技术
---

前段时间正在愉快的对电脑进行维护，但一不小心手滑敲错了命令，导致我用了两年的Windows系统盘被无意清空。因为之前有备份文件的习惯，所以对我的损失并不算大。但由于Blog的源文件存储在家里的机械盘中，找回不那么方便，加上感觉之前的主题有些较旧了，索性打算好好的重建一下Blog。

## 搜寻适合自己的静态网站生成器

开始的时候，我仍打算使用Hexo作为博客的生成器，于是考虑搜寻一个好看且较贴切个人风格的主题。但无意中看到了 Hugo，便本着尝鲜的想法，打开的Hugo的官网进行查看。于是第一步，就开始挑选主题。说起来可能有那么些不可思议，我第一眼便相中了 [Hugo Theme Stack](https://github.com/CaiJimmy/hugo-theme-stack) 这个主题，设计风格简单明了，也没有多余的元素，而且更好的是，他可以直接使用 [Hugo Theme Stack Starter](https://github.com/CaiJimmy/hugo-theme-stack-starter) 导入自己的Github仓库，并可以借助 Github Action 进行自动编译文章。这样一来，确实可以简化不少的工作流程，方便我随时随地使用CodeSpace更新文章。就好比这篇文章，我就使用Surface远程CodeSpace进行编辑的。

## 配置网站和熟悉逻辑

这部分其实没那么多好说的，我就是按照自己的风格进行调试，你可以在Github Commits里面看到我相关操作，因此没那么多可以聊的，按照帮助配置就好。

## 迁移文章和遇到的问题

网站按照自己的想法配置好后，就到了迁移文章的时候。前面说过，因为源文件丢失了，所以网站内容我都是直接从老Blog
复制粘贴下来的。因为前面的文章没有引用图片，所以开始并没有注意到问题，直到我开始迁移《记一次数据恢复和数据冷备份》 这篇文章的时候，才发现通过外链引用的图片居然打不开（心想，wtf，难道这个框架不支持图片的预览嘛），开始以为是配置没有开导致的，直到我开始看官方文档，直到我注意到

```
WARNING

Inserting external images is supported, but it is not recommended.

Features like image gallery and image zooming will not work with external images. Those feature needs to know the image's dimensions, which is not possible with external images.

```

啊？我当时注意到这个的时候还挺震惊的，因为我的习惯就是从外链引用图片，这下子确实麻烦起来了,甚至让我有了放弃使用的想法。不过好在经过简单搜索，发现了 [Hugo Theme Stack图库是怎么工作的？怎样才能支持外链图片？](https://ghjayce.github.io/p/static-site-generator/hugo/hugo-theme-stack-gallery-study) 这篇文章。

不过即使找到了解决方案，也没法修改。显而易见的是，我的仓库里并没有`layouts`文件夹，更别谈去修改了。我又开始打起了退堂鼓，不过之后又进行了一番搜索，发现官方的仓库中也说明了这一情况，但没说明白。通过`Hugo Modules` 方式加载，可以通过新建这个目录，本地的文件会自动覆盖编译时加载的文件。这个设计太赞了！于是，按照解决方案修改好之后，在`config.toml`中按照规则关闭`imageProcessing.content`改成`false`并添加有关项目，再次测试发现外链图片可正常查看了。

## Hugo的优势

用一个字形容就是"快"，而且并不需要像`node.js`那样在每次安装依赖时都弄得很复杂。这个主题支持使用Github Action编译，也使得我在每次写完之后可以只管上传就行，流程方面也能更快了。总而言之，这个工作流让我很满意。下一步来说，我应该会考虑专注于创作点优质的Blog内容，而系统化的内容仍会在 [酷丁的笔记](https://note.cold04.com) 中持续更新。