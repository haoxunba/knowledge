
# jQuery

> - jquery中一般获取的都是匹配到的元素集合中第一个元素，而设置添加的话则是对匹配到的元素集合中每个元素都设置

## jQuery基础介绍

### jQuery解决问题

1. window.onload 事件只能出现一次
2. 浏览器兼容性问题
3. 简单功能实现很繁琐
4. 属性或方法的名字很长容易出错、

### 版本介绍

v1.xx 版本：          兼容 IE6-8，以及其他浏览器

> 如果你需要兼容IE6-8，你可以继续使用1.12版本。

v2.xx 和 v3.xx 版本： 兼容 IE9+， 以及其他浏览器

### jquery和js入口函数区别

- 加载个数
js只能加载一个，jquery可以加载多个

- 执行时机
这些文件资源包括：页面文档、外部的js文件、外部的css文件、图片等。
jQuery的入口函数，是在文档加载完成后，就执行。文档加载完成指的是：DOM树加载完成后，就可以操作DOM

```
window.onload = function() {}

```

```
$(document.ready(function() {}))

$(function() {})

```

### jquery对象和DOM对象相互转换

```
var demo = document.getElementById('demo');
var $('demo') = $(demo);

var demo = $('demo')[0];
var demo = $('demo').get(0);

```

## 选择器

### 原生js

```
var demo = document.getElementById('demo');
var demo = document.getElementByTagName('demo')

```

### jquery

```
$('#id')

$('.class')

$('div')

$('div p') // 后代选择器

$('ul>li') // 父子选择器

$('div1,div2.div3')

$('li:eq(3)') // 伪类选择器

$('li:even')

$('li:odd')

$('li:first')

$('li:last')

$(':checkbox') // 表单选择器

$(':checked')

$(':text')

$('li[id]') // 属性选择器

$("li[id='']") //


```

### jquery 通过方法获取DOM元素

```
$('div').parent() // 获取直接父级

$('div').parent('p') // 获取父级中所有的p标签，直到tml

$('div').children() // 获取直接子级

$('div').find('span') // 获取所有后代span元素，用的较多

$('div').siblings() // 获取所有兄弟节点

$('div').next() // 获取下一个兄弟节点

$('div').nextAll() // 获取后面所有的兄弟节点

$('div').prev()

$('div').prevAll()

$('div').eq(n) // 获取第n个div

$('div').first()

$('div').last()

$('div').filter('.class') // 获取所有包含class类名的div

$('div').not('.class') // 同上相反

$('div:eq(n)').index() // 获取当前元素的索引号

```

## jquery操作DOM方法

jquery方法和原生js方法不能通用

### 操作样式

#### 基本样式操作

```
$('div').css('color','red');

$('div').css({'color': 'red', 'font-size': '16px'});

$('div').css('color');

```

> ** 设置的样式为行内式， 获取的样式包括行内和外部css **

#### 类样式操作

```
$('div').addClass('demo');

$('div').removeClass('demo');

$('div').hasClass('demo'); // true false

$('div').toggleClass('demo'); //有就除，没有就加

```

> - 添加、移除、切换样式都可以传多个参数，如`demo1 demo2 demo3`

> - `addClass`还可以传函数参数,应用场景就是针对jquery的DOM集合条件性的添加或移除类

> ```
div.addClass(function(index, currentClass) {
   // index jquery伪数组中当前元素所在的下标
   // currentClass 当前元素的类名
   if(currentClass !== 'dubble') {
      return 'select'
    }
 })
```

#### 尺寸样式

- `height()` `width()`

设置时传递的常用参数格式是`300 '300px'`，还有一种也可以但不常用`'300'`

```
$('div').height(); //获取值为数值类型

$('div').width();
```

```
// 下述不适用于window document
$('div').innerWidth() // 获取的宽度值包括padding

$('div').innnerHeight()

$('div').outWidth() // 获取宽度值包括padding和border

$('div').outHeight()

$('div').outWidth(true) // 获取的宽度值包括padding和border和margin

$('div').outHeight(true)

```

> 上述六中方法方法存在许多争议，建议不要使用

#### 坐标值

```
$('div').offset(); // 获取相对于文档左上角（document）位置 分为left值和top值

```
- position

> 获取相对于offset parent的（该元素最近的定位祖先元素）位置

> 返回值是一个包括left和top属性的对象

> 如果没有定位的祖先元素那么作用和`.offset()`相同

```
$('div').position(); // 获取相对于offset parent的（该元素最近的定位祖先元素）位置

```

```

$('div').scrollLeft();

$('div').scrollTop(); // 获取或设置相当于滚动条顶部的位置

```

### jquery动画

#### 显示、隐藏、滑入、滑出、淡入、淡出

```
$('div').show() // 默认显示，没有动画效果

$('div').show(slow) // slow对应600ms normal对应400ms fast对应200ms

$('div').show(2000) // 也可以自定义输入动画执行时间

$('div').show(2000, callback) // 可以添加回调函数

```

`后面的`hide slideDown slideUp slideToggle fadeIn fadeOut fadeToggle animate`所传的参数和上面的`show`相同

#### animate自定义动画

```
$('div').animate({params}, speed, callback)

// {params} 代表css属性，必传参数，并且对应属性值去掉单位必须是数值型

// speed 动画执行时间，可选参数，不写就代表动画立即执行

// callback 动画回调函数，可选参数，动画执行完之后做的事情

```

#### stop停止动画

```
$('div').stop(arg1, arg2)

// arg1 布尔值 是否清空后续的动画 可选参数 默认false

// arg2 布尔值 是否立即执行完当前动画 可选参数 默认false

```


### 节点操作

#### 获取节点

```
$('div').html();

$('div').text();

$('div').offsetParent(); // 获取最近的定位的祖先元素

```

#### 创建节点

```
var $span = $('<span></span>')

$('div').html('<span></span>')

$('div').text('文本')

$('div').clone() // 深拷贝


```

#### 添加节点

```
$('div').html($span);

$('div').text('文本');

$('div').append('<span></span>');

$('div').append($('span')); // 将span追加到div中

$('span').appendTo($div); // 同上相反

$('span').prepend($div); // 将span添加到div的第一个节点前面

$('span').first($div); // 将span作为兄弟元素添加到div前面

$('span').after($div); // 同上相反

```

- 如果`span`元素已经存在，则从原来位置删除，添加到`div`中去

#### 删除节点

```
$('div').html('');

$('div').empty(); // 这两个表示清空div的子节点

$('div').remove(); // 表示清空所有包括自己

```

### 属性操作

```
$('div').attr('data-key'); // 获取

$('div').attr('data-key', 'hello'); // 设置

$('div').removeAttr('data-key'); // 移除

```

> _对于表单的`disabled checked selected`等动态状态要使用`prop()`代替`attr()`_

### form表单

```
$('input').val(); // 获取

$('input').val('hello'); // 设置

```

## jquery事件

### 鼠标事件

#### click

```
<script>
  $('div').click(function() {
     console.log('执行了吗');
     $(this).addClass('select');
  })
</script>

```

> <font color="red">函数中的this是原生DOM中的，一定要转换成jquery，才能调用jquery的方法</font>

#### scroll

```
$('div').scroll(function);

```

## jquery补充

## jquery插件
