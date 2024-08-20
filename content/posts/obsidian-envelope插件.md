---
share: true
title: obsidian-envelope插件设置详解
date: 
description: envelope插件用于将obsidian库中的文档一键推送到github的`content/posts`文件夹, 并且自动对obsidian格式的markdown语法进行正则表达式替换，使其能够正常被hugo网站解析为html
tags:
  - review_knowledge_obsidian
featured_image: ""
images: /images/notebook.jpg
categories: obsidian
comment: false
draft: false
---

#review/knowledge/obsidian 


---
>envelope插件用于将obsidian库中的文档一键推送到github的`content/posts`文件夹
>并且自动对obsidian格式的markdown语法进行正则表达式替换，使其能够正常被hugo网站解析为html


---
## 正则表达式替换

### 正则表达式替换完整代码
```json
"censorText": [
      {
        "entry": "/\\]\\(([^)\\.]+)\\.md/",
        "replace": "]({{< relref \"$1.md\" >}}",
        "flags": "",
        "after": true,
        "inCodeBlocks": false
      },
      {
        "entry": "/cover\\.image/",
        "replace": "cover:\\n    image",
        "flags": "",
        "after": false
      },
      {
        "entry": "/\\]\\(([^/\\)]+?)\\..(png|jpg|jpeg|webp|gif)/",
        "replace": "](/images/$1.$2",
        "flags": "",
        "after": true
      },
      {
        "entry": "/\\!\\[\\[([^/\\]]+?)\\.(png|jpg|jpeg|webp|gif)\\          (\\d+)(x(\\d+))?\\]\\]/",
        "replace": "{{< figure src=\"/images/$1.$2\"  width=\"$3\" height=\"$5\">}}",
        "flags": "",
        "after": false
      },
      {
        "entry": "/\\!\\[\\[([^/\\]]+?)\\.(png|jpg|jpeg|webp|gif)\\          ([^\\          ]*?)(\\          (\\d+)(x(\\d+))?)?\\]\\]/",
        "replace": "{{< figure src=\"/images/$1.$2\" caption=\"$3\" width=\"$5\" height=\"$7\">}}",
        "flags": "",
        "after": false
      }
```

