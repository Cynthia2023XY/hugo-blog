---
sr-due: 2025-09-10
sr-interval: 427
sr-ease: 270
tags:
  - review_knowledge_code_js
  - review_knowledge_八股
---

#review/knowledge/code/js
#review/knowledge/八股
### 自锁（Deadlock）

**定义**：自锁是指在多线程或多进程的环境中，几个进程或线程因为互相等待对方持有的资源而无法继续执行的一种情况。每个进程或线程持有一部分资源，同时等待获取其他进程或线程持有的资源，导致所有相关进程或线程都无法继续执行。
```ad-hint
类似于三个和尚没水喝，小和尚拿着扁担，大和尚拿着桶，结果大家都没有水喝
```

**特点**：
- 出现在多线程或多进程编程中。
- 涉及资源分配和同步问题。
- 解决方法包括资源分配策略、锁定顺序、死锁检测与恢复等。

![[解决线程死锁DeadLock|解决线程死锁DeadLock]]