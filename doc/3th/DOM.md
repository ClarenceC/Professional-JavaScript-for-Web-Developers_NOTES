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




