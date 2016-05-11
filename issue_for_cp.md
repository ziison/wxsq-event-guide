##京东购物H5活动CP重构规范 

京东购物H5活动CP重构规范，旨在统一CP开发人员的编码规范、提高代码质量，方便后期协作，提高开发效率。

>提示：初次阅读者请务必认真阅读本文，谢谢合作。在页面重构启动前，请先认真查看需求文档、页面交互图以及设计规范，充分了解需求后，再与需求方确认会出现页面业务逻辑场景（特别注意各种弹窗、浮层、模块状态等情况），避免页面最终交付时产生缺失，减少开发过程中的不必要的反复修改。

## 一、页面视觉输出标准

目前，活动页面将采用750px的视觉设计稿输出，由之前以iPhone4为基准点变成iPhone6，用于适配iPhone以及一般的Android机型。

## 二、页面性能要求

### 页面兼容的目标（机型/系统）

- 主流移动设备：iPhone 4+ 、三星、魅族、华为、红米、小米1S 以上及主流 Android 千元机型；请特别关注iPhone4/4s、魅族MX4、华为P6等机型
-  操作系统：iOS 7.0+ 与 Android 4 .0+；


### 加载速度、请求数与资源压缩

- 以 3G 网络环境 100kb/s 的网络速度计算，详情页首次加载过程中，可以在 2s 内看到 Loading 页面；
- 活动样式文件1个，脚本文件须经过压缩处理；实色banner大图应以jpg格式为主，图片资源须进行压缩，JPG 图片使用 80% 以下质量，PNG 图片使用[TinyPNG](https://tinypng.com/)进行压缩；
- 应当对小图片进行 CSS 雪碧图 合并，因低版本系统及低端设备渲染问题，单张图片尺寸不超过 2000x2000 像素，超过时需切分成多张雪碧图；
- 禁止使用gif动画图片
- canvas游戏页面，png图片宽高都应该控制在1024px内

**资源文件格式标准**

| 文件类似 |	格式	 | 大小 | 其它备注 |
| :----: | :----: | :----: | :----: |
|背景音乐 | mp3 | 小于200 KB | 开头和结尾音轨建议做使用淡入淡出处理|
|图片 |	jpg、png | 尽量保持在 100 KB 左右| 单张图片尺寸不超过 2000x2000 像素 |
|视频文件 | mov/mp4/avi | 暂无要求 | 分辨率>=960×540，视频码率>=1500kbps，<br />画面比例 4:3或16:9|

### HTML,CSS,JS注意点
- HTML结构层级保持足够简单，标签嵌套不宜过深，尽量控制在 5 级内
- 使用CSS3动画代替JS动画，CSS3动画常用属性：opacity，translate，rotate，scale，不使用伪类做动画处理
- 尽可能少的使用CSS高级选择器与通配选择器
- 页面中使用到z-index的值必须小于700（尽量保持在100以内）
- css中 不使用@import
- 避免在低端机上使用大量CSS3渐变阴影效果，可考虑降级效果来提升流畅度
- 避免 iOS 300+ms 点击延时问题 
- 无JS内存泄漏，无逻辑死循环

## 三、开发工作流程

### 开发目录结构

```
 webapp/
    ├── css/
    ├── sass/
    ├── images/
    ├── js/
    ├── index.html
    └── other.html
```

### JS框架*

重构页面有涉及到脚本交互，需要使用JS框架请统一采用 `zepto.js`，引入如下CDN地址。

```
<script src="http://wq.360buyimg.com/fd/promote/base/zepto.min.js"></script>
<script src="//wq.360buyimg.com/fd/promote/base/zepto.min.js"></script>
```


### REM标准

- 页面的尺寸适配统一使用`REM`来完成（各个模块尺寸、字体大小使用rem单位），部分场景可以结合使用`zoom/scale`。
- `375px`宽度下，`<html>`节点的`font-size`为`20px`

使用过程中，可以手动转换模块尺寸到rem单位，也可以结合sass函数（推荐）。

适用REM转换公式：
```
    REM值 ＝ 750设计稿中模块实践尺寸／40
```

sass转换函数：
```
@function pxTorem($px) {
    @if $px == 0 {
        @return 0;
    }
    @if $px <=2 and $px > 0 {
        @return 1px;
    }
    @else {
        @return $px / ($px * 0 + 1) / 40 * 1rem;
    }

}

//使用方式：
.body{
    width:pxTorem(750px);
    //或 带不带px皆可
     width:pxTorem(750);
}

```


了解rem参考文章：[web app变革之rem](http://isux.tencent.com/web-app-rem.html)


### HTML统一页面结构

为了方便快速接入开发页面，统一页面结构：

```
<!DOCTYPE HTML>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" >
    <meta name="format-detection" content="telephone=no" >
    <meta http-equiv="x-dns-prefetch-control" content="on"/>
    <link rel="dns-prefetch" href="//wq.360buyimg.com">
    <title>专题活动名XX</title>
  	<link rel="stylesheet" type="text/css" href="css/style.css">
    <!-- 动态兼容多设备显示脚本 -->
	<script type="text/javascript">
	!function(){var c=768;document.write('<style id="o2HtmlFontSize"></style>');var d=function(){var e,f;if(document&&document.documentElement){e=document.documentElement.clientWidth,f=document.documentElement.clientHeight}if(!e||!f){if(window.localStorage["o2-cw"]&&window.localStorage["o2-ch"]){e=parseInt(window.localStorage["o2-cw"]),f=parseInt(window.localStorage["o2-ch"])}else{a();return}}var h=c&&c<e?c/375:e/375,g=f/603;window.localStorage["o2-cw"]=e,window.localStorage["o2-ch"]=f;window.zoom=window.o2Zoom=h;document.getElementById("o2HtmlFontSize").innerHTML="html{font-size:"+(h*20)+"px;}.o2-zoom,.zoom{zoom:"+(h/2)+";}.o2-scale{-webkit-transform: scale("+h/2+"); transform: scale("+h/2+");}"},b,a=function(){if(b){return}b=setInterval(function(){document&&document.documentElement&&document.documentElement.clientWidth&&document.documentElement.clientHeight&&(d(),clearInterval(b),b=undefined)},100)};d();window.addEventListener("resize",d)}();
	</script>
</head>
<body>
    <!-- S 页面内容 -->
    <div class="wrapper"></div>
    <!-- E 页面内容 -->
</body>
</html>
```

### 统一的reset css

```css
@charset "utf-8";
/* 重置样式 */
* {
	-webkit-tap-highlight-color: rgba(0,0,0,0);
	outline: 0
}

*,blockquote,body,button,dd,dl,dt,fieldset,form,h1,h2,h3,h4,h5,h6,hr,input,legend,li,ol,p,pre,td,textarea,th,ul {
	margin: 0;
	padding: 0;
	vertical-align: baseline
}

img {
	border: 0 none;
	vertical-align: top
}

em,i {
	font-style: normal
}

ol,ul {
	list-style: none
}

button,h1,h2,h3,h4,h5,h6,input,select {
	font-size: 100%;
	font-family: inherit
}

table {
	border-collapse: collapse;
	border-spacing: 0
}

a {
	text-decoration: none
}

a,body {
	color: #666
}

body {
	margin: 0 auto;
	height: 100%;
	font-size: 14px;
	font-family: Helvetica,STHeiti STXihei,Microsoft JhengHei,Microsoft YaHei,Arial;
	line-height: 1.5;
	-webkit-text-size-adjust: 100%!important;
	text-size-adjust: 100%!important
}

input[type=text],textarea {
	-webkit-appearance: none;
	-moz-appearance: none;
	appearance: none
}
```
## 三、常见业务编码注意规范

基本的html/css的命名以及编写规范，请认真阅读，参考：[凹凸实验室前端编码规范](http://aotu.io/guide/docs/name/htmlcss.html)。此处，主要规定开发活动页面过程中，常见的需特别注意的编码规范。

### 统一使用`<div class="wrapper"></div>`做为根节点

强制使用`<div class="wrapper"></div>`做为根节点，每一个页面的只存在一个根节点 `class="wrapper"`，并为这个节点强制添加以下样式：
```html
    <!-- S 页面内容 -->
    <div class="wrapper"> 包含住页面内容</div>
    <!-- E 页面内容 -->
```

```css
.wrapper{
    width: pxTorem(750px);
    height: auto;//如果没有高度限制，使用auto
    overflow: hidden;//必选
    margin:auto 0;
}
```

### 嵌套层级

标签结构尽量简单，嵌套不宜过深，尽量控制在 5 级内。

```
<div class="level1">
    <div class="level2">
        <div class="level3">
            <div class="level4">
                <div class="level5">
                    不能再嵌套了哦！！
                </div>
            </div>
        </div>
    </div>
</div>
```

### TAB标准

吸顶或吸底的TAB须做一个占位的节点，规定`height`值。采用以下标准：

```html
<div class="Tabs">
    <nav>
        <a href="#">TAB</a>
        <a href="#">TAB</a>
        <a href="#">TAB</a>
        <a href="#">TAB</a>
    </nav>
</div>
```
结构：
<pre>
占位节点  
    └── TAB容器  
    ├── TAB1  
    ├── TAB2  
    ├──  ...  
    └── TABn
</pre>

### 关于1px像素的border

不建议直接使用rem,而是直接使用1px，通过`transform:scale(.5)`进行缩放处理。




### 弹窗和引导分享的蒙层添加毛玻璃效果

ios8.x以后的 safari支持毛玻璃效果，建议统一加透明蒙层添加毛玻璃效果。

以下是毛玻璃实现方式，使用以下代码
```
//参考例子
-webkit-backdrop-filter: blur($px);
```


### 活动规则弹窗

活动规则弹窗，如果视觉稿对滚动条没有做定制，统一为活动规则的容器添加弹性滚动的样式：

```css
-webkit-overflow-scrolling:touch;
```

如果有滚动条定制需求，由于上述代码会与滚动条css冲突，所以定制滚动的情况下不添加弹性滚动。如下：

```css
/*定制滚动条*/
::-webkit-scrollbar{width:pxTorem(6px); height:pxTorem(4px);}
::-webkit-scrollbar-button{width:0;height:0;}
::-webkit-scrollbar-corner{display:block; }
::-webkit-scrollbar-thumb{background-clip:padding-box;background-color:#ffffff;}
```

## 四、交付注意点（非常重要）
### 优惠券布局自适应
常见优惠券，通常由2个到三个并列展示，但随着活动时间的推移，会出现优惠券的减少变成一个这类情况，因此，需要做自适应居中处理，构建稿需包含优惠券展示多种形式。

`示例：`

![弹窗](http://jdc.jd.com/halo/cpguide/coupon.png)

### 产品模块热点区域
产品模块通常是整块热点点击区域，并不是按钮区域，请特别注意。
`示例：`
![弹窗](http://jdc.jd.com/halo/cpguide/hot.png)

### 多行文字模块定高问题
模块存在多行文字情况，采用定高处理方式，溢出省略号，防止文字过少样式错乱，请特别注意。
`示例：`
![弹窗](http://jdc.jd.com/halo/cpguide/txt.png)

### 弹窗浮层

页面存在多种交互逻辑的，必须将业务涉及的弹窗浮层提供齐全，并统一写在页面上（建议新开页面专门写弹窗如alert.html）。如以下浮层：提示浮层、页面逻辑浮层、选择用户/地区浮层、活动规则浮层、操作反馈提示等

`示例：`

![弹窗](http://jdc.jd.com/halo/cpguide/tanchuang.png)

> 提示：在页面重构启动前，先认真查看需求文档和页面交互图，充分了解需求后，再与需求方确认会出现那些浮层，避免页面最终交付时缺失相关浮层或状态。

### 活字与图片的鉴别处理（涉及到文字部分尽量做成活字）

须认真审视设计稿，分辨出哪些地方活字处理，哪些地方需要图片代替（不可一味图方便简单图片处理），常见的按钮、标题这些，须尽量做成活字，宽度自适应，方便开发后期维护更改。

`示例：`

![按钮](http://jdc.jd.com/halo/cpguide/button.jpg)

### TAB的选中与非选中状态

常见的TAB多种状态，须在HTML代码注释中，标明选中与非选中状态时切换的class名称，使用方式说明清楚明了。

`示例：`
![按钮](http://jdc.jd.com/halo/cpguide/tab.jpg)

```

<div class="ft_nav_tab ">
    <!-- 开发注意，选中态添加"on" -->
   <a href="" class＝"on">手机数码</a>
   <a href="">家电电器</a>
   <a href="">潮流女包</a>
</div>
```

###  模块的显示和隐藏状态
在HTML代码注释中，标明显示与隐藏状态时切换的class名称。
```
<!-- 模块显示的class为"show" -->
<div class="mod_sharetips ">
   <p>点击右上角，<br>分享到【朋友圈】或发送给微信好友</p>
</div>
```

###  模块的状态切换
凡涉及到商品列表售罄或其他状态，均需要提供齐全。
`示例：`
![商品状态](http://jdc.jd.com/halo/cpguide/pro.jpg)

### 局部滚动区域
针对如活动规则和一些内容比较多的区域，需要在当前页面内做局部滚动的，请勿遗漏。
`示例：`
![局部滚动](http://jdc.jd.com/halo/cpguide/guen.jpg)

### 吸顶元素
含有需要吸顶的头部导航或吸顶菜单，均需要提供齐全各种状态，且吸顶时需有占位处理。
`示例：`
![吸顶部分](http://jdc.jd.com/halo/cpguide/nav.jpg)
```
<!--S sns菜单 -->
<!-- 开发注意吸顶类 fix_sns_menu -->
<div class="sns_menu_wp fix_sns_menu">
    <div class="sns_menu_mod">
        <a href="#">品牌一</a>
        <a href="#" class="on">品牌二</a>
        <a href="#">品牌三</a>
        <a href="#">品牌四</a>             
        
    </div>
</div>
<!--E sns菜单 -->
```

### 固定角标元素
模块含有固定角标元素的，均需要提供有固定角标和没有固定角标的多种样式展示。
`示例：`
![角标](http://jdc.jd.com/halo/cpguide/tag.jpg)

## 五、交付页面流程

- CP应提供在线预览地址，让需求方扫描二维码体验确认
- 需求方反馈问题和修改点，CP修改后待需求方再次确认
- 直至页面全部完成后，将文件包邮件给到需求方和相关开发人员
- 开发过程中若缺失部分页面内容，CP应全力配合补全，直至页面上线







