# 京东购物H5活动CP重构规范
## 一、规范背景

随着京东微信手q业务发展，H5营销活动页面需求量急增，部分业务需要外接给CP处理，但鉴于CP开发能力良莠不齐，为此制定京东购物H5活动CP重构规范，旨在统一CP开发人员的编码规范、提高代码质量，方便后期协作，提高开发效率。

>提示：初次阅读者请务必认真阅读本文，谢谢合作。在页面重构启动前，请先认真查看需求文档、页面交互图以及设计规范，充分了解需求后，再与需求方确认会出现页面业务逻辑场景（特别注意各种弹窗、浮层、模块状态等情况），避免页面最终交付时产生缺失，减少开发过程中的不必要的反复修改。

## 二、页面视觉输出标准

目前，活动页面将采用750px的视觉设计稿输出，以iPhone6 WeChat页面为基准，全面采用`rem`单位构建页面布局。

`rem`布局标准指页面的尺寸适配统一使用REM来完成（各个模块尺寸、字体大小使用rem单位），根据页面有效宽度进行计算，调整 rem 的大小，动态缩放以达到适配的效果，部分场景可以结合使用zoom/scale。

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
为方便快速开发，提供下载 [模块文件](http://jdc.jd.com/halo/cpguide/webapp.zip)

### JS框架*

重构页面有涉及到脚本交互，需要使用JS框架请统一采用 `zepto.js`，引入如下CDN地址。

```
//生产阶段CDN地址，适用http协议
<script src="http://wq.360buyimg.com/fd/promote/base/zepto.min.js"></script>

//交付阶段正式CDN地址，适用http协议、https协议
<script src="//wq.360buyimg.com/fd/promote/base/zepto.min.js"></script>
```


### REM标准换算

- 规定`375px`宽度下，`<html>`节点的`font-size`为`20px`

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

blockquote,body,button,dd,dl,dt,fieldset,form,h1,h2,h3,h4,h5,h6,hr,input,legend,li,ol,p,pre,td,textarea,th,ul {
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
## 四、常见业务编码注意规范

基本的HTML/CSS的命名以及编写规范，请认真阅读、参考：[凹凸实验室前端编码规范](http://aotu.io/guide/docs/name/htmlcss.html)。此处，主要规定开发活动页面过程中，常见的需特别注意的编码规范。

### 统一使用`<div class="wrapper"></div>`做为根节点

强制使用`<div class="wrapper"></div>`做为根节点，每一个页面的只存在一个根节点 `class="wrapper"`，并为这个节点强制添加以下样式：
```html
    <!-- S 页面内容 -->
    <div class="wrapper"> 包含住页面内容</div>
    <!-- E 页面内容 -->
```

```css
.wrapper{
    width: 18.75rem;  // 计算方式  750/40 ＝ 18.75rem
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

### z-index使用范围

页面中普通元素、floating、吸顶吸底、loading、分享蒙层常涉及到`z-index`的使用，为了防止`z-index`层级滥用影响性能，规定使用`z-index`最大值不得超过700。

强烈建议`z-index`值参考如下规则范围：

- 普通元素z-index值须在0～100之间
- floating和吸顶吸底模块z-index值须在101～200之间
- 弹窗z-index值须在201～300之间
- loading和分享蒙层z-index值须在301～400之间

### 关于1px像素的border

不建议直接使用rem,而是直接使用1px，通过`transform:scale(.5)`进行缩放处理。

`示例：`

```
.border:before {
    border: 1px solid #e5e5e5;
    top: 0;
    left: 0;
    right: -100%;
    bottom: -100%;
    z-index: 1;
    -webkit-transform: scale(.5);
    -ms-transform: scale(.5);
    transform: scale(.5);
    -webkit-transform-origin: 0 0;
    -ms-transform-origin: 0 0;
    transform-origin: 0 0;
    pointer-events: none;
}
```

### 活动规则弹窗

活动规则弹窗，如果视觉稿对滚动条没有做定制，统一为活动规则的容器添加弹性滚动的样式：

```css
-webkit-overflow-scrolling:touch;
```

如果有滚动条定制需求，由于上述代码会与滚动条css冲突，所以定制滚动的情况下不添加弹性滚动。如下：

```css
/*定制滚动条*/
::-webkit-scrollbar{width:2px; height:2px;}
::-webkit-scrollbar-button{width:0;height:0;}
::-webkit-scrollbar-corner{display:block; }
::-webkit-scrollbar-thumb{background-clip:padding-box;background-color:#ffffff;}
```

## 五、交付注意点（非常重要）

### 优惠券布局自适应以及券额占位

#### 1.优惠券展示形式自适应

常见优惠券，通常由2个到三个并列展示，但随着活动时间的推移，会出现优惠券的减少变成一个这类情况，因此，需要做自适应居中处理，构建稿需包含优惠券展示多种形式。

#### 2.券额占位

通常优惠券的额度涉及到2位、3位或更多，务必参考标准，做好优惠券额度占位。

`示例：`

![弹窗](http://jdc.jd.com/halo/cpguide/coupon.png)

### 产品模块链接点击区域

产品模块链接通常是整块点击区域，并不是按钮区域，请特别注意。

`示例：`

![弹窗](http://jdc.jd.com/halo/cpguide/hot.png)

### 固定角标元素

模块含有固定角标元素的，均需要提供有固定角标和没有固定角标的多种样式展示。

`示例：`

![角标](http://jdc.jd.com/halo/cpguide/tag.jpg)

### 多行文字模块定高问题

模块存在多行文字情况，采用定高处理方式，溢出省略号，防止文字过少样式错乱，请特别注意。

`示例：`

![弹窗](http://jdc.jd.com/halo/cpguide/txt.png)

###  模块的状态切换

凡涉及到商品列表售罄或其他状态，均需要提供齐全。

`示例：`

![商品状态](http://jdc.jd.com/halo/cpguide/pro.jpg)

### 模块活字与图片的问题（涉及到文字部分尽量做成活字）

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

### 吸顶吸底元素
含有需要吸顶的头部导航、吸顶菜单以及吸底模块，均需要提供齐全各种状态，且吸顶时需有占位处理。

`示例：`

![吸顶部分](http://jdc.jd.com/halo/cpguide/nav.png)
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

<!-- S吸底类  -->
<div class="fix_bot">
    <a href="./mall.html">去卖场逛逛</a>
    <a href="./share.html">分享给好友</a>
</div>
<!-- E吸底类  -->
```

### 弹窗浮层

页面存在多种交互逻辑的，必须将业务涉及的弹窗浮层提供齐全，并统一写在页面上（建议新开页面专门写弹窗如alert.html）。如以下浮层：提示浮层、页面逻辑浮层、选择用户/地区浮层、活动规则浮层、操作反馈提示等

`示例：`

![弹窗](http://jdc.jd.com/halo/cpguide/tanchuang.png)
> 提示：在页面重构启动前，先认真查看需求文档和页面交互图，充分了解需求后，再与需求方确认会出现那些浮层，避免页面最终交付时缺失相关浮层或状态。

### 局部滚动区域
针对如活动规则和一些内容比较多的区域，需要在当前页面内做局部滚动的，请勿遗漏。

`示例：`

![局部滚动](http://jdc.jd.com/halo/cpguide/guen.jpg)

## 六、页面性能要求

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

| 文件类似 |    格式   | 大小 | 其它备注 |
| :----: | :----: | :----: | :----: |
|背景音乐 | mp3 | 小于200 KB | 开头和结尾音轨建议做使用淡入淡出处理|
|图片 |   jpg、png | 尽量保持在 100 KB 左右| 单张图片尺寸不超过 2000x2000 像素 |
|视频文件 | mov/mp4/avi | 暂无要求 | 分辨率>=960×540，视频码率>=1500kbps，<br />画面比例 4:3或16:9|

### HTML,CSS,JS性能要求点

- HTML结构层级保持足够简单，标签嵌套不宜过深，尽量控制在 5 级内
- 使用CSS3动画代替JS动画，CSS3动画常用属性：opacity，translate，rotate，scale，不使用伪类做动画处理
- 尽可能少的使用CSS高级选择器与通配选择器
- 页面中使用到z-index的值必须小于700，保持在100以内为宜
- css中 不使用@import
- 避免使用CSS3渐变阴影效果，可考虑降级效果来提升流畅度

## 七、页面交付基本原则

- 1.开发目录结构须严格遵守
- 2.交互页面稿须严格测试，性能要求达到第六大点提到的`页面性能要求`
- 3.交互页面稿须认真参考第五大点提到的`交付注意点`，解决好常见的业务问题
- 4.交互页面稿须展示清楚设计稿中含有的各种状态、业务场景，使用方式注释清晰明了


## 八、页面交付流程

- CP须提供在线预览地址，让需求方扫描二维码体验确认
- 需求方反馈问题和修改点，CP应及时修改反馈，之后待需求方再次确认
- 直至页面全部完成后，将文件包邮件发送给需求方和相关开发人员
- 开发过程中若缺失部分页面内容，CP应全力配合补全，直至页面上线







