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

## dataTransfer 对象

`dataTransfer` 对象，用于从被拖动的元素向放置目标传递字符串格式。

```javascript
// 设置接收文本字符串
event.dataTransfer.setData('text','some text')
var text = event.dataTransfer.getData('text')

// 设置和接收 URL
event.dataTransfer.setData('URL','http://www.wrox.com/')
var url = event.dataTransfer.getData('URL')
```

## `dropEffect` 和 `effectAllowed`

通过 **dropEffect** 可以知道被拖动的目标元素能够有那些放置行为。下面是 `dropEffect` 的值。

- none 不能把拖动的元素放在这里。
- move 应该把拖动的元素移动到放置目标。
- copy 应该把拖动的元素复制到放置目标。
- link 表示放置目标会打开拖动的元素 有URL。

`dropEffect` 要搭配 **effectAllowed** 属性才有用。 `effectAllowed` 是允许有那些的放置行为。

可以拖动的元素需要设计 `draggable` 属性

```javascript
// 不能拖动图像
<img src='smile.gif' draggable='false' alt='Smiley face'/>
// 可以拖动的元素
<div draggable='true'>...</div>
```

## 媒体元素
插入视频元素使用 `video`,插入音频元素使用 `audio`

```javascript
// 视频元素
<video src='conference.mpg' id='myVideo'>Video player not available.</video>
// 音频元素
<audio src='song.mp3' id='myAudio'>Audio player not available.</audio>
```

当然可以自定义视频和音频元素的组件，视频和音频的组件也具有很多触发的事件这里不一一详述了。

## 历史状态管理

在浏览器历史状态中，每次操作不一定会打开一个全新的页面，因此历史状态不是每一次都能记录到。

脚本可以通过 `pushState()` 方法添加历史状态记录到历史状态栈。`pushState()` 接收三个参数： 状态对象、新状态的标题和可选的相对 URL。
```javascript
history.pushState(
    {
        name: 'Nicholas'
    },
    'Nicholas \s page',
    'nicholas.html'
)
```

URL 状态的变化会触发 `hashchange` 事件。
历史状态栈的变化会触发 `popstate` 事件。

history 还有一些更改历史状态栈的方法
- replaceState() 更换历史栈状态
- pushState() 压进新历史栈


