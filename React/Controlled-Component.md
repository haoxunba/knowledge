

# Controlled Components

https://facebook.github.io/react/docs/forms.html
https://goshakkk.name/controlled-vs-uncontrolled-inputs-react/ (官网推荐)

## 官网案例

```javascript
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}

ReactDOM.render(
  <NameForm />,
  document.getElementById('root')
);
```
上述案例中注意
1. `onSubmit`的经典用法，即可以按键盘enter键触发，也可以点击submit按钮触发。
2. `event.target.value`获取当前input的值
3. `label`标签的应用
4. `event.preventDefault()`阻止submit表单默认提交事件

<b>handling Multiple Inputs</b>利用input共有的属性type和name实现
```javascript
class Reservation extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isGoing: true,
      numberOfGuests: 2
    };

    this.handleInputChange = this.handleInputChange.bind(this);
  }

  handleInputChange(event) {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }

  render() {
    return (
      <form>
        <label>
          Is going:
          <input
            name="isGoing"
            type="checkbox"
            checked={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <br />
        <label>
          Number of guests:
          <input
            name="numberOfGuests"
            type="number"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
    );
  }
}
```
注意一下事项
1. `event.target`的应用
2. `name`在`setState`中应用
3. 三元运算符

##
 受控组件原理

input输入框输入一个值，触发onChange事件，从而调用setState函数，state的值改变触发render重新渲染，此时input的value值始终和最新的this.state的值相等

### 常见的应用场景

1. in-place feedback, like validations
2. disabling the button unless all fields have valid data
3. enforcing a specific input format, like credit card numbers

<b>不同form表单类型的获取value不同</b>

| Element  | Value property | Change callbacew | value in the callback  |
| ------------ | ------------ | ------------ | ------------ |
|`<input type="text" />` | value="string" | onChange | event.target.value|
|`<input type="checkbox" />` | checked={boolean} | onChange | event.target.checked|
|`<input type="radio" />` | checked={boolean} | onChange | event.target.checked|
|`<textarea />` | value="string" | onChange | event.target.value|
|`<select />` | value="option value" | onChange | event.target.value|


# Uncontroller Components

https://facebook.github.io/react/docs/uncontrolled-components.html

> In a controlled component, form data is handled by a React component. The alternative is uncontrolled components, where form data is handled by the DOM itself.

```javascript
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.input.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" ref={(input) => this.input = input} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```
注意点：
1. use a ref to get form values from the DOM.
2. use uncontrolled components sometimes easier to integrate React and non-React.

<b>Default Values</b>
use defaultValue instead of value attribute
```javascript
render() {
  return (
    <form onSubmit={this.handleSubmit}>
      <label>
        Name:
        <input
          defaultValue="Bob"
          type="text"
          ref={(input) => this.input = input} />
      </label>
      <input type="submit" value="Submit" />
    </form>
  );
}
```
> Likewise, `<input type="checkbox">` and `<input type="radio"> `support `defaultChecked`, and `<select>` and `<textarea>` supports `defaultValue`.

# 补充区分event.target和event,currentTarget

## event.currentTarget

> https://developer.mozilla.org/zh-CN/docs/Web/API/Event/currentTarget

当事件遍历DOM时，标识事件的当前目标。它总是引用事件处理程序附加到的元素，而不是event.target，它标识事件发生的元素。

例子
event.currentTarget很有用, 当将相同的事件处理程序附加到多个元素时。

```
function hide(e){
  e.currentTarget.style.visibility = "hidden";
  console.log(e.currentTarget);
  // 该函数用作事件处理器时: this === e.currentTarget
}

var ps = document.getElementsByTagName('p');

for(var i = 0; i < ps.length; i++){
  // console: 打印被点击的p元素
  ps[i].addEventListener('click', hide, false);
}
// console: 打印body元素
document.body.addEventListener('click', hide, false);
```

## event.target

一个触发事件的对象的引用。它与event.currentTarget不同， 当事件处理程序在事件的冒泡或捕获阶段被调用时。


event.target 属性可以用来实现事件委托 (event delegation)。
```
// Make a list
var ul = document.createElement('ul');
document.body.appendChild(ul);

var li1 = document.createElement('li');
var li2 = document.createElement('li');
ul.appendChild(li1);
ul.appendChild(li2);

function hide(e){
  // e.target refers to the clicked <li> element
  // This is different than e.currentTarget which would refer to the parent <ul> in this context
  e.target.style.visibility = 'hidden';
}

// Attach the listener to the list
// It will fire when each <li> is clicked
ul.addEventListener('click', hide, false);
```

event.target 属性在实现事件代理时会被用到。

```
// 假定一个 list 变量为 ul 元素
function hide(e) {
  // 点击列表项目（li）区域，e.target 与 e.currentTarget 不同
  e.target.style.visibility = 'hidden';
}

list.addEventListener('click', hide, false);

// If some element (<li> element or a link within an <li> element for instance) is clicked, it will disappear.
// It only requires a single listener to do that
```