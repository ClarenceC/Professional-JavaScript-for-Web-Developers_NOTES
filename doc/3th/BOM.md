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