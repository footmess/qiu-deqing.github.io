---
title: 无线web页面开发调试
---

# [Weinre][3]远程调试

WEb Inspector REmote使用[webkit web inspector项目][2]提供的代码在电脑端浏览器为无线端页面提供开发工具. UC, 手淘, 微信页面调试都能够支持.

[3]: https://people.apache.org/~pmuellr/weinre/docs/1.x/1.5.0/
[2]: http://trac.webkit.org/wiki/WebInspector

## android手机使用chrome devTool远程调试页面

[原文连接][1]

工具:

- PC端安装Chrome版本>=32
- 数据线
- 安装chrome浏览器的Android4.0+手机


### 设置手机

1. 在手机中找到: **设置 -> 开发者选项**. (从Android4.2开始开发者选项默认隐藏, 需要在**设置 -> 关于手机**下找到**内部版本号**敲击七次)
2. 在开发者选项中选中**USB调试**, 点击确认框的**OK**
3. 用数据线连接手机之后, PC Chrome浏览器访问`chrome://inspect`, 下面会列出连接到电脑的手机以及手机浏览器上打开的页面
4. 在手机弹出的询问窗口中同意USB调试
5. 在PC浏览器下看到的操作界面中选择手机端打开页面,**inspect** 会使用PC Chrome devtool打开无线端页面



[1]: https://developers.google.com/web/tools/chrome-devtools/debug/remote-debugging/remote-debugging
