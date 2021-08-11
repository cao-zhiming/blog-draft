---
title: 陛下，臣以为可以试试PWA…
categories:
 - 网站
tags:
 - 网站
 - 科技
 - PWA
 - Web前端
date: 2021/8/10 21:23:20
permalink: /2021/08/10/pwa-setup/
---

2016年，[Google](https://google.com)提出了一个叫PWA的概念。

2017年，该项目正式落地。随后，各大公司与浏览器开始宣布支持PWA。~~Internet Explorer不是一款浏览器。~~

2021年，本站全站支持PWA。

<!-- more -->

PWA全称**Progressive Web App**，即**渐进式网络应用**（自己翻译的，如有冲突请以官方为准）。它能够让你的网站有一种像应用的感觉~~（并没有好吧）~~，并且……

- 可以在后台加载，*妈妈再也不用担心关闭页面就不加载了*；
- 可以离线使用，Service Worker会缓存一些文件，*妈妈再也不用担心我的网站离线不能访问了*；
- UI（图形界面）好评，*妈妈再也不用担心我的网页访问起来不好看了*；
- 无需下载和安装，*妈妈再也不用担心我的安装包到处都是了*；
- 软件初版、迭代更新不必提交到App平台审核，*妈妈再也不用担心我的应用无缘由地审核不过了*；
- 不必开发Windows、MacOS X、Linux、Android、iOS、Android、iPadOS这么一堆版本，*妈妈再也不用担心我的电脑被开发的不同版本的软件塞满了*。

好吧，那么，我们就开始整PWA吧！

首先，我认为你需要先有一个~~bug不太多的~~网站。静态的动态的都随意；自建的用框架的都可以。如果想用Hexo，我认为我可以~~强迫~~让你去看看我的[Hexo指南](https://blog.caozhiming.tk/2021/05/04/how-to-setup-hexo/)，写得超弱的。🤣

## Web Manifest
接着，我们需要配置Web Manifest。它相当于一个PWA网站的配置记录。示例如下：

```json
{
  "name": "百度一下，你可能就不知道",
  "short_name": "百度",
  "display": "standalone",
  "start_url": "/",
  "theme_color": "#8888ff",
  "background_color": "#aaaaff",
  "icons": [
    {
      "src": "logo.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
```

解释一下以上都是些什么东西哈。

### ```name```
网站的名称。

### ```short_name```
网站的简短名称。有时候网不好，或者什么其他原因，就用它。

### ```display```
这标志着这个PWA的显示形式。这里展示一下所有的显示形式以及其相关内容。

| 显示模式 | 描述 | 后备显示模式 |
| --- | --- | --- |
| fullscreen | 全屏显示, 所有可用的显示区域都被使用, 并且不显示状态栏。 | standalone |
| standalone | 让这个应用看起来像一个独立的应用程序，包括具有不同的窗口，在应用程序启动器中拥有自己的图标等。这个模式中，用户代理将移除用于控制导航的UI元素，但是可以包括其他UI元素，例如状态栏。 | minimal-ui |
| minimal-ui | 该应用程序将看起来像一个独立的应用程序，但会有浏览器地址栏。 样式因浏览器而异。 | browser |
| browser | 该应用程序在传统的浏览器标签或新窗口中打开，具体实现取决于浏览器和平台。 这是默认的设置。 | 呃……没有 |

### ```start_url```
这标志着你的PWA一打开会跳转到什么URL。一般就是根目录嘛。输入```/```就好了。

### ```theme_color```和```background_color```
分别是主题颜色和背景颜色，颜色转换请百度搜索“颜色代码在线转换”。

### ```icons```
src是图标的路径。如果是相对路径，那么基本路径就是这个manifest所在的路径。

sizes是大小。**请注意，经过反复尝试，我发现图标大小必须大于512x512，并且中间的乘号应输入小写字母x**。

type是图标的种类，与解码有关，常用的是image/png等等。**不一定与图标文件名后缀名相同。**

现在，把它保存成一个JSON文件，名字随便，最好类似于```manifest.json```，并上传到你网站的根目录。（特别地，Hexo请上传到/source/目录下）接下来，在所有想要在PWA中包括的页面中的源代码的head部分，加入：

```html
<link rel="manifest" href="/manifest.json"/>
```

如果有需要（例如，有些页面不直接在根目录下），可以在href中使用绝对地址。（特殊地，Hexo请在/themes/<你安装并启用的主题>/layout/layout.ejs的head标签内嵌入上方代码，并在其第一个斜杠前加上你的网站的根URL，包括协议前缀，例如<strong>https://blog.caozhiming.tk</strong>）

## Service Worker

这时，PWA才真正开始。我建议你使用[sw-toolbox](https://github.com/cao-zhiming/ss-caozhimingtk/blob/main/js/sw-toolbox.js)，把它全部复制下来，保存为一个sw-toolbox.js，上传到网站根目录。


