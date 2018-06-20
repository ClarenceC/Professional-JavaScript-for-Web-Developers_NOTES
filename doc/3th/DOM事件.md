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



