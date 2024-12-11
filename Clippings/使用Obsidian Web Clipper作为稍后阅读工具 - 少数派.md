---
title: "使用Obsidian Web Clipper作为稍后阅读工具 - 少数派"
source: "https://sspai.com/post/94637"
author:
  - "[[KyrieII]]"
published: 2024-12-09
created: 2024-12-11
description: "Omnivore是我之前一直使用的稍后阅读软件。但是在今年它宣布要关闭服务。因此我开始寻找替代的软件。正好在不久前，Obsidian推出了网页剪藏的插件ObsidianWebClipper。在体验它的 ..."
tags:
  - "clippings"
---
Omnivore是我之前一直使用的稍后阅读软件。但是在今年它宣布要关闭服务。因此我开始寻找替代的软件。

正好在不久前，Obsidian推出了网页剪藏的插件Obsidian Web Clipper。在体验它的过程中，我想到我是否可以直接使用Obsidian作为稍后阅读工具。

经过简单的尝试，我形成了一个用Obsidian Web Clipper来剪藏，并在Obsidian中阅读的Read Later的流程。

\# 用Obsidian Web Clipper剪藏网页

从插件商店中安装Obsidian Web Clipper之后，我们首先需要在Web Clipper设置中新建一个模版，用来保存网页。这里比较重要的几个设置包括笔记存放的Vault和文件夹。我这边是专门在Vault里新建一个叫做ReadLater的文件夹专门用来存放所有稍后阅读的文章。这里我还改了一下默认的tag，我把每一个新剪藏都打上一个readlater的tag。

![](https://cdnfile.sspai.com/2024/12/08/ee07b13f262f9e62fed9ba3d63130c38.png?imageView2/2/format/webp)

Obsidian Web Clipper设置

在设置完成模板之后，在一个需要剪藏的网页上点击Web Clipper插件，然后Add to Obsidian就可以将这个网页剪藏成一个Note。之后我们直接在Obsidian中阅读就可以了。

![](https://cdnfile.sspai.com/2024/12/08/aebc5b89568d569edb55b7587794b856.png?imageView2/2/format/webp)

剪藏网页

![](https://cdnfile.sspai.com/2024/12/08/4ed74420a99b02f4774deb21c79ff7d8.png?imageView2/2/format/webp)

剪藏后的文档

但是只是把所有要读的文章放到一个文件夹里，不够优雅。我希望能够有一个List来把所有没读的和读过的文章都列出来，并且可以直接点击链接来打开相应的文章。好在这个需求在Obsidian中并不难实现。

用Dataview插件生成Reading List

我们可以通过dataview这个插件来实现这一功能。dataview是一个第三方的插件，它可以让我们对Obsidian的Vault像数据库那样query。
可以用tag来区分文章是还没读的和已经读完的。在剪藏的时候设置的是默认打上readlater的tag。在我们读完之后，可以把tag改成archived。
我们再新建一个笔记，然后在里面输入如下代码，就可以做两个表格，一个是未读的文章列表，另一个是已读的文章列表。它的意思就是把所有带上tag readlater 和 archived 的笔记都汇总到一个表格里，表格的列分别是标题，作者，创建时间，tag。这些都是来源于note的元数据，可以任意更改。

```null
\`\`\`dataview
TABLE title, author, created, tags
FROM #readlater 
\`\`\`

\`\`\`dataview
TABLE title, author, created, tags
FROM #archived 
\`\`\`
```

最终的效果就是这样。点击最左边列的链接就可以转到对应文章进行阅读。

![](https://cdnfile.sspai.com/2024/12/08/16d38dd33456797cb88c11292851ba0b.png?imageView2/2/format/webp)

Reading List

\# 优缺点

我本身是obsidian重度用户，这样做的优点省去了从其他readlater软件备份到obsidian的步骤。如果平常是保持obsidian的vault在多端同步的话，这样也解决了同步的问题。另外就是完全免费。

目前体验下来最大的缺陷是部分网页在剪藏的时候图片无法显示。它是把图片以链接的形式剪藏的，所以原因主要在源网页，这个比较难解决。

总得来说，这套方案我体验下来认为比较好地解决了我的需求，也供有类似需求的各位参考。
