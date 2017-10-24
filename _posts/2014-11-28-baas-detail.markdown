---
layout: post
title:  了解Baas（Backend as a Service）
date:   2014-11-28 01:40
category: "Cloud Platform"
---

<h3>了解了解Baas</h3>
<hr />

作为开发者，了解新技术，新的工具是必不可少的，有时候发现一个好的开发工具，使用起来非常顺手，效率也提高了，
然而如果不了解新的技术，不了解这个行业的发展趋势，那样的后果是不堪回首的，反正，与时俱进，与时俱变是没有错啦。

<h4>本文主要有以下几个步骤：</h4>

- <a href="#what-is-baas">什么是Baas 呢?</a>

- <a href="#baas-service">搭建Android 开发环境；</a>

- <a href="#baas-service-company">了解Android 运行原理以及一个工程项目结构；</a>

<br />

<h3 id="what-is-baas">什么是Baas 呢？</h3>
<hr />

前一段时间在知乎上看到AVOSCloud（现在改名叫LeanCloud 了），不知道这是什么东西，偶尔在微博看到有人说在用，
昨天有无意间看到了，就索性把这个弄清楚了，这个东西叫Baas（又称mBaas），全称叫Backend as a Service（后端作为一种服务），
怎么说呢，为移动应用开发者提供后端服务；
作为一个移动应用开发者（Android & iPhone），除了开发应用程序，还要做后台的服务器端，当然，大多移动应用开发者并非都懂后端开发，
这让一个应用应用开发者开发一款应用不容易，然而有了Baas，开发者就可以不用管后端开发了，Baas 提供商，提供了API 我们可以调用，
把主要精力放在应用本身，大大提高了效率；
当然也不一定是移动应用开发者，当然也可以Web 开发，当初Baas 主要是为了解决移动应用开发者的后端服务问题，Baas 也提供其他的服务；

<br />

<h3 id="baas-service">那么Baas 都提供哪些服务呢？</h3>
<hr />

以[Parse](https://www.parse.com/) 为例，主要有以下功能：<br />

- 消息推送

- 数据分析

- 保存数据到云平台

- 简单的社交服务

- 本地数据储存

- 在云服务上运行客户端代码

- 后台工作

- 实时查看应用程序的数据，数据推送，日志等

<br />

其实可以吐槽一下，其实实现的功能比较基础的，一般有后端开发经验的开发者，也不是什么难事，但是自己要搭建服务器，还要一系列的服务要实现，
也是费时费工，有这样的服务，提供的简单的API，使用起来上手非常容易，也提供了一些 Demo，只要自己在服务后台创建应用，申请API_ID，API_KEY，即可

<br />

<h3 id="baas-service-company">Baas 服务商都有哪些呢？</h3>
<hr />

![](http://optimusmobile.wpengine.netdna-cdn.com/wp-content/uploads/2013/04/top_5_mobile_back-end-providers.png)

 <h4> <a href="http://www.appcelerator.com/cloud/)">Appcelerator Cloud</a> </h4>
<hr />

它要依赖Appcelerator Titanium SDK 一起使用，官方宣称目前共有265728526 设备运行着Appcelerator Cloud 服务提供的应用程序，客户不乏有Ebay，vmWare；

<br />

 <h4> <a href="https://www.parse.com/">Parse</a> </h4>
<hr />
也算是特别出名的，刚刚介绍Baas 的时候，就拿它当例子，他的出名更归功于跟了Facebook；

<br />

 <h4> <a href="http://www.kinvey.com/">Kinvey</a> </h4>
<hr />

<br />

<h4> <a href="https://www.stackmob.com/">StackMob</a> </h4>
<hr />

<br />

 <h4> <a href="http://www.applicasa.com/">Applicasa</a> </h4>
<hr />

<br />

 <h4> <a href="http://usergrid.incubator.apache.org/">UserGrid</a> </h4>
<hr />
Apache 提供了一个Baas 框架，支持多平台，

<br />

 <h4> <a href="https://leancloud.cn/">LeanCloud（AVOSCloud）</a> </h4>
<hr />
和Facebook Parse 提供的重合度很高， <br />

- 社交IM 服务

- 消息推送服务

- 数据分析

- 支持任何类型的结构化 / 半结构化数据，还有地理位置存储

- 并且提供CDN 存储

- 云代码

- 集成GitHub、Bitbucket 或 CSDNCode

- 对每一个应用，每个月免费提供 500 万次的请求配额

<br />

![](http://deveconomics.wpengine.netdna-cdn.com/wp-content/uploads/2013/05/BaaS-Shootout.png)

<br />

<hr />
> 本文作者：[Tikitoo](http://weibo.com/lyf0306) <br />
> 本文出处：[http://tiktioo.github.io/blog/comcloud platform/2014/11/28/baas-detail.html](http://tiktioo.github.io/blog/comcloud platform/2014/11/28/baas-detail.html) <br />
> 转载请在开头注明作者[详细信息](http://weibo.com/lyf0306)和[本文出处](http://tikitoo.github.io/blog)。


