# DOM Level

## DOM

DOM(文档对象模型) 是针对 HTML 和 XML 文档的一个API(应用程序编程接口)，DOM 描绘了一个层次化的节点树，允许开发人员添加、移除和修改页面的某一部分。DOM 定义了访问 HTML 和 XML 文档的标准，可以允许脚本动态地更新文档内容、结构和样式, 简单点说就是提供一个文档对象模型给 JS 动态修改文档状态。 DOM 是一个共 W3C 公发布的标准，所以可以在所有浏览器上使用 DOM 的这些 API。

![image](/img/1200px-DOM-model.svg.png)

## DOM 的发展史

### DOM | DOM0

最初 DOM 还没有被标准化，还只是在 Netscape Navigator3 上的模型，结果 IE3 和 Netscape Navigator3 上的模型都各自不同，相互冲突，那个时候被叫 DOM Level 0。

### DOM 1

在浏览器各厂商进行大战的同时，W3C 结合大家的优点推出了一个标准化的 DOM ，被叫为 DOM1。 DOM1 被定义为一个与平台和编程语言无关的接口，可以通过这些 API 接口访问和修改文档的内容、结构和样式。DOM1 是比较基础的，主要定义了 HTML 和 XML 文档的底层结构，DOM1 由三部分组成，包括: 核心 (core)、 HTML接口。

- DOM Core （DOM 核心）：DOM Core 规定了基于 XML 的文档结构标准，通过这个标准简化了对文档中 XML 的访问和操作。

- DOM HTML ：DOM HTML 则在 DOM 核心的基础上加以扩展，添加了针对 HTML 的对象和方法。

### DOM 2
DOM2 在 DOM1 的基础上引入了更多的交互能力，支持更高级的 XML 特性。 DOM2 扩充了，鼠标、用户界面事件、范围、遍历等细分模块，而且增加了对 CSS 的支持。

* DOM2核心(DOM2 Core): 为节点添加更多方法和属性。
* DOM级视图(DOM2 HTML): 在 DOM1 上添加更多属性、方法和新接口。
* DOM视图 (DOM2 Views): 为文档定义了基于样式信息的不同视图。
* DOM事件 (DOM2 Events): 说明了如何使用事件与 DOM 文档交互。
* DOM样式 (DOM2 Style): 定义了如何方式访问和改变 CSS 样式的信息。
* DOM遍历和范围 (DOM2 Traversal and Range): 引入了遍历 DOM 文档和选择其特定部分的新接口。

DOM2 样式围绕，外部样式表，`<style/>`样式表，和针对特定元素的样式，提交API。

```javascript
    // 可以直接对 Node_Element 节点的 style属性进行添加样式
    const myDiv = document.getElementById('myDiv')
    myDiv.style.backgroundColor = 'red'
    myDiv.style.width = '100px'
    myDiv.style.border = '1px solid black'

    // 还可以直接设置 style.cssText,直接设置能大量修改样式特性
    myDiv.style.cssText = 'width: 25px; height: 100px; background-color: green;'

    //访问 style 的时候和访问 Node 节点一样
    for(const i = 0 ,len=myDiv.style.length; i < len; i++) {
        const divStyle =  myDiv.style[i] // 数组形式访问， 获取 CSS 属性名
        const divItemStyle = myDiv.style.item(i) // item函数访问, 获取 CSS 属性名
        // 通过 getPropertyValue 获取属性的值
        const value = myDiv.style.getPropertyValue(divStyle)
        // 如果访问cssText
        const value = myDiv.style.getPropertyCSSSValue(prop)
    }
    // 删除style属性
        myDiv.style.removeProperty('border')
```

### DOM 3
DOM3 扩展了 DOM，增加了一些扩展模块：

* DOM3加载和保存模块(DOM Load and Save): 引入了以统一方式加载和保存文档的方法。
* DOM3验证模块 (DOM Validation): 定义了难文档的方法
* DOM核心的扩展 (DOM Style): 支持 XML 1.0规范，涉及 XML Infoset、XPath 和 XML Base 

DOM2 和 DOM3 都不同程度对 XML 命名空间有扩展，比较少使用到就不展开讨论了。

### DOM 接口

DOM 模型的接口有30多个，但常用的只有4~5个

#### Document

Document接口是对文档进行操作的入口，它是从Node接口继承过来的。 

#### Node
　　Node接口是其他大多数接口的父类。在DOM树中，Node接口代表了树中的一个节点。

#### NodeList
　　NodeList接口是一个节点的集合，它包含了某个节点中的所有子节点。它提供了对节点集合的抽象定义，并不包含如何实现这个节点集的定义。NodeList用于表示有顺序关系的一组节点，比如某个节点的子节点序列。在DOM中，NodeList的对象是live的，对文档的改变，会直接反映到相关的NodeList对象中。


#### NamedNodeMap
　　NamedNodeMap接口也是一个节点的集合，通过该接口，可以建立节点名和节点之间的一一映射关系，从而利用节点名可以直接访问特定的节点，这个接口主要用在属性节点的表示上。尽管NamedNodeMap所包含的节点可以通过索引来进行访问，但是这只是提供了一种枚举方法，NamedNodeMap所包含的节点集中节点是无序的。与NodeList相同，在DOM中，NamedNodeMap对象也是live的。

### DOM 事件