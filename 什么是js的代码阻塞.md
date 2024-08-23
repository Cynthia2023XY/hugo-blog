---
sr-due: 2025-05-11
sr-interval: 305
sr-ease: 270
tags:
  - review_knowledge_code_js
  - review_knowledge_八股
---

#review/knowledge/code/js #review/knowledge/八股 

代码阻塞问题通常出现在同步代码执行中，尤其是当执行耗时操作（如复杂计算、文件操作、网络请求等）时。<u>由于 JavaScript 是单线程的，这意味着在任何给定时刻，只能执行一段代码</u>。
[[为什么js要设计成单线程语言|为什么js要设计成单线程语言]]
#### 如果某段代码执行时间过长，它会阻塞后续代码的执行，导致应用程序响应变慢，甚至出现无响应的情况，这对于需要与用户互动的Web页面来说是不可接受的。

[[死锁Deadlock|死锁Deadlock]]
[[解决线程死锁DeadLock|解决线程死锁DeadLock]]
[[多线程操作的解决方案-同步机制+锁机制|多线程操作的解决方案-同步机制+锁机制]]
