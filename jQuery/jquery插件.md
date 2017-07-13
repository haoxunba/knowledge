
## [jQuery.color](https://github.com/jquery/jquery-color)

�ò������cssHack���`jquery.animate()`���ܸı䱳��ɫ������

```

<body>
    <div>
        jquery�����Ҫ�ֲ�jquery�������ܸı䱳��ɫ,�ṩһ��cssHack
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

�����ز������ȵ�ͼƬ����������Ŀ�������󣬲Ż����ͼƬ
�����ǲ������ͼƬ�ģ�

### data-original����

ԭ������ʵ��ͼƬ·���ŵ��� data-original ������
lazyload��������ڲ����� data-original ���ԣ��ж����ͼƬ������
�����������Ὣ data-original ��ֵ������Ϊ ͼƬ��src����

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
> ע�⣺ͼƬ`img`�������óߴ�

### ��������

�﷨����

```
 $("img.lazy").lazyload({
   effect: "fadeIn"
 })

```

- `placeholder : "img/grey.gif"`, ��ͼƬ��ǰռλ

    placeholder,ֵΪĳһͼƬ·��.��ͼƬ����ռ�ݽ�Ҫ���ص�ͼƬ��λ��,��ͼƬ����ʱ,ռλͼ�������,������һ���ַ��������÷�������

- `effect: "fadeIn"`, ����ʹ�ú���Ч��

   effect(��Ч),ֵ��show(ֱ����ʾ),fadeIn(����),slideDown(����)��,����fadeIn

- `threshold: 200`,  ��ǰ��ʼ����

   threshold,ֵΪ����,����ҳ��߶�.������Ϊ200,��ʾ����������Ŀ��λ�û���200�ĸ߶�ʱ�Ϳ�ʼ����ͼƬ,�������������û����

- `event: 'click'`, �¼�����ʱ�ż���

  event,ֵ��click(���),mouseover(��껮��),sporty(�˶���),foobar(��).����ʵ�����Ī������ͼƬ�ſ�ʼ����,������ֵδ���ԡ�

- `container: $("#container")`, ��ĳ�����е�ͼƬʵ��Ч��

  container,ֵΪĳ����.lazyloadĬ�������������������ʱ��Ч,���������������������ĳDIV�Ĺ�����ʱ���μ������е�ͼƬ

- `failurelimit : 10` ͼƬ�������ʱ

   failurelimit,ֵΪ����.lazyloadĬ�����ҵ���һ�Ų��ڿɼ��������ͼƬʱ���ټ�������,����HTML�������ҵ�ʱ����ܳ��ֿɼ�������ͼƬ��û���س��������,failurelimit���ڼ���N�ſɼ��������ͼƬ,�Ա�������������.

