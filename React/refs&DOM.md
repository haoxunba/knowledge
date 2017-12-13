# Refs and the DOM

通常我们通过props进行父子组件交互，但是当我们需要操作子组件实例或者真实的DOM元素时，就要用到props



1. Managing focus, text selection, or media playback.
2. Triggering imperative animations.
3. Integrating with third-party DOM libraries.

> 不要过度使用refs，尽量用state代替

**挂到组件（这里组件指的是有状态组件）上的ref表示对组件实例的引用，而挂载到dom元素上时表示具体的dom元素节点。**

ReactDOM.render()返回值其实就是ref代表的组件实例或dom元素，当ReactDOM.render()渲染组件时那么它返回的是组件实例；而渲染dom元素时，返回是具体的dom节点。

例如，下面代码:

```
    const domCom = <button type="button">button</button>;
    const refDom = ReactDOM.render(domCom，container);
    //ConfirmPass的组件内容省略
    const refCom = ReactDOM.render(<ConfirmPass/>,container);
    console.log(refDom);
    console.log(refCom);
```

## Adding a Ref to a DOM Element

```
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    this.focusTextInput = this.focusTextInput.bind(this);
  }

  focusTextInput() {
    // Explicitly focus the text input using the raw DOM API
    this.textInput.focus();
  }

  render() {
    // Use the `ref` callback to store a reference to the text input DOM
    // element in an instance field (for example, this.textInput).
    return (
      <div>
        <input
          type="text"
          ref={(input) => { this.textInput = input; }} />
        <input
          type="button"
          value="Focus the text input"
          onClick={this.focusTextInput}
        />
      </div>
    );
  }
}

```

> React will call the ref callback with the DOM element when the component mounts, and call it with null when it unmounts. ref callbacks are invoked before componentDidMount or componentDidUpdate lifecycle hooks.


## Adding a Ref to a Class Component

```
class AutoFocusTextInput extends React.Component {
  componentDidMount() {
    this.textInput.focusTextInput();
  }

  render() {
    return (
      <CustomTextInput
        ref={(input) => { this.textInput = input; }} />
    );
  }
}

```

上面案例的功能就是当组件实例挂载完成时，立即执行实例中的focusTextInput方法，从而实现页面加载完成让对应的输入框获得焦点的体验

包装组件通过ref获取被包装组件实例,从而可以获取 被包装组件的props和state和方法

## Refs and Functional Components

You may not use the ref attribute on functional components because they don’t have instances:

```
function MyFunctionalComponent() {
  return <input />;
}

class Parent extends React.Component {
  render() {
    // This will *not* work!
    return (
      <MyFunctionalComponent
        ref={(input) => { this.textInput = input; }} />
    );
  }
}

```

You can, however, use the ref attribute inside a functional component as long as you refer to a DOM element or a class component:

```
function CustomTextInput(props) {
  // textInput must be declared here so the ref callback can refer to it
  let textInput = null;

  function handleClick() {
    textInput.focus();
  }

  return (
    <div>
      <input
        type="text"
        ref={(input) => { textInput = input; }} />
      <input
        type="button"
        value="Focus the text input"
        onClick={handleClick}
      />
    </div>
  );  
}

```

## Exposing DOM Refs to Parent Components

While you could add a ref to the child component, this is not an ideal solution, as you would only get a component instance rather than a DOM node. Additionally, this wouldn’t work with functional components.

```
function CustomTextInput(props) {
  return (
    <div>
      <input ref={props.inputRef} />
    </div>
  );
}

class Parent extends React.Component {
  render() {
    return (
      <CustomTextInput
        inputRef={el => this.inputElement = el}
      />
    );
  }
}

```

父组件Parent可以通过this.inputElement获得functional component的真实input元素,可以看出这种方式是适合functional component的

下面的案例是对于更深层次的调用后代组件的真实dom方法

```
function CustomTextInput(props) {
  return (
    <div>
      <input ref={props.inputRef} />
    </div>
  );
}

function Parent(props) {
  return (
    <div>
      My input: <CustomTextInput inputRef={props.inputRef} />
    </div>
  );
}


class Grandparent extends React.Component {
  render() {
    return (
      <Parent
        inputRef={el => this.inputElement = el}
      />
    );
  }
}

```
> Note that this approach requires you to add some code to the child component. If you have absolutely no control over the child component implementation, your last option is to use findDOMNode(), but it is discouraged.

```
export default class Parent extends React.Component {

  componentDidMount() {
    console.log(ReactDOM.findDOMNode(this.child))
  }

  render() {
    return <Child ref={(child)=>{this.child=child}}/>
    // return <div ref={(div)=>{this.test=div}}>124</div>
  }
}
```

上面ReactDOM.findDOMNode拿到的是子组件Child render出来的所有DOM元素


## Legacy API: String Refs

```
<input type="text" ref={'input'}/>
```

使用this.refs.input会存在未知的bug，react官网不推荐使用