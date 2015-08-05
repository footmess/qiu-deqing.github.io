---
title: "[译]使用chrome dev tool评估网络性能"
---

原文出处:[chrome devtools doc: Evaluating network performance][3]

Network面板记录程序的每一个网络操作信息,包括时间信息, HTTP请求,响应头, cookie, WebSocket数据等.
Network面板能够解答web app的网络性能问题, 如:

- 哪个资源最晚开始接收
- 那个资源加载消耗最多时间
- 某个网络请求是谁发起的
- 在不同网络情况下某个资源加载需要多少时间

## 关于Resource Timing API

网络面板使用[Resource Timing API]来提供网络时序的详细信息. 例如这个API可以告诉你HTTP请求一张图片开始的准确时间, 以及这张图片完全接收到的时间. 下图展示API所能提供的网络时间信息.

![][2]

这个API在所有网页中都存在, 不仅仅是DevTools下. 在Chrome下, 暴露在`window.performance`下.`performance.getEntries()`返回包含页面所有资源请求信息的"resource timing object"数组.




[3]: https://developer.chrome.com/devtools/docs/network
[2]: https://cloud.githubusercontent.com/assets/5894015/9089192/7979aa6a-3bc6-11e5-823e-685ca0c84b78.png
[1]: http://www.w3.org/TR/resource-timing

