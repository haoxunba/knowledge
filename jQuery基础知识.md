
# jQuery

> - jquery��һ���ȡ�Ķ���ƥ�䵽��Ԫ�ؼ����е�һ��Ԫ�أ���������ӵĻ����Ƕ�ƥ�䵽��Ԫ�ؼ�����ÿ��Ԫ�ض�����

## jQuery��������

### jQuery�������

1. window.onload �¼�ֻ�ܳ���һ��
2. ���������������
3. �򵥹���ʵ�ֺܷ���
4. ���Ի򷽷������ֺܳ����׳���

### �汾����

v1.xx �汾��          ���� IE6-8���Լ����������

> �������Ҫ����IE6-8������Լ���ʹ��1.12�汾��

v2.xx �� v3.xx �汾�� ���� IE9+�� �Լ����������

### jquery��js��ں�������

- ���ظ���
jsֻ�ܼ���һ����jquery���Լ��ض��

- ִ��ʱ��
��Щ�ļ���Դ������ҳ���ĵ����ⲿ��js�ļ����ⲿ��css�ļ���ͼƬ�ȡ�
jQuery����ں����������ĵ�������ɺ󣬾�ִ�С��ĵ��������ָ���ǣ�DOM��������ɺ󣬾Ϳ��Բ���DOM

```
window.onload = function() {}

```

```
$(document.ready(function() {}))

$(function() {})

```

### jquery�����DOM�����໥ת��

```
var demo = document.getElementById('demo');
var $('demo') = $(demo);

var demo = $('demo')[0];
var demo = $('demo').get(0);

```

## ѡ����

### ԭ��js

```
var demo = document.getElementById('demo');
var demo = document.getElementByTagName('demo')

```

### jquery

```
$('#id')

$('.class')

$('div')

$('div p') // ���ѡ����

$('ul>li') // ����ѡ����

$('div1,div2.div3')

$('li:eq(3)') // α��ѡ����

$('li:even')

$('li:odd')

$('li:first')

$('li:last')

$(':checkbox') // ��ѡ����

$(':checked')

$(':text')

$('li[id]') // ����ѡ����

$("li[id='']") //


```

### jquery ͨ��������ȡDOMԪ��

```
$('div').parent() // ��ȡֱ�Ӹ���

$('div').parent('p') // ��ȡ���������е�p��ǩ��ֱ��tml

$('div').children() // ��ȡֱ���Ӽ�

$('div').find('span') // ��ȡ���к��spanԪ�أ��õĽ϶�

$('div').siblings() // ��ȡ�����ֵܽڵ�

$('div').next() // ��ȡ��һ���ֵܽڵ�

$('div').nextAll() // ��ȡ�������е��ֵܽڵ�

$('div').prev()

$('div').prevAll()

$('div').eq(n) // ��ȡ��n��div

$('div').first()

$('div').last()

$('div').filter('.class') // ��ȡ���а���class������div

$('div').not('.class') // ͬ���෴

$('div:eq(n)').index() // ��ȡ��ǰԪ�ص�������

```

## jquery����DOM����

jquery������ԭ��js��������ͨ��

### ������ʽ

#### ������ʽ����

```
$('div').css('color','red');

$('div').css({'color': 'red', 'font-size': '16px'});

$('div').css('color');

```

> ** ���õ���ʽΪ����ʽ�� ��ȡ����ʽ�������ں��ⲿcss **

#### ����ʽ����

```
$('div').addClass('demo');

$('div').removeClass('demo');

$('div').hasClass('demo'); // true false

$('div').toggleClass('demo'); //�оͳ���û�оͼ�

```

> - ��ӡ��Ƴ����л���ʽ�����Դ������������`demo1 demo2 demo3`

> - `addClass`�����Դ���������,Ӧ�ó����������jquery��DOM���������Ե���ӻ��Ƴ���

> ```
div.addClass(function(index, currentClass) {
   // index jqueryα�����е�ǰԪ�����ڵ��±�
   // currentClass ��ǰԪ�ص�����
   if(currentClass !== 'dubble') {
      return 'select'
    }
 })
```

#### �ߴ���ʽ

- `height()` `width()`

����ʱ���ݵĳ��ò�����ʽ��`300 '300px'`������һ��Ҳ���Ե�������`'300'`

```
$('div').height(); //��ȡֵΪ��ֵ����

$('div').width();
```

```
// ������������window document
$('div').innerWidth() // ��ȡ�Ŀ��ֵ����padding

$('div').innnerHeight()

$('div').outWidth() // ��ȡ���ֵ����padding��border

$('div').outHeight()

$('div').outWidth(true) // ��ȡ�Ŀ��ֵ����padding��border��margin

$('div').outHeight(true)

```

> �������з�����������������飬���鲻Ҫʹ��

#### ����ֵ

```
$('div').offset(); // ��ȡ������ĵ����Ͻǣ�document��λ�� ��Ϊleftֵ��topֵ

```
- position

> ��ȡ�����offset parent�ģ���Ԫ������Ķ�λ����Ԫ�أ�λ��

> ����ֵ��һ������left��top���ԵĶ���

> ���û�ж�λ������Ԫ����ô���ú�`.offset()`��ͬ

```
$('div').position(); // ��ȡ�����offset parent�ģ���Ԫ������Ķ�λ����Ԫ�أ�λ��

```

```

$('div').scrollLeft();

$('div').scrollTop(); // ��ȡ�������൱�ڹ�����������λ��

```

### jquery����

#### ��ʾ�����ء����롢���������롢����

```
$('div').show() // Ĭ����ʾ��û�ж���Ч��

$('div').show(slow) // slow��Ӧ600ms normal��Ӧ400ms fast��Ӧ200ms

$('div').show(2000) // Ҳ�����Զ������붯��ִ��ʱ��

$('div').show(2000, callback) // ������ӻص�����

```

`�����`hide slideDown slideUp slideToggle fadeIn fadeOut fadeToggle animate`�����Ĳ����������`show`��ͬ

#### animate�Զ��嶯��

```
$('div').animate({params}, speed, callback)

// {params} ����css���ԣ��ش����������Ҷ�Ӧ����ֵȥ����λ��������ֵ��

// speed ����ִ��ʱ�䣬��ѡ��������д�ʹ���������ִ��

// callback �����ص���������ѡ����������ִ����֮����������

```

#### stopֹͣ����

```
$('div').stop(arg1, arg2)

// arg1 ����ֵ �Ƿ���պ����Ķ��� ��ѡ���� Ĭ��false

// arg2 ����ֵ �Ƿ�����ִ���굱ǰ���� ��ѡ���� Ĭ��false

```


### �ڵ����

#### ��ȡ�ڵ�

```
$('div').html();

$('div').text();

$('div').offsetParent(); // ��ȡ����Ķ�λ������Ԫ��

```

#### �����ڵ�

```
var $span = $('<span></span>')

$('div').html('<span></span>')

$('div').text('�ı�')

$('div').clone() // ���


```

#### ��ӽڵ�

```
$('div').html($span);

$('div').text('�ı�');

$('div').append('<span></span>');

$('div').append($('span')); // ��span׷�ӵ�div��

$('span').appendTo($div); // ͬ���෴

$('span').prepend($div); // ��span��ӵ�div�ĵ�һ���ڵ�ǰ��

$('span').first($div); // ��span��Ϊ�ֵ�Ԫ����ӵ�divǰ��

$('span').after($div); // ͬ���෴

```

- ���`span`Ԫ���Ѿ����ڣ����ԭ��λ��ɾ������ӵ�`div`��ȥ

#### ɾ���ڵ�

```
$('div').html('');

$('div').empty(); // ��������ʾ���div���ӽڵ�

$('div').remove(); // ��ʾ������а����Լ�

```

### ���Բ���

```
$('div').attr('data-key'); // ��ȡ

$('div').attr('data-key', 'hello'); // ����

$('div').removeAttr('data-key'); // �Ƴ�

```

> _���ڱ���`disabled checked selected`�ȶ�̬״̬Ҫʹ��`prop()`����`attr()`_

### form��

```
$('input').val(); // ��ȡ

$('input').val('hello'); // ����

```

## jquery�¼�

### ����¼�

#### click

```
<script>
  $('div').click(function() {
     console.log('ִ������');
     $(this).addClass('select');
  })
</script>

```

> <font color="red">�����е�this��ԭ��DOM�еģ�һ��Ҫת����jquery�����ܵ���jquery�ķ���</font>

#### scroll

```
$('div').scroll(function);

```

## jquery����

## jquery���
