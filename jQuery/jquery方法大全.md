
##  Miscellaneous

### DOM Element Methods

`.toArray()` ��jquery��ȡ��DOMα�༯��תΪԭ��DOM����

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

����jQuery�����DOM����󶨲�ͬ�¼���ʱ�򣬾��õ�each()������
```
$("#ulList").children("li").each(function (index,ele) {
       $(ele).fadeTo(1,(index+1)/10);
  })
 ```

## [Utilities](https://api.jquery.com/category/utilities/)

### extend

#### �﷨

`jQuery.extend([deep], target, object1, object2)`

> `[deep]`�����Ƿ������true�������������ò�����Ĭ��ǳ����

> ��һ���������ܴ�false����Ϊһ����false�ͻ���ֹ�ϲ���ֱ�ӷ���`target`

> ���`target` `object1` `object2`����������ͬ�������һ�����ĸ���ǰ��ģ�`object2`����`object1`��`object1`����`target`

#### extend�����ǳ��������

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

�������һ��ϲ���Ĭ��ǳ����

```
// Merge object2 into object1
$.extend( object1, object2 );

console.log(object1);
// {"apple":0,"banana":{"price":200},"cherry":97,"durian":100}


```
����������Ľ��

```
$.extend( true, object1, object2 );

console.log(object1);
//{"apple":0,"banana":{"weight":52,"price":100},"cherry":97}
```

�����Ǵ���false�Ľ��

```
$.extend( false, object1, object2 );

console.log(object1);
//{"apple":0,"banana":{"weight":52,"price":200},"cherry":97,"durian":100}
```

#### Ӧ�ó���

`jQuery.extend(object);`Ϊ��չjQuery�౾��.Ϊ������µķ�����

```
 $.extend({
 hello:function(){alert('hello');}
 });
 //���þ���$.hello();
```

## Ajax

### Global Ajax Event Handlers

> As of jQuery 1.9, all the handlers for the jQuery global Ajax events, must be attached to `document`.

����`$(docuemnt).ajaxStart(function() {})`

> If `$.ajax()` or `$.ajaxSetup()` is called with the global option set to false, the other method will not fire.

#### ajaxSetup

`jQuery.ajaxSetup({})`

ajax��ʼ��Ĭ��ֵ�����浥�����õ�ajax�Ḳ��

>  strongly recommend against using this API

#### ajaxStart

`$(document).ajaxStart(fucntion)`

ֻҪ��һ��ajax���������ִ��

#### ajaxStop

`$(document).ajaxStop(fucntion)`

ҳ��������ajax������ɣ�ִ��stop

#### ajaxSend

`$(document).ajaxSend(function)`

ÿ��ajax���󶼻ᴥ��`ajaxSend`

����ͨ����������`ajaxSend`ÿ��ִ�е����ݲ�ͬ
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

���������������`ajaxSend`��ͬ

Ҳ��������`$(document).ajaxSuccess(function(event, xhr, settings, data))`,ֻ������`ajaxSend`����һ��������

> You can get the returned Ajax contents by looking at `xhr.responseXML` or `xhr.responseText` for `xml` and `html` respectively.


#### ajaxError

> This handler is not called for cross-domain script and cross-domain JSONP requests.

Ӧ��ͬ����һ����Ψһ���������������

`$(document).ajaxError(function(event, xhr, settings, throwError))`

#### ajaxComplete

Ӧ��ͬ����һ����Ψһ���������������

`$(document).ajaxError(function(event, xhr, settings))`

> You can get the returned Ajax contents by looking at `xhr.responseText.`


����ajax�¼�����
```
$('div').ajax{
  beforeSend:{},
  success:{},
  error:{}
}
```