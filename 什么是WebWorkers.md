---
sr-due: 2035-04-21
sr-interval: 3937
sr-ease: 270
tags:
  - review_knowledge_code_js
  - review_knowledge_八股
---

#review/knowledge/code/js #review/knowledge/八股 
Web Workers 提供了一种方式，允许JavaScript代码在主执行线程之外的后台线程中运行。[[什么是js的代码阻塞|什么是js的代码阻塞]] [[为什么js要设计成单线程语言|为什么js要设计成单线程语言]]
这意味着可以执行计算密集型或长时间运行的任务，而不会阻塞用户界面或影响前端的性能。Web Workers 对于提高复杂Web应用程序的响应性和性能非常有用。
[[常见单线程和多线程语言|常见单线程和多线程语言]]

---
### 基本概念

- **主线程隔离**：Web Worker 运行在与主JavaScript执行线程（通常是负责UI的线程）完全隔离的后台线程中。这意味着<u>Web Worker不能直接访问DOM或使用某些与主线程交互密切的Web API</u>（如`window`或`document`对象）。[[DOM查询操作|DOM查询操作]]
- **消息传递**：主线程和Web Worker之间的通信通过消息传递完成。这些消息以复制的方式传递，而不是通过共享内存的方式，这样可以避免并发问题。

---
### 使用Web Workers

1. **创建Web Worker**：通过指定一个包含JavaScript代码的脚本文件来创建一个新的Worker对象。
   ```javascript
   var myWorker = new Worker('worker.js');
   ```

2. **与Web Worker通信**：主线程和Web Worker可以通过`postMessage`方法发送消息，并通过`onmessage`事件处理器接收消息。
   ```javascript
   // 在主线程中
   myWorker.postMessage('Hello, worker!');

   // 在 worker.js 中
   onmessage = function(e) {
     console.log('Message received from main script:', e.data);
     postMessage('Hi from worker');
   };
   ```

3. **终止Web Worker**：可以从主线程调用`terminate()`方法来停止Web Worker的执行。
   ```javascript
   myWorker.terminate();
   ```

### 使用场景

- **计算密集型任务**：如图像处理、大数据分析等。
- **长时间运行的任务**：如拉取大量数据、执行长时间的数学计算等。
- **后台任务**：如定时检查更新、预先处理数据等，这些任务可以在用户与页面交互时在后台运行。

### 注意事项

- **兼容性**：虽然大多数现代浏览器都支持Web Workers，但在使用时仍需考虑浏览器兼容性。
- **性能考量**：虽然Web Workers可以提高应用的响应性和性能，但创建和维护Worker线程也会带来额外的性能开销。因此，只有在执行较为复杂或耗时的任务时才推荐使用。

Web Workers提供了一种强大的机制，通过允许Web应用程序利用多核CPU进行并行计算，从而扩展了JavaScript的能力，提高了应用的性能和用户体验。