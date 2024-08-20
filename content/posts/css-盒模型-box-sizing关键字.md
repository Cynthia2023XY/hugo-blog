---
title: CSS盒模型详解
share: true
comment: false
draft: false
date: 2024-08-20
categories: CSS
description: "CSS盒模型：box-sizing关键字，默认为`box-sizing: content-box;`标准盒模型，也可以设置`box-sizing: border-box;`为怪异盒模型"
sr-due: 2024-08-04
sr-interval: 89
sr-ease: 270
tags:
  - review_knowledge_code_css
  - review_knowledge_八股
---


#review/knowledge/code/css
#review/knowledge/八股

一个元素的盒模型由四个部分组成：
1. 内容（content）
2. 内边距（padding）
3. 边框（border）
4. 外边距（margin）

CSS 盒模型有两种：标准盒模型和IE盒模型（也称为怪异盒模型）
- **标准盒模型**：在此模型中，<u>元素的宽度和高度仅包括内容区域</u>。内边距、边框和外边距在内容区域之外计算，会额外增加元素的总尺寸。可以通过设置 `box-sizing: content-box` 来应用此模式。

- **IE盒模型（怪异盒模型）**：在此模型中，<u>元素的宽度和高度包括内容、内边距和边框，但不包括外边距</u>。这意味着元素的尺寸计算方式不同，更容易控制元素的实际大小。可以通过设置 `box-sizing: border-box` 来应用此模式


```ad-seealso
IE盒模型（怪异盒模型）的名称与IE（Internet Explorer）浏览器有关。
这种盒模型的命名来源于早期的IE浏览器在处理盒模型时与标准不一致的方式。

在CSS早期版本中，W3C定义了一种盒模型，即所谓的“标准盒模型”。根据这个标准，元素的`width`和`height`属性仅包括内容区域，不包括内边距（padding）、边框（border）和外边距（margin）。

然而，IE浏览器在实现盒模型时采用了不同的解释：在IE的盒模型中，元素的`width`和`height`包括了内容、内边距和边框，但不包括外边距。这导致了在不同浏览器之间布局的不一致性。

为了解决这一问题，CSS3引入了`box-sizing`属性，允许开发者选择使用哪种盒模型：

- `box-sizing: content-box`：应用“标准盒模型”，元素的尺寸（宽度和高度）仅包括内容区域。
- `box-sizing: border-box`：应用“IE盒模型”或“怪异盒模型”，元素的尺寸包括内容、内边距和边框。
```


```ad-hint
如果不声明DOCTYPE， 那么浏览器会默认使用怪异模式来渲染网页，
但是如果不声明CSS盒模型的格式，那么默认会用标准盒模型来渲染
```
![DOCTYPE(⽂档类型) 的作⽤](DOCTYPE(%E2%BD%82%E6%A1%A3%E7%B1%BB%E5%9E%8B)%20%E7%9A%84%E4%BD%9C%E2%BD%A4.md#^95291f)