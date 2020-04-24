---
layout: post
#标题配置
title:  Swan-module
#时间配置
date:   2020-4-24 21:00:00 +0800
#大类配置
categories: document
#小类配置
tag: note derive-front-end
---

* content
{:toc}


# 原生组件

## 概述

原生组件是由客户端创建的原生组件。

**包括**：[canvas](https://smartprogram.baidu.com/docs/develop/component/canvas/)、[map](https://smartprogram.baidu.com/docs/develop/component/map/)、[animation-view](https://smartprogram.baidu.com/docs/develop/component/base_animation-view-Lottie/)、[textarea](https://smartprogram.baidu.com/docs/develop/component/formlist_textarea/)、[cover-view](https://smartprogram.baidu.com/docs/develop/component/view_cover-view/)、[cover-image](https://smartprogram.baidu.com/docs/develop/component/view_cover-image/)、[camera](https://smartprogram.baidu.com/docs/develop/component/media_camera/)、[video](https://smartprogram.baidu.com/docs/develop/component/media_video/)、[live-player](https://smartprogram.baidu.com/docs/develop/component/media_live-player/)、[input](https://smartprogram.baidu.com/docs/develop/component/formlist_input/)。

**其中，video组件（v3.70.0起）、input组件（v3.105.0起）已支持同层渲染。**

为了解决原生组件层级最高的限制。小程序专门提供了 **cover-view 和 cover-image 组件**，可以覆盖在部分原生组件上面。这两个组件也是原生组件，但是使用限制与其他原生组件有所不同。

## 使用方法

```html
<cover-image 
    class="cover-image"
    src="https://b.bdstatic.com/miniapp/image/cover-image.png"
    bindload="imageLoad"
    binderror="imageError">
</cover-image>
```

## 意义

## 使用限制

由于原生组件脱离在 web-view 渲染流程外，因此在使用时有以下限制：

- 原生组件的层级是最高的，所以页面中的其他组件无论设置 z-index 为多少，都无法盖在原生组件上。后插入的原生组件可以覆盖之前的原生组件。
- 原生组件无法在 scroll-view、swiper、picker-view、movable-view 中使用，下面示例为错误写法。
- 部分CSS样式无法应用于原生组件，例如：
  - 无法对原生组件设置 CSS 动画；
  - 不能在父级节点使用 overflow: hidden 来裁剪原生组件的显示区域。
- 在IOS下，video 组件暂时不支持触摸相关事件。
- 原生组件会遮挡 vConsole 弹出的调试面板。

在工具上，原生组件是用web组件模拟的，因此很多情况并不能很好的还原真机的表现，建议开发者在使用到原生组件时尽量在真机上进行调试。

## 同层渲染

同层渲染是为了解决原生组件的层级问题，在支持同层渲染后，原生组件与其它组件可以随意叠加，有关层级的限制将不再存在。**当前 video, input 组件已支持同层渲染**。

在同层渲染模式下：

- 无需使用 cover-view、cover-image 组件来覆盖同层渲染组件。
- 可在滚动组件，如 scroll-view、swiper、movable-view 等组件中使用同层渲染组件。
- 可直接通过 z-index 属性对同层渲染组件进行层级控制。
- 同层渲染组件不会遮挡 sConsole 弹出的调试面板。

# 视图容器

## cover-image 图片视图

覆盖在[原生组件](https://smartprogram.baidu.com/docs/develop/component/native/)之上的图片视图（与 cover-view 相比，仅支持图片）,支持嵌套在 [cover-view](https://smartprogram.baidu.com/docs/develop/component/view_cover-view/) 里。

| 属性名    | 类型        | 默认值 | 必填 | 说明                                                     |
| :-------- | :---------- | :----- | :--- | :------------------------------------------------------- |
| src       | String      |        | 否   | 图标路径，支持临时路径、网络地址。暂不支持 base64 格式。 |
| bindload  | EventHandle |        | 否   | 图片加载成功时触发                                       |
| binderror | EventHandle |        | 否   | 图片加载失败时触发                                       |

**示例**

[ 在开发者工具中打开](swanide://fragment/a1d48f7900a056964094d7f1572307301585119691234)

### 注意

- 支持 css transition 动画，transition-property 只支持 transform (translateX, translateY) 与 opacity。
- 文本建议都套上 cover-view 标签，避免排版错误。
- 只支持基本的定位、布局、文本样式。不支持设置单边的 border、background-image、shadow、overflow: visible 等。
- 建议子节点不要溢出父节点。
- 默认设置的样式有：white-space: nowrap; line-height: 1.2; display: block。
- 建议不要频繁改变 s-if 表达式的值控制显隐，否则会导致 cover-view 显示异常。
- Bug：IOS端暂不支持一个页面有多个video时嵌套cover-view。
- cover-view 和 cover-image 从基础库版本1.12.0开始支持事件捕获、冒泡。
- cover-image和cover-view的渲染顺序与页面中的标签使用顺序一致。

+ cover-view 为原生组件，原生组件为系统提供的控件不支持单边设置；对于 cover-view 只支持基本的定位、布局、文本样式。不支持设置单边的 border、background-image、shadow、overflow: visible 等。

## cover-view 文本视图

覆盖在[原生组件](https://smartprogram.baidu.com/docs/develop/component/native/)之上的文本视图。只支持嵌套cover-view、cover-image组件。客户端创建的[原生组件](https://smartprogram.baidu.com/docs/develop/component/native/)，不支持嵌套在其它组件中使用。

| 属性       | 类型   | 默认值 | 必填 | 说明                                                         |
| :--------- | :----- | :----- | :--- | :----------------------------------------------------------- |
| scroll-top | number |        | 否   | 设置顶部滚动偏移量，仅在设置了overflow-y: scroll成为滚动元素后生效 |

**示例**

[在开发者工具中打开](swanide://fragment/ad3dfe2f031ec797da085790565926ef1585119690412)

## movable-area 可移动视图区域

`movable-view` 的可移动区域。

| 属性名     | 类型    | 默认值 | 必填 | 说明                                                         |
| :--------- | :------ | :----- | :--- | :----------------------------------------------------------- |
| scale-area | Boolean | false  | 否   | 当里面的movable-view设置为支持双指缩放时，设置此值可将缩放手势生效区域修改为整个movable-area 。 |

**示例**

[ 在开发者工具中打开](swanide://fragment/80398decc4d8f47f6aea76ebfb74fd111585119690957)

## movable-view 可移动视图容器

可移动的视图容器，在页面中可以拖拽滑动。

| 属性名        | 类型        | 默认值 | 必填 | 说明                                                         |
| :------------ | :---------- | :----- | :--- | :----------------------------------------------------------- |
| direction     | String      | none   | 否   | movable-view 的移动方向，属性值有 all 、 vertical 、 horizontal 、 none |
| inertia       | Boolean     | false  | 否   | movable-view 是否带有惯性                                    |
| out-of-bounds | Boolean     | false  | 否   | 超过可移动区域后，movable-view 是否还可以移动。              |
| x             | Number      |        | 否   | 定义 x 轴方向的偏移，如果 x 的值不在可移动范围内，会自动移动到可移动范围；改变 x 的值会触发动画。 |
| y             | Number      |        | 否   | 定义 y 轴方向的偏移，如果 y 的值不在可移动范围内，会自动移动到可移动范围；改变 y 的值会触发动画。 |
| damping       | Number      | 20     | 否   | 阻尼系数，用于控制 x 或 y 改变时的动画和过界回弹的动画，值越大移动越快。 |
| friction      | Number      | 2      | 否   | 摩擦系数，用于控制惯性滑动的动画，值越大摩擦力越大，滑动越快停止；必须大于 0，否则会被设置成默认值。 |
| disabled      | Boolean     | false  | 否   | 是否禁用                                                     |
| scale         | Boolean     | false  | 否   | 是否支持双指缩放，默认缩放手势生效区域是在movable-view内。   |
| scale-min     | Number      | 0.5    | 否   | 定义缩放倍数最小值                                           |
| scale-max     | Number      | 10     | 否   | 定义缩放倍数最大值                                           |
| scale-value   | Number      | 1      | 否   | 定义缩放倍数，取值范围为 0.5 - 10 。                         |
| animation     | Boolean     | true   | 否   | 是否使用动画                                                 |
| bindchange    | EventHandle |        | 否   | 拖动过程中触发的事件，event.detail = {x: x, y: y, source: source}，其中source表示产生移动的原因，值可为touch（拖动）。 |
| bindscale     | EventHandle |        | 否   | 缩放过程中触发的事件，event.detail = {x: x, y: y, scale: scale} |
| htouchmove    | EventHandle |        | 否   | 手指初次触摸后发生横向移动，如果catch此事件，则意味着touchmove事件也被catch |
| vtouchmove    | EventHandle |        | 否   | 手指初次触摸后发生纵向移动，如果catch此事件，则意味着touchmove事件也被catch |

**示例**

[在开发者工具中打开](swanide://fragment/80398decc4d8f47f6aea76ebfb74fd111585119690957)

### 注意

- movable-view 必须设置 width 和 height 属性，不设置默认为 10px。
- movable-view 默认为绝对定位，top 和 left 属性为 0px。
- 当 movable-view 小于 movable-area 时，movable-view 的移动范围是在 movable-area 内。
- 当 movable-view 大于 movable-area 时，movable-view 的移动范围必须包含 movable-area（x 轴方向和 y 轴方向分开考虑）。
- movable-view 必须在组件中，并且必须是直接子节点，否则不能移动。

## scroll-view 可滚动视图区域

可滚动视图区域，可实现横向滚动和竖向滚动。使用竖向滚动时，需要给定一个固定高度，可以通过css来设置height。

| 属性名                | 类型             | 默认值 | 必填 | 说明                                                         |
| :-------------------- | :--------------- | :----- | :--- | :----------------------------------------------------------- |
| scroll-x              | Boolean          | false  | 否   | 允许横向滚动                                                 |
| scroll-y              | Boolean          | false  | 否   | 允许纵向滚动                                                 |
| upper-threshold       | Number \| String | 50     | 否   | 距顶部/左边多远时（单位 px），触发 scrolltoupper 事件        |
| lower-threshold       | Number \| String | 50     | 否   | 距底部/右边多远时（单位 px），触发 scrolltolower 事件        |
| scroll-top            | Number \| String |        | 否   | 设置竖向滚动条位置。要动态设置滚动条位置，用法`scroll-top="{= scrollTop =}"` |
| scroll-left           | Number \| String |        | 否   | 设置横向滚动条位置。要动态设置滚动条位置，用法`scroll-left="{= scrollLeft =}"` |
| scroll-into-view      | String           |        | 否   | 值应为某子元素 id（id 不能以数字开头）,设置滚动方向后，按方向滚动到该元素，动态设置用法`scroll-into-view="{= scrollIntoView =}"`。 |
| scroll-with-animation | Boolean          | false  | 否   | 在设置滚动条位置时使用动画过渡                               |
| enable-back-to-top    | Boolean          | false  | 否   | ios点击顶部导航栏、安卓双击标题栏时，滚动条返回顶部，只支持竖向 |
| bindscrolltoupper     | EventHandle      |        | 否   | 滚动到顶部/左边，会触发 scrolltoupper 事件                   |
| bindscrolltolower     | EventHandle      |        | 否   | 滚动到底部/右边，会触发 scrolltolower 事件                   |
| bindscroll            | EventHandle      |        | 否   | 滚动时触发， event.detail = {scrollLeft, scrollTop, scrollHeight, scrollWidth, deltaX, deltaY} |

### 参考示例

横向滚动嵌套纵向滚动

纵向锚点：[ 在开发者工具中打开](swanide://fragment/6ee800a0957e3701c72c6c4ccff649d41576571802737)

### 注意

- 请勿在 scroll-view 中使用 textarea、map、canvas、video 组件；更多请看[原生组件说明](https://smartprogram.baidu.com/docs/develop/component/native/)。
- scroll-into-view 的优先级低于 scroll-top、scroll-left。
- 在滚动 scroll-view 时会阻止页面回弹，所以在 scroll-view 中滚动，是无法触发 onPullDownRefresh。
- 若要使用下拉刷新，请使用页面的滚动，而不是 scroll-view。
- scroll-into-view、scroll-top、scroll-left 需要在页面数据高度（或宽度）撑开时生效，若有异步加载数据，请在数据渲染完成时，重新动态赋值，才可生效。
- 在设置 scroll-view 组件 height 属性不是内容可视区总高度时，使用 swan.pageScrollTo() API 无法生效。
- 使用竖向滚动时，需要给 `<scroll-view> `一个固定高度，通过 CSS 设置 height，否则可能导致`scroll-top`无效

## swiper 滑块视图容器

滑块视图容器。内部只允许使用`<swiper-item>`组件描述滑块内容，否则会导致未定义的行为。

| 属性名                 | 类型        | 默认值            | 必填 | 说明                                                         | 最低版本                                                     |
| :--------------------- | :---------- | :---------------- | :--- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| indicator-dots         | Boolean     | false             | 否   | 是否显示面板指示点                                           | -                                                            |
| indicator-color        | Color       | rgba(0, 0, 0, .3) | 否   | 指示点颜色                                                   | -                                                            |
| indicator-active-color | Color       | #333              | 否   | 当前选中的指示点颜色                                         | -                                                            |
| autoplay               | Boolean     | false             | 否   | 是否自动切换                                                 | -                                                            |
| current                | Number      | 0                 | 否   | 当前所在页面的 index                                         | -                                                            |
| current-item-id        | String      |                   | 否   | 当前所在滑块的 item-id，不能与 current 被同时指定            | 1.11 低版本请做[兼容性处理](https://smartprogram.baidu.com/docs/develop/swan/compatibility/) |
| interval               | Number      | 5000              | 否   | 自动切换时间间隔，单位ms                                     | -                                                            |
| duration               | Number      | 500               | 否   | 滑动动画时长，单位ms                                         | -                                                            |
| circular               | Boolean     | false             | 否   | 是否采用衔接滑动                                             | -                                                            |
| vertical               | Boolean     | false             | 否   | 滑动方向是否为纵向                                           | -                                                            |
| previous-margin        | String      | `"0px"`           | 否   | 前边距，可用于露出前一项的一小部分，支持px和rpx              | 1.11 低版本请做[兼容性处理](https://smartprogram.baidu.com/docs/develop/swan/compatibility/) |
| next-margin            | String      | `"0px"`           | 否   | 后边距，可用于露出后一项的一小部分，支持px和rpx              | 1.11 低版本请做[兼容性处理](https://smartprogram.baidu.com/docs/develop/swan/compatibility/) |
| display-multiple-items | Number      | 1                 | 否   | 同时显示的滑块数量                                           | 1.11 低版本请做[兼容性处理](https://smartprogram.baidu.com/docs/develop/swan/compatibility/) |
| bindchange             | EventHandle |                   | 否   | current 改变时会触发 change 事件，event.detail = {current: current, source: source} | -                                                            |
| bindanimationfinish    | EventHandle |                   | 否   | 动画结束时会触发 animationfinish 事件，event.detail 同上     | 1.11 低版本请做[兼容性处理](https://smartprogram.baidu.com/docs/develop/swan/compatibility/) |

### change 事件 source 返回值

change事件中的source字段，表示触发change事件的原因，可能值如下：

| 值       | 说明                     |
| :------- | :----------------------- |
| autoplay | 自动播放导致的swiper切换 |
| touch    | 用户划动导致的swiper切换 |
| ""       | 其他原因将返回空字符串   |

### 示例

基础示例：[ 在开发者工具中打开](swanide://fragment/ecdf40636738995f3e5cd356b92a367c1585119691223)

参考示例 1：用于实现顶部标签切换 [ 在开发者工具中打开](swanide://fragment/82da7e569b409a1fa4fb753a010fd35e1575120753274)

参考示例 2: 自定义底部切换圆点 [ 在开发者工具中打开](swanide://fragment/74666ea390cd54afd971879d8028578d1575819223978)

### 注意

- 如果在 bindchange 的事件回调函数中使用 setData 改变 current 值，则会导致 setData 被重复调用，因而通常情况下请在改变 current 值前检测 source 字段来判断是否是由于用户触摸引起的。
- 其中只可放置 swiper-item 组件，否则会导致未定义的行为。
- 自定义指示点的样式：[参见swiper参数](https://smartprogram.baidu.com/docs/develop/component/view_swiper/)，可以去 dot 显示之后，自己定义 dot 样式。

## swiper-item 滑块视图容器子项

滑块视图容器子项，仅可放置在``组件中，宽高自动设置为100%。

| 属性名  | 类型   | 默认值 | 必填 | 说明                  | 最低版本                                                     |
| :------ | :----- | :----- | :--- | :-------------------- | :----------------------------------------------------------- |
| item-id | String |        | 否   | 该swiper-item的标识符 | 1.11 低版本请做[兼容性处理](https://smartprogram.baidu.com/docs/develop/swan/compatibility/) |

## view 视图容器

视图容器，相当于 html 中的 div ，可将页面分割为独立的、不同的部分。如果需要使用滚动视图，请使用 [scroll-view](https://smartprogram.baidu.com/docs/develop/component/view_scroll-view/)。

| 属性名                 | 类型    | 默认值 | 必填 | 说明                                                         |
| :--------------------- | :------ | :----- | :--- | :----------------------------------------------------------- |
| hover-class            | String  | none   | 否   | 指定按下去的样式类。当 hover-class="none" 时，没有点击态效果 |
| hover-stop-propagation | Boolean | false  | 否   | 指定是否阻止本节点的祖先节点出现点击态                       |
| hover-start-time       | Number  | 50     | 否   | 按住后出现点击态的时间长度，单位毫秒                         |
| hover-stay-time        | Number  | 400    | 否   | 手指松开后点击态保留的时间长度，单位毫秒                     |

**示例**

[在开发者工具中打开](swanide://fragment/6470ffc8ba55fa0b04bd37c82b2dd4e91585119690935)

### 注意

- 如果需要使用滚动视图，请使用 scroll-view。
- 从基础库版本1.12.0开始支持事件捕获、冒泡。

# 基础内容

https://smartprogram.baidu.com/docs/develop/component/base_icon/

# 表单组件

https://smartprogram.baidu.com/docs/develop/component/formlist_button/

# 基础API

### [swan.canIUse](https://smartprogram.baidu.com/docs/develop/api/device_sys/swan-canIUse/)

判断智能小程序的API，回调，参数，组件等是否在当前版本和当前系统下可用。

**参数**

String schema：

- `${API}.${method}.${param}.${option}`
- `${class}.${method}.${param}.${option}`
- `${component}.${attribute}.${option}`

| 参数         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| ${API}       | API 名字                                                     |
| ${class}     | 类名                                                         |
| ${method}    | 调用方式，有效值为return, object, 回调函数的名称（多数为success和callback） |
| ${param}     | 参数或者返回值                                               |
| ${option}    | 参数的有效值或者返回值的属性或者组件属性的有效值             |
| ${component} | 组件名字                                                     |
| ${attribute} | 组件属性                                                     |

**返回值**

Boolean 当前版本是否可用

**异常**

入参类型错误时，会抛出一个标准的`Error`对象。

**示例**

[ 在开发者工具中打开](swanide://fragment/2d9c4a9cd34aac2e3f951cecc6b1fe0e1569499882524)

```js
Page({
    canIUse() {
        // 组件
        console.log('view.hover-class', swan.canIUse('view.hover-class'));
        console.log('scroll-view.scroll-x', swan.canIUse('scroll-view.scroll-x'));
        console.log('cover-view', swan.canIUse('cover-view'));
        console.log('button.size.default', swan.canIUse('button.size.default'));

        // API: ${method} 为 object
        console.log('request.object.method.OPTIONS', swan.canIUse('request.object.method.OPTIONS'));

        // API: ${method} 为 success
        console.log('ai.imageAudit.success.data.stars.name', swan.canIUse('ai.imageAudit.success.data.stars.name'));

        // API: ${method} 为 callback
        console.log('onAppShow.callback.entryType.user', swan.canIUse('onAppShow.callback.entryType.user'));

        // API: ${method} 为 return
        console.log('getEnvInfoSync.return.env.trial', swan.canIUse('getEnvInfoSync.return.env.trial'));

        // API: 类
        console.log('VideoContext.requestFullScreen.object.direction', swan.canIUse('VideoContext.requestFullScreen.object.direction'));
        console.log('CanvasContext.fill', swan.canIUse('CanvasContext.fill'));
    }
});
```

**注意**

- 回调函数的名称以文档为准；
- 不支持 fail 和 complete 回调函数的判断；
- 支持success回调参数的判断，举例如下：

```js
swan.canIUse('request.success.data');
```

- 纯 number 类型的属性不做支持；
- 带有`.`或空格的属性不做支持；
- 如果参数是 Array.<Object> 或 Array.<string> 类型，校验方式举例如下：

```js
// swan.ai.textReview   Array.<Object>
swan.canIUse('ai.textReview.success.result.reject.label');

// swan.chooseImage  Array.<string>
swan.canIUse('chooseVideo.object.sourceType.album');
```

### swan.nextTick

> 基础库 3.15.104 开始支持，低版本需做兼容处理。

延迟一部分操作到下一个时间片再执行。（类似于 setTimeout）

**参数**

Function callback

`callback`参数说明

自定义组件中的 setData 和 triggerEvent 等接口为同步操作，当这几个接口被连续调用时，都是在一个同步流程中执行完的，因此若逻辑不当可能会导致出错。

示例 [ 在开发者工具中打开](swanide://fragment/4b59fe8b260a04431f3e14e3c24fce421576567107528)

```js
this.setData({number: 1}) // 直接在当前同步流程中执行
console.log(this.data.number);
swan.nextTick(() => {
  this.setData({number: 3}) // 在当前同步流程结束后，下一个时间片执行         
  console.log(this.data.number);
})
this.setData({number: 2}) // 直接在当前同步流程中执行
console.log(this.data.number);
```



## 应用级事件

见Swan-应用级-应用级事件

## URLQuery

### swan.getURLQuery

> 基础库 3.100.6 开始支持，低版本需做[兼容处理](https://smartprogram.baidu.com/docs/develop/swan/compatibility/)。

 获取当前页面的 URL query 对象值。在当前页面的 URL Query 更新后，只能通过此 API 主动获取当前页面最新的 URL query。

**示例**

[在开发者工具中打开](swanide://fragment/90124cfb526387c6f9c1c497f0fc7d411585660102748)

### swan.setURLQuery

> 基础库 3.100.6 开始支持，低版本需做[兼容处理](https://smartprogram.baidu.com/docs/develop/swan/compatibility/)。

 设置当前页面的 URL query。

**参数**

newURLquery：必须是值为字符串的对象，否则调用 setURLQuery 会抛错。新设置的 URL query 会与当前的 URL query 融合。

**示例**

[ 在开发者工具中打开](swanide://fragment/050dcdc146ec6150092a5ec67ec57c771585660836847)

```js
Page({
    data: {
        tabs: [
            {name: 'movie', label: '电影'},
            {name: 'food', label: '美食'},
            {name: 'sports', label: '体育'}
        ]
    },

    onLoad(query) {
        swan.setURLQuery({channel: 'movie'});
    },

    onURLQueryChange({newURLQuery, oldURLQuery}) {
        console.log(newURLQuery, oldURLQuery);
    }
});
```

**注意**

+ 调用 swan.setURLQuery 则会触发 onURLQueryChange 页面函数。

### onURLQueryChange

> 百度小程序的页面与其 URL query 有对应关系，页面内容改变后，如有必要请调用 swan.setURLQuery，以便更好地被搜索引擎收录。

在 Page 中定义 onURLQueryChange 处理函数，监听页面 URL query 改变。引起页面 URL query 更新的原因有：调用 [swan.setURLQuery](https://smartprogram.baidu.com/docs/develop/api/url_query/swan-setURLQuery/) 。

**Object参数**

| 属性名      | 类型   | 默认值 | 必填 | 说明               |
| :---------- | :----- | :----- | :--- | :----------------- |
| newURLQuery | Object |        |      | 改变后的 URL query |
| oldURLQuery | Object |        |      | 改变前的 URL query |

**示例**

[在开发者工具中打开](swanide://fragment/901a4d2917c2746f48200c5161eb130a1585659426046)

```js
Page({
    data: {
        tabs: [
            { name: 'movie', label: '电影' },
            { name: 'food', label: '美食' },
            { name: 'sports', label: '体育' }
        ],
        content: '电影'
    },
    onURLQueryChange({newURLQuery, oldURLQuery}) {
        console.log(newURLQuery, oldURLQuery);
        this.setData({
            content: `${oldURLQuery.channel || '电影'} To ${newURLQuery.channel}`
        })
    }
})
```

## 更新

### swan.getUpdateManager

> 基础库 1.13.4 开始支持，低版本需做兼容处理。

获取全局唯一的版本更新管理器，用于管理小程序更新。

**Web 态说明**：Web 态小程序暂不支持手动管理小程序更新。

**返回值：**[UpdateManager](https://smartprogram.baidu.com/docs/develop/api/open/UpdateManager/)

**示例**

[在开发者工具中打开](swanide://fragment/a215f5f8430d830160fc485621797da81575376239973)

```js
Page({
    onLoad(){
        const updateManager = swan.getUpdateManager();
        this.updateManager =  updateManager;
    },
    applyUpdate() {
        this.updateManager.onCheckForUpdate(function (res) {
            // 请求完新版本信息的回调
            console.log("res", res.hasUpdate);
            if(!res.hasUpdate){
                swan.showModal({
                    title: '更新提示',
                    content: '无可用更新版本',
                });
            }
            else {
                this.updateManager.onUpdateReady(function (res) {  
                    swan.showModal({
                        title: '更新提示',
                        content: '新版本已经准备好，是否重启应用？',
                        success(res) {
                            if (res.confirm) {
                                // 新的版本已经下载好，调用 applyUpdate 应用新版本并重启
                                updateManager.applyUpdate();
                            }
                        }
                    });
                });
                this.updateManager.onUpdateFailed(function (err) {
                    // 新的版本下载失败
                    swan.showModal({
                        title: '更新提示',
                        content: '新版本下载失败，请稍后再试'
                    });
                });
            }
        }); 
    }
});
```

疑问：是什么时候向后台请求信息的呢？只能是swan.getUpdateManager()，但是事件明明是点击button之后才绑定的

### UpdateManager

#### onCheckForUpdate

检查更新事件，当向百度后台请求完新版本信息，会进行回调。

**Web 态说明：**由于 Web 态小程序始终用最新版本小程序包生成，因此 Web 态该值始终为 false

**回调函数参数**

| 属性      | 类型    | 说明           |
| --------- | ------- | -------------- |
| hasUpdate | Boolean | 是否有新的版本 |

**示例**

[在开发者工具中打开](swanide://fragment/1d5e55c4a591129c35adf0cb7bd4c2f21574070810759)

#### onUpdateReady

更新就绪事件，当新版本下载完成，会进行回调

**Web 态说明：**由于 Web 态小程序暂不支持手动管理小程序更新，因此 UpdateManager.onUpdateReady 不会执行。

#### applyUpdate

当新版本下载完成，调用该方法会强制当前小程序应用上新版本并重启。

**Web 态说明**：由于 Web 态小程序暂不支持手动管理小程序更新，因此 UpdateManager.applyUpdate 不会执行。

#### onUpdateFailed

当新版本下载失败，会进行回调

**Web 态说明**：由于 Web 态小程序暂不支持手动管理小程序更新，因此 UpdateManager.onUpdateFailed 不会执行。

### 注意

- 检查更新操作由宿主APP在小程序冷启动时自动触发，不需由开发者主动触发，开发者只需监听检查结果即可。
- onUpdateReady(callback) 回调结果说明：当宿主APP检查到小程序有新版本，会主动触发下载操作（无需开发者触发），当下载完成后，会通过 onUpdateReady 告知开发者。
- onUpdateFailed(callback) 回调结果说明：当宿主APP检查到小程序有新版本，会主动触发下载操作（无需开发者触发），如果下载失败（可能是网络原因等），会通过 onUpdateFailed 告知开发者。
- 当小程序新版本下载完成时（即收到 onUpdateReady 回调），可以通过此接口强制重启小程序并应用最新版本。
- 当新版本未下载完成时，调用此接口将返回`undefined`；当新版本下载完成时，若接口调用失败，会抛出一个标准的`Error`对象。

## 定时器

### interval和timeout

## 调试

见Swan-入门-调试



# 路由

## 基础

### 页面栈

框架以栈的形式维护了当前的所有页面。 当发生路由切换的时候，页面栈的表现如下：

| 路由方式   | 页面栈表现                          |
| ---------- | ----------------------------------- |
| 初始化     | 新页面入栈                          |
| 打开新页面 | 新页面入栈                          |
| 页面重定向 | 当前页面出栈，新页面入栈            |
| 页面返回   | 页面出栈                            |
| Tab 切换   | 页面全部出栈，只留下初始的 Tab 页面 |
| 重加载     | 页面全部出栈，只留下新的页面        |

**注意**：开发者可以使用 getCurrentPages() 函数获取当前页面栈。

### getCurrentPages()

getCurrentPages() 函数用于获取当前页面栈的实例，以数组形式按栈的顺序给出，第一个元素为首页，最后一个元素为当前页面。

[在开发者工具中预览效果](swanide://fragment/be265192b32b09af4deb17093bfb73cb1576048350631)

### 路由方式

对于路由的触发方式以及页面生命周期函数如下：

| 路由方式   | 触发时机                                                     | 路由前页面 | 路由后页面         |
| ---------- | ------------------------------------------------------------ | ---------- | ------------------ |
| 初始化     | 智能小程序打开的第一个页面                                   |            | onLoad, onShow     |
| 打开新页面 | 调用 API [swan.navigateTo](https://smartprogram.baidu.com/docs/develop/api/show/tab_swan-navigateTo/) 或使用[组件](https://smartprogram.baidu.com/docs/develop/component/nav/) < navigator open-type="navigateTo"/ > | onHide     | onLoad, onShow     |
| 页面重定向 | 调用 API [swan.redirectTo](https://smartprogram.baidu.com/docs/develop/api/show/tab_swan-redirectTo/) 或使用[组件](https://smartprogram.baidu.com/docs/develop/component/nav/) < navigator open-type="redirectTo"/ > | onUnload   | onLoad, onShow     |
| Tab 切换   | 调用 API [swan.switchTab](https://smartprogram.baidu.com/docs/develop/api/show/tab_swan-switchTab/) 或使用[组件](https://smartprogram.baidu.com/docs/develop/component/nav/) < navigator open-type="switchTab"/ > 或用户切换 Tab |            | 各种情况请参考下表 |
| 页面返回   | 调用 API [swan.navigateBack](https://smartprogram.baidu.com/docs/develop/api/show/tab_swan-navigateBack/) 或使用[组件](https://smartprogram.baidu.com/docs/develop/component/nav/) < navigator open-type="navigateBack"/ > 或用户按左上角返回按钮 | onUnload   | onShow             |
| 重启动     | 调用 API [swan.reLaunch](https://smartprogram.baidu.com/docs/develop/api/show/tab_swan-reLaunch/) 或使用[组件](https://smartprogram.baidu.com/docs/develop/component/nav/) < navigator open-type="reLaunch"/ > | onUnload   | onLoad, onShow     |

Tab 切换对应的生命周期（以 A、B 页面为 Tabbar 页面，C 是从 A 页面打开的页面，D 页面是从 C 页面打开的页面为例）：

| 当前页面        | 路由后页面    | 触发的生命周期（按顺序）                           |
| --------------- | ------------- | -------------------------------------------------- |
| A               | A             | Nothing happend                                    |
| A               | B             | A.onHide(), B.onLoad(), B.onShow()                 |
| A               | B（再次打开） | A.onHide(), B.onShow()                             |
| C               | A             | C.onUnload(), A.onShow()                           |
| C               | B             | C.onUnload(), B.onLoad(), B.onShow()               |
| D               | B             | D.onUnload(), C.onUnload(), B.onLoad(), B.onShow() |
| D（从转发进入） | A             | D.onUnload(), A.onLoad(), A.onShow()               |
| D（从转发进入） | B             | D.onUnload(), B.onLoad(), B.onShow()               |

[在开发者工具中预览效果](swanide://fragment/587cfc623ceb2b4608e69b31ddb73f801577942344726)

> 建议在开发者工具中的控制台查看，工具与真机略有差异，以真机的生命周期为准。

**注意**:

- navigateTo, redirectTo 只能打开非 tabBar 页面。
- switchTab 只能打开 tabBar 页面。
- reLaunch 可以打开任意页面。
- 页面底部的 tabBar 由页面决定，即只要是定义为 tabBar 的页面，底部都有 tabBar。
- 调用页面路由带的参数可以在目标页面的 onLoad 中获取。

**关于页面传参:**
从其它页面跳转到由自定义组件构造的页面时，如跳转到页面 /components/custom/custom?paramA=123&paramB=xyz ，你可以在由custom组件构造的页面onLoad生命周期中获取传递的query字段。

## 路由API

以下 5 个 API 都有与之功能一致的 [navigator 组件](https://smartprogram.baidu.com/docs/develop/component/nav/) 声明方式。

如果两种方式都能满足您的使用场景，推荐您使用 [navigator 组件](https://smartprogram.baidu.com/docs/develop/component/nav/) 实现相应的导航功能，以便更好的被搜索引擎理解。

### swan.switchTab

跳转到 tabBar 页面，并关闭其他所有非 tabBar 页面。

**object参数说明**

| 属性名   | 类型     | 必填 | 说明                                                         |
| :------- | :------- | :--- | :----------------------------------------------------------- |
| url      | String   | 是   | 需要跳转的 tabBar 页面的路径（需在 app.json 的 tabBar 字段定义的页面），路径后不能带参数。 |
| success  | Function | 否   | 接口调用成功的回调函数                                       |
| fail     | Function | 否   | 接口调用失败的回调函数                                       |
| complete | Function | 否   | 接口调用结束的回调函数（调用成功、失败都会执行）             |

### swan.reLaunch

关闭所有页面，打开到应用内的某个页面。

**object参数说明**

| 属性名   | 类型     | 必填 | 说明                                             | Web 态说明 |
| :------- | :------- | :--- | :----------------------------------------------- | :--------- |
| url      | String   | 是   | 需要跳转的应用内页面路径 , 路径后可以带参数。    |            |
| success  | Function | 否   | 接口调用成功的回调函数                           | 不支持     |
| fail     | Function | 否   | 接口调用失败的回调函数                           | 不支持     |
| complete | Function | 否   | 接口调用结束的回调函数（调用成功、失败都会执行） | 不支持     |

参数与路径之间使用 ? 分隔，参数键与参数值用=相连，不同参数用 & 分隔；如 ‘path?key=value&key2=value2’

**注意：**如果跳转的页面路径是 tabBar 页面则不能带参数。

> 实际测试，可以通过getURLQuery获取参数

### swan.redirectTo

关闭当前页面，跳转到应用内的某个页面。

**object参数说明**

| 属性名   | 类型     | 必填 | 说明                                                         |
| :------- | :------- | :--- | :----------------------------------------------------------- |
| url      | String   | 是   | 需要跳转的应用内**非 tabBar 的页面**的路径，路径后可以带参数。 |
| success  | Function | 否   | 接口调用成功的回调函数                                       |
| fail     | Function | 否   | 接口调用失败的回调函数                                       |
| complete | Function | 否   | 接口调用结束的回调函数（调用成功、失败都会执行）             |

**注意：**该方法不能跳转到 tabBar 页面

### swan.navigateTo

保留当前页面，跳转到应用内的某个页面，但是不能跳转到 tabbar 页面，使用 swan.navigateBack 可以返回到原页面。

**object参数说明**

| 属性名   | 类型     | 必填 | 默认值 | 说明                                                         |
| :------- | :------- | :--- | :----- | :----------------------------------------------------------- |
| url      | String   | 是   |        | 需要跳转的应用内非 tabBar 的页面的路径 , 路径后可以带参数。参数与路径之间使用?分隔，参数键与参数值用=相连，不同参数用&分隔；如 ‘path?key=value&key2=value2’。 |
| success  | Function | 否   |        | 接口调用成功的回调函数                                       |
| fail     | Function | 否   |        | 接口调用失败的回调函数                                       |
| complete | Function | 否   |        | 接口调用结束的回调函数（调用成功、失败都会执行）             |

**示例**

[在开发者工具中打开](swanide://fragment/0d35934b50b1749bc787786f3cd140241574138803752)

**注意**

jssdk 在 web-view 中使用 swan.navigateTo 接口跳转 success、fail、complete 回调函数不显示。

### swan.navigateBack

关闭当前页面，返回上一页面或多级页面。

**object参数说明**

| 属性名   | 类型     | 必填 | 默认值 | 说明                                                     |
| :------- | :------- | :--- | :----- | :------------------------------------------------------- |
| delta    | Number   | 否   | 1      | 返回的页面数，如果 delta 大于现有页面数，则返回到首页1。 |
| success  | function | 否   |        | 接口调用成功的回调函数                                   |
| fail     | function | 否   | -      | 接口调用失败的回调函数                                   |
| complete | function | 否   |        | 接口调用结束的回调函数（调用成功、失败都会执行）         |

**示例**

从小程序原生页面返回到 H5 页面，并需要刷新： [ 在开发者工具中打开](swanide://fragment/285b2bcaa6e473ea04d92ae23f2f73ff1575878402143)

## navigator 页面导航

页面链接，控制小程序的跳转，既可在当前小程序内部进行跳转，也可跳转至其他小程序。`navigator` 的子节点背景色应为透明色。

### 属性说明

| 属性名                 | 类型    | 默认值          | 必填 | 说明                                                         | 最低版本                                                     |
| :--------------------- | :------ | :-------------- | :--- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| target                 | String  | self            | 否   | 在哪个目标上发生跳转，默认当前小程序，有效值self/miniProgram | 2.5.2 低版本请做[兼容性处理](https://smartprogram.baidu.com/docs/develop/swan/compatibility/) |
| url                    | String  |                 | 否   | 应用内的跳转链接                                             | -                                                            |
| open-type              | String  | navigate        | 否   | 跳转方式                                                     | -                                                            |
| delta                  | Number  |                 | 否   | 当 open-type 为 'navigateBack' 时有效，表示回退的层数        | -                                                            |
| app-id                 | String  |                 | 否   | 当target="miniProgram"时有效，要打开的小程序 App Key (小程序后台设置-开发设置中) | 2.5.2 低版本请做[兼容性处理](https://smartprogram.baidu.com/docs/develop/swan/compatibility/) |
| path                   | String  |                 | 否   | 当target="miniProgram"时有效，打开的页面路径，如果为空则打开首页。 | 2.5.2 低版本请做[兼容性处理](https://smartprogram.baidu.com/docs/develop/swan/compatibility/) |
| extra-data             | Object  |                 | 否   | 当target="miniProgram"时有效，需要传递给目标小程序的数据，目标小程序可在 App.onLaunch()，App.onShow() 中获取到这份数据。[详情](https://smartprogram.baidu.com/docs/develop/framework/app_service_register/) | 2.5.2 低版本请做[兼容性处理](https://smartprogram.baidu.com/docs/develop/swan/compatibility/) |
| version                | String  | release         | 否   | 当target="miniProgram"时有效，要打开的小程序版本，有效值 develop（开发版），trial（体验版），release（正式版），仅在当前小程序为开发版或体验版时此参数有效；如果当前小程序是正式版，则打开的小程序必定是正式版。 | 2.5.2 低版本请做[兼容性处理](https://smartprogram.baidu.com/docs/develop/swan/compatibility/) |
| hover-class            | String  | navigator-hover | 否   | 指定点击时的样式类，当`hover-class="none"`时，没有点击态效果。 |                                                              |
| hover-stop-propagation | Boolean | false           | 否   | 指定是否阻止本节点的祖先节点出现点击态。                     | -                                                            |
| hover-start-time       | Number  | 50              | 否   | 按住后多久出现点击态，单位毫秒。                             | -                                                            |
| hover-stay-time        | Number  | 600             | 否   | 手指松开后点击态保留时间，单位毫秒。                         | -                                                            |
| bindsuccess            | String  |                 | 否   | 当target="miniProgram"时有效，跳转小程序成功。               | 2.5.2 低版本请做[兼容性处理](https://smartprogram.baidu.com/docs/develop/swan/compatibility/) |
| bindfail               | String  |                 | 否   | 当target="miniProgram"时有效，跳转小程序失败。               | 2.5.2 低版本请做[兼容性处理](https://smartprogram.baidu.com/docs/develop/swan/compatibility/) |
| bindcomplete           | String  |                 | 否   | 当target="miniProgram"时有效，跳转小程序完成。               | 2.5.2 低版本请做[兼容性处理](https://smartprogram.baidu.com/docs/develop/swan/compatibility/) |

**target 有效值**

| 值          | 说明               |
| :---------- | :----------------- |
| self        | 当前小程序         |
| miniProgram | 跳转到另一个小程序 |

**version 有效值**

| 值      | 说明   |
| :------ | :----- |
| develop | 开发版 |
| trial   | 体验版 |
| release | 正式版 |

**open-type 有效值**

| 值           | 说明                                                         | 最低版本 |
| :----------- | :----------------------------------------------------------- | :------- |
| navigate     | 对应 [`swan.navigateTo`](https://smartprogram.baidu.com/docs/develop/api/show/tab_swan-navigateTo/) 的功能 |          |
| redirect     | 对应 [`swan.redirectTo`](https://smartprogram.baidu.com/docs/develop/api/show/tab_swan-redirectTo/) 的功能 |          |
| switchTab    | 对应 [`swan.switchTab`](https://smartprogram.baidu.com/docs/develop/api/show/tab_swan-switchTab/) 的功能 |          |
| navigateBack | 对应 [`swan.navigateBack`](https://smartprogram.baidu.com/docs/develop/api/show/tab_swan-navigateBack/) 的功能 |          |
| reLaunch     | 对应 [`swan.reLaunch`](https://smartprogram.baidu.com/docs/develop/api/show/tab_swan-reLaunch/) 的功能 |          |
| exit         | 退出小程序，target="miniProgram"时生效                       | 2.5.2    |

`navigator-hover` 默认为:

```css
{
    background-color: rgba(0, 0, 0, 0.1);
    opacity: 0.7;
}
```

### 示例

基础展示：[ 在开发者工具中打开](swanide://fragment/82f2bab467de0215c6236d44d838b3491585123905100)

接受H5页面传来的参数：[ 在开发者工具中打开](swanide://fragment/a14269966d2c94a0321940a9f9cca8401585124150197)

# 网络

## 使用注意事项

### 服务器域名配置

每个智能小程序需要事先设置一个通讯域名，小程序可以跟指定的域名与进行网络通信。包括普通 HTTPS 请求（request）、上传文件（uploadFile）、下载文件（downloadFile) 和 WebSocket 通信（connectSocket）。

### 配置流程

服务器域名请在 “[智能小程序后台](https://smartprogram.baidu.com/mappconsole/main/set?tabCur=1)->设置->开发设置->服务器域名” 中进行配置，配置时需要注意：

- 域名只支持 https (request、uploadFile、downloadFile) 和 wss (socket) 协议；
- 域名不能使用 IP 地址、localhost或端口号；
- 域名必须经过 ICP 备案；
- 出于安全考虑，openapi.baidu.com 不能被配置为服务器域名，相关API也不能在小程序内调用。开发者应将 App Secret 保存到自有后台服务器中，通过服务器使用App Secret获取 access_token，并调用相关 API；
- 对于每个接口，分别可以配置最多 20 个域名。

### 网络请求

超时时间

- 默认超时时间和最大超时时间都是 60s；
- 超时时间可以在 app.json 中配置。

使用限制

- [request](https://smartprogram.baidu.com/docs/develop/api/net/request/) 最大并发数在 iOS 端为 6；Android 端同一域名下最大并发数为 5，且在 Android 端上同一个小程序最大并发数上限为 64。
- [websocket](https://smartprogram.baidu.com/docs/develop/api/net/websocket/) 最大并发数5。
- 网络请求的 header 中 referer 不可设置。
  - 其格式固定为{域名}/{appKey}/{version}/page-frame.html 。
  - 自基础库版本V3.170.0起，其中域名由原来的 https://smartapp.baidu.com更改为 https://smartapps.cn 。
  - 其中 {appkey} 为小程序的 appkey。
  - {version} 为小程序的版本号，版本号为 0 表示为开发版、体验版以及审核版本，版本号为 devtools 表示为开发者工具，其余为正式版本，正式版本号发布前在开发者工具中设置。

## 请求

### swan.request

发起网络请求，请参考[使用注意事项](https://smartprogram.baidu.com/docs/develop/api/net/net_rule/)进行开发。

`object`参数说明 ：

| 属性名       | 类型          | 必填 | 默认值       | 说明                                                         | 最低支持版本 | Web 态说明                           |
| :----------- | :------------ | :--- | :----------- | :----------------------------------------------------------- | :----------- | :----------------------------------- |
| url          | String        | 是   |              | 开发者服务器接口地址，不需要写端口                           |              |                                      |
| data         | Object/String | 否   |              | 请求的参数                                                   |              |                                      |
| header       | Object        | 否   |              | 设置请求的 header，header 中不能设置 Referer。               |              |                                      |
| method       | String        | 否   | GET （大写） | 有效值：OPTIONS、GET、HEAD、POST、PUT、DELETE、 TRACE/CONNECT(仅 Android 支持)。 |              | 有效值：HEAD、GET、POST、PUT、DELETE |
| dataType     | String        | 否   | json         | 有效值：string、json。 如果设为 json，会尝试对返回的数据做一次 JSON.parse 。 |              |                                      |
| responseType | String        | 否   | text         | 设置响应的数据类型, 有效值：text、arraybuffer。              | 1.11.20      |                                      |
| success      | Function      | 否   |              | 收到开发者服务成功返回的回调函数。                           |              |                                      |
| fail         | Function      | 否   |              | 接口调用失败的回调函数。                                     |              |                                      |
| complete     | Function      | 否   |              | 接口调用结束的回调函数（调用成功、失败都会执行）。           |              |                                      |

success 返回参数说明 ：

| 参数       | 类型          | 说明                                    |
| :--------- | :------------ | :-------------------------------------- |
| data       | Object/String | 开发者服务器返回的数据                  |
| statusCode | Number        | 开发者服务器返回的 HTTP 状态码          |
| header     | Object        | 开发者服务器返回的 HTTP Response Header |

fail 返回参数说明 ：

Android

| 错误码 | 说明                             |
| :----- | :------------------------------- |
| 201    | 解析失败，请检查调起协议是否合法 |
| 202    | 解析失败，请检查参数是否正确     |
| 302    | 找不到调起协议对应端能力方法     |
| 1001   | 执行失败                         |

iOS

| 错误码       | 说明                             |
| :----------- | :------------------------------- |
| 202          | 解析失败，请检查调起协议是否合法 |
| errorCode为4 | URL无效                          |

**data 数据说明** 

最终发送给服务器的数据都是 String 类型，如果传入的 data 不是 String 类型，会被转换成 String 。转换规则如下：

1. 对于 GET 方法的数据，会将数据转换成 query string（encodeURIComponent(k)=encodeURIComponent(v)&encodeURIComponent(k)=encodeURIComponent(v)...）；
2. 对于 POST 方法且 header['content-type'] 为 application/json 的数据，会对数据进行 JSON 序列化；
3. 对于 POST 方法且 header['content-type'] 为 application/x-www-form-urlencoded 的数据，会将数据转换成 query string （encodeURIComponent(k)=encodeURIComponent(v)&encodeURIComponent(k)=encodeURIComponent(v)...）

**示例**

https://smartprogram.baidu.com/docs/develop/api/net/request/

### RequestTask

网络请求任务对象

```js
this.requestTask = swan.request({})
```

### RequestTask.abort

中断请求任务

```js
this.requestTask = swan.request({})
this.requestTask.abort();
```

## 上传与下载

https://smartprogram.baidu.com/docs/develop/api/net/uploadfile/

## WebSocket

https://smartprogram.baidu.com/docs/develop/api/net/websocket/

# 界面组件API

## 交互反馈

### swan.showToast

显示消息提示框，用以提供成功、警告和错误等反馈信息。设计文档详见 [消息提示框](https://smartprogram.baidu.com/docs/design/component/toast/)。

**object参数说明**

| 属性名   | 类型     | 必填 | 默认值  | 说明                                             |
| :------- | :------- | :--- | :------ | :----------------------------------------------- |
| title    | String   | 是   |         | 提示的内容                                       |
| icon     | String   | 否   | success | 图标，有效值`"success"、"loading"、"none"`。     |
| image    | String   | 否   |         | 自定义图标的本地路径，image 的优先级高于 icon    |
| duration | Number   | 否   | 2000    | 提示的延迟时间，单位毫秒。                       |
| success  | Function | 否   |         | 接口调用成功的回调函数                           |
| fail     | Function | 否   |         | 接口调用失败的回调函数                           |
| complete | Function | 否   |         | 接口调用结束的回调函数（调用成功、失败都会执行） |
| mask     | Boolean  | 否   | false   | 是否显示透明蒙层，防止触摸穿透。                 |

**icon有效值**

| 有效值  | 说明                                                       |
| :------ | :--------------------------------------------------------- |
| success | 显示成功图标，此时 title 文本最多显示 7 个汉字长度。默认值 |
| loading | 显示加载图标，此时 title 文本最多显示 7 个汉字长度。       |
| none    | 不显示图标，此时 title 文本最多可显示两行。                |

**示例** [ 在开发者工具中打开](swanide://fragment/6ab6a7ea0d57b42271c6d6817f0707c01574132977216)

**注意：**

- [swan.showLoading](https://smartprogram.baidu.com/docs/develop/api/show/toast_swan-showLoading/) 和 swan.showToast 同时只能显示一个。
- swan.showToast 应与 [swan.hideToast](https://smartprogram.baidu.com/docs/develop/api/show/toast_swan-hideToast/) 配对使用。

### swan.hideToast

隐藏消息提示框，无需识别特定的Toast，统一隐藏

**object参数说明**

| 属性名   | 类型     | 必填 | 默认值 | 说明                                             |
| :------- | :------- | :--- | :----- | :----------------------------------------------- |
| success  | function | 否   |        | 接口调用成功的回调函数                           |
| fail     | function | 否   |        | 接口调用失败的回调函数                           |
| complete | function | 否   |        | 接口调用结束的回调函数（调用成功、失败都会执行） |

### swan.showLoading

显示 loading 提示框, 需**主动**调用 hideLoading 才能关闭提示框。

**object参数说明** 

| 属性名   | 类型     | 必填 | 默认值 | 说明                                             |
| :------- | :------- | :--- | :----- | :----------------------------------------------- |
| title    | String   | 是   |        | 提示的内容                                       |
| mask     | Boolean  | 否   | false  | 是否显示透明蒙层，防止触摸穿透。                 |
| success  | Function | 否   |        | 接口调用成功的回调函数                           |
| fail     | Function | 否   |        | 接口调用失败的回调函数                           |
| complete | Function | 否   |        | 接口调用结束的回调函数（调用成功、失败都会执行） |

示例 [ 在开发者工具中打开](swanide://fragment/6960611628839f267d8df02ca3521a241574135233401)

**注意：**

+ showToast通过icon属性也可以实现showLoading效果。区别在于：
  + showToast可以延时关闭
  + showLoading只能主动关闭

### swan.hideLoading

隐藏 loading 提示框

**object参数说明**

| 属性名   | 类型     | 必填 | 默认值 | 说明                                             |
| :------- | :------- | :--- | :----- | :----------------------------------------------- |
| success  | function | 否   |        | 接口调用成功的回调函数                           |
| fail     | function | 否   |        | 接口调用失败的回调函数                           |
| complete | function | 否   |        | 接口调用结束的回调函数（调用成功、失败都会执行） |



### swan.showModal

显示模态弹窗。用于同步用户重要信息，并要求用户进行确认，或执行特定操作以决策下一步骤。设计文档详见 [模态对话框](https://smartprogram.baidu.com/docs/design/component/showModal/)。弹窗标题需 [措辞](https://smartprogram.baidu.com/docs/design/foundation/writing/) 精简，建议控制在8个中文字符内。如果提示的内容简单，可以去掉弹窗标题。

**object参数说明**

| 属性名       | 类型     | 必填 | 默认值  | 说明                                             |
| :----------- | :------- | :--- | :------ | :----------------------------------------------- |
| title        | String   | 是   |         | 提示的标题                                       |
| content      | String   | 是   |         | 提示的内容                                       |
| showCancel   | Boolean  | 否   | true    | 是否显示取消按钮 。                              |
| cancelText   | String   | 否   | 取消    | 取消按钮的文字，最多 4 个字符。                  |
| cancelColor  | HexColor | 否   | #000000 | 取消按钮的文字颜色。                             |
| confirmText  | String   | 否   | 确定    | 确定按钮的文字，最多 4 个字符。                  |
| confirmColor | HexColor | 否   | #3c76ff | 确定按钮的文字颜色。                             |
| success      | Function | 否   |         | 接口调用成功的回调函数                           |
| fail         | Function | 否   |         | 接口调用失败的回调函数                           |
| complete     | Function | 否   |         | 接口调用结束的回调函数（调用成功、失败都会执行） |

**success返回参数说明**

| 参数名  | 类型    | 说明                                  |
| ------- | ------- | ------------------------------------- |
| confirm | Boolean | 为 true 时，表示用户点击了确定按钮 。 |
| cancel  | Boolean | 为 true 时，表示用户点击了取消。      |

示例 [ 在开发者工具中打开](swanide://fragment/35d07dce512008b2cd12cc231e86b0f41569463801299)

### showModal和showToast是否可共存

[在开发者工具中打开](swanide://fragment/2a833f9c7f164efca05ade83ff9869de1576559710455)

```js
swan.showToast({
    title, 
    icon,
    mask: false, // 此属性设置为true不能打断toast
    success: res => {
      console.log('showToast success', res);
    },
    fail: err => {
      console.log('showToast fail', err);
    }
})
```

### swan.showActionSheet

显示操作菜单，设计文档详见 [底部操作菜单](https://smartprogram.baidu.com/docs/design/component/actionsheet/)。

**object参数说明**

| 属性名    | 类型           | 必填 | 默认值  | 说明                                             |
| :-------- | :------------- | :--- | :------ | :----------------------------------------------- |
| itemList  | Array.<string> | 是   |         | 按钮的文字数组，数组长度最大为6个。              |
| itemColor | HexColor       | 否   | #3c76ff | 按钮的文字颜色。                                 |
| success   | Function       | 否   |         | 接口调用成功的回调函数，详见返回参数说明。       |
| fail      | Function       | 否   |         | 接口调用失败的回调函数                           |
| complete  | Function       | 否   |         | 接口调用结束的回调函数（调用成功、失败都会执行） |

**success返回参数说明**

| 参数名   | 类型   | 说明                                      |
| -------- | ------ | ----------------------------------------- |
| tapIndex | Number | 用户点击的按钮，从上到下的顺序，从0开始。 |

示例 [ 在开发者工具中打开](swanide://fragment/c96f91497b4edc0c93df37756919a2001574135826789)

## 导航栏

### swan.showNavigationBarLoading

该方法在当前页面显示导航条加载动画。

示例 [ 在开发者工具中打开](swanide://fragment/072bca954324649b05962f16c9d69de61574136587133)

### swan.hideNavigationBarLoading

隐藏导航条加载动画。

示例 [ 在开发者工具中打开](swanide://fragment/072bca954324649b05962f16c9d69de61574136587133)

### swan.setNavigationBarTitle

动态设置当前页面的标题，设计文档详见 [顶部导航栏](https://smartprogram.baidu.com/docs/design/component/topnav/)。

**object参数说明**

| 属性名   | 类型     | 必填 | 默认值 | 说明                                             |
| :------- | :------- | :--- | :----- | :----------------------------------------------- |
| title    | String   | 是   |        | 页面标题                                         |
| success  | Function | 否   |        | 接口调用成功的回调函数                           |
| fail     | Function | 否   |        | 接口调用失败的回调函数                           |
| complete | Function | 否   |        | 接口调用结束的回调函数（调用成功、失败都会执行） |

示例 [ 在开发者工具中打开](swanide://fragment/01552c32fa4399c3ae2b3465ecd5977c1574136270589)

### swan.setNavigationBarColor

动态设置当前页面导航条的颜色。

**object参数说明**

| 属性名          | 类型     | 必填 | 默认值 | 说明                                                         |
| :-------------- | :------- | :--- | :----- | :----------------------------------------------------------- |
| frontColor      | String   | 是   |        | 前景颜色值，包括按钮、标题、状态栏的颜色，有效值 #ffffff 和 #000000。 |
| backgroundColor | String   | 是   |        | 背景颜色值，有效值为十六进制颜色。                           |
| animation       | Object   | 否   |        | 动画效果                                                     |
| success         | Function | 否   |        | 接口调用成功的回调函数                                       |
| fail            | Function | 否   |        | 接口调用失败的回调函数                                       |
| complete        | Function | 否   |        | 接口调用结束的回调函数（调用成功、失败都会执行）             |

**animation**

| 属性名     | 类型   | 必填 | 默认值 | 说明                       |
| :--------- | :----- | :--- | :----- | :------------------------- |
| duration   | Number | 否   | 0      | 动画变化时间，单位：毫秒。 |
| timingFunc | String | 否   | linear | 动画变化方式               |

**animation.timingFunc 有效值**

| 值        | 说明                         |
| --------- | ---------------------------- |
| linear    | 动画从头到尾的速度是相同的。 |
| easeIn    | 动画以低速开始               |
| easeOut   | 动画以低速结束。             |
| easeInOut | 动画以低速开始和结束。       |

示例 [在开发者工具中打开](swanide://fragment/dace5658a19b604ff4d62d0c760fb7351574136817988)

```js
swan.setNavigationBarColor({
    frontColor: '#ffffff',
    backgroundColor: '#3C76FF',
    animation: {
      duration: 500,
      timingFunc: 'linear'
    },
    success: res => {
      console.log('setNavigationBarColor success');
    },
    fail: err => {
      console.log('setNavigationBarColor fail', err);
    }
});
```

## tabBar

### swan.showTabBar

显示 tabBar

**object参数说明**

| 属性名    | 类型     | 必填 | 默认值 | 说明                                             |
| :-------- | :------- | :--- | :----- | :----------------------------------------------- |
| animation | Boolean  | 否   | 无     | 是否需要动画效果。                               |
| success   | Function | 否   |        | 接口调用成功的回调函数                           |
| fail      | Function | 否   |        | 接口调用失败的回调函数                           |
| complete  | Function | 否   |        | 接口调用结束的回调函数（调用成功、失败都会执行） |

**示例**

[ 在开发者工具中打开](swanide://fragment/c45f4a5af785986752460c8d6e3a56801575210342255)

**注意：**

+ animation为true时，建议在真机上看效果，工具暂不支持

### swan.hideTabBar

隐藏 tabBar

object参数说明

| 属性名    | 类型     | 必填 | 默认值 | 说明                                             |
| :-------- | :------- | :--- | :----- | :----------------------------------------------- |
| animation | Boolean  | 否   | false  | 是否需要动画效果。                               |
| success   | Function | 否   |        | 接口调用成功的回调函数                           |
| fail      | Function | 否   |        | 接口调用失败的回调函数                           |
| complete  | Function | 否   |        | 接口调用结束的回调函数（调用成功、失败都会执行） |

示例 [ 在开发者工具中打开](swanide://fragment/5aa2f4593413e51fd723d5effc62d8221574138247912)

### swan.setTabBarItem

动态设置 tabBar 某一项的内容

**object参数说明**

| 属性名           | 类型     | 必填 | 默认值 | 说明                                                         |
| :--------------- | :------- | :--- | :----- | :----------------------------------------------------------- |
| index            | Number   | 是   |        | tabBar的哪一项，从左边算起。                                 |
| text             | String   | 否   |        | tab 上按钮文字                                               |
| iconPath         | String   | 否   |        | 图片绝对路径，icon 大小限制为 40KB，建议尺寸为 81px * 81px，当 position 为 top 时，此参数无效，不支持网络图片。 |
| selectedIconPath | String   | 否   |        | 选中时的图片的绝对路径，icon 大小限制为 40KB，建议尺寸为 81px * 81px ，当`position`为 top 时，此参数无效。 |
| success          | Function | 否   |        | 接口调用成功的回调函数                                       |
| fail             | Function | 否   |        | 接口调用失败的回调函数                                       |
| complete         | Function | 否   |        | 接口调用结束的回调函数（调用成功、失败都会执行）             |

示例 [ 在开发者工具中打开](swanide://fragment/8b445d78cbdabc8066c1dbec707dbefd1574138075367)

### swan.setTabBarStyle

动态设置 tabBar 的整体样式，底部标签栏位于小程序底部，方便用户在不同功能模块之间进行快速切换。为保证可用性，底部导航栏承载2-5个功能，如果超出5个功能项，请酌情移入页面或菜单内。设计文档详见 [底部标签栏](https://smartprogram.baidu.com/docs/design/component/bottomtab/)。

**object参数说明**

| 属性名          | 类型     | 必填 | 默认值 | 说明                                             |
| :-------------- | :------- | :--- | :----- | :----------------------------------------------- |
| color           | HexColor | 否   |        | tab 上的文字默认颜色                             |
| selectedColor   | HexColor | 否   |        | tab 上的文字选中时的颜色                         |
| backgroundColor | HexColor | 否   |        | tab 的背景色                                     |
| borderStyle     | String   | 否   |        | tabbar上边框的颜色， 有效值 black/white          |
| success         | Function | 否   |        | 接口调用成功的回调函数                           |
| fail            | Function | 否   |        | 接口调用失败的回调函数                           |
| complete        | Function | 否   |        | 接口调用结束的回调函数（调用成功、失败都会执行） |

**示例**

[在开发者工具中打开](swanide://fragment/44d27e9d57b8848544201181fe547cb01574137906215)

### swan.setTabBarBadge

为 tabBar 某一项的右上角添加文本，可以使用底部标签栏徽标（TabBarBadge）进行提示，告知用户对应的标签页中有内容更新。

**object参数说明**

| 属性名   | 类型     | 必填 | 默认值 | 说明                                             |
| :------- | :------- | :--- | :----- | :----------------------------------------------- |
| index    | Number   | 是   |        | tabBar的哪一项，从左边算起                       |
| text     | String   | 是   |        | 显示的文本，超过 3 个字符则显示成“…”             |
| success  | Function | 否   |        | 接口调用成功的回调函数                           |
| fail     | Function | 否   |        | 接口调用失败的回调函数                           |
| complete | Function | 否   |        | 接口调用结束的回调函数（调用成功、失败都会执行） |

示例 [ 在开发者工具中打开](swanide://fragment/6fa8cb5655d510b33220f6203e4e02c51574137194069)

### swan.removeTabBarBadge

移除tabBar某一项右上角的文本。

object参数说明

| 属性名   | 类型     | 必填 | 默认值 | 说明                                             |
| :------- | :------- | :--- | :----- | :----------------------------------------------- |
| index    | Number   | 是   |        | tabBar的哪一项，从左边算起                       |
| success  | Function | 否   |        | 接口调用成功的回调函数                           |
| fail     | Function | 否   |        | 接口调用失败的回调函数                           |
| complete | Function | 否   |        | 接口调用结束的回调函数（调用成功、失败都会执行） |

示例 [ 在开发者工具中打开](swanide://fragment/6fa8cb5655d510b33220f6203e4e02c51574137194069)

### swan.showTabBarRedDot

显示 tabBar 某一项的右上角的红点，可以使用底部标签栏红点（TabBarRedDot）进行提示，告知用户对应的标签页中有内容更新。

**object参数说明**

| 属性名   | 类型     | 必填 | 默认值 | 说明                                             |
| :------- | :------- | :--- | :----- | :----------------------------------------------- |
| index    | Number   | 是   |        | tabBar的哪一项，从左边算起                       |
| success  | Function | 否   |        | 接口调用成功的回调函数                           |
| fail     | Function | 否   |        | 接口调用失败的回调函数                           |
| complete | Function | 否   |        | 接口调用结束的回调函数（调用成功、失败都会执行） |

示例 [ 在开发者工具中打开](swanide://fragment/c0cd5b823043904c1690f5e2b51fe2591574137456000)

### swan.hideTabBarRedDot

**object参数说明**

| 属性名   | 类型     | 必填 | 默认值 | 说明                                             |
| :------- | :------- | :--- | :----- | :----------------------------------------------- |
| index    | Number   | 是   |        | tabBar的哪一项，从左边算起                       |
| success  | Function | 否   |        | 接口调用成功的回调函数                           |
| fail     | Function | 否   |        | 接口调用失败的回调函数                           |
| complete | Function | 否   |        | 接口调用结束的回调函数（调用成功、失败都会执行） |

示例 [ 在开发者工具中打开](swanide://fragment/c0cd5b823043904c1690f5e2b51fe2591574137456000)

## 胶囊菜单

### swan.getMenuButtonBoundingClientRect

> 基础库 3.20.3 开始支持，低版本需做兼容处理。

获取菜单按钮（右上角胶囊按钮）的布局位置信息。坐标信息以屏幕左上角为原点。

**返回值**

| 参数   | 类型   | 说明                 |
| ------ | ------ | -------------------- |
| width  | number | 宽度，单位：px       |
| height | number | 高度，单位：px       |
| top    | number | 上边界坐标，单位：px |
| right  | number | 右边界坐标，单位：px |
| bottom | number | 下边界坐标，单位：px |
| left   | number | 左边界坐标，单位：px |

示例 - 属性全集 [ 在开发者工具中打开](swanide://fragment/b7950613332a792d444e4e4842d063291569477029937)

## 关注引导组件

### swan.showFavoriteGuide

> 引导组件有统一的策略，若用户未执行过关注操作，则3天内不再出现引导组件；若用户执行过关注操作，则引导组件对该用户将不再出现。最低支持版本 3.20.4 。工具和真机中的实现有区别，详见[API 实现差异](https://smartprogram.baidu.com/docs/develop/devtools/diff/)。

支持在小程序内调起关注小程序引导组件，引导用户关注小程序。引导组件设计文档详见：[关注小程序引导](https://smartprogram.baidu.com/docs/design/component/guide_add/)。

**object参数说明** 

| 属性名   | 类型     | 必填 | 默认值                                              | 说明                                             |
| :------- | :------- | :--- | :-------------------------------------------------- | :----------------------------------------------- |
| type     | String   | 否   |                                                     | 引导组件类型，有效值： bar,tip；                 |
| content  | String   | 否   | bar: 关注小程序； tip: 关注小程序，下次使用更便捷。 | 引导组件文字，不支持开发者自定义。               |
| success  | Function | 否   |                                                     | 接口调用成功的回调                               |
| fail     | Function | 否   |                                                     | 接口调用失败的回调函数                           |
| complete | Function | 否   |                                                     | 接口调用结束的回调函数（调用成功、失败都会执行） |

示例 [ 在开发者工具中打开](swanide://fragment/b4a2aabc87380bf9507eed71b58fdcd91586514996486)

**浮层引导(type=bar)**

一直展现：用户点击关闭，浮层引导消失；点击关注按钮可直接关注小程序。

bar类型在百度APP 11.22 及以上版本中运行的所有基础库版本，将不再支持。

**气泡引导(type=tip)**

引导组件 5s 后自动消失，组件箭头指向小程序菜单。

**注意**：

+ 浮层引导样式（type=bar）在百度APP 11.22 及以上版本中运行的所有基础库版本，将不再支持。
+ 浮层引导(type=bar)时，用户点击左侧关闭也会触发此API的success回调。
+ 百度App11.22及以上版本，当type为空时，回调为fail，百度App11.22以下版本，type为空时，回调为success，前端展现浮层样式。

# 界面操作API

## 滚动 Scroll

### swan.pageScrollTo

将页面滚动到目标位置（可以设置滚动动画时长）。

**object参数说明**

| 属性名    | 类型     | 必填 | 默认值 | 说明                                             |
| :-------- | :------- | :--- | :----- | :----------------------------------------------- |
| scrollTop | Number   | 是   |        | 滚动到页面的目标位置（单位 px）                  |
| duration  | Number   | 否   | 300    | 滚动动画的时长，（单位 ms）                      |
| success   | Function | 否   |        | 接口调用成功的回调函数                           |
| fail      | Function | 否   |        | 接口调用失败的回调函数                           |
| complete  | Function | 否   |        | 接口调用结束的回调函数（调用成功、失败都会执行） |

**示例**

页面滚动到顶部 [ 在开发者工具中打开](swanide://fragment/25ef2f9fbdaaa9271329c02d7dafe8cc1575223153548)

页面滚动到底部 [ 在开发者工具中打开](swanide://fragment/0e4af77bf4d678bb744766e5faca641b1575223056610)

```js
swan.createSelectorQuery()
    .select(".image")
    .boundingClientRect(function(rect) {
    swan.pageScrollTo({
        scrollTop: rect.bottom,
        duration: 300,
        success: res => {
          	console.log('pageScrollTo success', res);
        },
        fail: err => {
          	console.log('pageScrollTo fail', err);
        }
    });
})
```

## 下拉刷新

### onPullDownRefresh

页面的事件处理函数，监听用户下拉动作。

详情参见[页面相关事件处理函数](https://smartprogram.baidu.com/docs/develop/framework/app_service_page/#页面相关事件处理函数/)

### swan.startPullDownRefresh

开始下拉刷新，调用后触发下拉刷新动画，效果与用户手动下拉刷新一致。

`object`参数说明

| 属性名   | 类型     | 必填 | 默认值 | 说明                                             |
| :------- | :------- | :--- | :----- | :----------------------------------------------- |
| success  | Function | 否   |        | 接口调用成功的回调函数                           |
| fail     | Function | 否   |        | 接口调用失败的回调函数                           |
| complete | Function | 否   |        | 接口调用结束的回调函数（调用成功、失败都会执行） |

### swan.stopPullDownRefresh

停止当前页面下拉刷新。

示例 [ 在开发者工具中打开](swanide://fragment/37955e937e5e221c983f1129861c38ae1569476821334)

## 背景

### swan.setBackgroundColor

> 基础库 3.10.4 版本开始支持。

设置窗口的背景颜色。

`object`参数说明 ：

| 属性名                | 类型     | 必填 | 默认值 | 说明                                                         |
| :-------------------- | :------- | :--- | :----- | :----------------------------------------------------------- |
| backgroundColor       | String   | 否   |        | 需设置的背景窗口颜色，支持十六进制颜色值。11.3 低版本请做[兼容性处理](https://smartprogram.baidu.com/docs/develop/swan/compatibility/) |
| backgroundColorTop    | String   | 否   | -      | 需设置的顶部背景窗口颜色，支持十六进制颜色值，仅ios有效。 11.3 低版本请做[兼容性处理](https://smartprogram.baidu.com/docs/develop/swan/compatibility/) |
| backgroundColorBottom | String   | 否   | -      | 需设置的底部背景窗口颜色，支持十六进制颜色值，仅ios有效。11.3 低版本请做[兼容性处理](https://smartprogram.baidu.com/docs/develop/swan/compatibility/) |
| success               | Function | 否   |        | 接口调用成功的回调函数                                       |
| fail                  | Function | 否   |        | 接口调用失败的回调函数                                       |
| complete              | Function | 否   |        | 接口调用结束的回调函数（调用成功、失败都会执行）             |

示例 [ 在开发者工具中打开](swanide://fragment/66f332965704ae69bbdcaefe3db158fa1575139212532)

### swan.setBackgroundTextStyle

> 基础库 3.10.4 版本开始支持。

设置窗口**下拉背景loading样式**。

`object`参数说明 ：

| 属性名    | 类型     | 必填 | 默认值 | 说明                                                         |
| :-------- | :------- | :--- | :----- | :----------------------------------------------------------- |
| textStyle | String   | 是   |        | loading图样式，有效值light和dark 。11.3 低版本请做[兼容性处理](https://smartprogram.baidu.com/docs/develop/swan/compatibility/) |
| success   | Function | 否   |        | 接口调用成功的回调函数                                       |
| fail      | Function | 否   |        | 接口调用失败的回调函数                                       |
| complete  | Function | 否   |        | 接口调用结束的回调函数（调用成功、失败都会执行）             |

示例 [ 在开发者工具中打开](swanide://fragment/1a44f9e8f4e4cf559c3750e2e0ebc1061574253034280)

## 动画

动画的API需要模板指令的配合

### swan.createAnimation

创建一个动画实例 animation

**返回值**

Animation

**object 参数说明**

| 属性名          | 类型   | 必填 | 默认值        | 说明                     |
| :-------------- | :----- | :--- | :------------ | :----------------------- |
| duration        | Number | 否   | 400           | 动画持续时间，单位 ms 。 |
| timingFunction  | String | 否   | `‘linear’`    | 定义动画的效果           |
| delay           | Number | 否   | 0             | 动画延迟时间，单位 ms 。 |
| transformOrigin | String | 否   | `‘50% 50% 0’` | 动画                     |

**timingFunction 有效值**

| 值          | 说明                                         |
| ----------- | -------------------------------------------- |
| linear      | 以相同速度开始至结束。                       |
| ease        | 慢速开始，然后变快，然后慢速结束。           |
| ease-in     | 慢速开始的过渡效果。                         |
| ease-in-out | 动画以低速开始和结束。                       |
| ease-out    | 动画以低速结束。                             |
| step-start  | 动画第一帧就跳至结束状态直到结束。           |
| step-end    | 动画一直保持开始状态，最后一帧跳到结束状态。 |

### Animation实例

动画实例可以调用以下方法来描述动画，调用结束后会返回自身，支持链式调用的写法。

**普通样式**

| 属性            | 参数   | 说明                                                         |
| --------------- | ------ | ------------------------------------------------------------ |
| opacity         | value  | 透明度，参数范围 0~1                                         |
| backgroundColor | color  | 颜色值                                                       |
| width           | length | 长度值，如果传入 Number 则默认使用 px，可传入其他自定义单位的长度值。 |
| height          | length | 长度值，如果传入 Number 则默认使用 px，可传入其他自定义单位的长度值。 |
| top             | length | 长度值，如果传入 Number 则默认使用 px，可传入其他自定义单位的长度值。 |
| left            | length | 长度值，如果传入 Number 则默认使用 px，可传入其他自定义单位的长度值。 |
| bottom          | length | 长度值，如果传入 Number 则默认使用 px，可传入其他自定义单位的长度值。 |
| right           | length | 长度值，如果传入 Number 则默认使用 px，可传入其他自定义单位的长度值。 |

**动画方法**

与CSS3逐个对应

[Animation.matrix](https://smartprogram.baidu.com/docs/develop/api/show/createanimation_Animation-matrix/)

[Animation.translate3d](https://smartprogram.baidu.com/docs/develop/api/show/createanimation_Animation-translate3d/)

### **动画队列**

调用动画操作方法后要调用 step() 来表示一组动画完成，可以在一组动画中调用任意多个动画方法，一组动画中的所有动画会同时开始，一组动画完成后才会进行下一组动画。

**示例** [在开发者工具中打开](swanide://fragment/c1cd19f4bd6c53b0c272aa1d2bce10481557729887965)

```js
Page({
    data: {
        animationData: {}
    },
    startToAnimate() {
        const animation = swan.createAnimation();
        animation.rotate(90).translateY(10).step();
        animation.rotate(-90).translateY(-10).step(); // 这一步 实际上 旋转了180deg
        this.setData({
            animationData: animation.export()
        });
    }
});
```

每一组动画的参考目标都是原始样式，但是动画的开始样式是上一组动画结束时的样式

因此上例最终的样式是：-90deg旋转，-10Y轴位移

## 观察节点

### IntersectionObserver

ntersectionObserver 对象，用于推断某些节点是否可以被用户看见、有多大比例可以被用户看见。

**IntersectionObserver 对象的方法列表 ：**

| 方法               | 说明                                     |
| ------------------ | ---------------------------------------- |
| relativeTo         | 使用选择器指定一个节点，作为参照区域之一 |
| relativeToViewport | 指定页面显示区域作为参照区域之一         |
| observe            | 指定目标节点并开始监听相交状态变化情况   |
| disconnect         | 停止监听。回调函数将不再触发             |

### relativeTo

使用选择器指定一个节点，作为参照区域之一。

**参数**

+ String selector，选择器

+ Object margins，用来扩展（或收缩）参照节点布局区域的边界

**margins 参数说明**

| 属性名 | 类型   | 必填 | 默认值 | 说明                 |
| :----- | :----- | :--- | :----- | :------------------- |
| left   | number | 否   |        | 节点布局区域的左边界 |
| right  | number | 否   |        | 节点布局区域的右边界 |
| top    | number | 否   |        | 节点布局区域的上边界 |
| bottom | number | 否   |        | 节点布局区域的下边界 |

### relativeToViewport

指定页面显示区域作为参照区域之一

**参数：** Object margins，用来扩展（或收缩）参照节点布局区域的边界

### observe

指定目标节点并开始监听相交状态变化情况

**参数**

+ String targetSelector，选择器
+ Function callback，监听相交状态变化的回调函数

**callback object参数说明**

| 属性               | 类型   | 说明               |
| ------------------ | ------ | ------------------ |
| intersectionRatio  | number | 相交比例           |
| intersectionRect   | Object | 相交区域的边界     |
| boundingClientRect | Object | 目标边界           |
| relativeRect       | Object | 参照区域的边界     |
| time               | number | 相交检测时的时间戳 |

```js
intersectionObserver.relativeTo('.scroll-view').observe('.ball', res => {
  console.log('observe', res)
  // boundingClientRect: 目标边界，这里指小球的位置情况，这个位置是相对于整个页面的，不是相对于参照元素的scroll-view的
  // dataset: 观察对象携带的数据。
  // id: 观察对象的id，它与上面的dataset多使用于一次观察多个对象的场景，用于区分不同的对象。
  // intersectionRatio: 相交比例，为0时说明两者不相交。
  // intersectionRect: 相交区域，height 为 0 时说明此时没有相交。
  // relativeRect: 参照区域的边界，作为参考物，它的值一般是不会变的。可以对比它于开始相交时一直没变，因为它就是一个scroll-view的组件在页面上呈现出的位置信息。
  // time: 监测到两者相交时的时间戳
  this.setData('appear', res.intersectionRatio > 0);
});
```

**intersectionRect、boundingClientRect、relativeRect 结构说明**

| 属性   | 类型   | 说明   |
| ------ | ------ | ------ |
| left   | number | 左边界 |
| right  | number | 右边界 |
| top    | number | 上边界 |
| bottom | number | 下边界 |

### disconnect

> 与页面显示区域的相交区域并不准确代表用户可见的区域，因为参与计算的区域是“布局区域”，布局区域可能会在绘制时被其他节点裁剪隐藏（如遇祖先节点中 overflow 样式为 hidden 的节点）或遮盖（如遇 fixed 定位的节点）。

停止监听，回调函数将不再触发。

### swan.createIntersectionObserver

 创建并返回一个 IntersectionObserver 对象实例。在自定义组件中，可以使用 this.createIntersectionObserver([options]) 来代替。

**object参数说明**

| 属性名       | 类型    | 必填 | 默认值 | 说明                                                         |
| :----------- | :------ | :--- | :----- | :----------------------------------------------------------- |
| thresholds   | Array   | 否   | [0]    | 一个数值数组，包含所有阈值。                                 |
| initialRatio | number  | 否   | 0      | 初始的相交比例，如果调用时检测到的相交比例与这个值不相等且达到阈值，则会触发一次监听器的回调函数。 |
| selectAll    | boolean | 否   | false  | 是否同时观测多个目标节点（而非一个），如果设为 true ，observe 的 targetSelector 将选中多个节点（注意：同时选中过多节点将影响渲染性能） |

thresholds initialRatio 理解不能 @@@

### 示例

基础示例：[ 在开发者工具中打开](swanide://fragment/9e13f19179f3ff25f0b2ffbbe17e978e1574307679898)

options为thresholds时 ： [ 在开发者工具中打开](swanide://fragment/b82b0e3dd9bff9173965078c876d6bd01574304101063)

```js
const intersectionObserver = swan.createIntersectionObserver({
  // 阈值设置少，避免触发过于频繁导致性能问题
  thresholds: [1]
});
// 监测scroll-view可视区域 和 小球元素节点的相交情况
//  我配置了 thresholds：[1]，那么当监听对象和参照物相交比例达到 1 时，会触发监听器的回调函数
intersectionObserver.relativeTo('.scroll-view').observe('.ball', res => {
  console.log('observe', res)
  // intersectionRatio: 相交比例，为0时说明两者不相交。
  this.setData('appear', res.intersectionRatio > 0);
});
this.intersectionObserver = intersectionObserver;
},
```

options为selectAll时 ： [ 在开发者工具中打开](swanide://fragment/cfae8c63d16efcf6a7b4d6b6b2f5d4f71574931774175)

```js
const intersectionObserver = swan.createIntersectionObserver({
  selectAll: true
});
// 监听两个小球
intersectionObserver.relativeTo('.scroll-view').observe('.ball .ball2', res => {
  console.log('observe', res);
  this.setData('appear', res.intersectionRatio > 0);
});
this.intersectionObserver = intersectionObserver;
},
```

## 选择器

### SelectorQuery

`selectorQuery`对象的方法列表 ：

| 方法           | 参数                | 说明                                                         |
| -------------- | ------------------- | ------------------------------------------------------------ |
| in             | Component component | 参考[selectorQuery.in](https://smartprogram.baidu.com/docs/develop/api/show/query_SelectorQuery-in/)详细介绍 |
| select         | selector            | 参考[selectorQuery.select](https://smartprogram.baidu.com/docs/develop/api/show/query_SelectorQuery-select/)详细介绍 |
| selectAll      | selector            | 参考[selectorQuery.selectAll](https://smartprogram.baidu.com/docs/develop/api/show/query_SelectorQuery-selectAll/)详细介绍 |
| selectViewport |                     | 参考[selectorQuery.selectViewport](https://smartprogram.baidu.com/docs/develop/api/show/query_SelectorQuery-selectViewport/)详细介绍 |
| exec           | callback            | 参考[selectorQuery.exec](https://smartprogram.baidu.com/docs/develop/api/show/query_SelectorQuery-exec/)详细介绍 |

### in

将选择器的选取范围更改为自定义组件 component 内（初始时，选择器仅选取页面范围的节点，不会选取任何自定义组件中的节点）。

```js
var SelectorQuery = swan.createSelectorQuery().in(this)
// 会在组件内部寻找'.name'
SelectorQuery.select('.name').boundingClientRect(function(res){
  swan.showModal({
    title: '节点信息为',
    content: JSON.stringify(res)
  });
}).exec()
```

### select

在当前页面下选择第一个匹配选择器 selector 的节点，返回一个 NodesRef 对象实例，可以用于获取节点信息。

**参数：**String selector。selector 类似于 CSS 的选择器，但仅支持下列语法。

1. ID 选择器：#the-id
2. class 选择器（可以连续指定多个）：.a-class.another-class
3. 子元素选择器：.the-parent > .the-child
4. 后代选择器：.the-ancestor .the-descendant
5. 多选择器的并集：#a-node, .some-other-nodes

**返回值：**NodesRef

### selectAll

在当前页面下选择匹配选择器 selector 的节点，返回一个 NodesRef 对象实例。 与 `select(selector)` 不同的是，它选择所有匹配选择器的节点。

### selectViewport

选择显示区域，可用于获取显示区域的尺寸、滚动位置等信息，返回一个NodesRef对象实例。

### exec

执行所有的请求，请求结果按请求次序构成数组，在 callback 的第一个参数中返回。

```js
// 获取方式一
swan.createSelectorQuery().select('.target').boundingClientRect(function(rect){
  console.log(rect);
  swan.showModal({
    title: 'NodesRef.boundingClientRect的返回值',
    content: JSON.stringify(rect)
  });
}).exec()

// 获取方式二
query.select('.target').boundingClientRect();

query.exec(res => {
  console.log(res)
  const rect = res[0]
  }
});
```

## 节点信息

### NodesRef

节点信息对象

### boundingClientRect

添加节点的布局位置的查询请求，相对于显示区域，以像素为单位。其功能类似于 DOM 的 getBoundingClientRect。返回值是 nodesRef 对应的 selectorQuery。

**参数：**callback

**返回值**

返回的节点信息中，每个节点的位置用 left、right、top、bottom、width、height 字段描述。如果提供了 callback 回调函数，在执行 selectQuery 的 exec 方法后，节点信息会在 callback 中返回。

| 参数   | 类型   | 说明           |
| ------ | ------ | -------------- |
| left   | Nunber | 节点左边界坐标 |
| right  | Nunber | 节点右边界坐标 |
| top    | Nunber | 节点上边界坐标 |
| bottom | Nunber | 节点下边界坐标 |
| width  | Nunber | 节点宽度       |
| height | Nunber | 节点高度       |

示例 [ 在开发者工具中打开](swanide://fragment/edf1aa51425ccf5e213e181ca267b5211574770246796)

```js
// 获取方式一
swan.createSelectorQuery().select('.target').boundingClientRect(function(rect){
  console.log(rect);
  swan.showModal({
    title: 'NodesRef.boundingClientRect的返回值',
    content: JSON.stringify(rect)
  });
}).exec()

// 获取方式二
query.select('.target').boundingClientRect();

query.exec(res => {
  console.log(res)
  const rect = res[0]
  }
});
```

### fields

获取节点的相关信息，需要获取的字段在 fields 中指定。返回值是 nodesRef 对应的 selectorQuery 。

**参数**

+ Object fileds 
+ Function callback

**fileds 参数说明**

| 参数名        | 类型           | 必填 | 默认值 | 说明                                                         |
| ------------- | -------------- | ---- | ------ | ------------------------------------------------------------ |
| id            | boolean        | 否   |        | 是否返回节点 id                                              |
| dataset       | boolean        | 否   |        | 是否返回节点 dataset                                         |
| rect          | boolean        | 否   |        | 是否返回节点布局位置（left right top bottom）                |
| size          | boolean        | 否   |        | 是否返回节点尺寸（width height）                             |
| scrollOffset  | boolean        | 否   |        | 是否返回节点的 scrollLeft scrollTop ，节点必须是 scroll-view 或者 viewport |
| properties    | Array.<string> | []   |        | 指定属性名列表，返回节点对应属性名的当前属性值（只能获得组件文档中标注的常规属性值， id class style 和事件绑定的属性值不可获取） |
| computedStyle | Array.<string> | []   |        | 指定样式名列表，返回节点对应样式名的当前值                   |

示例 [ 在开发者工具中打开](swanide://fragment/074e13432730548b49ba90f0fbcb8fac1574492260859)

```js
Page({
  data: {
    data: ''
  },
  queryNodeInfo: function(){
    let that = this;
    swan.createSelectorQuery().select('.target').fields({
      dataset: true,
      size: true,
      scrollOffset: true,
      properties: ['scrollX', 'scrollY'],
      computedStyle: ['margin', 'backgroundColor']
    }, function(res){
      console.log(res)
      that.setData('data', JSON.stringify(res));
      res.dataset    // 节点的dataset
      res.width      // 节点的宽度
      res.height     // 节点的高度
      res.scrollLeft // 节点的水平滚动位置
      res.scrollTop  // 节点的竖直滚动位置
      res.scrollX    // 节点 scroll-x 属性的当前值
      res.scrollY    // 节点 scroll-y 属性的当前值
      // 此处返回指定要返回的样式名
      res.margin
      res.backgroundColor
    }).exec()
  }
});
```

### scrollOffset

添加节点的滚动位置查询请求，以像素为单位。节点必须是 scroll-view 或者 viewport 。返回值是 nodesRef 对应的 selectorQuery 。

**参数：**Function callback

**返回值：**返回的节点信息中，每个节点的滚动位置用 scrollLeft 、scrollTop 字段描述。如果提供了 callback 回调函数，在执行 selectQuery 的 exec 方法后，节点信息会在 callback 中返回。

| 参数       | 类型   | 说明             |
| ---------- | ------ | ---------------- |
| scrollLeft | Nunber | 节点水平滚动位置 |
| scrollTop  | Nunber | 节点竖直滚动位置 |

示例 [ 在开发者工具中打开](swanide://fragment/33db2d08cc65630cc939ec8d268bf0481574974671748)

```js
getNodeRef() {
  const node = swan.createSelectorQuery().select('.scroll-view');
  node.scrollOffset(res => {
    console.log('scrollOffset:::', res);
    swan.showModal({
      title: 'scrollOffset',
      content: JSON.stringify(res)
    });
  }).exec();
}
```

# 与H5页面的互动@@@

路由接受H5页面的参数

从小程序返回H5页面