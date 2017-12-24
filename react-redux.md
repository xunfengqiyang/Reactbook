一、UI 组件

React-Redux 将所有组件分成两大类：UI 组件（presentational component）和容器组件（container component）。



UI 组件有以下几个特征。



只负责 UI 的呈现，不带有任何业务逻辑

没有状态（即不使用this.state这个变量）

所有数据都由参数（this.props）提供

不使用任何 Redux 的 API

下面就是一个 UI 组件的例子。





const Title =

  value =&gt; &lt;h1&gt;{value}&lt;/h1&gt;;

因为不含有状态，UI 组件又称为"纯组件"，即它纯函数一样，纯粹由参数决定它的值。



二、容器组件

容器组件的特征恰恰相反。



负责管理数据和业务逻辑，不负责 UI 的呈现

带有内部状态

使用 Redux 的 API

总之，只要记住一句话就可以了：UI 组件负责 UI 的呈现，容器组件负责管理数据和逻辑。



你可能会问，如果一个组件既有 UI 又有业务逻辑，那怎么办？回答是，将它拆分成下面的结构：外面是一个容器组件，里面包了一个UI 组件。前者负责与外部的通信，将数据传给后者，由后者渲染出视图。



React-Redux 规定，所有的 UI 组件都由用户提供，容器组件则是由 React-Redux 自动生成。也就是说，用户负责视觉层，状态管理则是全部交给它。



三、connect\(\)

React-Redux 提供connect方法，用于从 UI 组件生成容器组件。connect的意思，就是将这两种组件连起来。





import { connect } from 'react-redux'

const VisibleTodoList = connect\(\)\(TodoList\);

上面代码中，TodoList是 UI 组件，VisibleTodoList就是由 React-Redux 通过connect方法自动生成的容器组件。



但是，因为没有定义业务逻辑，上面这个容器组件毫无意义，只是 UI 组件的一个单纯的包装层。为了定义业务逻辑，需要给出下面两方面的信息。



（1）输入逻辑：外部的数据（即state对象）如何转换为 UI 组件的参数



（2）输出逻辑：用户发出的动作如何变为 Action 对象，从 UI 组件传出去。



因此，connect方法的完整 API 如下。





import { connect } from 'react-redux'



const VisibleTodoList = connect\(

  mapStateToProps,

  mapDispatchToProps

\)\(TodoList\)

上面代码中，connect方法接受两个参数：mapStateToProps和mapDispatchToProps。它们定义了 UI 组件的业务逻辑。前者负责输入逻辑，即将state映射到 UI 组件的参数（props），后者负责输出逻辑，即将用户对 UI 组件的操作映射成 Action。



四、mapStateToProps\(\)

mapStateToProps是一个函数。它的作用就是像它的名字那样，建立一个从（外部的）state对象到（UI 组件的）props对象的映射关系。



作为函数，mapStateToProps执行后应该返回一个对象，里面的每一个键值对就是一个映射。请看下面的例子。





const mapStateToProps = \(state\) =&gt; {

  return {

    todos: getVisibleTodos\(state.todos, state.visibilityFilter\)

  }

}

上面代码中，mapStateToProps是一个函数，它接受state作为参数，返回一个对象。这个对象有一个todos属性，代表 UI 组件的同名参数，后面的getVisibleTodos也是一个函数，可以从state算出 todos 的值。



下面就是getVisibleTodos的一个例子，用来算出todos。





const getVisibleTodos = \(todos, filter\) =&gt; {

  switch \(filter\) {

    case 'SHOW\_ALL':

      return todos

    case 'SHOW\_COMPLETED':

      return todos.filter\(t =&gt; t.completed\)

    case 'SHOW\_ACTIVE':

      return todos.filter\(t =&gt; !t.completed\)

    default:

      throw new Error\('Unknown filter: ' + filter\)

  }

}

mapStateToProps会订阅 Store，每当state更新的时候，就会自动执行，重新计算 UI 组件的参数，从而触发 UI 组件的重新渲染。



mapStateToProps的第一个参数总是state对象，还可以使用第二个参数，代表容器组件的props对象。





// 容器组件的代码

//    &lt;FilterLink filter="SHOW\_ALL"&gt;

//      All

//    &lt;/FilterLink&gt;



const mapStateToProps = \(state, ownProps\) =&gt; {

  return {

    active: ownProps.filter === state.visibilityFilter

  }

}

使用ownProps作为参数后，如果容器组件的参数发生变化，也会引发 UI 组件重新渲染。



connect方法可以省略mapStateToProps参数，那样的话，UI 组件就不会订阅Store，就是说 Store 的更新不会引起 UI 组件的更新。



五、mapDispatchToProps\(\)

mapDispatchToProps是connect函数的第二个参数，用来建立 UI 组件的参数到store.dispatch方法的映射。也就是说，它定义了哪些用户的操作应该当作 Action，传给 Store。它可以是一个函数，也可以是一个对象。



如果mapDispatchToProps是一个函数，会得到dispatch和ownProps（容器组件的props对象）两个参数。





const mapDispatchToProps = \(

  dispatch,

  ownProps

\) =&gt; {

  return {

    onClick: \(\) =&gt; {

      dispatch\({

        type: 'SET\_VISIBILITY\_FILTER',

        filter: ownProps.filter

      }\);

    }

  };

}

从上面代码可以看到，mapDispatchToProps作为函数，应该返回一个对象，该对象的每个键值对都是一个映射，定义了 UI 组件的参数怎样发出 Action。



如果mapDispatchToProps是一个对象，它的每个键名也是对应 UI 组件的同名参数，键值应该是一个函数，会被当作 Action creator ，返回的 Action 会由 Redux 自动发出。举例来说，上面的mapDispatchToProps写成对象就是下面这样。





const mapDispatchToProps = {

  onClick: \(filter\) =&gt; {

    type: 'SET\_VISIBILITY\_FILTER',

    filter: filter

  };

}

六、&lt;Provider&gt; 组件

connect方法生成容器组件以后，需要让容器组件拿到state对象，才能生成 UI 组件的参数。



一种解决方法是将state对象作为参数，传入容器组件。但是，这样做比较麻烦，尤其是容器组件可能在很深的层级，一级级将state传下去就很麻烦。



React-Redux 提供Provider组件，可以让容器组件拿到state。





import { Provider } from 'react-redux'

import { createStore } from 'redux'

import todoApp from './reducers'

import App from './components/App'



let store = createStore\(todoApp\);



render\(

  &lt;Provider store={store}&gt;

    &lt;App /&gt;

  &lt;/Provider&gt;,

  document.getElementById\('root'\)

\)

上面代码中，Provider在根组件外面包了一层，这样一来，App的所有子组件就默认都可以拿到state了。



它的原理是React组件的context属性，请看源码。





class Provider extends Component {

  getChildContext\(\) {

    return {

      store: this.props.store

    };

  }

  render\(\) {

    return this.props.children;

  }

}



Provider.childContextTypes = {

  store: React.PropTypes.object

}

上面代码中，store放在了上下文对象context上面。然后，子组件就可以从context拿到store，代码大致如下。





class VisibleTodoList extends Component {

  componentDidMount\(\) {

    const { store } = this.context;

    this.unsubscribe = store.subscribe\(\(\) =&gt;

      this.forceUpdate\(\)

    \);

  }



  render\(\) {

    const props = this.props;

    const { store } = this.context;

    const state = store.getState\(\);

    // ...

  }

}



VisibleTodoList.contextTypes = {

  store: React.PropTypes.object

}

React-Redux自动生成的容器组件的代码，就类似上面这样，从而拿到store。



七、实例：计数器

我们来看一个实例。下面是一个计数器组件，它是一个纯的 UI 组件。





class Counter extends Component {

  render\(\) {

    const { value, onIncreaseClick } = this.props

    return \(

      &lt;div&gt;

        &lt;span&gt;{value}&lt;/span&gt;

        &lt;button onClick={onIncreaseClick}&gt;Increase&lt;/button&gt;

      &lt;/div&gt;

    \)

  }

}

上面代码中，这个 UI 组件有两个参数：value和onIncreaseClick。前者需要从state计算得到，后者需要向外发出 Action。



接着，定义value到state的映射，以及onIncreaseClick到dispatch的映射。





function mapStateToProps\(state\) {

  return {

    value: state.count

  }

}



function mapDispatchToProps\(dispatch\) {

  return {

    onIncreaseClick: \(\) =&gt; dispatch\(increaseAction\)

  }

}



// Action Creator

const increaseAction = { type: 'increase' }

然后，使用connect方法生成容器组件。





const App = connect\(

  mapStateToProps,

  mapDispatchToProps

\)\(Counter\)

然后，定义这个组件的 Reducer。





// Reducer

function counter\(state = { count: 0 }, action\) {

  const count = state.count

  switch \(action.type\) {

    case 'increase':

      return { count: count + 1 }

    default:

      return state

  }

}

最后，生成store对象，并使用Provider在根组件外面包一层。





import { loadState, saveState } from './localStorage';



const persistedState = loadState\(\);

const store = createStore\(

  todoApp,

  persistedState

\);



store.subscribe\(throttle\(\(\) =&gt; {

  saveState\({

    todos: store.getState\(\).todos,

  }\)

}, 1000\)\)



ReactDOM.render\(

  &lt;Provider store={store}&gt;

    &lt;App /&gt;

  &lt;/Provider&gt;,

  document.getElementById\('root'\)

\);

完整的代码看这里。



八、React-Router 路由库

使用React-Router的项目，与其他项目没有不同之处，也是使用Provider在Router外面包一层，毕竟Provider的唯一功能就是传入store对象。





const Root = \({ store }\) =&gt; \(

  &lt;Provider store={store}&gt;

    &lt;Router&gt;

      &lt;Route path="/" component={App} /&gt;

    &lt;/Router&gt;

  &lt;/Provider&gt;

\);



