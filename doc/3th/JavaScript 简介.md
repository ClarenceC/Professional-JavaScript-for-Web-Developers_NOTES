# JavaScript 简介

JavaScript诞生于 1995年，当时，它的主要目的是处理以前由服务器端语言（Perl）负责的一些输入验证操作。现在的JavaScript 用途已经不再局限于验证操作，而是Web发展的重要组成部分，所有可视化设备的交互能力都涉及到 JavaScript.
JavaScript 从一个简单的输入验证器发展成为一门强大的编程语言，要想全面理解和掌握 JavaScript，关键在于弄清楚它的本质，历史和局限性。


- Netscape 推出了 JavaScript 1.0 大获成功
- Netscape 之后推出了 JavaScript 1.1
- 微软搞局出现了 3个版本不同版本的 JavaScript [{Netscape: JavaScript},{Internet Explorer: Jscript},{ScriptEase: CEnvi}]
- 1997年, ECMA(European Computer Manufacturers Association) 欧洲计算机制造商协会，以JavaScript 1.1为蓝本定下标准。


JavaScript 其实是包括三个核心组成部分：
1. 核心（ECMAScript）

ECMAScript 就是对实现该标准规定的各个方面内容的语言的描述。包括语法、类型、语句、关键字、保留字、操作符、对象。常见的 Web 浏览器只是 ECMAScript 实现可能的宿主环境之一。

2. 文档对象模型（DOM）

文件对象模型（DOM，Document Object Model）是针对XML但经过扩展用于HTML的应用程序编程接口。是由 W3C（World Wide Web Consortium, 万维网联盟）着手规划的，意在限制各程序开发 DHTML 的纷争，让 Web 继续保持路平台的天性。

DOM 级别：

- DOM0 实际上是不存在的，只是 IE4 和 Netscape Navigator 4 的最初支持 DHTML 参照点。

- DOM1 级别由两部分组成： DOM核心（DOM Core） 和 DOM HTML。 DOM1 主要是映射文档的结构。

- DOM2 在原来的DOM基础上扩充：鼠标和用户界面事件，范围，遍历，和对 CSS 的支持。包括了 DOM 视图（DOM Views）、 DOM 事件（DOM Events）、DOM样式（DOM Style）、DOM 遍历和范围（DOM Traversal and Range）

- DOM3 级进一步扩展DOM： 引入了 DOM加载和保存(DOM Load and Save) 和 DOM 验证（DOM Validation）模块 和 对 DOM 核心的 XML 扩展。

3. 浏览器对象模型（BOM）
在很早以前 IE3 和 Netscape Navigator 3 都可以支持访问操作浏览器窗口的浏览器对象模型（BOM， Browser Object Model）BOM 是用来控制浏览器显示页面以外的部分。 HTML5 把 BOM 也正式写入规范标准起来。

BOM基本上来讲只处理浏览器窗口和框架，但人们也把一些针对浏览器的 JavaScript 扩展算作 BOM 的一部分，包括下面这些。

- 弹出新浏览器窗口的功能。
- 移动、缩放和关闭浏览器窗口的功能。
- 提供浏览器详细信息的 navigator 对象。
- 提供浏览器所加载页面的详细信息的 location 对象。
- 提供用户显示器分辨率详细信息的 screen 对象。
- 对 cookies 的支持。
- 像 XMLHttpRequest 和 IE 的 ActiveXObject 这样的自定义对象。