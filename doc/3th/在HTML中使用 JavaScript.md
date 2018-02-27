# 在HTML中使用JavaScript

### `<script>` 标签
向 HTML 页面插入 JavaScript 主要使用 Script 标签来插入到页面代码中.

### 有三种引入方式:
#### 一种的是直接写在 HTML 文件里面
```
<html>
    <head></head>
    <body></body>
    <script type="text/javascript">
        function sayHi(){
            ...
        }
    </script>
</html>
```

#### 第二种是外部本地文件引入
```
<script src="./local_path" type="text/javascript" (async|defer)></script>
```

#### 第三种是其它域或者其它网站引入
```
<script src="http://www.somewhere.com/afile.js" type="text/javascript"></script>
```

### `<script>` 标签的位置

1. w3c建议把 `<script>` 标签放在 `<head></head>` 里面，容易管理,在页面加载时加载，调用时才执行。

2. 把`<script>` 放在 `<body></body>` 里面，会在页面加载时被执行。

3. 把`<script>` 放在 `</html>` 后面，在页面载入完成之后被执行。

### `<script>` 两个延迟的脚本属性
- defer

    异步加载脚本,添加这个属性后可以在构建 HTML 树和 CSS 树的过程中加载脚本，加载完成后立刻执行。

- async

    异步加载脚本，加载完成后等 HTML 树和 CSS 树构建完成后，再执行脚本。


![image](/img/defer_async.jpeg)

### 总的来说 `<script>` 载入到页面可以分两步去考虑：

- 对于必须要在DOM加载之前运行的JavaScript脚本，我们需要把这些脚本放置在页面的head中，而不是通过外部引用的方式，因为外部的引用增加了网络的请求次数；并且我们要确保内敛的这些JavaScript脚本是很小的，最好是压缩过的，并且执行的速度很快，不会造成浏览器渲染的阻塞。

- 对于支持使用script标签的async和defer属性的浏览器，我们可以使用这两个属性；其中需要注意的点就是，async表示的意思是异步加载JavaScript文件，它的下载过程可以在HTML的解析过程中进行，加载完成之后立即执行这个文件的代码，执行文件代码的过程中会阻塞HTML的解析，它不保证文件加载的顺序。defer表示的意思是在HTML文档解析之后在执行加载完成的JavaScript文件，JavaScript文件的下载过程可以在HTML的解析过程中进行，它是按照script标签的先后顺序来加载文件的。

### 文档模式

在IE5.5 的时候就引入了文档模式的概念，这个功能是使用 `doctype` 来实现的，文档模式分为两种：

- 混杂模式（quirks mode） 又称为 怪异模式。
像下面这样的声明：
```html
<!--HTML 4.01 严格型-->
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/xhtmll/DTD/xhtmll-strict.dtd">
```

混杂模式是
- 标准模式 (standards mode) 又称为: 严格模式。

```html
<!-- HTML 5-->
<!DOCTYPE html>
```


这两种模式影响 CSS 内容的呈现，主要是用来区分旧 IE 版本和新 IE 标准定下的区别，而如果DOCTYPE缺失则在ie6，ie7，ie8下将会触发怪异模式（quirks 模式）。

### 平稳退化 `JavaScript`
```html
<html>
    <head></head>
    <body>
        <noscript>
            <p>本页面需要浏览器支持(启用) JavaScript</p>
        </noscript>
    </body>
</html>
```

