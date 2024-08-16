---
sr-due: 2024-11-25
sr-interval: 133
sr-ease: 270
tags:
  - review_knowledge_code_js
---

#review/knowledge/code/js

因为JSON 是⼀种<font color="#f79646">数据格式</font>，目的就是短小精悍减少冗余，所以它是不⽀持注释的。
> 但是诸如 json5 等插件可以让 JSON 来⽀持注释，当然也有其他技巧（⽐如 JSON.minify，{"comment":"foobar"}）来让JSON间接⽀持 注释。
> **程序员：我硬要注释！**

```ad-seealso
JSON的设计者（Douglas Crockford）意图让JSON尽可能的简单。
注释会增加解析器的复杂性，并且可能会被滥用（例如，将注释用作数据的一部分），这与JSON作为一种数据格式的初衷相悖
```

---

```ad-seealso
YAML 是 "YAML Ain't a Markup Language"（YAML 不是一种标记语言）的递归缩写。在开发的这种语言时，YAML 的意思其实是："Yet Another Markup Language"（仍是一种标记语言）。

YAML 的语法和其他高级语言类似，并且可以简单表达清单、散列表，标量等数据形态。它使用空白符号缩进和大量依赖外观的特色，特别适合用来表达或编辑数据结构、各种配置文件、倾印调试内容、文件大纲（例如：许多电子邮件标题格式和YAML非常接近）。

YAML 的配置文件后缀为 .yml，如：runoob.yml 。
```

YAML 最常见的用途之一是创建配置文件。
> 相比 JSON，因为 YAML 有更好的可读性，对用户更友好，所以通常建议用 YAML 来编写配置文件，尽管它们在大多数情况下可以互换使用。

除了在 Ansible 中使用之外，YAML 还用于 Kubernetes 资源和部署。

使用 YAML 的一大好处是，YAML 文件可以添加到源代码控制中，比如 Github，这样就可以跟踪和审计变更。

---

```ad-note
title: YAML基本语法

- 大小写敏感
- 使用缩进表示层级关系
- 缩进不允许使用tab，只允许空格
- 缩进的空格数不重要，只要相同层级的元素左对齐即可
- '#'表示注释
```
