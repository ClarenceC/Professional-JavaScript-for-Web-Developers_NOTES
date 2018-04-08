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


