React-Bootstrap 是可重用的前端组件库。与 Twitter Bootstrap 一致外观与感受，但通过 Facebook 的 React.js 框架获得更清爽的代码。



如果你想要一个名为 "Something" 按钮，点击时触发 someCallback 函数。采用原生应用时，可写成类似：



button\(size=SMALL, color=GREEN, text="Something", onClick=someCallback\)

使用流程的 Twitter Bootstrap 前端框架, 你需要在 HTML 写成类似代码如下:



&lt;button id="something-btn" type="button" class="btn btn-success btn-sm"&gt;

  Something

&lt;/button&gt;

并在 Javascript 代码中增加$\('\#something-btn'\).click\(someCallback\);



基于 web 的标准这已经是非常好的方法了，但有些繁琐了。 React-Bootstrap 可让你按如下方式来编写：



&lt;Button bsStyle="success" bsSize="small" onClick={someCallback}&gt;

  Something

&lt;/Button&gt;

这样 HTML/CSS 的实现细节就被抽象掉了, 让你可以用与其它编程语言最接近的方式来书写代码。



使用 React.js 来改善 Bootstrap API

Bootstrap 特色代码要反复出现的原因是因为 HTML 和 CSS 不支持组件库的方式来进行抽象。这导致我们需要在 button 中将 btn反复写三遍。

React.js 的解决方案是用 Javascript 直接来写。 React 接管页面的全部渲染工作。你只需要写出 Javascript 对象的树，然后告诉他在结点间如何传递状态。

例如，我们让 React 渲染出一个只显示一个按钮的页面，仍使用 Bootstrap CSS 的风格:



var button = React.DOM.button\({

  className: "btn btn-lg btn-success",

  children: "Register"

}\);



React.render\(button, mountNode\);

但我们在 Javascript 中可以放弃 HTML/CSS 提供使易理解的 API 形式:



var button = ReactBootstrap.Button\({

  bsStyle: "success",

  bsSize: "large",

  children: "Register"

}\);



React.render\(button, mountNode\);

React-Bootstrap 就是这样的一套组件库，让你轻松扩展和增强自已所需的功能。



JSX 语法

React 组件实质上是一个 Javascript 对象, 在写树形结构时将是非常乏味的。 React 鼓励使用 JSX，将树形结构写成类似 HTML 的语法形式：



var buttonGroupInstance = \(

  &lt;ButtonGroup&gt;

    &lt;DropdownButton bsStyle="success" title="Dropdown"&gt;

      &lt;MenuItem key="1"&gt;下垃链接&lt;/MenuItem&gt;

      &lt;MenuItem key="2"&gt;下垃链接&lt;/MenuItem&gt;

    &lt;/DropdownButton&gt;

    &lt;Button bsStyle="info"&gt;中间&lt;/Button&gt;

    &lt;Button bsStyle="info"&gt;右侧&lt;/Button&gt;

  &lt;/ButtonGroup&gt;

\);



React.render\(buttonGroupInstance, mountNode\);

许多人对 React.js 的第一印象是按这种方式混合来写 Javascript 和 HTML。 然后就增加一个上个例子中的下拉按钮来说，按照 Bootstrap Javascript 和 组件 文档，生一个下拉按钮需要将代码分别写在两处：首先你需要增加 HTML/CSS 元素, 然后调用 Javascript 相关代码将组件绑定在一起。

React-Bootstrap 组件库就是遵循 React.js 的机理将单一功能在一处实现。详细请参考当前 React-Bootstrap 组件库的 组件页面。

