# 京东购物H5活动CP重构规范

##目录

```
一、页面视觉输出标准

二、开发工作流程

    1.开发目录结构

    2.JS框架

    3.REM换算

    4.HTML统一页面结构

    5.统一的reset css

三、编码规范

    1.HTML/CSS的命名及书写规范

    2.唯一根节点

    3.嵌套层级

    4.TAB标准

    5.z-index标准

    6.1px像素边框

    7.活动规则弹窗

四、交付要点（非常重要）

    1.优惠券

    2.商品模块

    3.固定角标元素

    4.文字单行多行占位

    5.模块活字与图片

    6.模块的状态切换

    7.TAB的选中与非选中状态

    8.模块的显示和隐藏状态

    9.弹窗浮层

五、页面性能要求

    1.页面兼容的目标（机型/系统）

    2.加载速度、请求数与资源压缩

    3.其他性能要求点

六、页面交付验收点

七、页面交付流程
```

京东购物H5活动CP重构规范，旨在统一CP开发人员的编码规范、提高代码质量，方便后期协作，提高开发效率。

>提示：初次阅读者请务必认真阅读本文。在页面重构启动前，请先认真查看需求文档、页面交互图以及设计规范，充分了解需求后，再与需求方确认会出现页面业务逻辑场景（特别注意各种弹窗、浮层、模块状态等情况），避免页面最终交付时产生缺失，减少开发过程中的不必要的反复修改。

## 一、页面视觉输出标准

- 活动页面采用750px的视觉设计稿输出，全部采用`rem`单位构建页面布局，部分场景结合使用`zoom/scale`

- 以iPhone6 WeChat页面为基准

## 二、开发工作流程

### 1.开发目录结构

```
 webapp/
    ├── css/
    ├── sass/
    ├── images/
    ├── js/
    ├── index.html
    └── other.html
```
下载 [模块文件](http://jdc.jd.com/halo/cpguide/webapp.zip)

### 2.JS框架

重构页面有涉及到脚本交互，需要使用JS框架请统一采用 `zepto.js`，引入如下CDN地址。

生产阶段：
```
//生产阶段CDN地址，适用http协议
<script src="http://wq.360buyimg.com/fd/promote/base/zepto.min.js"></script>
```

交付阶段：
```
//交付阶段正式CDN地址，适用http协议、https协议
<script src="//wq.360buyimg.com/fd/promote/base/zepto.min.js"></script>
```



### 3.REM换算

规定`375px`宽度下，`<html>`节点的`font-size`为`20px`

使用过程中，可以手动转换模块尺寸到rem单位，也可以结合sass函数（推荐）。

REM转换公式：

```
    REM值 ＝ 750设计稿中模块实际尺寸／40
```

若使用sass，则可参考如下转换函数：
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


### 4.HTML统一页面结构

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

### 5.统一的reset css

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
## 三、编码规范
### 1.HTML/CSS的命名及书写规范

1）HTML/CSS文件命名规范

确保文件命名总是以字母开头而不是数字，且字母一律小写，以下划线连接且不带其他标点符号，更多信息参见：[凹凸实验室HTML/CSS文件命名规范](http://aotu.io/guide/docs/name/htmlcss.html)


2）ClassName命名规范

ClassName的命名应该尽量精短、明确，必须以字母开头命名，且全部字母为小写，单词之间统一使用下划线 “_” 连接，更多信息参见：[凹凸实验室ClassName命名规范](http://aotu.io/guide/docs/name/classname.html)。

### 2.唯一根节点

强制使用`<div class="wrapper"></div>`做为根节点，每一个页面的只存在一个根节点 `class="wrapper"`：

```html
    <!-- S 页面内容 -->
    <div class="wrapper"> 包含住页面内容</div>
    <!-- E 页面内容 -->
```

并为这个节点强制添加以下样式：

```css
.wrapper{
    width: 18.75rem;  // 计算方式  750/40 ＝ 18.75rem
    height: auto;//如果没有高度限制，使用auto
    overflow: hidden;//必选
    margin:0 auto;
}
```

### 3.嵌套层级

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

### 4.TAB标准

1）吸顶或吸底的TAB状态需齐全

2）吸顶时需有占位处理。

采用以下标准结构：

<pre>
占位节点  
    └── TAB容器  
            ├── TAB1  
            ├── TAB2  
            ├──  ...  
            └── TABn
</pre>

示例：

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

`示例：`

![吸顶部分](http://jdc.jd.com/halo/cpguide/nav.png?ver=1234)

### 5.z-index标准

z-index最大值不得超过700

z-index值参考如下范围：

- 普通元素：0～100之间
- floating/吸顶/吸底：101～200之间
- 弹窗：201～300之间
- loading/分享蒙层：301～400之间

### 6.1px像素边框

不使用rem，直接使用1px，通过`transform:scale(.5)`进行缩放处理。

`示例：`

![弹窗](http://jdc.jd.com/halo/cpguide/1border.png?ver=12345)

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

### 7.活动规则弹窗

活动规则弹窗，如果视觉稿对滚动条没有做定制，则统一为活动规则的容器添加弹性滚动的样式：

```css
-webkit-overflow-scrolling:touch;
```

如果有滚动条定制需求，由于上述代码会与滚动条css冲突，所以定制滚动的情况下不添加弹性滚动。如下是一个滚动条样式参考：

```css
/*定制滚动条*/
::-webkit-scrollbar{width:2px; height:2px;}
::-webkit-scrollbar-button{width:0;height:0;}
::-webkit-scrollbar-corner{display:block; }
::-webkit-scrollbar-thumb{background-clip:padding-box;background-color:#ffffff;}
```

## 四、交付要点（非常重要）
### 1.优惠券

#### 1）优惠券展示形式自适应

优惠券通常由2个到三个并列展示，需做自适应居中处理，并包含优惠券展示多种形式。

#### 2）券额占位

优惠券额度通常涉及到2位、3位或更多，务必做好券额度占位，防止位数过多错位。

`示例：`

![弹窗](http://jdc.jd.com/halo/cpguide/coupon.png?ver=1234)

### 2.商品模块

1）整块点击

商品模块外层包裹以及内层元素不允许使用a标签，模块具体的点击区域务必与产品经理沟通。

`示例：`

![弹窗](http://jdc.jd.com/halo/cpguide/hot.png?ver=1234)

2) 局部点击

若单品内有按钮交互行为，如领券，按钮，则不允许使用a标签实现，可用span、i等标签代替。

### 3.固定角标元素
固定角标元素，均需要提供有固定角标和没有固定角标的多种样式展示。

`示例：`

![角标](http://jdc.jd.com/halo/cpguide/tag.jpg?ver=1234)

### 4.文字单行多行占位

文字单行多行时，采用定高处理方式，溢出省略号，防止文字过少样式错乱，请特别注意。

`示例：`

![弹窗](http://jdc.jd.com/halo/cpguide/txt.png?ver=12345)

### 5.模块活字与图片

1) 凡文字部分尽量做成活字

2) 常见按钮、小标题、商品说明等，须做成活字，且节点宽度自适应

3) 艺术字体如主标题、slogan等明显需要艺术效果的可用图片代替

`示例：`

![按钮](http://jdc.jd.com/halo/cpguide/button.jpg?ver=1234)

###  6.模块的状态切换
凡涉及到商品列表售罄或其他状态，均需要提供齐全。

`示例：`

![商品状态](http://jdc.jd.com/halo/cpguide/pro.jpg?ver=1234)

### 7.TAB的选中与非选中状态

常见的TAB多种状态，须在HTML代码注释中，标明选中与非选中状态时切换的class名称，使用方式说明清楚明了。

`示例：`

![按钮](http://jdc.jd.com/halo/cpguide/tab.jpg?ver=1234)

```
<div class="ft_nav_tab ">
    <div class="nav_tab_mod">
        <!-- 开发注意，选中态添加"on" -->
       <a href="" class＝"on">手机数码</a>
       <a href="">家电电器</a>
       <a href="">潮流女包</a>        
    </div>
</div>
```

###  8.模块的显示和隐藏状态
在HTML代码注释中，标明显示与隐藏状态时切换的class名称。
```
<!-- 模块显示的class为"show" -->
<div class="mod_sharetips ">
   <p>点击右上角，<br>分享到【朋友圈】或发送给微信好友</p>
</div>
```

### 9.弹窗浮层

1）弹窗浮层罗列齐全

页面含多种弹窗浮层，须标明清楚提供齐全，并统一写在页面上（建议新开页面专门写弹窗如alert.html）。如以下浮层：提示浮层、页面逻辑浮层、选择用户/地区浮层、活动规则浮层、操作反馈提示等

`示例1 弹窗：`

![弹窗](http://jdc.jd.com/halo/cpguide/tanchuang.png?ver=1234)

> 提示：在页面重构启动前，先认真查看需求文档和页面交互图，充分了解需求后，再与需求方确认会出现那些浮层，避免页面最终交付时缺失相关浮层或状态。

`示例2 分享浮层：`

![弹窗](http://jdc.jd.com/halo/cpguide/share.png)

`示例3 返回顶部：`

![弹窗](http://jdc.jd.com/halo/cpguide/top.png)

结构：

```
<div class="ex_160425_7">
    <!--返回顶部箭头-->
    <img class="o2_backtop" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACQAAAAoCAMAAAChHKjRAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyJpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNiAoV2luZG93cykiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6NTQ4NjVEQTU0MERDMTFFNUJERDFEM0ZEOUJFOTE2RkMiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6NTQ4NjVEQTY0MERDMTFFNUJERDFEM0ZEOUJFOTE2RkMiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDo1NDg2NURBMzQwREMxMUU1QkREMUQzRkQ5QkU5MTZGQyIgc3RSZWY6ZG9jdW1lbnRJRD0ieG1wLmRpZDo1NDg2NURBNDQwREMxMUU1QkREMUQzRkQ5QkU5MTZGQyIvPiA8L3JkZjpEZXNjcmlwdGlvbj4gPC9yZGY6UkRGPiA8L3g6eG1wbWV0YT4gPD94cGFja2V0IGVuZD0iciI/PlO0CKoAAAAGUExURf///////1V89WwAAAACdFJOU/8A5bcwSgAAAH1JREFUeNrs0uEOgCAIBOC793/pWmQqHMZaP2XONv1McICFwDleoo5K131AdsEatTRWqCebo7GkDM2Fa2Sb15gVvCFAr+AN72lU8IZtHhS84fPpCt4QYikeEz+PCYg0Yymi4Pgo4ulCcwChcQwxQ0zaFyi070Yb/YlEHAIMAB6iBI03W6MyAAAAAElFTkSuQmCC">
    
</div>
```
样式：
```
.ex_160425_7 {
  position: fixed;
  bottom: 50px;
  right: 0;
  width: 40px;
  height: 40px;
  background-color: rgba(0, 0, 0, 0.9);
  border-top-left-radius: 2px;
  border-bottom-left-radius: 2px;
  text-align: center;
  font-size: 10px;
  color: #fff;
  line-height: 18px;
  z-index: 4;
}
.ex_160425_7 .o2_backtop {
  display: block;
  width: 18px;
  height: 20px;
  margin: 10px auto;
}
```



2）局部滚动区域

针对如活动规则和一些内容比较多的区域，需要在当前页面内做局部滚动的，请勿遗漏。

`示例：`

![局部滚动](http://jdc.jd.com/halo/cpguide/guen.jpg)

## 五、页面性能要求

### 1.页面兼容的目标（机型/系统）

- 主流移动设备：iPhone 4+ 、三星、魅族、华为、红米、小米1S 以上及主流 Android 千元机型；请特别关注iPhone4/4s、魅族MX4、华为P6等机型
-  操作系统：iOS 7.0+ 与 Android 4 .0+；


### 2.加载速度、请求数与资源压缩

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

### 3.其他性能要求点

- HTML结构层级保持足够简单，标签嵌套不宜过深，尽量控制在 5 级内
- 使用CSS3动画代替JS动画，CSS3动画常用属性：opacity，translate，rotate，scale，不使用伪类做动画处理
- 尽可能少的使用CSS高级选择器与通配选择器
- css中 不使用@import
- 避免使用CSS3渐变阴影效果，可考虑降级效果来提升流畅度

## 六、页面交付验收点

- 1.开发目录结构须严格遵守，[模块文件](http://jdc.jd.com/halo/cpguide/webapp.zip)下载
- 2.交付页面稿须严格测试，性能要求达到第五大点提到的`页面性能要求`
- 3.交付页面稿中含有css以及js，须提交无压缩和压缩文件（如，style.css/style.min.css、index.js/index.min.js）
- 4.交付页面稿须认真参考第四大点提到的`交付注意点`，解决好常见的业务问题
- 5.交付页面稿须展示清楚设计稿中含有的各种状态、业务场景，使用方式注释清晰明了


## 七、页面交付流程

- CP须提供在线预览地址，让需求方扫描二维码体验确认
- 需求方反馈问题和修改点，CP应及时修改反馈，之后待需求方再次确认
- 直至页面全部完成后，将文件包邮件发送给需求方和相关开发人员
- 开发过程中若缺失部分页面内容，CP应全力配合补全，直至页面上线







