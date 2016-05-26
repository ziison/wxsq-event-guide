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

    1.命名规范

    2.唯一根节点

    3.嵌套层级

    4.TAB标准

    5.z-index标准

四、交付要点（非常重要）

    1. 优惠券

    2. 商品列表

    3. 角标

    4. 1px像素边框

    5. 文本

    6. 模块活字与图片

    7. 模块的状态切换

    8. TAB的选中与非选中状态

    9. 模块的显示和隐藏状态

    10. 弹窗

    11. 分享蒙层

    12. floating

五、页面性能要求

    1. 页面兼容的目标（浏览器/机型/系统）

    2. 加载速度、请求数与资源压缩

    3. 其他性能要求点

六、页面交付验收点

七、页面交付流程
```

京东购物H5活动CP重构规范，旨在统一CP开发人员的编码规范、提高代码质量，方便后期协作，提高开发效率。

>提示：初次阅读者请务必认真阅读本文。在页面重构启动前，请先认真查看需求文档、页面交互图以及设计规范，充分了解需求后，再与需求方确认会出现页面业务逻辑场景（特别注意各种弹窗、浮层、模块状态等情况），避免页面最终交付时产生缺失，减少开发过程中的不必要的反复修改。

## 一、页面视觉输出标准

1. 视觉稿全部采用宽度750px标准  
2. 构建稿全部使用`rem`  
3. 部分场景（如逐帧动画）可结合`scale`完成, [参见DEMO](http://jdc.jd.com/halo/cpguide/scale/)。

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

@media screen and (min-width: 320px) {
    html {
        font-size:17.07px
    }
}

@media screen and (min-width: 360px) {
    html {
        font-size:19.2px
    }
}

@media screen and (min-width: 414px) {
    html {
        font-size:22.08px
    }
}

@media screen and (min-width: 420px) {
    html {
        font-size:22.4px
    }
}

@media screen and (min-width: 435px) {
    html {
        font-size:23.2px
    }
}

@media screen and (min-width: 768px) {
    html {
        font-size:40.96px
    }
}
```
## 三、编码规范
### 1.命名规范

1）文件命名规范

确保文件命名总是以字母开头而不是数字，且字母一律小写，以下划线连接且不带其他标点符号，更多信息参见：[凹凸实验室--HTML/CSS文件命名规范](http://aotu.io/guide/docs/name/htmlcss.html)


2）ClassName命名规范

ClassName的命名应该尽量精短、明确，必须以字母开头命名，且全部字母为小写，单词之间统一使用下划线 “_” 连接，更多信息参见：[凹凸实验室--ClassName命名规范](http://aotu.io/guide/docs/name/classname.html)。

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

标签嵌套层级必须控制在 5层以内。

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

1）确保TAB状态需齐全

2）吸顶、吸底时需有占位处理。

3) 采用以下标准结构：

<pre>
占位节点  
    └── TAB容器  
            ├── TAB1  
            ├── TAB2  
            ├──  ...  
            └── TABn
</pre>

`参考如下代码：`

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

z-index最大值不得超过700。

z-index取值范围：

- 普通元素：0～100
- floating/吸顶/吸底：101～200
- 弹窗：201～300
- loading/分享蒙层：301～400

## 四、交付要点（非常重要）
### 1.优惠券

#### 1）优惠券展示形式自适应

优惠券通常由2个到三个并列展示，需做自适应居中处理，并包含优惠券展示多种形式，参见【优惠券截图】。

#### 2）券额占位

优惠券额度通常涉及到2位、3位或更多，务必做好券额度占位，防止位数过多错位，参见【优惠券截图】。

`示例：`

![弹窗](http://jdc.jd.com/halo/cpguide/coupon.png?ver=1234)  
【优惠券截图】


### 2.商品列表

商品列表内的所有节点不允许使用 `<a></a>`。

`示例：`

![弹窗](http://jdc.jd.com/fd/promote/leeenx/cp/goodslist.png)


### 3. 角标

带角标模块需要在构建稿上展示**带角标**和**不带角标**的状态

`示例：`

![角标](http://jdc.jd.com/halo/cpguide/tag.jpg?ver=1234)

### 4. 1px像素边框

不使用rem，直接使用1px，通过`transform:scale(.5)`进行缩放处理。

`示例：`

![弹窗](http://jdc.jd.com/halo/cpguide/1border.png?ver=12345)

`参考如下代码：`

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

### 5. 文本

1) 文本溢出处理

对文本溢出，做`text-oveflow: ellipsis;` 处理。

示例：

![弹窗](http://jdc.jd.com/halo/cpguide/txt.png?ver=12345)

文本溢出css代码可参考如下：

```css
/*单行文本溢出*/
.oneline{
    font-size: 1.2rem;
    height: 1.2rem;
    width: 7.5rem;
    line-height: 1;
    overflow: hidden;
    text-overflow: ellipsis;
}
/*多行文本溢出*/
.twoline{
    font-size: 1.2rem;
    height: 3.6rem;
    width: 7.5rem;
    line-height: 1.5;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;
    overflow: hidden;
    text-overflow: ellipsis;
}
```

2) 文本节点占位

商品列表中所有的文本节点都必须指定 `height` 值，确保空内容节点仍然占位。

参见以下css:

```css
.desc{
    font-size: 1rem;
    line-height: 1;
    height: 1rem;//定高，确保绝对占位
}
```
示例：

![截图](http://jdc.jd.com/fd/promote/leeenx/cp/place.png)


### 6.模块活字与图片

1) 凡文字部分尽量做成活字

2) 常见按钮、小标题、商品说明等，须做成活字，且节点宽度自适应

3) 艺术字体如主标题、slogan等明显需要艺术效果的可用图片代替

`示例：`

![按钮](http://jdc.jd.com/halo/cpguide/button.jpg?ver=1234)

###  7.模块的状态切换
凡涉及到商品列表售罄或其他状态，均需要提供齐全。

`示例：`

![商品状态](http://jdc.jd.com/halo/cpguide/pro.jpg?ver=1234)

### 8.TAB的选中与非选中状态

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

###  9.模块的显示和隐藏状态
在HTML代码注释中，标明显示与隐藏状态时切换的class名称。
```
<!-- 模块显示的class为"show" -->
<div class="mod_sharetips ">
   <p>点击右上角，<br>分享到【朋友圈】或发送给微信好友</p>
</div>
```

### 10.弹窗

1）弹窗聚合展示

所有类型弹窗聚合在 `alert.html` 中展示，并做好相关注释。

`示例：`

![弹窗](http://jdc.jd.com/halo/cpguide/tanchuang.png?ver=1234)

> 提示：在页面重构启动前，先认真查看需求文档和页面交互图，充分了解需求后，再与需求方确认会出现那些弹窗，避免页面最终交付时缺失相关弹窗或状态。

2）弹窗局部滚动区域

针对如活动规则和一些内容比较多的区域，需在当前页面内做局部滚动。

`示例：`

![局部滚动](http://jdc.jd.com/halo/cpguide/guen.jpg)

3) 滚动条无定制样式，强制为滚动容器添加以下样式：
```css
-webkit-overflow-scrolling:touch;
```

4) 滚动条定制样式，强制不添加 3) 的样式。

定制滚动条的代码参考：

```css
/*定制滚动条*/
::-webkit-scrollbar{width:2px; height:2px;}
::-webkit-scrollbar-button{width:0;height:0;}
::-webkit-scrollbar-corner{display:block; }
::-webkit-scrollbar-thumb{background-clip:padding-box;background-color:#ffffff;}
```

### 11. 分享蒙层

所有的分享蒙层统一在 `share.html` 中展示，并做好相关注释。

多蒙层注释参考如下：

```html
<!--S 微信分享蒙层-->
<div class="wx_share_mask">
    <div class="share_tips"></div>
</div>
<!--E 微信分享蒙层-->
<!--S 手Q分享蒙层-->
<div class="sq_share_mask" style="display: none">
    <div class="share_tips"></div>
</div>
<!--E 手Q分享蒙层-->
```

![弹窗](http://jdc.jd.com/halo/cpguide/share.png?ver=123)

### 12. floating

1) 返回顶部

卖场页面必须自带 **返回顶部** floating。

示例：

![弹窗](http://jdc.jd.com/halo/cpguide/top.png)

下载 [返回顶部](http://jdc.jd.com/component/list?cate=floating&filename=ex_160425_7)

2) 吸睛floating

确保吸睛floating不与**返回顶部**位置冲突。多状态floating，需要做好相关注释。

示例：

![floating](http://jdc.jd.com/fd/promote/leeenx/cp/floating.png)




## 五、页面性能要求

### 1.页面兼容的目标（浏览器/机型/系统）
- 浏览器：微信，手q浏览器
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

- 使用CSS3动画代替JS动画，不使用伪类做动画处理
- 尽可能少的使用CSS高级选择器与通配选择器
- css中 不使用@import
- 避免使用CSS3渐变阴影效果，可考虑降级效果来提升流畅度

## 六、页面交付验收点

1. 开发目录结构须严格遵守，[模块文件](http://jdc.jd.com/halo/cpguide/webapp.zip)下载
2. 交付页面稿中所有的图片需使用[TinyPNG](https://tinypng.com/)进行压缩处理
3. 交付页面稿中含有css以及js，须提交无压缩和压缩文件（如，style.css/style.min.css、index.js/index.min.js）
4. 交付页面稿须严格测试，性能要求达到第五大点提到的`页面性能要求`
5. 交付页面稿须认真参考第四大点提到的`交付注意点`，解决好常见的业务问题
6. 交付页面稿须展示清楚设计稿中含有的各种状态、业务场景，使用方式注释清晰明了


## 七、页面交付流程

- CP须提供在线预览地址，让需求方扫描二维码体验确认
- 需求方反馈问题和修改点，CP应及时修改反馈，之后待需求方再次确认
- 直至页面全部完成后，将文件包邮件发送给需求方和相关开发人员
- 开发过程中若缺失部分页面内容，CP应全力配合补全，直至页面上线
