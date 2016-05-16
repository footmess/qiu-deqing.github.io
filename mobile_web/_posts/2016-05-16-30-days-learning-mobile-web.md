---
title: 无线端web开发学习
---

记录30天无线web学习历程


# 第一天2016-05-16

谷歌搜索: **mobile web development**


[Fundamental of Mobile Web Development][1]: 一系列web开发资源

## [fundamentals/getting-started/your-first-progressive-web-app][2]

[Progress Web App][3]特点:

- Progresssive: 适合所有用户, 核心思想是渐进增强功能
- Responsive: 适配所有设备: PC, 手机, 平板
- Connectivity independent: 在慢速或者离线环境下使用**service workers**增强体验
- App-like: 建立在**app shell model**上以提供app一样的交互, 导航
- safe: HTTPS防止泄密
- Discoverable: W3C manifests和`service worker registration scope`使应用可以被搜索引擎发现并且识别为app
- re-engageable: 消息推送等功能使得re-engagement变得容易
- installable: 允许用户将快捷方式保存到桌面(不需要通过app store下载)
- Linkable: 通过URL就可以分享, 不需要安装

本教程使用Progressive Web App技术创建一个天气预报app

## 1. [Architect the App Shell][4]

app的内核指app所需的最核心的HTML, CSS和JavaScript, 它是确保app高性能的起点. 它加载起来非常快并且马上被缓存, 不需要每次都加载,
只需要在使用的时候按需加载内容.

App内核架构将app核心, UI与数据分离开. 所有的UI和基础资源都讲通过service worker缓存到本地, app后续使用只需要获取相关数据即可.

App内核架构模式让你更好的关注性能, 瞬时加载, 方便更新

## 设计app内核

首先需要将系统拆分为核心组件, 需要考虑一下问题:

1. 什么需要立刻展现在屏幕上
2. 哪些是app的关键UI组件
3. app内核需要哪些资源(图片, js, css等)

我们创建天气预报app, 核心组件包含以下内容:

-

[4]: https://developers.google.com/web/fundamentals/getting-started/your-first-progressive-web-app/step-01?hl=en
[3]: https://developers.google.com/web/progressive-web-apps
[2]: https://developers.google.com/web/fundamentals/getting-started/your-first-progressive-web-app/
[1]: https://developers.google.com/web/updates/2014/12/Fundamental-of-Mobile-Web-Development?hl=en
