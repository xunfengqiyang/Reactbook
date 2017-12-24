###### React Router 是一个基于 React 之上的强大路由库，它可以让你向应用中快速地添加视图和数据流，同时保持页面与 URL 间的同步。

###### 

###### 为了向你说明 React Router 解决的问题，让我们先来创建一个不使用它的应用。所有文档中的示例代码都会使用 ES6/ES2015 语法和语言特性。

###### 

###### 不使用 React Router

###### import React from 'react'

###### import { render } from 'react-dom'

###### 

###### const About = React.createClass\({/\*...\*/}\)

###### const Inbox = React.createClass\({/\*...\*/}\)

###### const Home = React.createClass\({/\*...\*/}\)

###### 

###### const App = React.createClass\({

######   getInitialState\(\) {

######     return {

######       route: window.location.hash.substr\(1\)

######     }

######   },

###### 

######   componentDidMount\(\) {

######     window.addEventListener\('hashchange', \(\) =&gt; {

######       this.setState\({

######         route: window.location.hash.substr\(1\)

######       }\)

######     }\)

######   },

###### 

######   render\(\) {

######     let Child

######     switch \(this.state.route\) {

######       case '/about': Child = About; break;

######       case '/inbox': Child = Inbox; break;

######       default:      Child = Home;

######     }

###### 

######     return \(

######       &lt;div&gt;

######         &lt;h1&gt;App&lt;/h1&gt;

######         &lt;ul&gt;

######           &lt;li&gt;&lt;a href="\#/about"&gt;About&lt;/a&gt;&lt;/li&gt;

######           &lt;li&gt;&lt;a href="\#/inbox"&gt;Inbox&lt;/a&gt;&lt;/li&gt;

######         &lt;/ul&gt;

######         &lt;Child/&gt;

######       &lt;/div&gt;

######     \)

######   }

###### }\)

###### 

###### React.render\(&lt;App /&gt;, document.body\)

###### 当 URL 的 hash 部分（指的是 \# 后的部分）变化后，&lt;App&gt; 会根据 this.state.route 来渲染不同的 &lt;Child&gt;。看起来很直接，但它很快就会变得复杂起来。

###### 

###### 现在设想一下 Inbox 下面嵌套一些分别对应于不同 URL 的 UI 组件，就像下面这样的列表-详情视图：

###### 

###### path: /inbox/messages/1234

###### 

###### +---------+------------+------------------------+

###### \| About   \|    Inbox   \|                        \|

###### +---------+            +------------------------+

###### \| Compose    Reply    Reply All    Archive      \|

###### +-----------------------------------------------+

###### \|Movie tomorrow\|                                \|

###### +--------------+   Subject: TPS Report          \|

###### \|TPS Report        From:    boss@big.co         \|

###### +--------------+                                \|

###### \|New Pull Reque\|   So ...                       \|

###### +--------------+                                \|

###### \|...           \|                                \|

###### +--------------+--------------------------------+

###### 还可能有一个状态页，用于在没有选择 message 时展示：

###### 

###### path: /inbox

###### 

###### +---------+------------+------------------------+

###### \| About   \|    Inbox   \|                        \|

###### +---------+            +------------------------+

###### \| Compose    Reply    Reply All    Archive      \|

###### +-----------------------------------------------+

###### \|Movie tomorrow\|                                \|

###### +--------------+   10 Unread Messages           \|

###### \|TPS Report    \|   22 drafts                    \|

###### +--------------+                                \|

###### \|New Pull Reque\|                                \|

###### +--------------+                                \|

###### \|...           \|                                \|

###### +--------------+--------------------------------+

###### 为了让我们的 URL 解析变得更智能，我们需要编写很多代码来实现指定 URL 应该渲染哪一个嵌套的 UI 组件分支：App -&gt; About, App -&gt; Inbox -&gt; Messages -&gt; Message, App -&gt; Inbox -&gt; Messages -&gt; Stats，等等。

###### 

###### 使用 React Router 后

###### 让我们用 React Router 重构这个应用。

###### 

###### import React from 'react'

###### import { render } from 'react-dom'

###### 

###### // 首先我们需要导入一些组件...

###### import { Router, Route, Link } from 'react-router'

###### 

###### // 然后我们从应用中删除一堆代码和

###### // 增加一些 &lt;Link&gt; 元素...

###### const App = React.createClass\({

######   render\(\) {

######     return \(

######       &lt;div&gt;

######         &lt;h1&gt;App&lt;/h1&gt;

######         {/\* 把 &lt;a&gt; 变成 &lt;Link&gt; \*/}

######         &lt;ul&gt;

######           &lt;li&gt;&lt;Link to="/about"&gt;About&lt;/Link&gt;&lt;/li&gt;

######           &lt;li&gt;&lt;Link to="/inbox"&gt;Inbox&lt;/Link&gt;&lt;/li&gt;

######         &lt;/ul&gt;

###### 

######         {/\*

######           接着用 \`this.props.children\` 替换 \`&lt;Child&gt;\`

######           router 会帮我们找到这个 children

######         \*/}

######         {this.props.children}

######       &lt;/div&gt;

######     \)

######   }

###### }\)

###### 

###### // 最后，我们用一些 &lt;Route&gt; 来渲染 &lt;Router&gt;。

###### // 这些就是路由提供的我们想要的东西。

###### React.render\(\(

######   &lt;Router&gt;

######     &lt;Route path="/" component={App}&gt;

######       &lt;Route path="about" component={About} /&gt;

######       &lt;Route path="inbox" component={Inbox} /&gt;

######     &lt;/Route&gt;

######   &lt;/Router&gt;

###### \), document.body\)

###### React Router 知道如何为我们搭建嵌套的 UI，因此我们不用手动找出需要渲染哪些 &lt;Child&gt; 组件。举个例子，对于一个完整的 /about 路径，React Router 会搭建出 &lt;App&gt;&lt;About /&gt;&lt;/App&gt;。

###### 

###### 在内部，router 会将你树级嵌套格式的 &lt;Route&gt; 转变成路由配置。但如果你不熟悉 JSX，你也可以用普通对象来替代：

###### 

###### const routes = {

######   path: '/',

######   component: App,

######   childRoutes: \[

######     { path: 'about', component: About },

######     { path: 'inbox', component: Inbox },

######   \]

###### }

###### 

###### React.render\(&lt;Router routes={routes} /&gt;, document.body\)

###### 添加更多的 UI

###### 好了，现在我们准备在 inbox UI 内嵌套 inbox messages。

###### 

###### // 新建一个组件让其在 Inbox 内部渲染

###### const Message = React.createClass\({

######   render\(\) {

######     return &lt;h3&gt;Message&lt;/h3&gt;

######   }

###### }\)

###### 

###### const Inbox = React.createClass\({

######   render\(\) {

######     return \(

######       &lt;div&gt;

######         &lt;h2&gt;Inbox&lt;/h2&gt;

######         {/\* 渲染这个 child 路由组件 \*/}

######         {this.props.children \|\| "Welcome to your Inbox"}

######       &lt;/div&gt;

######     \)

######   }

###### }\)

###### 

###### React.render\(\(

######   &lt;Router&gt;

######     &lt;Route path="/" component={App}&gt;

######       &lt;Route path="about" component={About} /&gt;

######       &lt;Route path="inbox" component={Inbox}&gt;

######         {/\* 添加一个路由，嵌套进我们想要嵌套的 UI 里 \*/}

######         &lt;Route path="messages/:id" component={Message} /&gt;

######       &lt;/Route&gt;

######     &lt;/Route&gt;

######   &lt;/Router&gt;

###### \), document.body\)

###### 现在访问 URL inbox/messages/Jkei3c32 将会匹配到一个新的路由，并且它成功指向了 App -&gt; Inbox -&gt; Message 这个 UI 的分支。

###### 

###### &lt;App&gt;

######   &lt;Inbox&gt;

######     &lt;Message params={ {id: 'Jkei3c32'} } /&gt;

######   &lt;/Inbox&gt;

###### &lt;/App&gt;

###### 获取 URL 参数

###### 为了从服务器获取 message 数据，我们首先需要知道它的信息。当渲染组件时，React Router 会自动向 Route 组件中注入一些有用的信息，尤其是路径中动态部分的参数。我们的例子中，它指的是 :id。

###### 

###### const Message = React.createClass\({

###### 

######   componentDidMount\(\) {

######     // 来自于路径 \`/inbox/messages/:id\`

######     const id = this.props.params.id

###### 

######     fetchMessage\(id, function \(err, message\) {

######       this.setState\({ message: message }\)

######     }\)

######   },

###### 

######   // ...

###### 

###### }\)

###### 你也可以通过 query 字符串来访问参数。比如你访问 /foo?bar=baz，你可以通过访问 this.props.location.query.bar 从 Route 组件中获得 "baz" 的值。

###### 

###### 这就是 React Router 的奥秘。应用的 UI 以盒子中嵌套盒子的方式来表现；然后你可以让这些盒子与 URL 始终保持同步，而且很容易地把它们链接起来。

###### 

###### 这个关于 路由配置 的文档深入地描述了 router 的功能。

###### 



