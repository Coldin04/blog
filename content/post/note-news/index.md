---
title: 新站上线！用Vuepress搭建的个人笔记库
description: Vuepress构建的笔记仓库，里面不少好东西
slug: note-news
date: 2023-10-30 22:00:00+0800
#image: 2.jpg
categories:
    - vuepress
    - note
tags:
    - vuepress
    - note
    - 笔记
    - 酷丁
    - 想法
---

自从转到计算机专业之后，因为很多命令在电脑上执行，故开始尝试使用了Obsidian记录笔记。一个学期下来，也攒了不少内容。本着分享的精神，我最初的想法是将Obsidian仓库发布，也方便对笔记进行备份。后来发现，Obsidian的第三方发布服务均有一定的缺陷，而官方发布服务又需要付费，价格又不太理想，很长一段时间都放弃去做这个事情了。

后来，在看到一些文档的的时候，发现了很多人都使用这一套“风格”进行搭建，如[小林Coding](https://xiaolincoding.com/) 、[ArchLinux简明指南](https://xiaolincoding.com/) 等。发现均以Vuepress为基础进行搭建，于是我也开始尝试使用Vuepress搭建自己的笔记库。（其实之前考虑过Notion等工具，但后来放弃）

## Vuepress

VuePress 由两部分组成：第一部分是一个极简静态网站生成器 (opens new window)，它包含由 Vue 驱动的主题系统和插件 API，另一个部分是为书写技术文档而优化的默认主题，它的诞生初衷是为了支持 Vue 及其子项目的文档需求。

更多可以看[官方文档](https://vuepress.vuejs.org/zh/)。

## 搭建过程
### 小插曲
其实，Vuepress的安装非常简单，只需要使用yarn即可:

```
mkdir vuepress-starter && cd vuepress-starter
yarn init
yarn add -D vuepress
```

当然，也别急着就开始了，想让网站更丰富，可以考虑安装第三方的主题。当然，如果想要了解更多安装和配置的细节，也可以进入[官方文档](https://vuepress.vuejs.org/zh/guide/getting-started.html) 了解更多细节。

我使用的是[vuepress-theme-hope](https://theme-hope.vuejs.press/zh/) 这个主题，这个主题主题很大程度上继承了 VuePress 默认主题的配置，并在此基础上添加了大量功能与布局优化。而且，这个主题也有blog功能，有兴趣的话可以了解一下。

标题说这是小插曲，那也就顺便聊聊。当时我拿到手，也是一股脑的按照上面的步骤安装了`Vuepress`，顺便熟悉了一下基本配置。我有个小习惯，当我正在研究的或者正在学习的东西（前提是感兴趣），我会去B站搜索看看别人的视频，或者去搜索引擎上以这个名称为关键词看看别人的配置过程这样的。无意就发现了`Vuepress-theme-hope`和`Vuepress-theme-repo`这两个主题。

当我点开了这两者的搜索条目，才发现这两个主题并非像Wordpress或者Hexo那样导入进去，或者放入一个文件夹就能用，而是单独通过包管理器（pnpm或yarn）进行新建。后来，我含泪去转移了已经整理完成的笔记，不过这些的工作量也并不大就是。

## 正式开始
### 安装

建议参考：[小白教程](https://theme-hope.vuejs.press/zh/get-started/)
首先，保证安装了Node.js、yarn\Pnpm和VS Code（或其他编辑器），然后根据提示安装即可。

但我既然在这里提到这件事，说明我也遇到了一些问题，这里也顺便记录一下。

在运行时，我首先遇到了这样的错误：

```
Error [ERR_MODULE_NOT_FOUND]: Cannot find package 'katex' imported from /home/coldin04/Documents/webnote/docs/node_modules/@mdit/plugin-katex/lib/index.mjs
    at new NodeError (node:internal/errors:406:5)
    ......
```

这个错误的原因是因为`vuepress-theme-hope`的依赖包`katex`没有找到，查询后发现，可通过:`yarn add katex`解决。

>tips:<br>之后查询到，这个依赖包是用于数学公式的，如果不需要，可以不安装，去配置中将其注释掉即可。

然后，我又遇到了这样的错误：

```
Error: Cannot find module 'reveal.js/dist/reveal.css'
Require stack:
- /home/coldin04/Documents/xxx/@mr-hope/vuepress-plugin-pwa/lib/client.js
  .....
```

这个错误的原因是因为`vuepress-theme-hope`的依赖包`reveal.js`没有找到，查询后发现，可通过:`yarn add reveal.js`解决。

此时，如果执行`yarn dev`，若能够正常运行，则即可进行下一步进行仔细的配置了。当然，如果还有别的运行问题，也可以借助搜索引擎或类似于ChatGPT语言大模型进行解决。

### 配置
因为Vuepress也配置过好几天了，具体也无法记的很详细了，这里就简单的说一下。当然，每个人的风格不同，配置是自然不同的，我的操作更多的是提供思路，请不要被我的步骤局限住了。

#### base
首当其冲的是配置这个选项，可参考[Base](https://vuejs.press/zh/reference/config.html#base)。这个选项是用于设置网站的基础路径，如果你准备发布到 `https://.github.io//` ，也就是说你的仓库地址是 `https://github.com//` ，则将 base 设置为 “//“。

#### 导航栏
这个选项是用于设置导航栏的.

显而易见的是，我的导航栏并没有什么特殊的设置，只是简单把写过的笔记进行分类整理，然后放在了导航栏中。

#### 侧边栏
对于侧边栏，我是选择了通过文件结构生成，毕竟因为是笔记站，这样配置起来，可以更方便的去查找笔记。不过随着之后内容的增多，也可能会考虑对不同笔记目录单独生成不同的侧边栏。

#### 评论
~~主要是因为懒~~ ，我选择直接将相关配置注释，关闭评论区，之后可能会考虑使用第三方评论系统。

#### SEO
这个主题的一大优点，便是自带SEO功能，且默认是开启的，可参考SEO。

对于其他功能，我也并没有去仔细关注，毕竟这个主题的配置项还是很多的，而且也不是每个人都需要的，对于笔记站来说，我觉得这些功能也并不是很重要，在之后的使用中，如果有需要，再去配置也不迟。

## 结语

这个笔记站的搭建，也是我第一次使用Vuepress，也是第一次使用Vue，所以在搭建过程中，也遇到了很多问题，也是在解决问题的过程中，学习了很多知识。在这里，也希望能够帮助到其他人，也希望能够帮助到自己。

在这次的配置过程中，确实让我整个人都得到了一个放松。前段时间状态确实不太行。在这次配置的过程中不断面对问题，解决问题，让我感到了久违的满足感和获得感。也让我重新找回了学习的乐趣。

感谢vuepress、vuepress-theme-hope等开源项目的开发者，感谢他们的付出，让我能够使用这么好的工具。最后，也感谢你能够看到这里，希望你能够喜欢这个笔记站。