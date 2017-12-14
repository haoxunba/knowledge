# Props

简单的应用就不介绍了，下面主要讲讲平时中注意的小知识点

## props和this.props应用场景

在functional component中用props，在class中用this.props

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);

```

```
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}

class App extends React.Component {
  render() {
    return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
  }
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);

```

## this.props.children

注意this.props.children是this.props中唯一的特例

props.children is available on every component. It contains the content between the opening and closing tags of a component. For example:
```
// Welcome在被当做子组件调用时

<Welcome>Hello world!</Welcome>
```
The string Hello world! is available in props.children in the Welcome component:

```
// 此时在Welcome组件内部可以拿到Hello world!

function Welcome(props) {
  return <p>{props.children}</p>;
}
```
For components defined as classes, use this.props.children:

```
// 上面是functional component的写法，下面是class写法，再次区分props和this.props

class Welcome extends React.Component {
  render() {
    return <p>{this.props.children}</p>;
  }
}
```