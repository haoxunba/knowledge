
##  Miscellaneous

### DOM Element Methods

`.toArray()` 将jquery获取的DOM伪类集合转为原生DOM数组

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>toArray demo</title>
  <style>
  span {
    color: red;
  }
  </style>
  <script src="https://code.jquery.com/jquery-1.10.2.js"></script>
</head>
<body>

Reversed - <span></span>

<div>One</div>
<div>Two</div>
<div>Three</div>

<script>
function disp( divs ) {
  var a = [];
  for ( var i = 0; i < divs.length; i++ ) {
    a.push( divs[ i ].innerHTML );
  }
  $( "span" ).text( a.join( " " ) );
}

disp( $( "div" ).toArray().reverse() );
</script>

</body>
</html>
```

## .each()

当给jQuery里面的DOM对象绑定不同事件的时候，就用到each()，例如
```
$("#ulList").children("li").each(function (index,ele) {
       $(ele).fadeTo(1,(index+1)/10);
  })
 ```

## [Utilities](https://api.jquery.com/category/utilities/)

### extend

#### 语法

`jQuery.extend([deep], target, object1, object2)`

> `[deep]`代表是否深拷贝，true代表深拷贝，不填该参数则默认浅拷贝

> 第一个参数不能传false，因为一旦传false就会终止合并，直接返回`target`

> 如果`target` `object1` `object2`存在属性相同的情况，一般后面的覆盖前面的，`object2`覆盖`object1`，`object1`覆盖`target`

#### extend深拷贝与浅拷贝区别

```
var object1 = {
  apple: 0,
  banana: { weight: 52, price: 100 },
  cherry: 97
};
var object2 = {
  banana: { price: 200 },
  durian: 100
};
```

下面的是一般合并，默认浅拷贝

```
// Merge object2 into object1
$.extend( object1, object2 );

console.log(object1);
// {"apple":0,"banana":{"price":200},"cherry":97,"durian":100}


```
下面是深拷贝的结果

```
$.extend( true, object1, object2 );

console.log(object1);
//{"apple":0,"banana":{"weight":52,"price":100},"cherry":97}
```

下面是传入false的结果

```
$.extend( false, object1, object2 );

console.log(object1);
//{"apple":0,"banana":{"weight":52,"price":200},"cherry":97,"durian":100}
```

#### 应用场景

`jQuery.extend(object);`为扩展jQuery类本身.为类添加新的方法。

```
 $.extend({
 hello:function(){alert('hello');}
 });
 //调用就是$.hello();
```

## Ajax

### Global Ajax Event Handlers

> As of jQuery 1.9, all the handlers for the jQuery global Ajax events, must be attached to `document`.

例如`$(docuemnt).ajaxStart(function() {})`

> If `$.ajax()` or `$.ajaxSetup()` is called with the global option set to false, the other method will not fire.

#### ajaxSetup

`jQuery.ajaxSetup({})`

ajax初始化默认值，后面单独设置的ajax会覆盖

>  strongly recommend against using this API

#### ajaxStart

`$(document).ajaxStart(fucntion)`

只要有一个ajax发送请求就执行

#### ajaxStop

`$(document).ajaxStop(fucntion)`

页面中所有ajax请求完成，执行stop

#### ajaxSend

`$(document).ajaxSend(function)`

每次ajax请求都会触发`ajaxSend`

可以通过传参来让`ajaxSend`每次执行的内容不同
> event: event object
jqxhr: XMLHttpRequest
settings: Ajax request object

```
$(document).ajaxSend(function(event, jqxhr, settings) {

  if(settings.url === 'example.php') {
    alert('ajaxSend handler');
  }
})

```

#### ajaxSuccess

`$(document).ajaxSuccess(function)`

整体描述和上面的`ajaxSend`相同

也可以设置`$(document).ajaxSuccess(function(event, xhr, settings, data))`,只不过比`ajaxSend`多了一个参数。

> You can get the returned Ajax contents by looking at `xhr.responseXML` or `xhr.responseText` for `xml` and `html` respectively.


#### ajaxError

> This handler is not called for cross-domain script and cross-domain JSONP requests.

应用同上面一样，唯一的区别就是最后参数

`$(document).ajaxError(function(event, xhr, settings, throwError))`

#### ajaxComplete

应用同上面一样，唯一的区别就是最后参数

`$(document).ajaxError(function(event, xhr, settings))`

> You can get the returned Ajax contents by looking at `xhr.responseText.`


单个ajax事件就是
```
$('div').ajax{
  beforeSend:{},
  success:{},
  error:{}
}
```