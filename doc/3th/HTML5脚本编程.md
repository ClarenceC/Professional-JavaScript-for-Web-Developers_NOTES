# HTML5 脚本编程
## 跨文档消息传递

跨文档消息传送 (cross-document messaging)， 简称 XDM 意思是指在来自不同域的页面间传递消息。XDM 把这种机制规范化，让我们能稳妥又简单地实现跨文档通信。

XDM 的核心是 postMessage() 方法。在 HTML5 规范中,目的都是只有一个： 向另一个地方传递数据。"另一个地方有两种情况"：

1. 包含在当前页面中的 `<iframe>` 元素。
2. 由当前页面弹出的窗口。

```javascript
    var iframeWindow = document.getElementById('myframe').contentWindow
    // 两个参数，一个是字符串消息另一个是接收方的域名
    iframeWindow.postMessage('A secret','http://www.wrox.com')
```

在后来的浏览器都实现了 `postMessage()` 第一个参数允许传入任何数据结构，但是并非所有浏览器都有实现，所以传字符串的时候，最好还是要 `JSON.stringify()` 和 `JSON.parse()` 来处理字符串的问题。

## 原生拖放

拖放的过程，重点在于拖放事件

1. dragstart 按下鼠标并开始移动元素时，触发事件
1. drag 触发 dragstart 事件后，随即会触发 drag 事件.
1. dragend 停止拖动时放到有效的目标里，会触发 dragend 事件。

当某个元素被拖动到一个有效的放置目标上时会触发下列事件。

1. dragenter 元素被放置到目标上时，会触发 dragenter 事件。
1. dragover 在被拖动的元素还在放置目标的范围内移动时，就会持续触发该事件。
1. dragleave 或 drop 元素被拖出放置目标元素会触发 dragleave 事件。

```javascript
var droptarget = document.getElementById('droptarget')
// 自定义 dragover 事件
droptarget.addEventListener('dragover',function(event) {
    event.preventDefault()
})
// 自定义 dragenter 事件
droptarget.addEventListener('dragenter', function(event) {
    event.preventDefault()
})
```