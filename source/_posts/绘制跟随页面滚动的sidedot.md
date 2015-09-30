title: 绘制跟随页面滚动的sidedot
date: 2015-09-30
tags: CSS
---

## 效果一览

![Alt text](/img/1443519867210.png)

![Alt text](/img/1443519937139.png)

- 随页面滚动，dot可自动变化
- 可对每个页面配置title
- CSS绘制样式
- 极少JS代码

## Step by step

### 基本样式

```html
<style>
    .side-dot {
        /*相对视口定位到右侧*/
        position: fixed;
        right: 20px;
        top: 45%;
    }
    .side-dot > ul {
        list-style: none;
    }
    .side-dot > ul > li > a {
        /*转变为块，以撑开外层容器*/
        display: block;
        /*加边距，防止dot挤在一起*/
        padding: 20px 10px;
    }
    .side-dot > ul > li > a:after {
        content: "";
        /*伪元素默认是行内元素，应转变为块*/
        display: block;
        height: 2px;
        width: 2px;
        /*dot左右居中*/
        margin: 0 auto;
        border-radius: 2px;
        border: 1px solid #888;
    }
    .side-dot > ul > li > a.active:after {
        content: "";
        height: 10px;
        width: 10px;
        border-radius: 10px;
    }
</style>

<aside class='side-dot'>
    <ul>
        <li><a href="#page1" class='active'></a></li>
        <li><a href="#page2"></a></li>
        <li><a href="#page3"></a></li>
    </ul>
</aside>

<section class="sp" id='page1' style='height:700px'>This is page 1</section>
<section class="sp" id='page2' style='height:700px'>This is page 2</section>
<section class="sp" id='page3' style='height:700px'>This is page 3</section>

<!--jquery-->
<script src="http://libs.baidu.com/jquery/2.1.4/jquery.min.js"></script>
<script>
    $(function() {
        var topTrigger = screen.height * 0.4;
        $(function() {
            pages = $('.sp');
            window.onscroll = function(e){
                pages.each(function(index, el) {
                    el = $(el);
                    var eTop = el.offset().top;
                    var sTop = $(document).scrollTop();
                    if(eTop>=sTop && eTop-sTop<topTrigger){
                        $('.side-dot a[href]').removeClass('active');
                        $('.side-dot a[href=#'+el.attr('id')+']').addClass('active');
                    }
                });
            }
        });
    });
</script>
```
- `aside>ul>li>a`空标签，做定位占位符；href属性指向页面id
- `section.sp#id` 每个页面都需要加上sp类、页面id

![Alt text](/img/1443523379234.png)

### 在dot旁显示title

原理：CSS查找`.side-dot>ul>li>a.active[title]`，并绝对定位`::before`到dot的左边。content属性写成`attr(title)`即可显示title的值。

CSS不应该查找没有title属性的`.side-dot>ul>li>a.active`，否则title容器内容为空会导致“残废”标签。

![Alt text](/img/1443607984008.png)

```
<style>
    .side-dot {
        /*相对视口定位到右侧*/
        position: fixed;
        right: 20px;
        top: 45%;
    }
    .side-dot > ul {
        list-style: none;
    }
    .side-dot > ul > li > a {
        /*转变为块，以撑开外层容器*/
        display: block;
        /*title的定位依据*/
        position: relative;
        /*加边距，防止dot挤在一起*/
        padding: 20px 10px;
    }
    .side-dot > ul > li > a:after {
        content: "";
        /*伪元素默认是行内元素，应转变为块*/
        display: block;
        height: 2px;
        width: 2px;
        /*dot左右居中*/
        margin: 0 auto;
        border-radius: 2px;
        border: 1px solid #888;
    }
    .side-dot > ul > li > a.active:after {
        content: "";
        height: 10px;
        width: 10px;
        border-radius: 10px;
    }
    .side-dot > ul > li > a.active[title]:before {
        content: attr(title);
        display: block;
        border: 1px solid #888;
        /*内部文字样式*/
        line-height: 18px;
        font-size: 13px;
        padding: 0 5px;
        /*定位*/
        position: absolute;
        right: 30px;
        top: 50%;
        margin-top: -10px;
    }
</style>

<aside class='side-dot'>
    <ul>
        <li><a href="#page1" class='active' title='page1'></a></li>
        <li><a href="#page2" title='page2'></a></li>
        <li><a href="#page3" title='page3'></a></li>
    </ul>
</aside>

<section class="sp" id='page1' style='height:700px'>This is page 1</section>
<section class="sp" id='page2' style='height:700px'>This is page 2</section>
<section class="sp" id='page3' style='height:700px'>This is page 3</section>

<!--jquery-->
<script src="http://libs.baidu.com/jquery/2.1.4/jquery.min.js"></script>
<script>
    $(function() {
        var topTrigger = screen.height * 0.4;
        $(function() {
            pages = $('.sp');
            window.onscroll = function(e){
                pages.each(function(index, el) {
                    el = $(el);
                    var eTop = el.offset().top;
                    var sTop = $(document).scrollTop();
                    if(eTop>=sTop && eTop-sTop<topTrigger){
                        $('.side-dot a[href]').removeClass('active');
                        $('.side-dot a[href=#'+el.attr('id')+']').addClass('active');
                    }
                });
            }
        });
    });
</script>
```

### 样式扩展

- 添加 `.side-dot>ul>li>a[title]:hover:before` 规则，可实现鼠标悬停显示标签
- 先占坑



