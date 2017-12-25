# antd-mobile

## 引入css的样式的两种方式

### 在入口处引入样式

```
// 一般入口文件是index.js
import 'antd-mobile/dist/antd-mobile.css';  // or 'antd-mobile/dist/antd-mobile.less'
```

或者下面这种方式引入样式

和上面的方法不同的地方就是，上面是直接从包文件的路径中引入样式，这个方法是将安装包里面lib目录下的antd-mobile.css复制到项目中指定的文件夹下（如style文件夹）

这样做的有点是可以在打包的时候编译指定文件夹下面的css

```
{
  test: /\.css$/,
  include: path.resolve(__dirname, 'src/styles'),
  use: [
    'style-loader',
    'css-loader',
  ],
}

```

但是上面两种引入样式的做法都不能做到按需加载，antd内部会在浏览器的控制台提示警告，并推荐安装`babel-plugin-import`

### 利用babel-plugin-import

- 使用 babel-plugin-import（推荐）。
```
// .babelrc or babel-loader option
{
  "plugins": [
    ["import", { "libraryName": "antd-mobile", "style": "css" }] // `style: true` 会加载 less 文件
  ]
}
```
然后只需从 antd-mobile 引入模块即可，无需单独引入样式。
```
// babel-plugin-import 会帮助你加载 JS 和 CSS
import { DatePicker } from 'antd-mobile';

```
- 手动引入
```
import DatePicker from 'antd-mobile/lib/date-picker';  // 加载 JS
import 'antd-mobile/lib/date-picker/style/css';        // 加载 CSS
// import 'antd-mobile/lib/date-picker/style';         // 加载 LESS
```

注意使用按需加载时，样式是直接引用node-modules中的样式，所以css-loader的配置里面不能在用类似上面的配置, 去掉include的配置，否则不能编译node-modules下面的css样式，会报错
```
{
  test: /\.css$/,
  // include: path.resolve(__dirname, 'src/styles'),
  use: [
    'style-loader',
    'css-loader',
  ],
}

```

## 自定义样式

```
import React,{Component} from 'react';
import {Button, NavBar, Icon} from 'antd-mobile';
import CreateCSSModules from 'react-css-modules';
import style from './antdStyleReset.scss';

class AntdStyleReset extends React.Component {

  render() {
    return (
      <div>
        <div className="antdStyleReset">
          <NavBar
            mode="light"
            className={style.nav1}
            styleName = 'nav2'
            style={{color: 'red'}}
            prefixCls="antdStyleReset-navbar"
            icon={<Icon type="left" />}
            onLeftClick={() => console.log('onLeftClick')}
            rightContent={[
              <Icon key="0" type="search" style={{ marginRight: '16px' }} />,
              <Icon key="1" type="ellipsis" />,
            ]}
          >NavBar</NavBar>
        </div>
        
        <Button  className={style.button} style={{color: 'red'}}>test1</Button>
      </div>
    )
  }
}

export default CreateCSSModules(AntdStyleReset, style, {
  allowMultiple: true
})
```

我尝试在引用的组件中利用`className`、`styleName`、`style`来自定义组件的样式，但是存在很多问题，比如

1. 设置的样式有可能被组件中权重更高的样式覆盖不起作用
2. 设置的样式不能精确指定到组件中某一个元素

还有的童鞋说定义`styleName`等于`NavBar`组件默认的类名比如`styleName = "am-navbar-left"`,这种方法也存在很多问题，比如

1. 比如定义`am-navbar-left`是想重置`NavBar`组件内部的某个元素的样式，这种做法只会将给类名设置到外层元素，不能设置到内层的元素

所以我推荐使用`prefixCls`来自定义组件，虽然这种方式比较笨拙，但是可以很好的解决上面的问题

根据上面的案例，总结相关步骤是

1. 自定义类名`prefixCls="antdStyleReset-navbar"`,我推荐定义方式是`组件名+antd的组件名`
2. 将相关的样式`node_modules/antd-mobile/lib/nav-bar/style/index.css`复制到`antdStyleReset`组件对应的样式`antdStyleReset.scss`中
3. 全局替换`am-navbar`为`antdStyleReset-navbar`
4. 更改相关元素类名的样式就可以了


