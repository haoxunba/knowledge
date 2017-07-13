
## 基本选择器

序号 |   选择器  |  含义
---|---|----
1.   | * |   通用元素选择器，匹配任何元素
2.   | E |   标签选择器，匹配所有使用E标签的元素
3.  |  .info |   class选择器，匹配所有class属性中包含info的元素
4.  |  #footer |   id选择器，匹配所有id属性等于footer的元素

## 多元素的组合选择器

序号 |   选择器  |  含义
---|---|---
5.  |  E,F  |  多元素选择器，同时匹配所有E元素或F元素，E和F之间用逗号分隔
6.   | E F  |  后代元素选择器，匹配所有属于E元素后代的F元素，E和F之间用空格分隔
7.  |  E > F |   子元素选择器，匹配所有E元素的子元素F
8.  |  E + F  |  毗邻元素选择器，匹配所有紧随E元素之后的同级元素F

```
<style>
  div[attr]+div {
    color:aqua;
  }
</style>

<div attr="demo">第一行</div>
<div>第二行</div> // 只有第二行的颜色变化
<div>第三行</div>

```

## CSS 2.1 属性选择器

序号  |  选择器 |   含义
---|---|---
9.  |  E[att] |   匹配所有具有att属性的E元素，不考虑它的值。（注意：E在此处可以省略，比如"[cheacked]"。以下同。）
10.  |  E[att="val"]  |  匹配所有att属性等于"val"的E元素
11.  |  E[att~="val"]  |  匹配所有att属性值当中有一个值等于"val"的E元素
12.  |  E[att&#124;="val"]  |  匹配所有att属性具有多个连字号分隔（-如zh-CN）的值、其中一个值以"val"开头的E元素，主要用于lang属性，比如"en"、"en-us"、"en-gb"等等

## CSS 2.1中的伪类

序号  |  选择器  |  含义
---|---|---
13.  |  E:first-child  |  匹配E的父元素的第一个子元素
14.  |  E:link  |  匹配所有未被点击的链接
15.  |  E:visited  |  匹配所有已被点击的链接
16.  |  E:active  |  匹配鼠标正在点击的E元素
17.  |  E:hover  |  匹配鼠标悬停其上的E元素
18.  |  E:focus  |  匹配获得当前焦点的E元素
19.  |  E:lang(c)  |  匹配lang属性等于c的E元素

`:first-child`有坑，慎用，参考网站http://www.cnblogs.com/wangmeijian/p/4562304.html

主要应用在一组相同元素的兄弟元素，如下面的案例，或者ul下的li标签

> **只要E元素是它的父级的第一个子元素，就选中。**它需要同时满足两个条件，一个是“第一个子元素”，另一个是“这个子元素刚好是E”。
同样的还适用于`last-chilad nth-child nth-of-type` 等。

```
span:first-child {
    background-color: lime;
}
上面的CSS作用于下面的HTML:

<div>
  <span>This span is limed!</span>
  <span>This span is not. :(</span>
</div>
```

`:link` 选择器不会设置已经访问过的链接的样式(即只要点击过新的链接，再怎么刷新页面也不会显示link里面设置的样式)。

`:active` 激活链接即代表的是用户按下按键和松开按键之间的时间。通常用于但并不限于 `<a>` 和 `<button>` HTML元素，因此我们常采用下面的先后顺序`link — :visited — :hover — :active。`

`:visited` 出于安全考虑只能在这里设置color相关的样式。

```
body { background-color: #ffffc9 }
a:link { color: blue } /* 未访问链接 */
a:visited { color: purple } /* 已访问链接 */
a:hover { font-weight: bold } /* 用户鼠标悬停 */
a:active { color: lime } /* 激活链接 */
```

## CSS 2.1中的伪元素

序号  |  选择器  |  含义
---|---|---
20. |   E:first-line |   匹配E元素的第一行
21.  |  E:first-letter |   匹配E元素的第一个字母
22. |   E:before  |  在E元素之前插入生成的内容
23.  |  E:after  |  在E元素之后插入生成的内容


## CSS 3的同级元素通用选择器

序号 |   选择器  |  含义
---|---|---
24.   | E ~ F  |  匹配任何在E元素之后的同级F元素

## CSS 3 属性选择器

序号  |  选择器 |   含义
---|---|---
25. |   E[att^="val"]   | 属性att的值以"val"开头的元素
26.  |  E[att$="val"]  |  属性att的值以"val"结尾的元素
27.   | E[att*="val"] |   属性att的值包含"val"字符串的元素

```
<style>
  a[href*="abc"] {
    color:red;
  }
</style>

<a href="abc">第一个子元素</a>
<a href="aabcdef">第二2个子元素</a> // 两个元素都会变色
```

## CSS 3表单元素

序号 |   选择器 |   含义
---|---|---
28.   | E:enabled |   匹配表单中激活的元素
29.   | E:disabled |   匹配表单中禁用的元素
30.   | E:checked   | 匹配表单中被选中的radio（单选框）或checkbox（复选框）元素
31.   | E::selection |   匹配用户当前选中的元素

不能用`input:text`

## CSS 3中的结构性伪类

序号  |  选择器  |  含义
---|---|---
32.  |  E:root |   匹配文档的根元素，对于HTML文档，就是HTML元素
33.  |  E:nth-child(n)   | 匹配其父元素的第n个子元素，第一个编号为1
34.  |  E:nth-last-child(n) |   匹配其父元素的倒数第n个子元素，第一个编号为1
35. |   E:nth-of-type(n)  |  与:nth-child()作用类似，但是仅匹配使用同种标签的元素
36.  |  E:nth-last-of-type(n)  |  与:nth-last-child() 作用类似，但是仅匹配使用同种标签的元素
37. |   E:last-child  |  匹配父元素的最后一个子元素，等同于:nth-last-child(1)
38.  |  E:first-of-type |   匹配父元素下使用同种标签的第一个子元素，等同于:nth-of-type(1)
39.  |  E:last-of-type |   匹配父元素下使用同种标签的最后一个子元素，等同于:nth-last-of-type(1)
40. |   E:only-child  |  匹配父元素下仅有的一个子元素，等同于:first-child:last-child或 :nth-child(1):nth-last-child(1)
41. |   E:only-of-type |   匹配父元素下使用同种标签的唯一一个子元素，等同于:first-of-type:last-of-type或 :nth-of-type(1):nth-last-of-type(1)
42. |   E:empty  |  匹配一个不包含任何子元素的元素，注意，文本节点也被看作子元素

## CSS 3的反选伪类

序号 |   选择器  |  含义
---|---|---
43.  |  E:not(s)  |  匹配不符合当前选择器的任何元素

## CSS 3中的 :target 伪类

序号 |   选择器  |  含义
---|---|---
44.   | E:target  |  匹配文档中特定"id"点击后的效果
