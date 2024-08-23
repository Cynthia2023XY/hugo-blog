---
date: 2024-08-23
share: true
title: js和ts区别
description: js和ts区别
categories: Obsidian
comment: false
draft: false
tags:
  - review_knowledge_code_js
  - review_knowledge_code_ts
---


#review/knowledge/code/js
#review/knowledge/code/ts
TypeScript 是 JavaScript 的超集，添加了静态类型系统和一些额外的语言特性，以提高代码的可读性、可维护性和安全性

1. TypeScript 是由<u>微软开发和维护的开源项目</u>，与现有的 JavaScript 生态系统兼容，并且可以轻松地与其他 JavaScript 库和框架集成
2. JavaScript 代码直接在浏览器中执行，不需要额外的编译步骤。<u>TypeScript 代码需要先经过编译成 JavaScript 才能在浏览器中执行</u>。
3. JavaScript 是一种<u>动态类型语言</u>，变量的类型在运行时才确定，开发者可以随时将一个变量从一个类型转换为另一个类型。TypeScript 是 JavaScript 的超集，添加了静态类型系统，<u>ts在编写代码时需要明确指定变量的类型，并且在编译时会进行类型检查</u>，以确保代码的类型安全性。
4. TypeScript 是一种<u>更严格的语言，具有更多的语法规则和约束</u>。它提供了一些额外的语言特性，如接口、枚举、泛型等，以增强代码的可读性和可维护性。

---

### TS也是单线程语言，可能是为了兼容js生态做了妥协
[[为什么js要设计成单线程语言|为什么js要设计成单线程语言]]