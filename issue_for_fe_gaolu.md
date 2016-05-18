# 运营活动规范 -- 建立中

由于运营活动的零散性，导致了团队不同人员所开发的页面在维护层面上很难形成统一，也增加了相互协助的难度，从而造成不同人员之间修改代码存在安全隐患的风险。为此，制定统一开发规范，方便团队后期协作、提高开发效率。
部分内容参考 [微信朋友圈广告规范](http://wximg.qq.com/wxp/wxmoment-doc/3.1.html)

## 1. 通用规范

### 1.1 页面兼容的目标（浏览器/机型/系统）
- 浏览器：微信，手q浏览器
- 主流移动设备：iPhone 4+ 、三星、魅族、华为、红米、小米1S 以上及主流 Android 千元机型；请特别关注iPhone4/4s、魅族MX4、华为P6等机型
-  操作系统：iOS 7.0+ 与 Android 4.0+


### 1.2 加载速度、请求数与资源压缩

- 以 3G 网络环境 100kb/s 的网络速度计算，详情页首次加载过程中，可以在 2s 内看到 Loading 页面；
- 活动样式文件1个，脚本文件一个（zepto.js等运营活动的依赖js不计入）。且必须是压缩过后的文件；
图片资源应当进行压缩，JPG 图片使用 80% 以下质量，PNG 图片使用 TinyPNG 进行压缩；
- 应当对小图片进行 CSS 雪碧图 合并，因低版本系统及低端设备渲染问题，单张图片尺寸不超过 2000x2000 像素，超过时需切分成多张雪碧图；
- canvas游戏页面，png图片宽高都应该控制在1024px内。

**资源文件格式标准**

| 文件类似 |	格式	 | 大小 | 其它备注 |
| :----: | :----: | :----: | :----: |
|背景音乐 | mp3 | 小于200 KB | 开头和结尾音轨建议做使用淡入淡出处理|
|分享缩略图 |	jpg | 小于 6 KB | 尺寸为 120x120 像素 |
|视频文件 | mov/mp4/avi | 暂无要求 | 分辨率>=960×540，视频码率>=1500kbps，<br />画面比例 4:3或16:9|

##  2.开发仓库目录
svn仓库地址：
```
http://svn.360buy-develop.com/repos3/wxsq/mobile_operation/trunk/resource/fd/promote/
```
git仓库地址：
```
http://source.jd.com/app/wxsq_event.git
```


## 3. 活动资源

#### 3.1 活动资源目录

静态资源（图片/css/js等）的目录统一存放在：`sftp://192.168.145.37/export/wxsq/resource/fd/promote/`
然后，按`年月`来建立当月的目录如下：
```
├── 201501  
├── 201502  
├── 201503  
├── 201504  
├── 201505  
├── 201506  
├── 201507  
├── 201508  
├── 201509  
├── 201510  
└── 201511  
```
> _日期目录的作用是便于管理，所以在日期目录下的项目都是对应月份上线的项目。例如：2016年的双十一的项目，在十月初进入开发，但是，项目应该存放在 `201611` 中_

#### 3.2 活动目录命名规范

变量名首字母必须为字母(a-z A-Z)，变量名只能是字母(a-z A-Z)，数字(0-9)，下划线(_)的组合，并且之间不能包含空格，数字不能放在变量名首位。

#### 3.3 创建新项目规范

- 在当月目录下`svn update/git fetch`
- 新建一个项目目录(athena创建module)
- 然后svn/git提交一下，避免不必要的冲突

####  3.4 代码提交
下班前，必须把手头上的项目提交到代码库。


##  4. 资源部署

- 所有的`静态文件`（图片/CSS/JS等）都要存放在 `http://wq.360buyimg.com` 上；
- 所有`页面`以及`资源`都必须使用**京东商城**域名下的；
- `jdc.jd.com` 域名下的资源如果投放在活动页面上，需要提前与报备。建议不要在正式页面上使用`jdc.jd.com` 上的资源。

##5. 前端工作流程

###  5.1 构建流程工具
####  5.1.1 Athena

Athena是由京东用户体验设计部`凹凸实验室`推出的一套项目流程工具，不仅包括了csslint/jshint代码检测、images压缩、csssprite雪碧图、css/js合并压缩等常用基本功能，还拥有独立的管理后台能够对资源进行实时监控，让你对项目情况一目了然！

如何安装与使用`athena` ，请点击[这里](https://github.com/o2team/athena)查看

####    5.1.2 _ko（仅用于维护旧项目）_

如何安装与使用`ko` ，请点击[这里](http://source.jd.com/app/ko)查看

###  5.2 JS框架*

鉴于团队内部大部分人员都是使用 `zepto.js` ，与我们对接的开发团队也是。所以开发时，可统一使用`zepto.js`，如需使用其他框架则因考虑是否与`zepto.js`产生冲突。

统一使用以下地址：
```html
<script src="//wq.360buyimg.com/fd/promote/base/zepto.min.js"></script>
```

###  5.3 REM标准

- 页面的尺寸适配统一使用 `REM` 与 `px` 结合来完成，定高不定宽（通常布局使用rem，模块大小采用px）,参考示例[demo](http://jdc.jd.com/fd/promote/201606/components/index2.html)；可以辅助地使用 `zoom`/`scale` ；
- `375px`宽度下，`<html>`节点的`font-size`为`20px`。

### 5.4 统一的页头

统一的页面结构如下（athena创建）：
```html
<!DOCTYPE HTML>
<html lang="zh-CN">
<head>
  <meta charset="utf-8" />
  <title>卖场页面</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0" />
  <meta name="apple-mobile-web-app-capable" content="yes" />
  <meta name="apple-mobile-web-app-status-bar-style" content="black" />
  <meta name="format-detection" content="telephone=no" />

  <!-- global:css -->
  <%= getCSS('gb.css', 'gb') %>
  <!-- endglobal -->

  <%= getCSS('style.css','sq618pre') %>

  <!-- S 引用前端页面片 -->
  <!--#include virtual="/sinclude/cssi/promote/201606/sqtsvenue/index.shtml" -->
  <!-- E 引用前端页面片 -->
  
  <script type="text/javascript">
!function(){
    var maxWidth=768;
    document.write('<style id="o2HtmlFontSize"></style>');
    var o2_resize=function(){
        var cw,ch;
        if(document&&document.documentElement){
            cw=document.documentElement.clientWidth,ch=document.documentElement.clientHeight;
        }
        if(!cw||!ch){
            if(window.localStorage["o2-cw"]&&window.localStorage["o2-ch"]){
                cw=parseInt(window.localStorage["o2-cw"]),ch=parseInt(window.localStorage["o2-ch"]);
            }else{
                chk_cw();//定时检查
                return ;//出错了
            }
        }

        var zoom=maxWidth&&maxWidth<cw?maxWidth/375:cw/375,zoomY=ch/504;
        window.localStorage["o2-cw"]=cw,window.localStorage["o2-ch"]=ch;
        
        window.zoom=window.o2Zoom=zoom;
        document.getElementById("o2HtmlFontSize").innerHTML='html{font-size:'+(zoom*20)+'px;}.o2-zoom,.zoom{zoom:'+(zoom/2)+';}.o2-scale{-webkit-transform: scale('+zoom/2+'); transform: scale('+zoom/2+');}';
    },
    siv,
    chk_cw=function(){
        if(siv)return ;//已经存在
        siv=setInterval(function(){
            //定时检查
            document&&document.documentElement&&document.documentElement.clientWidth&&document.documentElement.clientHeight&&(o2_resize(),clearInterval(siv),siv=undefined);
        },100);
    };
    o2_resize();//立即初始化
    window.addEventListener("resize",o2_resize);
}();
</script>

</head>
```

### 5.5 统一的reset sass

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

### 5.6 统一的common sass
整合了项目常用的sass函数，由athena创建，见6.2.1。

## 6. 编码规范

###  6.1 命名规范
#### 6.1.1 文件命名规范

确保文件命名总是以字母开头而不是数字，且字母一律小写，以下划线连接且不带其他标点符号，更多信息参见：[凸实验室–HTML/CSS文件命名规范](http://aotu.io/guide/docs/name/htmlcss.html)

#### 6.1.2 ClassName命名规范

ClassName的命名应该尽量精短、明确，必须以字母开头命名，且全部字母为小写，单词之间统一使用下划线 “_” 连接，更多信息参见：[凹凸实验室–ClassName命名规范](http://aotu.io/guide/docs/name/classname.html)。


###  6.2 统一常用sass函数名/mixin

####  6.2.1 px转rem：pxTorem

使用以下函数：

```sass
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
```

####  6.2.2 媒介查询确定html根节点字体大小：media

使用以下代码：

```sass
@mixin media( $limit ) {
    @media screen and ( min-width: $limit){
        &{
            @content;
        }
    }
}
```

####  6.2.3 雪碧图：s2b

使用以下代码：

```sass
//compass 二倍图转rem
@mixin s2b($sprite, $name, $toRem:true) {
    background:sprite-url($sprite) no-repeat;
    $width: ceil(image-width(sprite-file($sprite, $name)) / 2);
    $height: ceil(image-height(sprite-file($sprite, $name)) / 2);
    $pos_x: floor(nth(sprite-position($sprite, $name), 1) / 2);
    $pos_y: floor(nth(sprite-position($sprite, $name), 2) / 2);

    $size_w: ceil(image-width(sprite-path($sprite)) / 2);
    $size_h: ceil(image-height(sprite-path($sprite)) / 2);
    @if $toRem {
        $pos_x: pxTorem(floor(nth(sprite-position($sprite, $name), 1)));
        $pos_y: pxTorem(floor(nth(sprite-position($sprite, $name), 2)));
        $width: pxTorem(ceil(image-width(sprite-file($sprite, $name))));
        $height: pxTorem(ceil(image-height(sprite-file($sprite, $name))));
        $size_w: pxTorem(ceil(image-width(sprite-path($sprite))));
        $size_h: pxTorem(ceil(image-height(sprite-path($sprite))));
    }

    background-position: $pos_x $pos_y;
    background-size: $size_w $size_h;
    height: $height;
    width: $width;
}

```

####  6.2.4 单/多行文本截断: lineclamp

使用以下代码:

```sass
@mixin lineclamp($line:1){
    @if $line<=0{
        //一行
        $line:1;
    }
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: $line;
    overflow: hidden;
    text-overflow: ellipsis;
}
```
### 6.3 唯一根节点

强制使用`<div class="wrapper"></div>`做为根节点，每一个页面的只存在一个根节点 `class="wrapper"`：

```html
    <!-- S 页面内容 -->
    <div class="wrapper"> 包含住页面内容</div>
    <!-- E 页面内容 -->
```

并为这个节点强制添加以下样式：

```css
.wrapper{
    width: pxTorem(768px);  
    height: auto;//如果没有高度限制，使用auto
    overflow: hidden;//必选
    margin:0 auto;
}
```

###  6.4 嵌套层级

标签嵌套层级必须控制在 5层以内。

```html
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

### 6.5 TAB标准

1）确保TAB状态需齐全

2）吸顶、吸底时需有占位处理。

3）采用以下标准结构：

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

### 6.6 z-index标准

z-index最大值不得超过700。

z-index取值范围：

- 普通元素：0～100
- floating/吸顶/吸底：101～200
- 弹窗：201～300
- loading/分享蒙层：301～400

###  6.7 1px像素边框

不使用rem，直接使用1px，通过`transform:scale(.5)`进行缩放处理。

`示例：`

![弹窗](http://jdc.jd.com/halo/cpguide/1border.png?ver=12345)

`参考如下代码：`

```css
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

### 6.8 蒙层的毛玻璃效果*

ios8.x以后的 safari支持毛玻璃效果，建议统一加透明蒙层添加毛玻璃效果。

以下是毛玻璃的mixin: blur。使用以下代码
```sass
@mixin blur($px:4px) {
	 -webkit-backdrop-filter: blur($px);
}
```

### 6.9 活动规则弹窗

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

## 7.检查列表

1.使用`<div class="wrapper"></div>`做为根节点，且最大宽度值为768px
2.活动规则弹窗，统一为活动规则的容器添加弹性滚动的样式（定制滚动条除外）
3.单张图片尺寸不超过`2000x2000`像素，且大小小于200KB 
4.统一使用Athena项目流程工具
5.新项目代码每天提交






