# DOM 的扩展

## Selectors API

Selectors API 是由 W3C 制定的一个标准，目标是让浏览器支持 CSS 查询节点。主要两个核心方法:

- querySelector()
    
    querySelector() 方法接收一个CSS 选择符，返回与该匹配的第一个元素，没有匹配则返回 null.
    ```javascript
        // 通过 tagName 获取
        const body = document.querySelector('body') 
        // 通过 ID 获取
        const myDiv = document.querySelector('#myDiv') 
        // 通过 className 获取
        const selected = document.querySelector('.selected')
        // 通过子组件 获取
        const img = document.body.querySelector('img.button')
    ```

- querySelectorAll()

    querySelectorAll() 也是接收一个 CSS 选择符，返回的是一个 NodeList 的实例. 如果没找到则返回空.之后遍历 NodeList 就可以操作元素了。
    ```javascript
        const ems = document.getElementById('myDiv').querySelectorAll('em')
        const selecteds = document.querySelectorAll('.selected')
        const strongs = document.querySelectorAll('p strong')
    ```

- matchesSelector()

    matchesSelector() 这个方法接收一个参数，即 CSS 选择符，如果调用元素与该选择符匹配，则返回 true, 否则，返回 false.
    ```javascript
        if(document.body.matchesSelector('body.page1')){
            // true
        }
    ```


## HTML 5扩展

### 类相关扩展

由于 class 属性用得越来越多，另外还表示元素的语义，所以添加了相关操作 ClassName 的方法

- getElementsByClassName()
    通过类名获取 NodeList

    ```javascript
        const allCurrentUsernames = document.getElementsByClassName('username current')
    ```

- classList 属性
    classList 需要通过节点的 className 属性来访问.
    ```javascript
        // 通过节点的 className 属性分拆成数组 classList 访问
        const classNames = div.className.split(/\s+/)
        // 删除给定的值
        div.classList.remove('disabled')
        // 给定的值添加到classList 中
        div.classList.add('current')
        // 如果列表中已经存在给定的值，删除它，如果列表中没有给定的值，添加它。
        div.classList.toggle()
        // 如果存在则返回 true, 否则返回 false
        div.contains('enable')
    ```

### 焦点管理

在很多时候都会获取焦点，来给用户更好的操作体验。在 HTML5 里面，会通过 `document.activeElement` 来检查集点获取到的位置。首次加载页面后会保存为 document.body ,文档加载期间为 null。

```javascript
    const button = document.getElementById('myButton')
    // nodeElement 获取焦点
    button.focus()
    // 判断是否当前是否集点状态
    console.log(document.hasFocus())
```

### HTML5 自定义数据属性

HTML5 里面可以为元素添加自定义数据只需要在自定义属性前面加上前缀 `data-` 就可以了。

`<div id="myDiv" data-appId="12345" data-myname="Nicholas"></div>`

```javascript
    const div = document.getElementById('myDiv')
    // 获取的时候只需要获取 Node节点的dataset 属性即可，不需要加前缀 data-
    const appId = div.dataset.appId
    const myName = div.dataset.myname
```

### 插入标记

1. innerHTML 属性

HTML5 里面提供 innerHTML 属性，是基于 Node 节点下面的，会返回 Node 节点下面的节点 Dom 树,读和写也是没问题的，写的时候要记着写为 HTML语法字符串.

2. outerHTML 属性

outerHTML 属性和 innterHTML很像，但是会包含当前 Node 节点的属性.

3. insertAdjacentHTML 方法

insertAdjacentHTML 和上面的 innerHTML、outerHTML 是一样的，添加HTML字符串，不过 insertAdjacentHTML 会更加灵活，可以加到不同地方.

```javascript
    element.insertAdjacentHTML('beforebegin','<p>Hello world</p>')
    element.insertAdjacentHTML('afterbegin','<p>Hello world</p>')
    element.insertAdjacentHTML('beforeend','<p>Hello world</p>')
    element.insertAdjacentHTML('afterend','<p>Hello world</p>')
```

在使用 innterHTML、outerHTML、insertAdjacentHTML 的时候要注意内存问题，添加删除节点的时候要看看节点有没有挂载事件与方法。因为会影响到内存占用和性能问题。而且要控制好 innerHTML 的操作次数，每次调用都会创建 HTML 解析器会带来性能损失。

```javascript
    let itemsHtml = ''
    for(let i = 0, len = values.length; i < len; i++){
        itemsHtml += "<li>" + values[i] + "</li>"
    }
    ul.innerHTML = itemsHtml
```

### scrollIntoView() 方法

scrollIntoView() 滚动方法， HTML 上面所有元素都能调用。

