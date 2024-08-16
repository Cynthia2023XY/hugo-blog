---
date: 2024-08-14
share: true
title: hugo入门学习
description: 本文为hugo入门学习的笔记，主要涵盖hugo文件夹的层级结构和文档开头YAML格式等相关内容
tags:
  - review_knowledge_code_practice
  - review_knowledge_code_css
---
#review/knowledge/code/practice 
#review/knowledge/code/css 

[使用 Obsidian 免费建个人博客 | PrintLove](https://www.printlove.cn/obsidian-blog/)


### 插件的设置
`setting.json`复制导入到`envelope`插件
```json
{
  "github": {
    "branch": "main",
    "automaticallyMergePR": true,
    "dryRun": {
      "enable": false,
      "folderName": "github-publisher"
    },
    "tokenPath": "%configDir%/plugins/%pluginID%/env",
    "api": {
      "tiersForApi": "Github Free/Pro/Team (default)",
      "hostname": ""
    },
    "workflow": {
      "commitMessage": "[PUBLISHER] Merge",
      "name": ""
    },
    "verifiedRepo": true
  },
  "upload": {
    "behavior": "yaml",
    "defaultName": "content/posts",
    "rootFolder": "content",
    "yamlFolderKey": "dir",
    "frontmatterTitle": {
      "enable": false,
      "key": "title"
    },
    "replaceTitle": [],
    "replacePath": [],
    "autoclean": {
      "enable": false,
      "excluded": []
    },
    "folderNote": {
      "enable": false,
      "rename": "index.md",
      "addTitle": {
        "enable": false,
        "key": "title"
      }
    },
    "metadataExtractorPath": ""
  },
  "conversion": {
    "hardbreak": false,
    "dataview": true,
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
        "entry": "/\\]\\(([^/\\)]+?)\\.(png|jpg|jpeg|webp|gif)/",
        "replace": "](/images/$1.$2",
        "flags": "",
        "after": true
      },
      {
        "entry": "/\\!\\[\\[([^/\\]]+?)\\.(png|jpg|jpeg|webp|gif)\\|(\\d+)(x(\\d+))?\\]\\]/",
        "replace": "{{< figure src=\"/images/$1.$2\"  width=\"$3\" height=\"$5\">}}",
        "flags": "",
        "after": false
      },
      {
        "entry": "/\\!\\[\\[([^/\\]]+?)\\.(png|jpg|jpeg|webp|gif)\\|([^\\|]*?)(\\|(\\d+)(x(\\d+))?)?\\]\\]/",
        "replace": "{{< figure src=\"/images/$1.$2\" caption=\"$3\" width=\"$5\" height=\"$7\">}}",
        "flags": "",
        "after": false
      }
    ],
    "tags": {
      "inline": true,
      "exclude": [],
      "fields": []
    },
    "links": {
      "internal": false,
      "unshared": false,
      "wiki": true,
      "slugify": "lower"
    }
  },
  "embed": {
    "attachments": true,
    "overrideAttachments": [],
    "keySendFile": [],
    "notes": false,
    "folder": "static/images",
    "convertEmbedToLinks": "keep",
    "charConvert": "->",
    "forcePushAttachments": [],
    "useObsidianFolder": false
  }
}
```

---

### obsdian paper mod主题参考文章模版（可选）
```yaml
---
date: "{{date}}" # 创建时间，我这边生成的格式是 YYYY-MM-DDTHH:mm:ssZ
tags: 
	- 标签1
	- 标签2
title: "{{title}}"
slug: "{{time}}" # 自定义 URL 中文章的访问名称，默认用时间戳填充模板格式为X
share: false  # 配合 Github Publisher插件用的,true表示 obsidian 的文章可以发布
canonicalURL: "" # 之前文章在其他地方被发布的地址，避免搜索引擎重复，设置了该属性会优先展示 canonicalURL 执行的文章
keywords:   # 用于 SEO 优化，也可以不配置该内容默认会使用 tags 的内容
	- 关键字1
	- 关键字2
description: "" # 文章的描述 SEO 优化，为空时默认会截取文章前面的内容
series: "系列" # 系列文章
lastmod:  # 文章最后更新的时间
lang: "cn" # 默认不用写，配置文件会设置默认 cn 中文，en 英文等等
cover.image: "" # 文章封面图片地址
author: # 作者名称
dir: "posts" # 搭配 Github Publisher 插件设置文章上传的目录
---
```

- title 文章标题
- date 时间
- draft 是否草稿，发布时填写为 false（默认），如果为 true，则不可访问
- categories 分类
- tags 标签，对应 meta 的 keywords
- description 文章描述,不写默认截取文章内容
- notshow 为 true 则不显示在列表，但可以通过链接访问
- featured 为 true 标记为精选文章
- url 自定义访问地址


---

## hugo官方网站-目录结构：每个文件夹都是干啥的
[目录结构 | Hugo官方文档](https://hugo.opendocs.io/getting-started/directory-structure/)

### `archetypes` 目录包含用于新内容的模板: 里面有一个官方的默认模版，也可以添加新的自定义模版

里面的`default.md`会在项目构建时作为创建第一篇post的模版
使用以下格式，Hugo会创建以下内容文件：
`content/posts/my-first-post.md`
```yaml
---
date: "2023-08-24T11:49:46-07:00"
draft: true
title: 我的第一篇文章
---
```

---
也可以添加新的自定义模版，比如`posts.md`
```
archetypes/
├── default.md
└── posts.md
```

---
#### 只有一个自定义模版时，新建的post默认使用这个模版

Hugo在项目根目录中的`archetypes`目录中查找模版
如果不存在，则回退到主题或已安装模块中的`archetypes`目录。
**特定内容类型的模版优先于默认的模版`default.md`**。

例如，使用以下命令：
```sh
hugo new content posts/my-first-post.md
```

模版的查找顺序为：
1. archetypes/posts.md
2. archetypes/default.md
3. themes/my-theme/archetypes/posts.md
4. themes/my-theme/archetypes/default.md

如果这些都不存在，Hugo将使用内置的默认模版。

---

#### 使用多个自定义模版：使用`--kind`命令

在创建内容时，使用`--kind`命令来指定自定义模版。

例如，假设您的站点有两个部分：文章和教程。为每个内容类型创建一个模版：
```text
archetypes/
├── articles.md
├── default.md
└── tutorials.md
```

使用articles模版创建文章：
```sh
hugo new content articles/something.md
```

使用tutorials模版创建文章：
```sh
hugo new content --kind tutorials articles/something.md
```

---

### config目录：对于复杂项目会有这个文件夹，对于简单项目只有单个 `hugo.toml` 配置文件

`config` 目录包含站点配置，可能分为多个子目录和文件。对于配置较少或不需要在不同环境中以不同方式运行的项目，项目根目录中的单个 `hugo.toml` 配置文件就足够了
[json、yaml、toml作为配置文件的语法区别]({{< relref "json%E3%80%81yaml%E3%80%81toml%E4%BD%9C%E4%B8%BA%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9A%84%E8%AF%AD%E6%B3%95%E5%8C%BA%E5%88%AB.md" >}})

---
### `content/posts`目录：md文件存放处

理论上可以一个github仓库
content目录下整上好几个子文件夹
比如`post1` `post2` ...

就可以实现不同用户的obsidian仓库配合envelope插件推送到同一个github仓库
一个vercel站点部署上可以显示好几个人的文章，并且应用不同的css配置

---

### `static`目录：放笔记里的图片

---
### `themes` 目录包含主题，应该是渲染所用的CSS？

我用的是ananke主题
也可以用多个主题

---

## 文件分析

### ananke主题文件夹层级
{{< figure src="/images/Pasted image 20240815195508.png"  width="404" height="">}}



---

### diary主题参考模版分析
```yaml
---
title: "这里是大写的文章标题"
date:
description: "这里是文章的描述"
tags: [标签1，标签2]
featured_image: ""
images: "/images/notebook.jpg" #是文章封面的路径
categories: Todo #其他种类也行
#能否评论
comment: true
#是不是草稿，如果是的话不展示
draft: true
---
```
