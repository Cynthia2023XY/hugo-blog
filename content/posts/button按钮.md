---
share: true
title: button按钮的原生样式练习
date: 
description: 使用原生css实现鼠标悬浮在按钮上时的交互：包括水波扩散效果，横向波纹效果，下压立体按钮效果，悬停变色效果，悬停阴影效果，悬停箭头效果
categories: CSS
comment: false
draft: false
tags:
  - review_knowledge_code_css
---

#review/knowledge/code/css 

### 1. 水波扩散效果
```html
<div class="niceButton1"> <span>click me</span> </div>
```

```css
  .niceButton1 {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    box-sizing: border-box;
    width: 279rpx;
    height: 80rpx;
    padding: 24rpx;
    overflow: hidden;
    font-weight: 700;
    font-size: 26rpx;
    text-align: center;
    border-radius: 65rpx;
    &::after {
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      background-image: radial-gradient(circle, #ccc 10%, transparent 10.1%);
      transform: scale(10);
      opacity: 0;
      transition: all 0.45s;
      content: '';
    }

    &:active::after {
      transform: scale(0);
      opacity: 0.5;
      transition: 0s;
    }
  }
  .chat-now {
    color: #222;
    border: 1px solid #222;
  }
```
### 2. 横向波纹效果
### 3. 下压立体按钮效果
### 4. 悬停变色效果
### 5. 悬停阴影效果
### 6. 悬停箭头效果