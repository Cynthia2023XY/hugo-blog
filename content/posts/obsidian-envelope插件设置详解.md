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

---
### 1 将markdown链接替换为hugo格式：通过`](`定位
```json
"entry": "/\\]\\(([^)\\.]+)\\.md/",
"replace": "]({{< relref \"$1.md\" >}}"
```
**首先是匹配目标字符串：`"/\\]\\(([^)\\.]+)\\.md/"`**
`/`          `\\]`          `\\(`          `([^)\\.]+)`          `\\.`          `md/`
`\\]` 匹配字符`]`  (因为`]`是正则匹配的特殊字符，所以需要反斜杠转义)
`\\)` 匹配字符`(`  (因为`(`是特殊字符所以需要反斜杠转义)

`([^)\\.]+)`这是一个**捕获组**，匹配并捕获以下模式:
	`[^)\\.]`：匹配除了 `)` 和 `.` 之外的任意字符
	`[^...]` 表示匹配不在括号中的任意字符。
	`+`：表示匹配1次或多次前面的字符或模式

`\\.` 匹配字符`.`  (因为`.`是特殊字符所以需要反斜杠转义)

`md`匹配`md`

最后用开头的`/`和结尾的`/`把整个正则表达式包起来

---
**接着将匹配到的目标字符串替换为如下格式：`"]({{< relref \"$1.md\" >}}"`**
`](`：替换为字面意义上的文本 `](`。
`{{< relref "`：这是模板标签的开始，`relref` 是一个指令，用于创建相对引用的链接。
`$1`：这是一个反向引用，引用的是正则表达式中第一个**捕获组** `(` 和 `)` 之间的匹配内容。在这里，它将引用原始匹配的文件名（不包括 `.md` 扩展名）。
`.md`：文件扩展名 `.md`，表示Markdown文件。
`>}}`：模板标签的结束

---
#### 举例
markdown语法中的引用
```md
[example](file.md)
```
将被替换为hugo框架可解释的格式
```hugo
[example]({{< relref "document.md" >}})
```

---
### 2 YAML中文章封面图片地址`cover.image:`替换hugo格式
```json
"entry": "/cover\\.image/",
"replace": "cover:\\n    image"
```
`/cover\\.image/`匹配`cover.image`
替换为
```
cover:
    image
```
换成了严格的YAML格式（通过`.`来表示属性层级是TOML的语法，YAML不支持）

---
### 3 将Markdown格式的图片链接转换为相对路径的链接
```json
"entry": "/\\]\\(([^/\\)]+?)\\.(png|jpg|jpeg|webp|gif)/",
"replace": "](/images/$1.$2"
```
`.(png|jpg|jpeg|webp|gif)`可以匹配
```
.jpg 
.png
.jpeg
.webp
.gif
```

---
#### 举例
markdown语法中的图片显示
```
![alt text](/path/to/image.jpg)
```
应用上述正则表达式替换后，文本将变为：
```
![alt text](/images/path/to/image.jpg)
```

---
### 4 将Markdown格式的图片链接转换为相对路径的链接: 包含图片宽高
```json
"entry": "/\\!\\[\\[([^/\\]]+?)\\.(png|jpg|jpeg|webp|gif)\\|(\\d+)(x(\\d+))?\\]\\]/",
"replace": "{{< figure src=\"/images/$1.$2\"  width=\"$3\" height=\"$5\">}}"
```

#### 举例
```md
![[example.png|300x200]]
```
替换为
```
{{< figure src="/images/example.png" width="300" height="200">}}
```

---
### 5 将Markdown格式的图片链接转换为相对路径的链接: 包含图片宽高+图片标题
```json
"entry": "/\\!\\[\\[([^/\\]]+?)\\.(png|jpg|jpeg|webp|gif)\\|([^\\|]*?)(\\|(\\d+)(x(\\d+))?)?\\]\\]/",
"replace": "{{< figure src=\"/images/$1.$2\" caption=\"$3\" width=\"$5\" height=\"$7\">}}"
```

#### 举例
```
[[example.png|"A title"|300x200]]
```
替换为
```
{{< figure src="/images/example.png" caption="A title" width="300" height="200">}}
```

---
## Property key cheatsheet 属性键备忘录

Moreover, there are some properties keys that can be useful for your workflow.
The code below show the default settings, but feel free to change it to your needs in each notes!
此外，还有一些属性键可能对您的工作流程很有用。
下面的代码显示了默认设置，但请根据您的需要在每个笔记中自由更改！
>以下的这些属性设置会影响文件上传到github库的设置

It's also possible to use "dot" syntax, i.e. to denote nested keys with dots. 
For example, using `key.subkey: value`.
还可以使用“点”语法，即用点表示嵌套键。例如，使用 `key.subkey: value`。
>也就是说还支持toml语法
```yaml
share: true
dir: content/posts
path:  file.md # given as an example path
links:
  mdlinks: true
  convert: true
  internals: false
  nonShared: false
embed:
  send: false
  remove: keep
  char: ->
attachment:
  send: true
  folder: static/images
dataview: true
hardBreak: false
includeLinks: truerepo:
  owner: Cynthia2023XY
  repo: hugo-blog
  branch: main
  autoclean: false
copylink:
  base: https://hugo-blog.github.io/hugo-blog
```

---
### `share`: 这个关键字用来设置这个obsidian文档是否会被上传到github库 

This key is used to share a note with the plugin.
You could also use another shareKey based on the key set in « Manage other repo ». It allows you to separate your different repository. If the main and secondaries key are used, the main repo will be used.
> 如果设置自定义share关键字的话
> （例如自定义`myShareA`对应github库A，`myShareB`对应github库B，）
> 可以实现将一篇笔记上传到库A的同时，不上传到库B
> myShareA: true
> myShareB: false

---
### `dir: content/posts`: 这是上传到github库的根路径

如果修改的话，可以实现不同的用户向同一个github仓库中的不同文件夹递交文档
>（但是改了这个的话，对应的主题配置文件路径也需要改哦，不然主题不能正常渲染了）

假设github库中`content`文件夹下有两个子文件夹：`posts-Tom` 和 `posts-Bob`

那么Tom上传的md文件的yaml就设置为：
`dir: content/posts-Tom`

那么Bob上传的md文件的yaml就设置为：
`dir: content/posts-Bob`


---
### `path`: 可以设置上传文件的名字和相对根路径`content/posts`的位置
You can override all path settings using this key. 
The path will be relative to the root of your repository.

默认的话就是直接上传到`content/posts`文件夹下，名字就是自己的名字
但是如果写
```
path: ./ABC/file.md
```
就会上传到`content/posts`文件夹下的子文件夹`ABC`中，并且文件重命名为`file.md`

---
### `links`: 默认全部打开，对markdown的链接格式进行转换

1. `mdlinks`: Convert all `[[markdown alias]]`in `[alias](markdown)`
2. `convert`: Enable or disable the conversion of links. Disabling this will remove the `[[link]]`or `[](link)`syntax, while keeping the file name or the alternative text.
3. `internals`: Convert internals links to their counterpart in the website, with relative path. Disabled, the plugin will keep the internal link as is.
4. `nonShared`: Convert internal links pointing to a unshared file to their counterpart in the website, with relative path. Disabled, the plugin will keep the filename.

```yaml
links:
  mdlinks: true
  convert: true
  internals: false
  nonShared: false
```

1. `mdlinks`: 将所有的 `[[markdown alias]]` 转换为 `[alias](markdown)`。这意味着将Markdown的别名链接转换为标准的URL格式。
2. `convert`: 启用或禁用链接的转换。禁用此功能将移除 `[[link]]` 或 `[](link)` 语法，同时保留文件名或替代文本。
3. `internals`: 将内部链接转换为网站上的对应链接，并使用相对路径。如果禁用，插件将保持内部链接不变。
4. `nonShared`: 将指向未共享文件的内部链接转换为网站上的对应链接，并使用相对路径。如果禁用，插件将保留文件名。

---

### `embed`: 指定如何处理笔记中的嵌入笔记
```yaml
embed:
  send: false
  remove: keep
  char: ->
```

`send`: Send embedded note to GitHub
`remove`: Modify the aspect of the embedded files link. Can take the followed value:
	`remove true`: Delete the citation completely and leave an empty line
	`keep false`: Leave as in Obsidian
	`links`: Convert to links (delete or edit the "!")
	`bake`: Include the content of the embed (support blocks, heading and entire file)
`char`: Add a character(s) before the embedded links. Used only if you set "remove" to "links".

1. `send`: 将嵌入的笔记发送到GitHub，默认关闭。
2. `remove`: 修改嵌入文件链接的外观。可以取以下值：
    1. `remove`: `true` 删除引用并留下空行。
    2. `keep`: `false` 保持Obsidian中的状态。
    3. `links`: 转换为链接（删除或编辑感叹号）。
    4. `bake`: 包含嵌入内容（支持块、标题和整个文件）。
3. `char`: 在嵌入链接前添加一个或多个字符。仅当您将 "remove" 设置为 "links" 时使用。

---

### `attachment`: 如何处理笔记中的附件
默认设置将笔记中嵌入的附件（包括图片，文档等）上传到github库的`static/images`文件夹
>`static/images`文件夹和`content/posts`文件夹为并列关系
```yaml
attachment:
  send: true
  folder: static/images
```
1. `send`: Send all attachments to GitHub
2. `folder`: Change the default folder for the attachments

3. `send`: 将所有附件发送到GitHub。
4. `folder`: 更改附件的默认文件夹。

---
### `dataview`: 联动obsidian的dataview插件
Convert dataview queries to markdown.

---
### `hardbreak`: 将所有换行转换为Markdown的“硬换行”
Convert all linebreaks to markdown «hard break».

---
### `includeLinks`: 允许发送通过简单链接链接的文件
Allows sending files linked by a simple link, such as `[[markdown]]` or `[](markdown)`

### `shortRepo`: 允许使用在其他仓库设置中设置的仓库之一
Allow to use one of the repo set in other repo settings.

1. `owner`: Owner of the repo
2. `repo`: Repository name
3. `branch`: Branch of the repo
4. `autoclean`: Disable or enable autocleaning

---
### `title`: 设置或改变笔记的标题
Change the title of the note.

---
### `multipleRepo`：允许将同一篇笔记上传到多个github仓库

If you want to send your notes to multiple repository, you can use the `multipleRepo` key in your properties. The value of this key must be a list of repository. 
Each repository must have the following keys

`owner`
`repo`
`branch`
`autoclean`

The code below show an example based on your settings.
```yaml
multipleRepo:
   owner: Cynthia2023XY
     repo: hugo-blog
     branch: main
     autoclean: false
   owner: sandboxingRepo
     repo: my_second_blog
     branch: master
     autoclean: false
```
