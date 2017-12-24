
## [jQuery.color](https://github.com/jquery/jquery-color)

该插件利用cssHack解决`jquery.animate()`不能改变背景色的问题

```

<body>
    <div>
        jquery插件主要弥补jquery动画不能改变背景色,提供一种cssHack
    </div>
    <script src="../jquery-3.2.1.js"></script>
    <script src="./jquery.color.js"></script>
    <script>

        $('div').click(function() {
              $(this).animate({"backgroundColor": "red"}, 2000);
        })
    </script>
</body>
```
## [jquery.lazyload](https://github.com/tuupola/jquery_lazyload)

懒加载插件，会等到图片进入浏览器的可视区域后，才会加载图片
否则，是不会加载图片的！

### data-original属性

原理：把真实的图片路径放到了 data-original 属性中
lazyload插件会在内部解析 data-original 属性，判断如果图片进入了
可视区域，它会将 data-original 的值，设置为 图片的src属性

```
<body>
   <img class="lazy" data-original="img/example.jpg" width="640" height="480">
   <script src="jquery.js"></script>
    <script src="jquery.lazyload.js"></script>
   <script>
        $(function() {
            $("img.lazy").lazyload();
        });
   </script>

</body>
```
> 注意：图片`img`必须设置尺寸

### 其他属性

语法就是

```
 $("img.lazy").lazyload({
   effect: "fadeIn"
 })

```

- `placeholder : "img/grey.gif"`, 用图片提前占位

    placeholder,值为某一图片路径.此图片用来占据将要加载的图片的位置,待图片加载时,占位图则会隐藏,里面是一个字符串，不用发送请求。

- `effect: "fadeIn"`, 载入使用何种效果

   effect(特效),值有show(直接显示),fadeIn(淡入),slideDown(下拉)等,常用fadeIn

- `threshold: 200`,  提前开始加载

   threshold,值为数字,代表页面高度.如设置为200,表示滚动条在离目标位置还有200的高度时就开始加载图片,可以做到不让用户察觉

- `event: 'click'`, 事件触发时才加载

  event,值有click(点击),mouseover(鼠标划过),sporty(运动的),foobar(…).可以实现鼠标莫过或点击图片才开始加载,后两个值未测试…

- `container: $("#container")`, 对某容器中的图片实现效果

  container,值为某容器.lazyload默认在拉动浏览器滚动条时生效,这个参数可以让你在拉动某DIV的滚动条时依次加载其中的图片

- `failurelimit : 10` 图片排序混乱时

   failurelimit,值为数字.lazyload默认在找到第一张不在可见区域里的图片时则不再继续加载,但当HTML容器混乱的时候可能出现可见区域内图片并没加载出来的情况,failurelimit意在加载N张可见区域外的图片,以避免出现这个问题.

