# Lifecycle

## 三种Lifecycle

### Mounting
These methods are called when an instance of a component is being created and inserted into the DOM:

```
constructor()
componentWillMount()
render()
componentDidMount()
```


### Updating
An update can be caused by changes to props or state. These methods are called when a component is being re-rendered:

```
componentWillReceiveProps()
shouldComponentUpdate()
componentWillUpdate()
render()
componentDidUpdate()
```
### Unmounting
This method is called when a component is being removed from the DOM:

```
componentWillUnmount()
```

> 在life cycle中定义的方法叫做各自生命周期的 “lifecycle hooks”.
mountComponent 本质上是通过 **递归渲染** 内容的，由于递归的特性，父组件的 componentWillMount 一定在其子组件的 componentWillMount 之前调用，而父组件的 componentDidMount 肯定在其子组件的 componentDidMount 之后调用。

# 说明

生命周期共提供了10个不同的API。

## getDefaultProps

作用于组件类，只调用一次，返回对象用于设置默认的`props`，对于引用值，会在实例中共享。

With functions and ES6 classes `defaultProps` is defined as a property on the component itself:

```
class Greeting extends React.Component {
  // ...
}

Greeting.defaultProps = {
  name: 'Mary'
};

```

With `createReactClass()`, you need to define `getDefaultProps()` as a function on the passed object:

```
var Greeting = createReactClass({
  getDefaultProps: function() {
    return {
      name: 'Mary'
    };
  },

  // ...

});

```

## getInitialState

作用于组件的实例，在实例创建时调用一次，用于初始化每个实例的`state`，此时可以访问`this.props`。

注意： 如果组件是通过es6格式创建的，那么初始化的状态存放在constructor中，即
```
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = {count: props.initialCount};
  }
  // ...
}
```

With `createReactClass()`, you have to provide a separate `getInitialState` method that returns the initial state:

```
var Counter = createReactClass({
  getInitialState: function() {
    return {count: this.props.initialCount};
  },
  // ...
});

```

## constructor

`constructor(props)`
1.1 The constructor for a React component is called before it is mounted. 
1.2 ** you should call super(props) before any other statement. Otherwise, this.props will be undefined in the constructor, which can lead to bugs.**
1.3 The constructor is the right place to initialize state. If you don't initialize state and you don't bind methods, you don't need to implement a constructor for your React component.
1.4 It's okay to initialize state based on props if you know what you're doing. Here's an example of a valid React.Component subclass constructor:
```
constructor(props) {
  super(props);
  this.state = {
    color: props.initialColor
  };
}  
```

## componentWillMount

在完成首次渲染之前调用，此时仍可以修改组件的state。
This is the only lifecycle hook called on server rendering. Generally, we recommend using the constructor() instead.

如果此时在 componentWillMount 中调用 setState，是不会触发 reRender（重新渲染），而是进行 state 合并。

## render

The render() method is required.

When called, it should examine this.props and this.state and return a single React element. This element can be either a representation of a native DOM component, such as `<div />`, or another composite component that you've defined yourself.

You can also return null or false to indicate that you don't want anything rendered. When returning null or false, ReactDOM.findDOMNode(this) will return null.

The render() function should be pure, meaning that it does not modify component state, it returns the same result each time it's invoked, and it does not directly interact with the browser. If you need to interact with the browser, perform your work in componentDidMount() or the other lifecycle methods instead. Keeping render() pure makes components easier to think about.

Note
render() will not be invoked if shouldComponentUpdate() returns false.
必选的方法，创建虚拟DOM，该方法具有特殊的规则：

*   只能通过`this.props`和`this.state`访问数据
*   可以返回`null`、`false`或任何React组件
*   只能出现一个顶级组件（不能返回数组）
*   不能改变组件的状态
*   不能修改DOM的输出

## componentDidMount

真实的DOM被渲染出来后调用，在该方法中可通过`this.getDOMNode()`访问到真实的DOM元素。此时已可以使用其他类库来操作这个DOM。

_在服务端中，该方法不会被调用。_
**Setting state in this method will trigger a re-rendering.**

## componentWillReceiveProps

```
componentWillReceiveProps(nextProps) {
    if(this.props.parentValue != nextProps.parentValue) {
      this.setState({
        childValue: nextProps.parentValue
      })
      
    }
  }
```

<b>不会执行的情况：</b>

mounting期间
setState更改state值

<b>执行情况：</b>

接收到新的props

<b>主要作用：</b>

对比props，设置setState

<b>其他注意事项：</b>

由于componentWillReceiveProps和componentWillMount的执行顺序都是在render之前，所以在这两个里面设置setState是不会导致re-render，只是和initialState进行合并

> If you need to update the state in response to prop changes (for example, to reset it), you may compare this.props and nextProps and perform state transitions using this.setState() in this method.

-
> Note that React may call this method even if the props have not changed, so make sure to compare the current and next values if you only want to handle changes. This may occur when the parent component causes your component to re-render.



## shouldComponentUpdate

`shouldComponentUpdate()` is invoked before rendering when new props or state are being received. Defaults to `true`. This method is not called for the initial render or when `forceUpdate()` is used.

if `shouldComponentUpdate()` returns `false`, then [`componentWillUpdate()`](), [`render()`](), and [`componentDidUpdate()`]() will not be invoked.

If you are confident you want to write it by hand, you may compare `this.props` with `nextProps` and `this.state` with `nextState` and return `false` to tell React the update can be skipped.

<b>注意事项</b>

1. 与componentWillReceiveProps相比，不仅新的props能够触发shouldComponentUpdate，而且新的state也能够触发该周期

2. 主要应用就是对比新旧state或props，通过返回ture或false决定后续声明周期是否执行，主要是决定是否执行render

3. 不要在这里设置setState，重新设置state还会触发该周期，会陷入死循环

2. Returning `false` does not prevent child components from re-rendering when state changes.


## componentWillUpdate

接收到新的`props`或者`state`后，进行渲染之前调用，此时不允许更新`props`或`state`。

和shouldComponentUpdate一样，不要在这里设置setState，重新设置state还会触发该周期，会陷入死循环

用途

> *   you might want to set a variable which might be needed for the render for this particular state
*   you might want to animate your divs or
*   dispatch events to clear/set your stores.

https://stackoverflow.com/questions/33075063/what-is-the-exact-usage-of-componentwillupdate-in-reactjs

## componentDidUpdate

完成渲染新的`props`或者`state`后调用，此时可以访问到新的DOM元素。



## componentWillUnmount

组件被移除之前被调用，可以用于做一些清理工作，在`componentDidMount`方法中添加的所有任务都需要在该方法中撤销，比如创建的定时器或添加的事件监听器。


# 摘录
http://react-china.org/t/react/1740
https://facebook.github.io/react/docs/react-component.html

https://zhuanlan.zhihu.com/p/20312691 