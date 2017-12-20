# antd-mobile

## 引入css的样式的三种方式

### 利用babel-plugin-import

```
// .babelrc or babel-loader option
{
  "plugins": [
    ["import", { "libraryName": "antd-mobile", "style": "css" }] // `style: true` 会加载 less 文件
  ]
}
```

### 在入口处引入样式

```
// 一般入口文件是index.js
import 'antd-mobile/dist/antd-mobile.css';  // or 'antd-mobile/dist/antd-mobile.less'
```

### 在入口处引入样式（常用）

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