# DOM

DOM 是针对 HTML 和 XML 文档的一个 API应该程序编程接口。DOM 用来描绘了一个可添加、移除和修改的层次的节点树。DOM 是由 DHTML (动态 HTML)开始创立的，现在都是跨平台、语言中立的。

DOM 是一个树形结构的根节点集合组成的。

## 1. Node 类型

DOM1 级里面定义了一个 Node 接口, 所有节点类型都继承自 Node 类型, 所有的节点类型都有着相同的基本属性和方法。



| Node 常量数值  | Node 节点类型      |
|----|-----------------------------|
| 1  | ELEMENT_NODE                |
| 2  | ATTRIBUTE_NODE              |
| 3  | TEXT_NODE                   |
| 4  | CDATA_SECTION_NODE          |
| 5  | ENTITY_REFERENCE_NODE       |
| 6  | ENTITY_NODE                 |
| 7  | PROCESSING_INSTRUCTION_NODE |
| 8  | COMMENT_NODE                |
| 9  | DOCUMENT_NODE               |
| 10 | DOCUMENT_TYPE_NODE          |
| 11 | DOCUMENT_FRAGMENT_NODE      |
| 12 | DOCUMENT_NOTATION_NODE      |


### 1. `nodeType` Node节点的类型

`nodeType` 可以用来显示当前 Node节点类型。判断的时候可以用类型 `Node.ELEMENT_NODE`  或者常量数值也行，最好使用常量数值进行比较为了确保浏览器兼容。

```javascript
    if (someNode.nodeType === Node.ELEMENT_NODE) // 用来判断 Element 节点
    // or
    if (someNode.nodeType === 1 ) // 用来判断 Element 节点
```

### 2. `nodeName` Node节点的元素标签名

```javascript
    var element = document.getElementByID("testName")
    console.log(element.nodeName) // "DIV"
```

### 3. `nodeValue` Node节点元素的值

对于元素节点 `nodeName` 中保存的是元素的标签名，而 `nodeValue` 值为 null 。


### 1. NodeList

在 DOM 中每个节点都存在一个`childNodes`的属性, `childNodes` 属性保存的是 NodeList 的对象， NodeList 对象是一个拥有 length 属性的类数组，并不是一个真正的数组。 DOM 的结构变化能自动反映在 NodeList 对象中的， NodeList 是一个动态的对象。

如果想访问，NodeList 元素可以使用下面的方法:
```javascript
    //1
    var firstChild = someNode.childNodes[0] // 使用数组标，因为更贴近数组，所以更多开发人员在使用
    //2
    var secondCHild = someNode.childes.item(1) // 使用 item（）方法
```

或者把 NodeList 转变为数组

```javascript
    // 转换数组
    function convertToArray(nodes){
        var array = null
        try { // 通过 try catch 来捕获错误
            array = Array.prototype.slice.call(nodes, 0) // 非 IE8以下浏览器
        } catch(ex) {
            array = new Array()
            for(var i = 0, len = nodes.length; i < len; i++){
                array.push(nodes[i])
            }
        }
        return array
    }
```

### 2. 关于 parentNode 属性和 previousSibling、nextSibling属性。


![image](/img/parentNode_childNodes_method.png)

一图解释全部关系 Node 节点下面的子节点，有一个 `parentNode` 属性对回父类属性，如果 `nextSibling` 和 `previousSibling` 为最尾或者最头节点会返回 `null`。

`someNode.ownerDocument` 每个节点都有这个属性，指向最顶层的 HTML 节点。
`someNode.hasChildNodes()` 返回该节点是否有 childNodes。

### 3.操作节点

1. `appendChild()` 在指定的节点 childNodes 内追加一个元素节点在 childNodes 末尾。

`someNode.appendChild(someELement)` 添加子节点到 someNode 的 childNodes 最后面。如果节点已经存在在节点树中那么，就会从原来的位置移动到操作后的位置。

2. `insertBefore(newNode, someNode)` 把 newNode 插入到 someNode 节点之前。

如果传入的第二个参数为 `null` 则会和 `appendChild()` 操作效果一样。

3. `replaceChild(newNode, someNode)` 用 newNode节点把 someNode 节点替换了，`replaceChild` 可以删除节点。

4. `removeChild()` 移除节点

```javascript
    var formerFirstChild = someNode.removeChild(someNode.firstChild) // 移除第一个子节点
```
删除和移除节点后，节点并不是消失了，只是不在 DOM 树中，如果需要彻底删除需要 `formerFirstChild = null`,释放节点那么引用节点的事件才会跟着释放内存。

5. `cloneNode()` 创建复制副本

```javascript
    var deepList = myList.cloneNode(true) // 深度拷贝
    console.log(deepList.childNodes.length) // 3
    var shalloList = myList.cloneNode(false) // 浅拷贝
    console.log(shallowList.childNodes.length) // 0
```

## Document 类型

Document 代表整个文档。document 对象是 window对象的一个属性。

```javascript
    console.log(window.document.nodeType) // 9
    console.log(window.document.nodeName) // #document
    var html = document.querySelector("html") // 获取 html
    console.log(html.parentNode.nodeType) // 9 html的父类就是 document 节点
    console.log(document.parentNode) // document 的父类为 null
    console.log(document.childNodes) // NodeList(2) [<!DOCTYPE html>, html.module-default] 
```

对于 document 的直接子元素属性可以直接属性访问
```javascript
    console.log(document.doctype) // <!DOCTYPE html>
    console.log(document.doctype.nodeType) // 这能获得 10  DOCUMENT_TYPE_NODE 值
    console.log(document.body) // <body id="body" style>...</body>
    console.log(document.title) // 标题
```

document 里面还自带了，几个 BOM 对象属性非常实用
```javascript
    console.log(document.URL) // 取得连接
    console.log(document.domain) // 取得域名
    console.log(document.location) // 获得地址信息
    console.log(document.images) // 同样这方法也能获取全部文档 img 元素的节点
    console.log(document.anchors) // 包含文档中所有带 name 特性的 <a> 元素
    console.log(document.forms) // 包含文件中所有 <form> 元素
```


### 检测 DOM 的功能类型

可以通过 `document.implementation.hasFeature` 来判断是否包含 Dom 的部分功能。
```javascript
    var haxXmlDom = document.implementation.hasFeature("XML","1.0") 
```

### DOM写入流

除了操作 DOM 方法外，还可以使用写入流来插入 node 元素节点。有以下几个方法

1. `write()` 方法，把传入的字符串写入到 dom 流中。

```javascript
    document.write("<strong>" + (new Date()).toString() + "</strong>")
```

2. `writeln()` 方法，把传入的字符串写入到 dom 流中并最后换行。
3. `open()` 方法，用于打开写入流。页面加载其间不需要使用这方法。
4. `close()` 方法，用于关闭写入流。页面加载其间不需要使用这方法。


### 查找元素的方法

- `document.getElementById()` 通过节点 id 属性查找 ELement 节点,返回 ELEMENT_NODE
```javascript
var div = document.getElmentById('myDiv')
```

- `document.getElementsByTagName()` 通过元素的标签名返回找到的元素列表 HTMLCollection
```javascript
var images = document.getELementsByTagName("img")
// 访问 HTMLCollection 方法
console.log(images.length) // 图像数量
console.log(images[0]) // 数组访问
console.log(images.item(0)) // item 形式访问
console.log(images.namedItem('myImage')) // 访问 name 属性为 myImage 的元素
var allElements = document.getElementsByTagName("*") // 获取所有元素

```

- `document.getElementsByName` 通过元素的 name 特性，返回 HTMLCollection

通常会使用在单选按钮上面，获取同一类组别的元素。


### 创建元素节点

1. `createElement()`创建元素节点
2. `createComment` 创建解释节点


## Element 类型

Element 是 Web 编程中最常用的类型，用于表现 XML 或 HTML 元素，提供对元素的标签名，子节点特性访问等。

在Element的 `nodeName` 属性中，保存的是元素标签名, `nodeValue` 返回的是 null。


### 关于 Element 和 Node 的区别

很多人都会混淆了 Element 和 Node 的区别，其实两个是完全不一样的东西，Node 是属于 HTML DOM 中的基类，所有像 Element ,Attribut Text,都是 Node 的实例对象，
是特殊的 Node 分别叫做， Element_Node, Text_Node, Comment_Node , 所以 Element_Node 都会继承 Node 的属性。

### NodeList vs HTMLCollection

- NodeList 是由 Node 类型组成的列表
- HTMLCollection 是由 Element 节点组成的列表，操用 DOM 的时候都会操作 ElementList 即是 HTMLCollection ，而不会去操作 NodeList。
- 特别要注意的是 NodeList 和 HTMLCollection 都不是真正的数组，都是类数组。
- NodeList 会包含文本节点和空格节点
- NodeList 和 HTMLCollection 都是 live 的，会动态变化。
- NodeList 没有 nameditem() 方法，而 HTMLCollection 有。

### 怎样获取 NodeList 和 HTMLCollection

```javascript
    // 获取 HTMLCollection
    const div = document.getElementById('Id') // HTMLCollection
    document.getElementsByName('Name') // HTMLCollection
    document.getElementsByTagName('TagName') // HTMLCollection
    document.getElementsByClassName('style') // HTMLCollection

    document.querySelectorAll('style') // 返回的是 nodelist,但是实际上的是 元素集合，并且是静态的

    div.item(0) // HTMLCollection 可以通过 item 方法访问单独 node 节点

    node.childNodes // 返回 NodeList 
    div.item(0).childNodes // 返回 NodeList 
```

### Element 的属性

所有 HTML 元素都是由 HTMLElement 类型来表示的，HTMLElement 是直接继承自 Element 一些属性包括:

- id
- className
- title
- lang
- name
- ref

访问的时候可以直接获得一个node节点，再直接访问属性
```javascript
    var div = document.getElementById('myDiv')
    div.id
    div.className
    div.lang
    div.ref
```

或者可以使用 dom 方法来操作属性值

```javascript
    var div = document.getElementyById('myDiv')
    div.getAttribute('id')
    div.setAttribute('id',value)
```

有三种操作特性的方法

```javascript
    div.getAttribute('id') // 获取 属性 ID 的值
    div.setAttribue('id',value) // 设置 ID 的值
    div.removeAttribute('id') // 删除 ID 的属性
    // attributes 属性
    element.attributes.lenght // 通过 Element_node 的 attributes 属性访问
```

### 创建元素

创建元素节点可以通过下面方法创建
```javascript
    // 传参数是 tagName
    var div = document.createElement('div')
    // or
    var div = document.createElement("<div id=\"myNewDiv\" class=\"box\"></div>")
```

创建元素节点后是单独存在的，还需要添加到文档树中，才会在页面显示出来。
```javascript
    node.appendChild() // 在末尾添加子节点
    node.insertBefore() // 在某节点前添加子节点
    node.replaceChild() // 替换子节点
```

## Text 类型

