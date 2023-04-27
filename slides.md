---
theme: seriph
background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
highlighter: shiki
lineNumbers: false
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
drawings:
  persist: false
transition: slide-left
css: unocss
title: 微前端
---

# GeoScene JS API 微前端初探

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    专业服务事业部 / 张光耀
  </span>
</div>

<!--
各位同事好，很长时间没有和大家做技术分享了。
今天分享的主题叫《GeoScene JS API 微前端初探》。
主要内容是两部分，一部分讲微前端，一部分结合JSAPI做了一个demo。
-->

---
transition: slide-left
---

## 概念

微前端是一种架构的风格。
<br/>
<br/>
举例：将一个大型的电子商务网站分解为多个小型的应用程序，比如商品列表、购物车、结算等。每个应用程序都可以独立开发和部署，并且可以在不同的团队间进行协作。这些应用程序被组合成一个整体网站，用户可以使用它们来完成他们想要的任务，且并无感知。
<br/>
<br/>
是受微服务启发而来，
<br/>
背后思想是分治法：将单一代码库分解成多个较小的部分，以便多个相对独立的团队进行分工协作。

<!--
首先我们来了解概念。
从定义上讲，微前端是一种架构风格，举一个例子的话，就是将一个大型的电子商务网站分解为多个小型的应用程序，比如商品列表、购物车、结算等。每个应用程序都可以独立开发和部署，并且可以在不同的团队间进行协作。这些应用程序被组合成一个整体网站，用户可以使用它们来完成他们想要的任务，且并无感知。
微前端并不是一个很新的概念，它是受微服务启发而来。
微服务这种后端架构，韩松哥之前也做过分享。
背后的主要思想就是分治法：将单一代码库分解成多个较小的部分，以便多个相对独立的团队进行分工协作。
-->

---
transition: slide-left
---

## 风格选择

<br/>

- **微前端应用** - Micro FrontEnd, MFE
- **单页应用** - single-page application, SPA
- **同构应用** - Server Side Render, SSR
- **静态页面网站** - Static Website
- **JAMStack** - JavaScript、API&Markup

<!--
既然是一种风格，那么关于前端架构风格，
主流上，总过有这5种架构风格。
第一种就是今天的主角，微前端。
第二种就是我们现在的架构风格，单页面应用。
第三种叫同构应用，简单理解就是SSR，服务端渲染。
第四种，静态页面网站，就是老式的，通过锚点跳转的网站。
第五种，JAMStack，是JavaScript、API和标记语言的结合，被称为下一代前端开发架构。
那么前端生态在不断地发展中，孕育出了这些用于解决不同问题的不同架构。这些架构风格都有各自的优缺点。
微前端作为其中之一，为我们提供一种的选择。我们拥有选择它或不选择它的权利，那么我们现在去了解它，然后评判我们是否应该选择它。
-->

---
transition: slide-left
---

## 微服务

从单体到微服务

<div class="flex">
  <div class="flex items-center">
    <div>
      <p>geoplat-service</p>
    </div>
  </div>
  <div class="flex items-center">
    <div class="p-8">
      <p> -> </p>
    </div>
  </div>
  <div class="flex items-center">
    <div>
      <p>
      solr-service<br/>
      monitor-service<br/>
      file-service<br/>
      ...
      </p>
    </div>
  </div>
</div>

<!--
既然微前端是启发自微服务，那么我们需要简单交代一下微服务。拿我们公司的后台服务举例：
最初我们是用单体架构，所有的代码都放在一个代码库中，比如叫geoplat-service。随着业务线的拓展和功能的不断丰富，我们就从单体架构逐步地迁移到了微服务构架，派生出了如solr-service，专门做查询；monitor-service，专门做监控；和file-service，专门做文件管理，等等等。
在项目实施中，微服务也工作地很好。
既然微服务在后端上工作的很好，那么在前端是不是也可以借鉴过来。
-->

---
transition: slide-left
---

## 微前端原则

Sam NewMan 在《微服务设计》中总结了一些原则，也适用于微前端。

- **围绕业务领域建模** - domain-driven design,DDD
- **自动化文化** - 持续集成、流水线部署
- **隐藏实现细节** - 隐藏实现细节、按协议工作
- **分布式治理** - 分布式决策
- **独立部署** - 一个团队可以端到端地掌握一个业务领域
- **故障隔离** - 服务降级
- **高度可观察性** - 日志和监控，快速解决问题

<!--
在实践微前端之前，我们先了解一下微前端的原则。
这个大兄弟在《微服务设计》这本书中总结了7条原则，适用于微服务，同样也适用于微前端。
因为时间关系，我并不会逐一解释这些原则。
把他们放在这里，目的是想告诉大家，微前端也是有原则来指导项目开发过程的。
-->

---
transition: slide-left
---

## 探索微前端架构

<br/>

- **Micro Frontends** - 微前端们，理解为页面的一部分
- **Micro-FE Framework** - 框架，用来连接微前端和基座
- **Host Pages** - 基座

<!--
假如我们现在要做微前端，那么微前端的架构应该是怎么样的呢？
大体上，微前端的架构是分为了三个部分，
分别是微前端们，这里我没有想到一个特别好的名称，暂时就这么叫着吧，它理解为页面的一部分。比如说cim-map中的标绘组件，在地图的右侧有一个功能按钮，我们点击之后，弹出一块面板，面板内是标绘的业务逻辑。这块面板我们就可以拆分出来，作为一个微前端。可以近似地理解为微件，但是微前端和微件本质上有不同。
这是第一部分微前端们，第二部分是基座，也拿cim-map来举例的话，就是一个空的页面，只有一个纯净的底图，或者底图都没有，它只是充当的一个容器，它的职责应该做好一些系统最基本的功能，比如布局、路由、权限...然后暴露出很多的插槽给我们的微前端们。
这是第二部分，那么第三部分就是我们的框架，它的作用是用连接我们的基座和微前端，它要来管理我们的基座加载或者是卸载我们的微前端。
-->

---
transition: slide-left
---

## 微前端决策框架

框架的 4 个关键性决策：
|定义|组合/路由|通信|
| --- | --- | --- |
| <font color='blue'>横向拆分</font> <br/> <font color='gray'>纵向拆分</font> | <font color='blue'>客户端</font> <br/> <font color='gray'>服务器端</font> <br/><font color='gray'>边缘侧</font> | <font color='blue'>事件触发器</font> <br/> <font color='blue'>自定义事件</font> <br/><font color='blue'>Web Storage</font> <br/><font color='blue'>查询字符串</font>

<!--
我们首先来讨论框架，框架有4个关键性的决策，分别是定义、组合、路由和通信。
第一个定义，主要指的是拆分方式。拆分方式分两种，横向和纵向。横向拆分就是同一个视图中集成多个微前端；纵向拆分就是每个视图中只集成一个微前端。我们直接看一张示意图吧。
地图应用就非常适合横向拆分，而门户和运维管理则更适合纵向拆分。
第二和第三是组合和路由，就是指的在什么地方去组合和路由我们的微前端。有三种方式，分别是客户端、服务器端和边缘侧。直接在前端控制微服务的加载和路由，那么就是客户端。如果是在后端先组合好微前端，再一并推送到前端，这种方式就是服务器端。还有一种方式，就是在CDN层，由CND厂商提供方式进行微前端的组装，叫边缘侧。
最后是通信，微前端的通信方式主要有事件触发器、自定义事件、WebStorage、查询字符串四种。
-->

---
transition: slide-left
---

## 微前端技术实现

步骤：
1. 新建一个host应用，在host应用中写一个基本的地图
2. 新建一个remote应用，叫sketch，在remote/sketch中实现基本地图和一个Sketch微件
3. 配置host和remote的webpack5，使得host应用中引入remote/sketch
4. 再新建一个remote应用，叫panel，在remote/panel中实现一个布局，然后被host引入

<!--
我们直接来看一个简单的demo。
-->

---
transition: slide-left
---

## 构建和部署微前端

待研究...

<!--
构建和部署还待研究，就先不讲了。
-->

---
transition: slide-left
---

## 在组织中引入微前端？

<br/>
<div class="flex">
  <div class="w-1/2 p-4">
    好处：
    <ul>
    <li> 专注的开发环境 / 纯净的测试环境 </li>
    <li> 完善的代码组织结构 </li>
    <li> 渐进式的功能集成 </li>
    <li> ... </li>
    </ul>
  </div>
  <div class="w-1/2 p-4">
    坏处：
    <ul>
    <li> 糟糕的开发体验（由于JSAPI） </li>
    <li> GIS领域没有成熟的微前端方案 </li>
    <li> ... </li>
    <br/>
    </ul>
    满足什么条件才适合引入微前端：
    <ul>
    <li> ... </li>
    </ul>
  </div>
</div>

<!--
我们现在回到最开始的问题，来讨论是否要选择微前端这种架构风格。
从我实践这个demo看来，它有一些好处，如专注的开发环境 / 纯净的测试环境 、完善的代码组织结构和渐进式的功能集成。当然它有也一些坏处，如糟糕的开发体验，没有成熟的方案等。
我觉得我们可以等条件更加成熟后，再来尝试引入微前端，这是我调研的一个结论。
-->

---

## 参考资料

* 书籍：[《微前端设计与实现》](https://book.douban.com/subject/36014313/)
* 博客：[Module Federation in webpack](https://odoe.net/blog/webpack-module-federation)
* 视频：[Micro-Frontends in Just 10 Minutes](https://www.youtube.com/watch?v=s_Fs4AXsTnA&t=2s)
* 社区：[How to architect a micro-frontends project under ArcGIS JS API?](https://community.esri.com/t5/arcgis-javascript-maps-sdk-questions/how-to-architect-a-micro-frontends-project-under/m-p/1277816#M80846)
* 官网：[webpack5](https://webpack.js.org/concepts/module-federation/#dynamic-remote-containers) & [ArcGIS JS API](https://developers.arcgis.com/javascript/latest/es-modules/)

<!--
最后是一些参考资料。我是从3月份中旬开始准备分享材料。然后项目也比较忙。所以研究得很浅。主要参考的是书籍，2022年下半年才出的书，还算比较新。ppt的大纲就是直接抄的书。
然后是JSAPI的主要代码贡献者的博客，和esri的社区。我去上面提问，看有没有成熟的，优雅的方案，结果是没有的。
最后是反复看webpack5和JSAPI的官网文档。
-->
