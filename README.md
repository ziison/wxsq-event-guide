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
|背景音乐 | mp3 | 小于80 KB | 开头和结尾音轨建议做使用淡入淡出处理|
|分享缩略图 |	jpg | 小于 6 KB | 尺寸为 120x120 像素 |
|视频文件 | mov/mp4/avi | 暂无要求 | 分辨率>=960×540，视频码率>=1500kbps，<br />画面比例 4:3或16:9|

### 资源部署

- 所有的静态文件（图片，CSS以及JS文件等）都要存放在 `http://wq.360buyimg.com` 服务器上
- 所有页面的资源都必须使用**京东商城**域名下的。
- `JDC.JD.COM` 域名下的资源如果投放在活动页面上，需要提前与报备。建议是不要在正式页面上使用`JDC.JD.COM` 上的资源

## 前端工作流程

`kingfisher` （后面简称*ko*）是陈攀与何方舟等同学基于微信手Q运营活动团队量身打造的自动化开发工具。它集成了开发中的常见任务，可以简化开发工作，并在打包环节对资源进行压缩合并优化。在切换到 `athena` 之前，运营的同学都应该统一使用ko来开发。

如何安装与使用`ko` ，请点击[这里](http://git.pp.jd.com/hefangzhou/ko/blob/master/README.md)查看

## JS框架*

团队内部大部分人员都是使用 `zepto.js` ，与我们对接的开发团队也是。所以开发时需要考虑不使用与`zepto.js`冲突的框架。

## REM标准

- 页面的尺寸适配统一使用 `REM` 来完成。与前端开发配合时，可以辅助地使用 `zoom`/`scale` 。
- `320px`宽度下，`<html>`节点的`font-size`为`20px`


## 统一运营活动的头部格式

强制统一页面头部如下：

```html
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" >
<meta name="format-detection" content="telephone=no" >
<link rel="stylesheet" href="xxx.css" >
<script type="text/javascript">
!function(){
      var cw=document.documentElement.clientWidth||document.body.clientWidth,zoom=cw/320;
      var ch= document.documentElement.clientHeight || document.body.clientHeight;
      zoom = Math.min(cw/320,ch/500);
      document.write('\
		<style id="htmlzoom">\
		    html{font-size:'+(zoom*20)+'px;}\
		    .zoom,.o2-{zoom:'+(zoom/2)+';}\
        .o2-scale{-webkit-transform: scale('+zoom/2+'); transform: scale('+zoom/2+');}\
		</style>\
          ');
}();
</script>
```

 `ko`下统一使用以下结构：

```html
<% widget 
	path="/widget/header" 
	data="{'title':'页面标题','css':['css/home.css','css/ani.css'],'zoom':1,'scale':1,'rem':1}"
%>
<body>

</body>
</html>
```

头部的widget定义如下：

```html
<!DOCTYPE HTML>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0"  >
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black">
    <meta name="format-detection" content="telephone=no">
    <!-- 标题 -->
    <title><%= title %></title>
    <!-- 循环输出传入的参数 -->
  	<% for(var i=0; i<css.length; i++) {%>
  		<link rel="stylesheet" type="text/css" href="<%= css[i] %>">
  	<% } %>

<script type="text/javascript">
!function(){
      var cw=document.documentElement.clientWidth||document.body.clientWidth,zoom=cw/320;
      var ch= document.documentElement.clientHeight || document.body.clientHeight;

      zoom = Math.min(cw/320,ch/500);

      document.write('\
		<style id="htmlzoom">\
		    html{font-size:'+(zoom*20)+'px;}\
		    .zoom,.o2-{zoom:'+(zoom/2)+';}\
        .o2-scale{-webkit-transform: scale('+zoom/2+'); transform: scale('+zoom/2+');}\
		</style>\
          ');
}();
</script>
</head>
```



后续使用 `athena` 也要需要做一套统一的头部。

## 通用组件的样式名

运营活动的底部目前已经由运营侧自己负责了，往后的能用组件可能会增加，为了避免其它团队使用我们的样式时不冲突，通用组件的样式使用统一的样式名前缀：`o2-`。

例如： `.o2-goods-list{...}`

*这里会有一个问题：如果外部引用我们的底部或其它通用组件时，如果他们不使用REM，那就会出问题。所以做通用组件时，还需要考虑这情况*
