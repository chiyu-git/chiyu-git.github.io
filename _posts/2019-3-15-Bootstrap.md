---
layout: post
#标题配置
title:  Bootstrap Note
#时间配置
date:   2019-3-15 21:00:00 +0800
#大类配置
categories: document
#小类配置
tag: note front-end
---

* content
{:toc}


# 1. Bootstrap

- 简洁、直观、强悍的前端开发框架，让web开发更迅速、简单

- css基于less或者sass

- js基于jQuery

- 打包用的是grunt

- 要使用bootstrap要引入css样式表，font字体图标以及js（包括bootstrap.js和jQuery.js）
  - ![1548983762868](F:\OneDrive\JS\纲略合集\assets\1548983762868.png) 

-         bootstrap样式表，所有的盒模型都被改变成了border-box
-         ![1548983784461](F:\OneDrive\JS\纲略合集\assets\1548983784461.png) 
-         ![1548983788564](F:\OneDrive\JS\纲略合集\assets\1548983788564.png) 

- ![1548983797409](F:\OneDrive\JS\纲略合集\assets\1548983797409.png) 
- ![1548983808114](F:\OneDrive\JS\纲略合集\assets\1548983808114.png) 

## 容器

### 流体布局容器

- 容器的width为auto，只是两边加了15px的padding。

- ```html
  <div class="container-fluid">
   test
  </div>
  ```

- ![1548983868042](F:\OneDrive\JS\纲略合集\assets\1548983868042.png) 

### 固定布局

- 容器的width会随设备分辨率的不同而生产变化

**分辨率阈值**

- w >=1200   
  - 容器的width为1170   左右padding为15 （注意是borderBox）

- w >=992          
  - 容器的width为970     左右padding为15 （注意是borderBox）

- w >=768          
  - 容器的width为750     左右padding为15  （注意是borderBox）

- 768 > w      
  - 容器的width为auto    左右padding为15  （注意是borderBox）

- ![1548983922699](F:\OneDrive\JS\纲略合集\assets\1548983922699.png) 

## 栅格系统

### row

- 一行分为12列

### 列宽

- col-lg-x    

  - ```html
    <div class="row">
     <div class="col-lg-10">col-lg-10</div>
     <div class="col-lg-2">col-lg-2</div>
    </div>
    <div class="row">
     <div class="col-lg-6">col-lg-6</div>
     <div class="col-lg-6">col-lg-6</div>
    </div>
    <div class="row">
     <div class="col-lg-4">col-lg-4</div>
     <div class="col-lg-8">col-lg-8</div>
    </div>
    ```

  - 

- col-md-x

- col-sm-x

- col-xs-x

  - x默认拥有12个等级

### 列排序

- push的时候调整的是left，分13个等级（0到12）
  - 0时为auto        

- pull的时候调整的是right，分13个等级（0到12）
  - 0时为auto

### 列偏移

- 调整的是margin-left，分13个等级（0到12）
  - 0时为0%   

## 栅格系统源码分析

- ![1548984096733](F:\OneDrive\JS\纲略合集\assets\1548984096733.png) 

### 容器

**入口grid.less**

- 流体容器的**特定**样式

  - ```less
    //流体容器
    .container-fluid {
    .container-fixed();}
    ```
  ```
  
  ```

- 固定容器的**特定**样式

  - ```less
    .container {
      .container-fixed();
      @media (min-width: @screen-sm-min) {
        width: @container-sm; //720+槽宽
      }
      @media (min-width: @screen-md-min) {
        width: @container-md; //940+槽宽
      }
      @media (min-width: @screen-lg-min) {
        width: @container-lg; //1140+槽宽
      }
    ```

**mixin/grid.less**

- 流体容器&固定容器 公共样式

  - ```less
    // 固定和流体容器的公共样式
    //@grid-gutter-widt：槽宽
    .container-fixed(@gutter: @grid-gutter-width) {
      margin-right: auto;
      margin-left: auto;
      padding-left:  floor((@gutter / 2)); //15px
      padding-right: ceil((@gutter / 2));  //15px
      &:extend(.clearfix all);
    }
    ```

**variables.less**

- xs，sm，md，lg阈值的定义

  - ```less
    // Extra small screen / phone
    //** Deprecated `@screen-xs` as of v3.0.1
    @screen-xs:                  480px;
    //** Deprecated `@screen-xs-min` as of v3.2.0
    @screen-xs-min:              @screen-xs;
    //** Deprecated `@screen-phone` as of v3.0.1
    @screen-phone:               @screen-xs-min;
    
    
    // Small screen / tablet
    //** Deprecated `@screen-sm` as of v3.0.1
    @screen-sm:                  768px;
    @screen-sm-min:              @screen-sm;
    //** Deprecated `@screen-tablet` as of v3.0.1
    @screen-tablet:              @screen-sm-min;
    
    // Medium screen / desktop
    //** Deprecated `@screen-md` as of v3.0.1
    @screen-md:                  992px;
    @screen-md-min:              @screen-md;
    //** Deprecated `@screen-desktop` as of v3.0.1
    @screen-desktop:             @screen-md-min;
    
    // Large screen / wide desktop
    //** Deprecated `@screen-lg` as of v3.0.1
    @screen-lg:                  1200px;
    @screen-lg-min:              @screen-lg;
    //** Deprecated `@screen-lg-desktop` as of v3.0.1
    @screen-lg-desktop:          @screen-lg-min;
    
    // So media queries don't overlap when required, provide a maximum
    @screen-xs-max:              (@screen-sm-min - 1);
    @screen-sm-max:              (@screen-md-min - 1);
    @screen-md-max:              (@screen-lg-min - 1);
    ```

- 不同类型容器的最大宽度

  - ```less
    //## Define the maximum width of `.container` for different screen sizes.
    
    // Small screen / tablet
    @container-tablet:             (720px + @grid-gutter-width);
    //** For `@screen-sm-min` and up.
    @container-sm:                 @container-tablet;
    
    // Medium screen / desktop
    @container-desktop:            (940px + @grid-gutter-width);
    //** For `@screen-md-min` and up.
    @container-md:                 @container-desktop;
    
    // Large screen / wide desktop
    @container-large-desktop:      (1140px + @grid-gutter-width);
    //** For `@screen-lg-min` and up.
    @container-lg:                 @container-large-desktop;
    ```

### 行

**入口grid.less**

- 行的定义

  - ```less
    // 行
    .row {
      .make-row();
    }
    ```

**mixin/grid.less**

+ 行的样式

  + ```less
    // 行
    .make-row(@gutter: @grid-gutter-width) {
      margin-left:  ceil((@gutter / -2));
      margin-right: floor((@gutter / -2));
      &:extend(.clearfix all);
    }
    ```

### 列

**入口grid.less**

- 样式

  - ```less
    // 列，公共样式部分
    .make-grid-columns();
    // 移动优先，个别样式部分
    .make-grid(xs);
    @media (min-width: @screen-sm-min) {
      .make-grid(sm);
    }
    @media (min-width: @screen-md-min) {
      .make-grid(md);
    }
    @media (min-width: @screen-lg-min) {
      .make-grid(lg);
    }
    ```

**mixin/grid-frameworks.less**

- 整个文件都是在定义列

1. 定义列的公共部分

   - ```less
     // 列第一步
     .make-grid-columns() {
     
     .col(@index) { 
       @item: ~".col-xs-@{index}, 
                .col-sm-@{index}, 
                .col-md-@{index}, 
                .col-lg-@{index}";
     //仅仅是避免编译，并不是字符串，所以@index的值已经变成了1，这里同时变成了类的形式，但是不影响变量的值
       .col((@index + 1), @item);
     }
     .col(@index, @list) when (@index =< @grid-columns) { 
        @item: ~".col-xs-@{index}, 
                .col-sm-@{index}, 
                .col-md-@{index}, 
                .col-lg-@{index}";
     //同理，此处@index的值为2，且变成了类的形式
     .col((@index + 1), ~"@{list}, @{item}");
     //递归，当@index=13时，跳出递归
     }
     .col(@index, @list) when (@index > @grid-columns) { 
       @{list} {
     //给递归生成的所有类，添加样式
         position: relative; //列排序需要用到left，right
         min-height: 1px;
         padding-left:  ceil((@grid-gutter-width / 2));
         padding-right: floor((@grid-gutter-width / 2));
       }
     }
     
     .col(1); 
     
     }
     ```

   - **..col(1)的结果**

   - ```less
     /*  递归结束时的@list，总共12（@grid-columns）列
     .col(13,str)
     @list:.col-xs-1, .col-sm-1, .col-md-1, .col-lg-1,
           .col-xs-2, .col-sm-2, .col-md-2, .col-lg-2,
           ...
           .col-xs-12, .col-sm-12, .col-md-12, .col-lg-12
     */
     
     /*生成的样式表
         .col-xs-1, .col-sm-1, .col-md-1, .col-lg-1,
         .col-xs-2, .col-sm-2, .col-md-2, .col-lg-2,
         ...
         .col-xs-12, .col-sm-12, .col-md-12, .col-lg-12{
           position: relative;
           min-height: 1px;
           padding-left: 15px; //槽宽的一半
           padding-right: 15px;
         }
     */
     //由样式表可知，不同设备的列的公共样式都是一致的
     //并且Bootstrap的用户可以随意修改列数
     
     ```

2. 定义列的第二步，各设备特定部分的样式库

   - **@class在哪里决定的？**

   - ```less
     // 列第二步
     .make-grid(@class) {
         //2.1
       .float-grid-columns(@class);
         //2.2
       .loop-grid-columns(@grid-columns, @class, width);
         //2.3(列排序)
       .loop-grid-columns(@grid-columns, @class, pull);
       .loop-grid-columns(@grid-columns, @class, push);
         //2.4(列偏移)
       .loop-grid-columns(@grid-columns, @class, offset);
     }
     ```

   - 2.1使得符合设备的类 左浮动

   - ```less
     .float-grid-columns(@class) {
       .col(@index) { 
         @item: ~".col-@{class}-@{index}";
         .col((@index + 1), @item);
       }
       .col(@index, @list) when (@index =< @grid-columns) { // general
         @item: ~".col-@{class}-@{index}";
         .col((@index + 1), ~"@{list}, @{item}");
       }
       .col(@index, @list) when (@index > @grid-columns) { // terminal
         @{list} {
           float: left;
         }
       }
       .col(1); 
     }
     //整体结构与第一步相似的递归，不同的是值操作了当前设备的相关类
     //结果：给当前设备的所有相关类左浮动
     .col-xs-1,.col-xs-2,.col-xs-3,.col-xs-4,...col-xs-12{
         float: left;
      }
     ```

   - 2.2（width） 2.3（pull push） 2.4（offset）的入口

   - ```less
      //2.2（width） 2.3（pull push） 2.4（offset）的入口
     .loop-grid-columns(@index, @class, @type) when (@index >= 0) {
       .calc-grid-column(@index, @class, @type);
       .loop-grid-columns((@index - 1), @class, @type);
     }//递归混合，@index从12→0
     ```

   - 2.2定义符合设备的类的（width样式库）

   - ```less
     .calc-grid-column(@index, @class, @type) when (@type = width) and (@index > 0) {
     //要求@index>0,定义width时，@index从12→1
       .col-@{class}-@{index} {
     //给每个类分配宽度
         width: percentage((@index / @grid-columns));
       }
     }
     //结果：当我们给不同的列加上这些样式的时候就有了不同的宽度
     .col-xs-12{
          width:12/12;
      }
      .col-xs-11{
          width:11/12;
      }
      ...
      .col-xs-1{
          width:1/12;
     } 
     //@index从12→1
     ```

   - 2.3 列排序样式库

   - ```less
     .calc-grid-column(@index, @class, @type) when (@type = push) and (@index > 0) {
       .col-@{class}-push-@{index} {
         left: percentage((@index / @grid-columns));
       }
     }
     .calc-grid-column(@index, @class, @type) when (@type = push) and (@index = 0) {
       .col-@{class}-push-0 {
         left: auto;
       }
     }
     .calc-grid-column(@index, @class, @type) when (@type = pull) and (@index > 0) {
       .col-@{class}-pull-@{index} {
         right: percentage((@index / @grid-columns));
       }
     }
     .calc-grid-column(@index, @class, @type) when (@type = pull) and (@index = 0) {
       .col-@{class}-pull-0 {
         right: auto;
       }
     }
     //类似于2.2width的递归，定义符合设备的类的push,pull样式库
     //区别：@index从12→1，但是定义了@index（列数）为0的时候，left right 均为auto
     //结果：
     /*push                  pull:
      * .col-xs-push-12{         .col-xs-pull-12{      
      *     left:12/12;              right:12/12;
      * }                        }
      * .col-xs-push-11{
      *     left:11/12;
      * }
      * ...                      ...
      * .col-xs-push-1{
      *     left:1/12;
      * } 
      * .col-xs-push-0{           .col-xs-pull-0{
      *     left:auto;               right:auto;
      * }                         }
      * */
     ```

   - 2.4列偏移样式库

   - ```less
     .calc-grid-column(@index, @class, @type) when (@type = offset) {
       .col-@{class}-offset-@{index} {
         margin-left: percentage((@index / @grid-columns));
       }
     }
     //和2.2,2.3类似，定义符合设备的偏移样式库
     //区别：@index（列数）从12→0
     //结果：
     /*
      .col-xs-offset-12{
          margin-left:12/12;
      }
      .col-xs-offset-11{
          margin-left:11/12;
      }
      ...
      .col-xs-offset-1{
          margin-left:1/12;
      } 
      .col-xs-offset-0{
          margin-left:0;
      } 
     */
     ```

**variables.less**

- ```less
  //** Number of columns in the grid.
   @grid-columns:              12;
  ```

## 栅格组合

```html
<div class="col-lg-10 col-md-6">col-1</div>
<div class="col-lg-2 col-md-6">col-2</div>
```

- 给div添加不同设备的列类，可以实现不同设备下不同的布局

- 当设备为lg的时候，第一列为10/12，第二列为2/12

- 当设备变化为md的时候，第一列为6/12，第二列为6/12

### Bootstrap官网栅格实例

```html
<div  class="col-lg-3 col-md-4 col-sm-6"></div>
```

- 原本响应的时候，结构处于最后的会先换行，现在希望第一个先换行，让结构处于最后，然后排序到第一个去
  - 因为所有的栅格都是开启了相对定位的

- 从源码可知，bootstrap的栅格，left right 都是没有负值的，因为index都是大于0的，所以分配了left right之后也都是正直，所以往左移只能修改right，而不能让left为负值

- 从源码可知，当屏幕是lg的时候，xs、sm、md的样式也都存在，只是被覆盖了，所以如果只写了xs的排序（或偏移），而lg md sm 没有相对应的样式的话，那么无论在什么屏幕下都会用上xs的样式，所以必须在lg md sm 重置 所有xs的样式，在lg重置所有md sm ，在md 重置所有sm

- 因为bootstrap是移动优先

## 自定义栅格系统

- 把栅格部分源码，修改名字即可

- 使用的时候要主动加上全局样式

- box-sizing:border-box

## 响应式工具

- responsive-utilities.less

- 重置样式，令所有屏幕下的都为invisibility

```less
.visible-xs,
.visible-sm,
.visible-md,
.visible-lg {
  .responsive-invisibility();
}

.visible-xs {
  @media (max-width: @screen-xs-max) {
    .responsive-visibility(); //覆盖了被重置的display
  }
}
```

- mixin/responsive-visibility.less

```less
.responsive-visibility() {
  display: block !important;
  table&  { display: table !important; }
  tr&     { display: table-row !important; }
  th&,
  td&     { display: table-cell !important; }
}

.responsive-invisibility() {
  display: none !important;
}
```

- 全都是!important，单独调用即可实现覆盖

**结果：可以做到在不同的屏幕下，显示或隐藏不同的结构**

## clearfix的精妙之处

- 继承不可以加()，要注意，在bootstrap的源码当中所有的.clearfix()继承都加了括号，这是因为

- mixin/clearfix.less     

  - ```less
    .clearfix() {
      &:before,
      &:after {
        content: " "; // 1
        display: table; // 2
      }
      &:after {
        clear: both;
      }
    }
    ```

- utilities.less

  - ```less
    .clearfix {
      .clearfix();
    }
    ```

- 同时实现了clearfix的继承和混合，不加括号就是继承，加了括号就是混合

## 容器与栅格盒模型设计的精妙之处

### 列

- 15px左右padding
  - 两列之间的内容不会挨到一起，排版，美观

### 行

- -15px左右margin
  - 因为列有了15px左右padding，如果在列中嵌套行（行中有列）的话，会导致中间的槽宽变成15px（嵌套行的列的一半槽宽）+30（列中的槽宽），就变成了45px
  - 行如果加了左右-15pxmargin，正好抵消掉列的15pxpadding，维持了槽宽，和排版

### 容器

- 15px左右padding
  - 行有了-15px的margin之后，就溢出了容器，为了让容器继续包裹着行，所以加了左右padding

## Bootstrap定制化

```less
@import "../less/bootstrap.less";
@grid-gutter-width:200px;
```

- 最好不要直接修改源码，引入入口文件，然后在自己的一个less里修改对应的变量值即可

## Bootstrap实例

1. 起步→基本模板，先复制一份

2. 提高IE的兼容性和兼容移动端

   - ```html
     <meta http-equiv="X-UA-Compatible" content="IE=edge">
     <meta name="viewport" content="width=device-width, initial-scale=1,user-scalable=no">
     ```

2. 兼容IE9

   - ```html
     <!--[if lt IE 9]>
       <script src="js/html5shiv.js"></script>
       <script src="js/respond.min.js"></script>
     <![endif]-->
     ```

   - 也可以在cdn上在线引入

### 组件→导航条

- 反色导航条
  - `<nav class="navbar navbar-inverse">`

- 固体容器
  - `<div class="container">`

- 固定在顶部
  - navbar-fixed-top

### JS插件→Carousel

- 图片大小转换为响应式，跟着width即可或者用Bootstrap的组件
  - img{width: 100%; }

- 控制轮播速度
  - 用类 data-interval="1000"
  - 用jQuery

```js
$(function(){
  //1.链式调用  2.读写二合一  3.隐式迭代  4.编码函数化  $('.carousel').carousel({
   interval: 2000,
   pause:null,
   wrap:false
 })
```



### 三列布局

- 文本居中：text-center类

### 标签页

- js相关的要一一对应，改了tab的标题，js获取用的class aria-controls也要换成对应的

- 点击导航tab标签，调转到对应页面

### 模态框

- 一般写在页面结构的外部

- ![1548990764141](F:\OneDrive\JS\纲略合集\assets\1548990764141.png) 

## Bootstrap其他类

- img-responsive
  - 图片超出屏幕的话，自动适应屏幕

- text-center
- Bootstrap自带了一些预定义的按钮颜色。浅蓝色 `btn-info` 被用在那些用户可能会采取的操作上。

- Bootstrap自带了一些预定义的按钮颜色。红色`btn-danger`被用来提醒用户该操作具有“破坏性”，例如删除一张猫的图片。

# 2. UI框架

- ![1548990808580](F:\OneDrive\JS\纲略合集\assets\1548990808580.png) 

