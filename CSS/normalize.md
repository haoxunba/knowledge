# normalize.css



是一个包文件，一个体积非常小的css文件，主要解决不同浏览器默认样式的差异，同时对HTML5元素、排版、列表、嵌入的内容、表单和表格都进行了一般化

相比css Reset，它的特点是

保护有用的浏览器默认样式而不是完全去掉它们
一般化的样式：为大部分HTML元素提供
修复浏览器自身的bug并保证各浏览器的一致性
优化CSS可用性：用一些小技巧
解释代码：用注释和详细的文档来

## Usage

可以通过npm安装

```
npm install --save normalize.css
```

因为文件体积很小，也可以不要安装直接将css复制出来，放在自定义的css里面，并在入口处导入

https://github.com/necolas/normalize.css