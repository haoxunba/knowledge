
# CSS Modules

本文主要介绍原生的css modules和React Css Modules

## css modules

### 局部作用域

```javascript
import React from 'react';
import style from './App.css';

export default () => {
  return (
    <h1 className={style.title}>
      Hello World
    </h1>
  );
};
```

> 将类名添加到`style`（这里的`style`必须是引入`App.css`对应的变量）对象上，通过构建工具编译成哈希值，这样类名独一无二，实现局部作用域
```
<h1 class="_3zyde4l1yATCOkgn-DBWEL">
  Hello World
</h1>
```

### 配合webpack应用

CSS Modules 提供各种[插件](https://github.com/css-modules/css-modules/blob/master/docs/get-started.md)，支持不同的构建工具。其中构建工具Webpack 的[`css-loader`](https://github.com/webpack/css-loader#css-modules)插件对 CSS Modules 的支持最好

**webpack配置文件**

```
module.exports = {
  
  module: {
    {
      test: /\.css$/,
      use: [
        'style-loader',
        {
          loader: 'css-loader',
          options: {
            modules: true,
            sourceMap: true,
            importLoaders: 1 // 0 => 无 loader(默认); 1 => postcss-loader; 2 => postcss-loader, sass-loader,
            localIdentName: '[local]---[hash:base64:5]'
          }
        },
        'postcss-loader',
        'sass-loader'
      ]
  }
};
```

> 关键就是在插件`css-loader`后添加查询参数`modules`从而调用`css-modules`

### 添加多个类名

```
<div className={value.class + " " + value.class2}>{value.value}</div>
```
拼接字符串啊……
要不喜欢的话用字符串模板也行啊
```
<div className={`${value.class} ${value.class2}`}>{value.value}</div>
```
花括号里面就是可以运算的部分,注意两个类名之间要有空格

如果是数组的话直接join也行啊
```
<div className={classnames.join(" ")}>{value.value}</div>
```
本文最后提到的classnames插件也是可以做到的，这种插件办法对于动态判断添加类名是非常适合的

### 全局作用域

CSS Modules 允许使用:global(.className)的语法，声明一个全局规则。凡是这样声明的class，都不会被编译成哈希字符串。
App.css加入一个全局class。
```
.title {
  color: red;
}

:global(.title) {
  color: green;
}
```
App.js使用普通的class的写法，就会引用全局class。
```
import React from 'react';
import styles from './App.css';

export default () => {
  return (
    <h1 className="title">
      Hello World
    </h1>
  );
};
```

CSS Modules 还提供一种显式的局部作用域语法:local(.className)，等同于.className，所以上面的App.css也可以写成下面这样。
```
:local(.title) {
  color: red;
}

:global(.title) {
  color: green;
}
```
CSS Modules 是一个可以被多种方法实现的规范。react-css-modules 利用 webpack css-loader 所提供的功能启用了现有的 CSS Modules 的集成。

其实多数情况下，我们很少定义全局作用域的样式，我们通常会把用到的全局作用域的样式全部放到一个单独的文件中，然后在入口文件进行一个全局的引入。

### 定制哈希类名

上面的案例在引入css-loader时，在它的options中除了用到modules，还有一个属性localIdentName就是用来定时哈希类名的

```
module: {
    {
      test: /\.css$/,
      use: [
        'style-loader',
        {
          loader: 'css-loader',
          options: {
            modules: true,
            sourceMap: true,
            importLoaders: 1 // 0 => 无 loader(默认); 1 => postcss-loader; 2 => postcss-loader, sass-loader,
            localIdentName: '[local]---[hash:base64:5]'
          }
        },
        'postcss-loader',
        'sass-loader'
      ]
  }

```

也可以这样定义
```
localIdentName=[path][name]---[local]---[hash:base64:5]
```
编译的结果就类似于
```
demo03-components-App---title---GpMto
```

### 利用composes进行class的组合

在 CSS Modules 中，一个选择器可以继承另一个选择器的规则，这称为"组合"（"composition"）。
在App.css中，让.title继承.className 。
```
.className {
  background-color: blue;
}

.title {
  composes: className;
  color: red;
}
```
App.js不用修改。
```
import React from 'react';
import style from './App.css';

export default () => {
  return (
    <h1 className={style.title}>
      Hello World
    </h1>
  );
};
```

App.css编译成下面的代码。
```
._2DHwuiHWMnKTOYG45T0x34 {
  color: red;
}

._10B-buq6_BEOTOl9urIjf8 {
  background-color: blue;
}
```
相应地， h1的class也会编译成`<h1 class="_2DHwuiHWMnKTOYG45T0x34 _10B-buq6_BEOTOl9urIjf8">`。

选择器也可以继承其他CSS文件里面的规则。
another.css
```
.className {
  background-color: blue;
}
```
App.css可以继承another.css里面的规则。
```
.title {
  composes: className from './another.css';
  color: red;
}
```

### 输入变量

CSS Modules 支持使用变量，不过需要安装 `PostCSS` 和 `postcss-modules-values`。
```
$ npm install --save postcss-loader postcss-modules-values
```
把postcss-loader加入webpack.config.js。
```
var values = require('postcss-modules-values');

module.exports = {
  entry: __dirname + '/index.js',
  output: {
    publicPath: '/',
    filename: './bundle.js'
  },
  module: {
    loaders: [
      {
        test: /\.jsx?$/,
        exclude: /node_modules/,
        loader: 'babel',
        query: {
          presets: ['es2015', 'stage-0', 'react']
        }
      },
      {
        test: /\.css$/,
        loader: "style-loader!css-loader?modules!postcss-loader"
      },
    ]
  },
  postcss: [
    values
  ]
};
```
接着，在colors.css里面定义变量。
```
@value blue: #0c77f8;
@value red: #ff0000;
@value green: #aaf200;
```
App.css可以引用这些变量。
```
@value colors: "./colors.css";
@value blue, red, green from colors;

.title {
  color: red;
  background-color: blue;
}
```




## react-css-modules

webpack [css-loader](https://github.com/webpack/css-loader#css-modules) itself has several disadvantages:

*   You have to use `camelCase` CSS class names.
*   You have to use `styles` object whenever constructing a `className`.
*   Mixing CSS Modules and global CSS classes is cumbersome.
*   Reference to an undefined CSS Module resolves to `undefined` without a warning.

### styleName

<b>React CSS Modules component automates loading of CSS Modules using `styleName` property</b> 

Using `react-css-modules`:
*   You are warned when `styleName` refers to an undefined CSS Module ([`errorWhenNotFound`](https://www.npmjs.com/package/react-css-modules#errorwhennotfound)option).
*   You can enforce use of a single CSS module per `ReactElement` ([`allowMultiple`](https://www.npmjs.com/package/react-css-modules#allowmultiple) option).

<b>allowMultiple</b>
Default: false.
Allows multiple CSS Module names.
When false, the following will cause an error:
```
<div styleName='foo bar' />
```
<b>errorWhenNotFound</b>
Default: true.
Throws an error when styleName cannot be mapped to an existing CSS Module.

```javascript
import React from 'react';
import CSSModules from 'react-css-modules';
import styles from './table.css';
 
class Table extends React.Component {
    render () {
        return <div styleName='table'>
            <div styleName='row'>
                <div styleName='cell'>A0</div>
                <div styleName='cell'>B0</div>
            </div>
        </div>;
    }
}
 
export default CSSModules(Table, styles);
```
强制是否使用单一的css模块
```javascript
export default createCSSModules(Login, styles, {
  allowMultiple: true
});

```


<font color="blue"> `className`不能直接用less里面的类名，是因为在执行该处代码的时候，类名已经被编译过了，所以直接拿是拿不到的。 但是`styleName`里面是可以直接写类名的，因为他拿到类名自己会进行编译，得到的结果和之前`createCSSModules`编译的结果相同。</font>

### 同样需要webpack配合

```javascript
//webpack配置文件 webpack.config.js
loaders: [
      {
        test: /\.jsx?$/,
        include: [
          path.resolve(__dirname, '../src'),
        ],
        loader: 'babel-loader',
      }, {
        test: /\.less$/,
        loader: 'style!css?modules&importLoaders=1&localIdentName=[name]__[local]___[hash:base64:5]!postcss!less'
      }
    ]
```

根据上面的配置，生成的类名类似于

```html
<div class="table__table___32osj">
    <div class="table__row___2w27N">
        <div class="table__cell___1oVw5">A0</div>
        <div class="table__cell___1oVw5">B0</div>
    </div>
</div>
```

### classnames
https://www.npmjs.com/package/classnames

主要应用场景就是需要提前**对多个类名进行批量处理**，然后通过变量返回类名集合

#### 配合styleName应用

```javascript
import classNames from 'classnames';
...

getWrapperClass = () => {
    let { block, fixed, full } = this.props;
    let config = {
      full,
      'btn-wrapper': true,
      [`btn-wrapper${block ? '-block' : ''}`]: true,
    };

    if (fixed) {
      config = Object.assign(config, {
        [`fixed-${fixed}`]: true,
      });
    }

    return classname(config)
}

render() {
    const clsName = classname(class1, class2, { 'hideKeyboard': !this.state.showKeyboard });
    return(
        <div styleName={ this.getWrapperClass()} onClick={ this.closeModel}>
            <div styleName={clsName}></div>
        </div>
    )
}

```

`styleName`也可以实现和`classnames`等同的效果，批量处理类名作用，不过可读性维护性不好

```
// 多个类名之间用空格分开
<section styleName={this.props.modalPosition ? 'msgBox' : 'msgBox-Postion msgBox'}>
```


## 参考

http://www.ruanyifeng.com/blog/2016/06/css_modules.html

[react-css-module-npm](https://www.npmjs.com/package/react-css-modules#the-implementation)
[react-css-module-中文翻译](https://segmentfault.com/a/1190000004530909)