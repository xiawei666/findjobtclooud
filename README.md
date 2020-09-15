# 项目名称：快速找工作findjob

## 1. 项目介绍

首先感谢官方提供了这次比赛。刚好最近看到腾讯云云函数SCF 推出Custom Runtime定制化运行功能，正在使用C#封装Custom Runtime的云函数，需要找一个场景来进行验证工作，当前微信小程序使用js 作为开发语言， 云开发的云函数也支持多种语言，但是他们都不支持C#，腾讯云 SCF Custom Runtime的云函数可以让更多的后端程序员投身全栈开发。

本次参加这次大赛的初衷是以大赛的要求来充分验证使用C#打造全场景的云原生应用开发，参加比赛的场景是使用小程序快速查找各大招聘网站的岗位，用户在小程序种输入岗位关键词和城市【支持全部城市岗位（不选城市）】，将查询条件提交给后端云函数向各大招聘网站提交查询请求，合并查询结果返回给小程序，同时将相同查询条件的请求使用Redis缓存到服务端。用户在小程序端可以保存他感兴趣的岗位，也可以在微信小程序种发起申请岗位（调用招聘网站的小程序，这一部分理论上技术可行，更多的是商务，因此目前未实现）。用户在小程序端的相关操作进行一个访问统计，用户登录和保存感兴趣岗位数据，以及用户的访问统计数据使用云开发的云函数 封装api和数据库保存数据， 同时将云开发的数据通过Custom Runtime的云函数进行封装成API 提供给 运营站点进行展示， 运营站点使用基于WebAssembly技术的前端框架blazor 进行开发，通过云开发部署到静态网站托管。

这个项目充分调用了小程序的能力，并且结合云开发的优势，同时扩展云开发的云函数，当前云开发的云函数不支持Custom Runtime，云开发的云函数也是基于腾讯云SCF封装。微信小程序可以在ios和android上运行，解决了移动App必须去兼容多端的接口，减少工作量，开发出来的小程序稳定，用完就走，方便用户使用。 云开发进一步的简化了微信小程序的开发，真正做到云原生应用开发，当前云开发不支持C#语言，本项目的主要目的就是展示使用C#语言在云开发的应用。

## 2. 各页面功能展示
![小程序首页](/resources/miniprog.png)

默认界面是职位搜索界面，输入关键词后，点击键盘上的搜索按钮。小程序将向远程 scf 云函数发起请求，返回职位列表。如下：

![职位列表](/resources/miniprog2.png)

可以看到相关职位都已经列出来，搜索框下面有两个下拉选择，分别是地区选择，和职位来源选择。不选择则返回所有地区和所有来源网站的职位信息。点击地区选择时：

![条件选择](/resources/miniprog3.png)

这里可以切换地区，选中地区确认后，则只筛选指定地区的职位信息。这里可以看到左侧有个“定位”按钮，定位时，将根据 wx.getLocation API 获取用户当前经纬度，然后调用 scf 云函数返回用户城市。达到定位效果。重置按钮，则重置筛选条件，将城市设置为不限制。

![来源列表](/resources/miniprog4.png)

来源列表里这里支持“猎聘网”、“智联招聘”、“前程无忧”、“Boos直聘”这4个来源。默认不限来源。

![详情页面](/resources/miniprog5.png)

进入详情界面，可以看到最下方有 “分享”、“收藏”、“申请职位” 三个功能按钮。
点击分享，可以分享职位信息给朋友。
收藏按钮，可以收藏在我的收藏列表里，在“我的收藏”界面里展示。
申请职位按钮可以通过跳转别的网站小程序来实现，但是因为需要商务方面的工作，所以暂时没有实现申请职位。将来有机会，这个功能会实现的。
此详情页面设置了可以分享到朋友圈，点击右上角的 ”...“ 就可以分享到朋友圈。

![详情页面](/resources/miniprog6.png)

## 3. 项目架构

### 功能架构

项目 功能上由小程序 和运营网站构成， 用户使用微信小程序快速找工具，相关的运营数据保存在云开发的云存储中，通过运营网站进行展示。

![功能架构](/resources/funcarch.png)

### 系统架构

本项目以云开发为核心，主要包括：云函数，云数据库，云存储，和HTTP API（SCF云函数）和 .NET SCF Custom Runtime 五个部分。除了云开发外，还有小程序端，运营系统，第三方服务器等部分。

![系统架构](/resources/softarch.png)

## 4. 体验

小程序体验二维码

![小程序体验二维码](/resources/findjobqrcode.jpg)

运营网站体验地址：[https://findjob-1gcto7ln453287ca-1257277642.tcloudbaseapp.com/](https://findjob-1gcto7ln453287ca-1257277642.tcloudbaseapp.com/) 

## 5. 部署指南 

部署参考文档 [deployment.d](deployment.md)

## 开源协议

项目基于 MIT 协议， 详细请参考 [LICENSE](LICENSE) 

## 参考文档

- [云开发文档](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/basis/getting-started.html)

- [Custom Runtime](https://cloud.tencent.com/document/product/583/47274)
