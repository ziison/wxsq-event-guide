# 运营活动规范 -- 建立中

运营活动的零散性，产生成了不同人员开发的页面很难在维护层面上达统一。这造成了不同人员间相互协助的难度，也造成了不同人员相互修改会出现安全隐患的风险。为此，在代码需要在维护层面上达到一个标准。

部分内容参考 [微信朋友圈广告规范](http://wximg.qq.com/wxp/wxmoment-doc/3.1.html)

## 通用规范

### 页面兼容的目标（机型/系统）

- 主流移动设备：iPhone 4+ 、三星、魅族、华为、红米、小米1S 以上及主流 Android 千元机型；请特别关注iPhone4/4s、魅族MX4、华为P6等机型
-  操作系统：iOS 7.0+ 与 Android 4 .0+；


###加载速度、请求数与资源压缩

- 以 3G 网络环境 100kb/s 的网络速度计算，详情页首次加载过程中，可以在 2s 内看到 Loading 页面；
- 活动样式文件1个，脚本文件一个（zepto.js等运营活动的依赖js不计入）。且必须是压缩过后的文件；
图片资源应当进行压缩，JPG 图片使用 80% 以下质量，PNG 图片使用 TinyPNG 进行压缩；
- 应当对小图片进行 CSS 雪碧图 合并，因低版本系统及低端设备渲染问题，单张图片尺寸不超过 2000x2000 像素，超过时需切分成多张雪碧图；
- canvas游戏页面，png图片宽高都应该控制在1024px内

**资源文件格式标准**

| 文件类似 |	格式	 | 大小 | 其它备注 |
| :----: | :----: | :----: | :----: |
|背景音乐 | mp3 | 小于200 KB | 开头和结尾音轨建议做使用淡入淡出处理|
|分享缩略图 |	jpg | 小于 6 KB | 尺寸为 120x120 像素 |
|视频文件 | mov/mp4/avi | 暂无要求 | 分辨率>=960×540，视频码率>=1500kbps，<br />画面比例 4:3或16:9|

### 资源部署

- 所有的静态文件（图片，CSS以及JS文件等）都要存放在 `http://wq.360buyimg.com` 服务器上
- 所有页面的资源都必须使用**京东商城**域名下的。
- `JDC.JD.COM` 域名下的资源如果投放在活动页面上，需要提前与报备。建议是不要在正式页面上使用`JDC.JD.COM` 上的资源

## 前端工作流程

`kingfisher` （后面简称*ko*）是Simba Chen & Adam He 等同学基于微信手Q运营活动团队量身打造的自动化开发工具。它集成了开发中的常见任务，可以简化开发工作，并在打包环节对资源进行压缩合并优化。在切换到 `athena` 之前，运营的同学都应该统一使用ko来开发。

如何安装与使用`ko` ，请点击[这里](http://git.pp.jd.com/hefangzhou/ko/blob/master/README.md)查看

## JS框架*

团队内部大部分人员都是使用 `zepto.js` ，与我们对接的开发团队也是。所以开发时需要考虑不使用与`zepto.js`冲突的框架。

## REM标准

- 页面的尺寸适配统一使用 `REM` 来完成。与前端开发配合时，可以辅助地使用 `zoom`/`scale` 。
- `320px`宽度下，`<html>`节点的`font-size`为`20px`
- reset-sass ---- 统一的重置样式库
- common-sass ---- 统一共同sass库


## 统一运营活动的头部格式

强制统一页面头部如下：

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" >
    <meta name="format-detection" content="telephone=no" >
    
    <title>感恩节卖场</title>
    
  	
  		<link rel="stylesheet" type="text/css" href="css/index.css">
  	
<script type="text/javascript">
!function(){
    var maxWidth=640;
    var cw=document.documentElement.clientWidth||document.body.clientWidth,zoom=maxWidth&&maxWidth<cw?maxWidth/320:cw/320,ch= document.documentElement.clientHeight || document.body.clientHeight;
    window.zoom=window.o2Zoom=zoom;
    document.write('<style id="o2HtmlFontSize">html{font-size:'+(zoom*20)+'px;}.o2-zoom,.zoom{zoom:'+(zoom/2)+';}.o2-scale{-webkit-transform: scale('+zoom/2+'); transform: scale('+zoom/2+');}</style>');
    window.addEventListener("resize",function(e){
        var cw=document.documentElement.clientWidth||document.body.clientWidth,zoom=maxWidth&&maxWidth<cw?maxWidth/320:cw/320,ch= document.documentElement.clientHeight || document.body.clientHeight;
        window.zoom=window.o2Zoom=zoom;
        document.getElementById("o2HtmlFontSize").innerHTML='html{font-size:'+(zoom*20)+'px;}.o2-zoom,.zoom{zoom:'+(zoom/2)+';}.o2-scale{-webkit-transform: scale('+zoom/2+'); transform: scale('+zoom/2+');}';
    });
}();
</script>

</head>
```

 `ko`下统一使用以下结构：

```html
<% widget
	path="/widget/header"
	data="{'title':'双十二预热 三免一','css':['css/index.css'],'zoom':1,'scale':1,'rem':1,'fixIP5':1,'maxWidth':'640'}"
%>
<!--根据实际需求设置zoom,scale,rem。默认只开启rem。fixIP5表示强制IP5的微信webview的宽高比为极限比率-->
<body>

</body>
</html>
```

头部的widget定义如下：

```html
<!DOCTYPE HTML>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" >
    <meta name="format-detection" content="telephone=no" >
    <%# 标题 %>
    <title><%= title %></title>
    <%# 循环输出传入的参数 %>
  	<% for(var i=0; i<css.length; i++) {%>
  		<link rel="stylesheet" type="text/css" href="<%= css[i] %>">
  	<% } %>
</head>
<script type="text/javascript">
!function(){
    var maxWidth=<%=maxWidth%>;
    document.write('<style id="o2HtmlFontSize"></style>');
    var o2_resize=function(){
        var cw,ch;
        if(document&&document.documentElement){
            cw=document.documentElement.clientWidth||document.body.clientWidth,ch= document.documentElement.clientHeight || document.body.clientHeight;
        }else if(window.localStorage["o2-cw"]&&window.localStorage["o2-ch"]){
            cw=parseInt(window.localStorage["o2-cw"])||0,ch=parseInt(window.localStorage["o2-ch"])||0;
        }
        if(!cw||!ch)return ;//出错了
        
        var zoom=maxWidth&&maxWidth<cw?maxWidth/320:cw/320,zoomY=ch/504;
        window.localStorage["o2-cw"]=cw,window.localStorage["o2-ch"]=ch;
        <% if(fixIP5==1) {%>zoom=Math.min(zoom,zoomY);<%}%>
        <% if(zoom==1 || scale==1){ %>window.zoom=window.o2Zoom=zoom;<%}%>
        document.getElementById("o2HtmlFontSize").innerHTML='<% if(rem==1){ %>html{font-size:'+(zoom*20)+'px;}<%}%><% if(zoom==1){ %>.o2-zoom,.zoom{zoom:'+(zoom/2)+';}<%}%><%if(scale==1){ %>.o2-scale{-webkit-transform: scale('+zoom/2+'); transform: scale('+zoom/2+');}<%}%>';
    };
    document&&document.documentElement ? o2_resize() : setTimeout(o2_resize,500);
    window.addEventListener("resize",o2_resize);
}();
</script>
```



后续使用 `athena` 也要需要做一套统一的头部。

## 通用组件的样式名

运营活动的底部目前已经由运营侧自己负责了，往后的能用组件可能会增加，为了避免其它团队使用我们的样式时不冲突，通用组件的样式使用统一的样式名前缀：`o2_`。

例如： `.o2_goods_list{...}`

*这里会有一个问题：如果外部引用我们的底部或其它通用组件时，如果他们不使用REM，那就会出问题。所以做通用组件时，还需要考虑这情况*


## 统一的reset sass

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
	min-width: 20pc;
	max-width: 40pc;
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

## 关于1px像素的border

不建议直接使用rem,而是直接使用1px。

## 奇数pxborder的取舍

一般情况下，四舍五入。具体按实际效果做取舍

## 统一使用`<div class="wrapper"></div>`做为根节点

强制使用`<div class="wrapper"></div`做为根节点，并保证带以下css样式: `margin: 0 auto; position: relative`

## 活动资源目录

活动的静态资源（图片，css，js等）的目录统一存放在：`sftp://192.168.145.37//export/wxsq/resource/fd/promote/`
然后，按年月来建立当月的目录如下：

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

日期目录的作用是便于管理，所以在日期目录下的项目都是对应月份上线的项目。例如：2016年的双十一的项目，在十月初进入开发，但是，项目应该存放在 `201611` 中

## 活动目录命名规范

变量名首字母必须为字母(a-z A-Z)，变量名只能是字母(a-z A-Z)，数字(0-9)，下划线(_)的组合，并且之间不能包含空格，数字不能放在变量名首位。

## 创建新项目规范

创建新项目时，先在当月目录下`svn update`，后新建一个项目目录，然后再`svn add 新项目目录`和`svn commit -m 'build a project'`把项目名称占住，防止与其它人冲突。

## css/html的命名规范

css/html的命名规范请参考：http://aotu.io/guide/docs/name/htmlcss.html

## 代码提交

下班前，必须把手头上的项目提交到SVN上。

## 统一常见sass函数名/mixin名

px转rem统一使用函数名：pxTorem。使用以下函数：

```sass
@function pxTorem($px) {
    @if $px == 0 {
        @return 0;
    }
    @else {
        @return $px / ($px * 0 + 1) / 40 * 1rem;
    }

}
```

雪碧图mixin：s2b。使用以下代码：
```sass
//compass 二倍图转rem
@mixin s2b($sprite, $name, $toRem:true) {
    $width: ceil(image-width(sprite-file($sprite, $name)) / 2);
    $height: ceil(image-height(sprite-file($sprite, $name)) / 2);
    $pos_x: floor(nth(sprite-position($sprite, $name), 1) / 2);
    $pos_y: floor(nth(sprite-position($sprite, $name), 2) / 2);
    
    $size_w: ceil(image-width(sprite-path($sprite)) / 2);
    $size_h: ceil(image-height(sprite-path($sprite)) / 2);
    @if $toRem {
        $pos_x: pxTorem($pos_x);
        $pos_y: pxTorem($pos_y);
        $width: pxTorem($width);
        $height: pxTorem($height);
        $size_w: pxTorem($size_w);
        $size_h: pxTorem($size_h);
    }
    
    background: sprite-url($sprite) no-repeat $pos_x $pos_y;
    background-size: $size_w $size_h;
    height: $height;
    width: $width;
}
```

多行文本截断mixin: lineclamp。使用以下代码

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

IOS毛玻璃效果mixin: blur。使用以下代码
```sass
@mixin blur($px) {
	 -webkit-backdrop-filter: blur($px);
}
```



