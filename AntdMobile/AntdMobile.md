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

这样做的有点是可以在打包的时候编译指定文件夹下面的css,避免cssmodules的编译

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

从而不需要像其他的样式需要通过cssmodules编译

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

## 自定义样式

当我们需要将引入的模块样式进行编译时，需要我们自定义样式

虽然可以通过`className`、`style`、`styleName`这些方式设置组件的样式，但是存在很多不足，比如不能设置内部制定的标签样式，或者设置的样式被权重更高的样式覆盖，所以一般的方式就是

1. 在标签里面设置`prefixCls`,例如设置`prefixCls = "demo"`

2. 在公共样式里面复制所有相关的样式，将对应的class名全部替换为`demo`，如`am-navbar`替换为`demo`

3. 如果入口处已经单独引入`antd-mobile.css`，那么上面修改样式所在的文件夹引入顺序一定要在`antd-mobile.css`后面
如
```
import '../../styles/antd-mobile.css'
import '../../styles/antdStyleReset.scss'
```

其实不设置`prefixCls`只需要将相关类复制到新的文件夹，更改样式，然后在`antd-mobile.css`后面引入也可以实现，不过最大的问题就是如果有多个地方引入相同的模块，那么会产生相互污染

下面是整个案例
```

<NavBar
  mode="light"
  className={style.nav1}
  styleName = 'nav2'
  style={{color: 'red'}}
  prefixCls = "demo"
  icon={<Icon type="left" />}
  onLeftClick={() => console.log('onLeftClick')}
  rightContent={[
    <Icon key="0" type="search" style={{ marginRight: '16px' }} />,
    <Icon key="1" type="ellipsis" />,
  ]}
>NavBar</NavBar>
      
```