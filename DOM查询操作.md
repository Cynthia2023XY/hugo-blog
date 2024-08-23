---
sr-due: 2024-12-18
sr-interval: 161
sr-ease: 230
tags:
  - review_knowledge_code_html
  - review_knowledge_code_js
  - review_knowledge_八股
---

#review/knowledge/code/html
#review/knowledge/code/js 
#review/knowledge/八股

- document.querySelector() 选择第一个符合的元素
- document.querySelectorAll() 选择所有符合的元素

它们选择的对象可以是标签，可以是类(需要加点)，可以是ID(需要加#)
```ad-seealso
- 示例用法：如果要选择文档中<u>第一个</u>类名为 "example" 的元素，可以这样使用：`document.querySelector('.example')`。
- 示例用法：如果要选择文档中<u>所有</u>类名为 "example" 的元素，可以这样使用：`document.querySelectorAll('.example')`。
```

- **返回值类型**：`querySelector()` 返回一个单个元素（如果匹配到的话），而 `querySelectorAll()` 返回一个 NodeList，即使只有一个元素匹配也是如此。
- **返回结果**：`querySelector()` 返回的是匹配的第一个元素，而 `querySelectorAll()` 返回的是匹配的所有元素。
- **性能**：通常来说，`querySelector()` 的性能比 `querySelectorAll()` 更好，因为它只需要找到第一个匹配的元素就可以停止搜索了，而 `querySelectorAll()` 需要找到所有匹配的元素。因此，如果只需要选择单个元素，应优先使用 `querySelector()`

```ad-help
如果 `document.querySelector()` 和 `document.querySelectorAll()` 都没有查询到对应的元素，它们的返回值都将是 `null`。在这种情况下，它们的返回值没有区别，都表示没有找到匹配的元素。
```

---


### 关于DOM树的生成
![[浏览器解析并渲染网页内容的过程|浏览器解析并渲染网页内容的过程]]