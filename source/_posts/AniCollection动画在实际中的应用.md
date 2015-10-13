title: AniCollection动画在实际中的应用
date: 2015-10-13 23:17:49
tags: CSS
---


[AniCollection](http://anicollection.github.io/#/)是一个常用CSS3动画的在线“货架”，免费提供了许多如强调、渐入渐出、飞行、旋转、碰撞等的动画效果给开发者使用。

![Alt text](/img/1444741136041.png)

-----------------

AniCollection提供的动画源码，动效完全由CSS3的animation特性实现，用js完成动效的触发。

我们以元素fadeIn动效为例，简要说明如何借助AniCollection快速绘制网页动画。

![Alt text](/img/1444741621843.png)

## fadeIn动效源码概览与基本用法

主要思路是：
- html部分，对需要动画的元素加上`id`和`class='fadeIn animated'`。`id`存粹是为了js引用。`animated`类引入动效时长样式，`fadeIn`类定义动效帧。
- css部分，还包括`.animated.infinite`和`.animated.hinge`
- js部分，在元素上绑定`mouseover`事件，触发即为元素添加`class='fadeIn animated'`。当动效结束后，元素trigger`animationend`事件，js捕获这个事件，然后移除class`fadeIn animated`。

### html
```html
<div id="square" class="fadeIn animated"> Square </div>
```

### CSS
```
/*base code*/
.animated {
  -webkit-animation-duration: 1s;
  animation-duration: 1s;
  -webkit-animation-fill-mode: both;
  animation-fill-mode: both;
}
.animated.infinite {
  -webkit-animation-iteration-count: infinite;
  animation-iteration-count: infinite;
}
.animated.hinge {
  -webkit-animation-duration: 2s;
  animation-duration: 2s;
}
/*the animation definition*/
@-webkit-keyframes fadeIn {
  0% {
    opacity: 0
  }
  100% {
    opacity: 1
  }
}
@keyframes fadeIn {
  0% {
    opacity: 0
  }
  100% {
    opacity: 1
  }
}
.fadeIn {
  -webkit-animation-name: fadeIn;
  animation-name: fadeIn
}
```

### javascript
```javascript
// Tip: avoid this ton of code using AniJS ;)

var element = $('#square');

// when mouseover execute the animation
element.mouseover(function(){
  
  // the animation starts
  element.toggleClass('fadeIn animated');
  
  // do something when animation ends
  element.one('webkitAnimationEnd mozAnimationEnd MSAnimationEnd oanimationend animationend', function(e){
   
   // trick to execute the animation again
    $(e.target).removeClass('fadeIn animated');
  
  });
  
});
```

### 使用技巧

实际使用时，AniCollection提供的默认js代码并不方便，主要体现在
- 不一定希望绑定`mouseover`事件
- 动效名称被硬编码入js，缺乏灵活性

可写一个简单的jQuery插件，解决上述问题，充分挖掘AniCollection的使用效率:
- 可定义动效结束回调函数
- 元素行内传参

于是在html中，动效名称可写在元素的`ani-style`属性中
```html
<div id='need-animate' ani-style='fadeInRight'>
```

jQuery插件写法
```javascript
/**
 * animate动画jQuery插件
 * 需要与 AniCollection 配合
 * http://anicollection.github.io/
 *
 * Author: concefly@foxmail.com
 * 2015年10月13日
 */

/**
 * Example:
 * 
 * $(selector).aniCollection({
 *  style: 'fadeIn',
 *  callback: function (ele){...}
 * });
 *
 * - style
 * 动画类型，优先级 attr(ani-style) < options.style
 */

(function ($) {

    // 默认值
    var defaults = {};
    
    /**
     * Start animation
     * @param  {[type]} ele     [description]
     * @param  {[type]} options [description]
     * @return {[type]}         [description]
     */
    var animateStart = function (ele,options) {
        var aniStyle = options.style || ele.attr('ani-style');
        var classString = 'animated '+aniStyle;
        ele.addClass(classString);
        ele.one('webkitAnimationEnd mozAnimationEnd MSAnimationEnd oanimationend animationend', function(event) {
            ele.removeClass(classString);
            if (options.callback) {options.callback(ele)};
        });
        return ele; 
    }

    $.fn.aniCollection = function (options) {
        options = $.extend(defaults,options);
        return this.each(function(index, el) {
            animateStart($(el),options);
        });
    }

})(jQuery);
```