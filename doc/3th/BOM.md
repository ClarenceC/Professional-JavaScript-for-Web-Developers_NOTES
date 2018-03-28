# BOM

 ECMAScript 是 JavaScript 的核心，如果在浏览器中使用 JavaScript 那核心就是 BOM， BOM 提供了浏览器的核心访问功能和与浏览器的互操作性，以前各浏览器提供商都会按照自己的想法去扩展 BOM ，不过在最新的 BOM 的标准化已经在 HTML5 加入到规范中。

BOM 下的全局使用对象

 1. ## window 对象

window 对象比较特殊，在浏览器 BOM 当中有两层含义：
1. 在BOM 中，`window` 对象它表示浏览器的一个实例。
2. 同时 `window` 又代表着 ECMAScript 的核心全局作用对象 `Global` 对象。

因此如果你在全局作用域下声明变量或者函数，都会继承 ECMAScript 里面的属性和方法。不过定义全局变量和定义在 `window` 下面的属性还是有区别的：

```javascript
    var age = 29 // 这样定义会把 age [[Configurable]] 的特性设置为 false, 这样delete 不了。
    window.color = "red"

    delete window.age // 返回 false， 定义在全局作用域下变量不能删除。
    delete window.color // 返回 true, 定义在 window 对象属性下可以通过 delete 删除。
    console.log(window.age) //age
    console.log(window.color) // undefined
```

## 框架 Frame

在框架下每个框架都拥有自己的 window 对象,并保存在 `frames` 集合中。

访问框架下的 `window` 对象有下面几种方法:

- window.frames[0] // 访问frames数组的索引。
- window.frames['topFrame'] // 通过框架 frame 的 name 值属性来访问。
- top.frames[0] // 通过 top 来访问 frames 数组， top 代表最外层的 window 对象。
- top.frames['topFrame'] // 一般不建议使用 window ，因为 window 你不知道何时代替那个 框架的对象。

如果框架下面还有框架那就要注意了

```javascript
    var red = top.frames['rightFrame'].frames['redFrame'] // 要在框架下面再使用 frames 数组来访问
    red.parent === top.frames['rightFrame'] // 子框架的父类等于，最上层 top 获取的第一层框架
    window.self == window.window // true self 对象等于 window自身。
    self == window // true
    parent == self // 一般用来判断 是不是 iframe 里面，如果在 iframe 内层会返回 false, 否则返回 true
```
框架包括下面三个可用框架 Html 组件:

1. `frameset` 它称为框架标记，一般是用来告知 HTML 文件是框架模式，并设置可视窗口怎样划分。 （一般不建议使用）
2. `frame` 这个是设置具体的框架窗口中的参数属性。
3. `iframe` 这是可以在一个页面中嵌入一个框架窗口。 

## 窗口位置

window 的属性下有一些属性能访问到窗口位置如下：
1. `window.screenLeft` 用来显示最外层 `window` 窗口对象与显示器屏幕左边的距离。
```javascript
    var leftPos = (typeof window.screenLeft == "number") ?
                    window.screenLeft : window.screenX // 如果 screenLeft 不行就使用 screenX 在 Opera 下兼容。
```

2. `window.screenTop` 用来显示最外层 `window` 窗口对象与显示器屏幕顶端到屏幕顶端的距离。

```javascript
    var topPos = (typeof window.screenTop == "number") ?
                    window.screenTop : window.screenY // 兼容 Opera 的 screenY
```

3. 给定新坐标将窗口移动可以使用 `window.moveTo()` 方法。
4. 基于原坐标移动多少尺寸使用 `window.moveBy()` 方法。

## 窗口大小

1. innerWidth 表示浏览器内框视图宽度。
```javascript
    var pageWidth = window.innerWidth
    if(typeof pageWidth != "number") {
        if(document.compatMode == "CSS1Compat") {
            pageWidth = document.documentElement.clientWidth // 标准模式下
        } else {
            pageWidth = document.body.clientWidth // 混杂模式下有作用
        }
    }
```
2. innerHeight 表示浏览器内框视图高度。
```javascript
    var pageHeight = window.innerHeight
    if(typeof pageHeight != "number") {
        if(document.compatMode == "CSS1Compat") {
            pageWidth = document.documentElement.clientHeight // 标准模式下
        } else {
            pageWidth = document.body.clientHeight // 混杂模式下有作用
        }
    }
```
3. outerWidth 表示包括浏览器所占宽度。
4. outerHeight 表示包括浏览器所占高度。

5. 基于新数据调整窗口大小 `window.resizeTo()` 方法。
6. 基于原来数据调整窗口大小 `window.resizeBy()` 方法。

## 导航和打开窗口

1. `window.open()` 用来打开一个新的浏览器窗口。

```javascript
    var window = window.open(url, windowName, [windowFeatures])
```

2. `window.close()` 用来关闭一个浏览器的实例。

```javascript
    var wroxWin = window.open("http://www.163.com","newWindow", "height=400,width=400,top=10,left=10,resizable=yes")

    wroxWin.resizeTo(500,500) // 调整大小
    wroxWin.moveTo(100,100) // 调整位置
    wroxWin.close() // 关闭窗口
```

新建的 `window` 实例有一个 `opener` 属性是指向原来创建窗体的。

```javascript
    console.log(wroxWin.opener == window) // true
```

## 时钟函数

JavaScript 是单线程语言，它得通过设置 EventLoop 事件来触发定时函数。 JavaScript 有两个定时函数。

1. `setTimeout()` 到时间执行回调函数

```javascript
    var timeoutId = setTimeout(function() { // 生成后会生成时间 ID
        console.log('Hello world!') // 输出字符串
    }, 1000) // 1秒后运行
    clearTimeout(timeoutId) // 如果还没执行把这将要执行的方法清除掉
``` 
2. `setInterval()` 在时间间隔内循环执行回调函数
```javascript
    var num = 0
    var max = 10
    var intervalId = null
    function incrementNumber() {
        num ++
        if(num == max) { // 如果达到了max 值，则取消方法
            clearInterval(intervalId)
            console.log('Done')
        }
    }
    intervalId = setInterval(incrementNumber, 500) // 设置定时循环函数
```


## `window` 的对话框函数

平时调试代码或者测试的时候也会经常使用到对话框函数，但是要注意对话框函数是同步的，弹出框后会阻塞后续代码执行，除非这个话框架完成停止后，或者执行完成后。

1. `window.alert()` 弹出一个提示对话框。

2. `window.confirm()` 弹出一个是否确定对话框。

3. `window.prompt()` 弹出一个填写值对话框。

4. HTML 新元素 `dialog` (HTML 5.2) 也能弹出对话框

```javascript
    const modal = document.querySelector('dialog')

    // makes modal appear(adds `open` attribute)
    modal.showModal()
    // hides modal (removes `open` attribute)
    modal.close()
```

## `window` 的常用对象总结
| 属性名         | 描述解释                                                                                      |
|---------------|----------------------------------------------------------------------------------------------|
| closed        | 返回窗口是否已被关闭。                                                                       |
| defaultStatus | 设置或返回窗口状态栏中的默认文本。                                                           |
| frames        | 返回窗口中所有命名的框架。该集合是 Window 对象的数组，每个 Window 对象在窗口中含有一个框架。 |
| innerHeight   | 返回窗口的文档显示区的高度。                                                                 |
| innerWidth    | 返回窗口的文档显示区的宽度。                                                                 |
| length        | 设置或返回窗口中的框架数量。                                                                 |
| name          | 设置或返回窗口的名称。                                                                       |
| opener        | 返回对创建此窗口的窗口的引用。                                                               |
| outerHeight   | 返回窗口的外部高度，包含工具条与滚动条。                                                     |
| outerWidth    | 返回窗口的外部宽度，包含工具条与滚动条。                                                     |
| pageXOffset   | 设置或返回当前页面相对于窗口显示区左上角的 X 位置。                                          |
| pageYOffset   | 设置或返回当前页面相对于窗口显示区左上角的 Y 位置。                                          |
| parent        | 返回父窗口。                                                                                 |
| screenLeft    | 返回相对于屏幕窗口的x坐标                                                                    |
| screenTop     | 返回相对于屏幕窗口的y坐标                                                                    |
| screenX       | 返回相对于屏幕窗口的x坐标                                                                    |
| screenY       | 返回相对于屏幕窗口的y坐标                                                                    |
| self          | 返回对当前窗口的引用。等价于 Window 属性。                                                   |
| status        | 设置窗口状态栏的文本。                                                                       |
| top           | 返回最顶层的父窗口。                                                                         |
| screen        | `Screen` 是 `window` 的一个大子类属性，不过相对于其它子类属性没这么得要，主要是客户端屏幕的信息和浏览器能力|
| **document**      | `Document` 是 `window` 非常重要的对象，能让开发人员操作到 DOM 对象                             |
| **history**       | `window` 的重要子属性，history 对象保存着用户上网的历史记录对象和方法                            |
| **navigator**     | `window` 的重要子属性，提供当前有关浏览器信息和方法                                            |
| **location**      | location既是window对象的属性，也是 document对象的属性,包含导航 URL 的信息和操作方法            |

上面加粗的对象都是 `window` 的重要子属性将会重要详述。

## `window` 常用函数总结

| 函数             | 描述                                         |
|-----------------|----------------------------------------------------|
| blur()          | 把键盘焦点从顶层窗口移开。                         |
| createPopup()   | 创建一个 pop-up 窗口。                             |
| focus()         | 把键盘焦点给予一个窗口。                           |
| open()          | 打开一个新的浏览器窗口或查找一个已命名的窗口。     |
| close()         | 关闭浏览器窗口。                                   |
| print()         | 打印当前窗口的内容。                               |
| **弹出对话框方法**|
| alert()         | 显示带有一段消息和一个确认按钮的警告框。           |
| prompt()        | 显示可提示用户输入的对话框。                       |
| confirm()       | 显示带有一段消息以及确认按钮和取消按钮的对话框。   |
| **移动窗口方法**|
| moveBy()        | 可相对窗口的当前坐标把它移动指定的像素。           |
| moveTo()        | 把窗口的左上角移动到一个指定的坐标。               |
| **整调窗口大小方法**|
| resizeBy()      | 按照指定的像素调整窗口的大小。                     |
| resizeTo()      | 把窗口的大小调整到指定的宽度和高度。               |
| **滚动内容方法**  |
| scroll()        |                                                    |
| scrollBy()      | 按照指定的像素值来滚动内容。                       |
| scrollTo()      | 把内容滚动到指定的坐标。                           |
| **定时函数方法**  |
| setInterval()   | 按照指定的周期（以毫秒计）来调用函数或计算表达式。 |
| setTimeout()    | 在指定的毫秒数后调用函数或计算表达式。             |
| clearInterval() | 取消由 setInterval() 设置的 timeout。              |
| clearTimeout()  | 取消由 setTimeout() 方法设置的 timeout。           |