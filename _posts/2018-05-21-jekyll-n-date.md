---
layout: post
title: "jekyll对日期的敏感"
date: 2018-05-21 20:17:01 +800
categories: tutorial
---

0. 就在我写完上篇文章上传后却没有文章内容

1. 重新生成

 <code>jekyll build< /code>

 <code>jekyll server< /code>

 结果没有奏效

2. 更改 `layout` 

重复步骤一没有改变结果

3. 更改日期

在网络上查找资料后看到一种说法，文章的时间如果是“未来”的话 `jekyll` 不会渲染，更改时间，也没有奏效。

4. 找基本模板

在一个空文件夹中执行 <code>jekyll new dir< /code> 发现自带的 `Markdown` 文件的前缀是日期，将自己的更改的和它一样，完美运行。 