# DOM 事件

## DOM 事件流

JavaScript 和 HTML 之间的交互是通过事件实现的。最早的 DOM 事件是在 DOM2开始标准化的。

- DOM2 实现了浏览器事件的基础标准，但没有涵盖所有事件类型。

- DOM3 增加了 DOM事件 API，增加了包含 BOM 的事件操作。


在事件当中，有一个事件流的概念像下图。

<center>
    <img src="../../img/201817210161425.png" style="width:350px;">
</center>

- IE 事件冒泡

IE的事件流叫做 **事件冒泡(event bubbling)**，即触发事件开始时由最里面的元素接收，之后逐级向上传播到最上层节点。像上图从 6 传到 10。

- Netscap 事件捕获

Netscape 团队提出的另一种叫做 **事件捕获(event capturing)**，即角发事件开始从元素的最顶层父节点，一层层向下捕获，直到找到该解发事件的该元素。即上图从 1 捕获到 5。

由于老板本的不支持，因此很相对少人使用事件捕获，一般使用事件冒泡。

DOM2级事件流 包括三个阶段： 事件捕获阶段、处于目标阶段和事件冒泡阶段。

## 事件处理程序

事件处理程序包括事件程序的名字和事件处理程序。

```javascript
// 给元素 input 绑定 onclick 事件
<input type="button" value="Click Me" onclick="showMessage(event, this)">
<script type="text/javascript">
    function showMessage(event, input) { // 定义事件处理程序
        console.log('Hello world!')
        console.log(event) // 事件对象
        console.log(input) // 解发事件的元素本身，可以通过元素本身获取属性。
    }
</script>
```
这样定义处理程序，会创建一个封装着元素属性的函数，函数中会有一个局部变量事件对象 `event`。`this` 是指向当前事件的目标元素。不过这样定义 JavaScript 事件也是有缺点的，就是 HTML 和 JavaScript 代码耦合度很高，如果要更改代码就要更改 HTML 代码和 JavaScript 代码。

## DOM0 级事件处理程序

比事件绑定好的原因是，耦合度低，跨浏览器，脚本就能实现。缺点是：必须要完成加载 JS 脚本，才会有事件效果，不然未加载完成是触发不了事件。

```javascript
    const btn = document.getElementById('myBtn')
    btn.onclick = function() {  // 给 btn 添加 onclick 事件
        console.log('Clicked')
        console.log(this.id) // 'myBtn'
    }
    btn.onclick = null // 删除onclick 事件
```

## DOM2 级事件处理程序

DOM2 定义了两个方法用于处理指定和删除事件处理程序的操作,所有的 DOM 节点中都包含这两个方法，并接受3个参数。

- addEventListener()

```javascript
    const btn = document.getElementById('myBtn')
    btn.addEventListener('click', function() { // 添加 click 事件, 处理事件是匿名函数
        console.log(this.id)
    },false) // false 表示冒泡阶段才调用事件处理程序， true 表示在捕获阶段调用处理程序
```


```javascript
    const btn = document.getElementById('myBtn')
    btn.addEventListener('click', function(){ // 添加 click 事件1
        console.log(this.id)
    }, false)
    btn.addEventListener('click', function(){ // 添加 click 事件2
        console.log('Hello world!')
    }, false)
```
这里添加了两个 click 事件，解发的时候会按添加的顺序触发。

- removeEventListener()

在 `removeEventListener` 中是移除添加的解发事件，但是有一个问题如果是按上面方法，添加匿名函数来触发事件是没办法移除的。像下面例子

```javascript
    const btn = document.getElmentById('myBtn')
    btn.addEventListener('click', function(){
        console.log(this.id)
    },false)
    btn.removeEventListener('click', function(){ // 没有作用
        console.log(this.id)
    },false)
```
可以改成实名函数的方式

```javascript
    const btn = document.getElementById('myBtn')
    const handler = function(){ // handler 事件处理
        console.log(this.id)
    }
    btn.removeEventListener('click', handler, false) // work
```
如果不是特别需要，我们不建议在事件捕获阶段注册事件处理程序。


## IE 事件处理程序

- attachEvent()
- detachEvent()

`attachEvent`、`detachEvent` 和 `addEventListener`、 `removeEventListener` 使用上很像，不过要注意一点 `attachEvent` 上的 `this` 指向的是 `window`,`addEventListener` 指向上的是元素本身。

## DOM 中的事件对象 event

在 DOM0 或者 DOM2级中，绑定事件都会传入 `event` 对象。

```javascript
    // DOM2
    const btn = document.getElementById('myBtn')
    btn.onclick = function(event) {
        console.log(event.type) // 'click'
    }
    btn.addEventListener('click', function(event) {
        console.log(event.type) // 'click'
    }, false)

    // DOM0
    const handleOnClick = function(event) {
        console.log(event.type)
    }
    <input type="button" value="Click Me" onclick="handleOnCLick()" />
```

`event` 几个重要的属性与方法

- `event.eventPhase` 属性判断调事事件处于那个阶段 1 表示捕获阶段 2表示'处于目标'阶段 3 表示冒泡阶段

- `event.preventDefault()` 取消事件默认行为。

- `event.stopImmediatePropagation()` 取消事件的进一步捕获或冒泡，同时阻止任何DOM3事件处理程序被调用

- `event.stopPropgation()` 取消事件的进一步捕获或冒泡

- `event.target` 属性表示为当前事件的触发目标元素。

- `event.currentTarget` 属性为当前事件的注册元素。

- `event.type` 被触发的事件类型

- `event.cancelable` 表示是否可以取消事件的默认行为，如果为 true 则可以取消, false 则没有默认动作，或者不能阻止默认动作。

关于 `event.currentTarget` 和 `event.target` 和 `this` 的区别。

```javascript
    document.body.onclick = function(event) {
        console.log(event.currentTarget) // 注册事件的元素
        console.log(this) // 注册事件的元素
        console.log(event.target) // 触发事件的元素
    }
```


关于 `event.preventDefault()` 和 `event.stopPropgation` 和 `return false` 很容易分不清除区别

- `event.preventDefault()` 是取消阻止默认动作，比如表单事件的 submit。
- `event.stopPropgation` 是阻止继续冒泡或者阻止继续捕获。
- `return false` 是上面两个都会执行，还可以返回对象，跳出循环。

## IE 中的事件对象

在 IE中 `event` 对象会作为 window 对象的一个属性存在。

```javascript
    const btn = document.getElementById('myBtn')
    btn.onclick = function() {
        const event = window.event
        console.log(event.type) // 'click'
    }
```

- IE 的 `returnValue` 属性和 DOM 中的`preventDefault()` 作用相同，默认为true,设为 false 就可以取消事件的默认行为。

- IE 的 `event.cancelBubble` 属性和 DOM 中的 `stopPropagation()` 作用相同。默认为 false, 设为 true 就可以取消事件冒泡。

## 事件类型

DOM 里面有很多种类的事件类型，而 DOM3 中重新定义了事件模块。

- UI(User Interface, 用户界面)事件，当用户与页面上的元素交互时触发。
    * load：当页面完全加载后触发。
    * abort：用户停止下载过程时触发。
    * unload：当页面完全卸载后在 window 上面触发。
    * resize： 当窗口大小变化时在 window 上面触发。
    * scroll： 当用户滚动带条内容时在元素上面触发。

- 焦点事件，当元素获得或失去焦点时触发。
    * blur： 在元素失去焦点时触发，（**事件不冒泡**）
    * focus：在元素获得售楼 点时触发，（**事件不冒泡**）

- 鼠标事件，当用户通过鼠标在页面上执行操作时触发。
    * click： 在用户单击鼠标按钮时，或者按下回车键时触发。
    * dblclick：在用户双击鼠标按钮时触发。
    * mousedown：在用户按下了任意鼠标按钮时触发。
    * mouseenter：在鼠标从元素外部首次移动到元素范围内时触发，移动到后代元素上不触发。（**事件不冒泡**）
    * mouseleave： 在元素上方的鼠标光标移动到元素范围之外时触发，移动到后代元素上不触发。（**事件不冒泡**）
    * mousemove：鼠标在元素内部移动时重复地触发。
    * mouseout：鼠标指针位于一个元素的上方，然后移入另一个元素时触发。
    * mouseover：鼠标在元素外部首次移入另一个元素边界内时触发。
    * mouseup：在用户释放鼠标按钮时触发。

- 滚轮事件，当使用鼠标滚轮或设备时触发。
    * mousewheel： 当用户通过滚轮与页面交互就会触发。

- 文本事件&键盘事件，当在文档中输入文本时触发,当用户通过键盘在页面上执行操作时触发。
    * keydown：当用户按下键盘上的任意键时触发。
    * keypress：当用户按下键盘上的字twfy键时触发。
    * keyup： 当用户释放键盘上的键时触发。
    * textInput：当用户在可编辑区域中输入字符时，就会触发这个事件。(DOM3)

- 复合事件，当为 IME（Input Method Editor，输入法编辑器）输入字符时触发.
    * compositionupdate： 在向输入字段中插入新字符时触发。
    * compositionend： 在 IME 的文本复合系统关闭时触发，表示返回正常键盘输入状态。

- 变动事件，当底层 DOM 结构发生变化时触发。
    * DOMSubtreeModified：在DOM结构中发生任何变化时触发。这个事件在其他任何事件触发后都会触发。
    * DOMNodeInserted： 在一个节点作为子节点被插入到另一个节点中时触发。
    * DOMNODERemoved： 在节点从其父节点中被移除时触发。
    * DOMNodeInsertedIntoDocument： 在一个节点被直接插入文档或通过子树间接插入文档之后触发。
    * DOMNodeRemovedFormDocument： 在一个节点被直接从文档中移除或通过子树间接从文档中移除之前触发。
    * DOMAttrModified：在特性被修改之后触发。
    * DOMCharacterDataModified：在文本节点的值发生变化时触发。

- HTML5 事件，DOM规范里没有涵盖所有浏览器支持的所有事件。
    * contextmenu 事件，右键调出上下文，触发事件。
    * beforeunload 事件，在页面卸载前触发该事件。
    * DOMContentLoaded 事件，会在页面一切都加载完毕时触发，在形成完整的 DOM树之后就会触发，不理会图像，JavaScript CSS 其它资源文件。
    * readystatechange 事件，是用来提供与文档或元素的加载状态有关信息。
    * hashchange 事件，在 URL 的参数发生变化时通知开发人员。

## 事件如何管理内存和性能

过多的事件绑定会存在内存性能问题，如何管理内存和性能就变成是一个学术上的问题了。怎样解决呢？

### 事件季托

在多个需要事件处理的元素上层只注册一个处理事件，利用冒泡上传到父类组件时再处理。

```javascript
    <ul id="myLinks">
        <li id="goSomewhere">Go somewhere</li>
        <li id="doSomething">Do something</li>
        <li id="sayHi">Say hi</li>
    </ul>

    const list = document.getElmenetById('myLinks')
    list.addEventListener('click', function(event) {
        let event = event || window.event
        const target = event.target
        switch(target.id) {
            case 'doSomething':
                document.title = 'I changed the document\'s title'
                break
            case 'goSomewhere':
                location.href = 'http://www.wrox.com'
                break
            case 'sayHi':
                console.log('hi')
                break
        }
    },false)
```

### 移除事件处理程序

在错误移除事件处理程序，就会产生 空事件处理程序(dangling event handler),在残留在内存中，对内存性能造成影响。

对将会被`removeChild()`、`replaceChild()`、`innerHTML`移除或删除的元素，提前做好删除事件。

```javascript
    <div id="myDiv">
        <input type="button" value="Click Me" id="myBtn">
    </div>
    <script type="text/javascript">
        const btn = document.getElementById('myBtn')
        btn.onclick = function() {
            btn.onclick = null // 先移除事件处理程序
            document.getElementById('myDiv').innerHTML = 'Processing...' // 再对组件进行操作
        }
    </script>
```

另一种空事件处理程序情况发生在页面卸载，在页面卸载之前也最好调用 `onunload` 事件进行处理。

## 模拟 DOM 事件

DOM 事件一般是由 DOM 元素组件触发的，但是在某些时候也能通过 JavaScript 来触发特定的事件。

在触发事件之前我们先要创建这个事件对象 `event`。创建 `event`，可以在 document 对象上使用 `createEvent()` 来创建 `event` 对象，`createEvent()` 需要传事件字符参数。在 DOM3 中传参的字符串有所更改 DOM3 是单数形式。

| 事件描述 | DOM2             | DOM3|
|---|------------------------|-----|
| 一般化UI事件，在DOM3级中把鼠标和键盘都继承自UI事件。 | UIEvents             | UIEvent |
| 一般化的鼠标事件 | MouseEvents | MouseEvent |
| 一般化的 DOM 变动事件 | MutationEvents | MutationEvent  |
| 一般化的 HTML 事件 | HTMLEvents  | 没有对应的DOM3级事件，被分散到其他类别中 |


```javascript
    const btn = document.getElementById('myBtn')
    // 创建事件对象
    const event = document.createEvent('MouseEvents')
    // 初始化事件对象, 传入模拟事件的参数
    event.initMouseEvent('click',true, true, document.defaultView, 0, 0, 0, 0, 0, false, false, false, false, 0, null)
    // 通过 btn 触发事件
    btn.dispatchEvent(event)
```

其它类型创建触发也是这样，主要是创建事件对象和初始化事件对象有所不同。在 DOM3 能自定义事件。

```javascript
    const div = document.getElmentById('myDiv')
    // 创建处理函数
    const myeventHandle = function(event) {
        console.log('DIV: ' + event.detail)
    }
    // 添加处理事件
    div.addEventListener(myevent,myeventHandle,false)
    // 创建自定义事件对象
    const event = document.createEvent('CustomEvent')
    // 初始化自定义对象
    event.initCustomEvent('myevent', true, false, 'Hello world')
    // 传递对象触发事件
    div.dispatchEvent(event)
```

在 IE 中的模拟事件也有不同，兼容 IE 的时候需要注意了。

```javascript
    const textbox = document.getElementById('myTextbox')
    // 创建 IE 事件对象
    const event = document.createEventObject()
    // 初始化事件对象
    event.altKey = false
    event.ctrlKey = false
    event.shiftKey = false
    event.keyCode = 65
    // IE 下触发事件
    textbox.fireEvent('onkeypress',event)
```