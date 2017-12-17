# React16

该处主要介绍react16新增的功能

## React16.2.0

### Improved Support for Fragments

之前在react16.0版本，在render return中可以用数组代替外层标签的包裹，类似于

```
render() {
 return [
  "Some text.",
  <h2 key="heading-1">A heading</h2>,
  "More text.",
  <h2 key="heading-2">Another heading</h2>,
  "Even more text."
 ];
}
```

这种写法存在很多麻烦的地方，所以Fragments就应运而生了

React 16.2 is now available! The biggest addition is improved support for returning multiple children from a component’s render method. We call this feature fragments

fragments的原理就是在return的children外边会隐式的帮你添加一个空标签，从而让你不需要添加一个多余的div等标签来包裹了，类似于

```
render() {
  return (
    <>
      <ChildA />
      <ChildB />
      <ChildC />
    </>
  );
}
```

Fragments在react中的应用

第一种就是

```
const Fragment = React.Fragment;

<Fragment>
  <ChildA />
  <ChildB />
  <ChildC />
</Fragment>

// This also works
<React.Fragment>
  <ChildA />
  <ChildB />
  <ChildC />
</React.Fragment>
```

第二种就是上面提到的空标签。二者的区别就是空标签不能使用任何attribute，Fragment目前只支持key属性