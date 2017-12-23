# antd-mobile

## 引入css的样式的三种方式

### 在入口处引入样式

```
// 一般入口文件是index.js
import 'antd-mobile/dist/antd-mobile.css';  // or 'antd-mobile/dist/antd-mobile.less'
```

或者下面这种方式引入样式

和上面的方法不同的地方就是，上面是直接从包文件的路径中引入样式，这个方法是将antd-mobile.css复制到项目中指定的文件夹下（如style文件夹）

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

但是上面的做法不能做到按需加载，antd内部会在浏览器的控制台提示警告，并推荐安装`babel-plugin-import`

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

注意使用按需加载时，因为按需加载的话，样式是直接引用node-modules中的样式，所以在css-loader的配置里面不能在用类似上面的配置, 去掉include的配置，否则不能编译node-modules下面的css样式，会报错
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

同时，为了方便使用prefixCls重置样式，为了方便对应样式的查找，可以将`antd-mobile.css`单独复制到文件中，但是不引入组件
