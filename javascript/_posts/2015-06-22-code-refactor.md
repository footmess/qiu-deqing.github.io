---
title: 代码重构
---

不注意代码结构，代码难以理解，修改，维护，结合最近一个需求进行总结。

## 需求介绍

![][1]

上面是店铺活动页的礼盒，打开礼盒的逻辑如下(假设用户已登陆)：

- 检查是否已经收藏店铺
  + 如果用户已收藏，请求开礼盒接口
  + 如果用户未收藏，询问是否收藏，如果用户选择收藏，请求收藏店铺接口
    * 如果收藏成功，请求开礼盒接口
    * 如果收藏失败，显示失败原因
- 请求开礼盒会返回不同礼品，需根据不同礼品显示不同反馈


现有后端接口如下：

### 收藏店铺

url: `collectStore.json`

返回字段说明：

```
{
  info: {
    ok: true,
    code: 'E001',
    msg: 'message'
  },
  data: {}
}
```

可能返回的结果

|    code   |   msg    |  说明     |
|-----------|----------|----------|
|  E04      |  timeout | 超时      |
|  S01      |  sucess  | 收藏成功   |
|  E01      |  fail    | 收藏失败   |
|  E03      |  notlogin| 用户未登录 |

### 检查店铺收藏接口

url: `checkCollect.json`

返回字段说明：

```
{
  info: {
    ok: true,
    code: 'E001',
    msg: 'message'
  },
  data: {}
}
```

结果说明：


|    code   |   msg    |  说明     |
|-----------|----------|----------|
|  E04      |  timeout | 超时      |
|  E01      |  not exist  | 未收藏   |
|  E02      |  exist    | 已收藏   |
|  E03      |  notlogin| 用户未登录 |

### 领取礼盒

url: `opengift.json`


返回字段说明：

```
{
  info: {
    ok: true,
    code: 'E001',
    msg: 'message'
  },
  data: {
    awardName: '奖品1'
  }
}
```

结果说明：


|    code   |   msg    |  说明     |
|-----------|----------|----------|
|  E04      |  timeout | 超时      |
|  S01      |  success  | 获得奖品   |
|  E02      |  opened    | 已领取   |
|  E03      |  notlogin| 用户未登录 |


## 最直接的方案：

html:

```
<div class="gift"></div>
```

javascript(使用jQuery):

```
$('.gift').on('click', function (e) {
  $.ajax({
    url: 'checkCollect.json' // 检查是否收藏
  })
  .done(function (data) {
    switch (data.info.code) {
      case 'E04':
        console.log('timeout');
        break;

      // 未收藏，进行收藏
      case 'E01':
        $.ajax({
          url: 'collectStore.json'
        })
        .done(function (data) {
          switch (data.info.code) {
            case: 'E04':
              console.log('time out');
              break;

            case 'S01':
              $.ajax({
                url: 'opengift.json'
              })
              .done(function (data) {
                switch (data.info.code) {
                  case: 'E04':
                    console.log('time out');
                    break;

                  case: 'S01':
                    console.log(data.data.awardName);
                    break;

                  case: 'E02':
                    console.log('opened');
                    break;

                  case: 'E03':
                    console.log('not login');
                    break;
                }
              });
              break;

            case 'E01':
              console.log('collect fail');
              break;

            case 'E03':
              console.log('not login');
              break;
          }
        });
        break;

      // 已收藏，开礼盒
      case 'E02':
        $.ajax({
          url: 'opengift.json'
        })
        .done(function (data) {

          switch (data.info.code) {
            case 'E04':
              console.log('timeout');
              break;

            case 'S01':
              console.log(data.data.awardName);
              break;

            case 'E02':
              console.log('opened');
              break;

            case 'E03':
              console.log('not login');
              break;
          }
        });
        break;

      case 'E03':
        console.log('not login');
        break;
    }
  });
});
```

上面的代码把所有工作都放到了`click`监听器中执行，单一满足现在的需求还勉强能接受。但是存在一些问题：

- 打开礼盒这个逻辑在两个地方重复出现，如果第三个地方出现这个功能，那必须重写一遍，假设有10个地方用到，就需要复制10遍。第二天客户说要增加一种礼盒开奖结果。那么需要修改10个地方
- 所有的ajax使用switch处理不同情况，然后再嵌套ajax， switch很容易产生混乱，code的不同编码没有包含任何寓意，需要多处添加注释，否则需要回去查阅API文档
- 如果开礼盒引入积分规则，收藏之后需要判断积分是否足够才能开礼盒，需要修改的地方就更多了
- 如果页面其他地方需要使用收藏，检测收藏，开礼盒，代码需要重复复制
- 如果接口升级需要更换地址，需要到代码中到处搜索替换


目前简单想到的就是上面这些问题，所以在一开始的时候保持一个良好的代码结构能够让后续维护更加容易。下面是目前个人水平下能够想到的一些重构方法：

- 根据功能划分不同模块，如`util`，`ctrl`, `app`
- 将不同功能封装到对应模块

下面是修改之后的逻辑：

```
// 配置参数
var config = {
  // 接口地址
  api: {
    collectStore: 'collectStore.json',
    checkCollect: 'checkCollect.json',
    openGift: 'opengift.json'
  }
};

// 封装基础操作
var util = {};

// 空函数，默认参数常用
util.noop = function () {};


/**
 * 检查当前用户是否收藏，然后执行对应操作
 * arg {Object} 配置回调，内容见defaults
 **/
util.checkCollect = function (arg) {
  var opt = $.extend({}, util.checkCollect.defaults, arg);

  $.ajax({
    url: config.api.checkCollect,
    data: opt.data
  })
  .done(function (data) {
    switch (data.info.code) {
      case 'E04':
        opt.onTimeout(data);
        break;

      case 'E01':
        opt.onNotCollect(data);
        break;

      case 'E02':
        opt.onCollected(data);
        break;

      case 'E03':
        opt.onNotLogin(data);
        break;
    }
  });
};
util.checkCollect.defaults = {
  data: {},   // 请求接口时带的参数
  onTimeout: util.noop,       // 超时回调
  onNotCollect: util.noop,    // 没有收藏
  onCollected: util.noop,     // 已收藏
  onNotLogin: util.noop       // 未登录
};


/**
 * 执行收藏操作，调用对应回调
 * arg {Object} 设置回调，内容见defaults
 **/
util.collect = function (arg)) {
  var opt = $.extend({}, util.collect.defaults, arg);

  $.ajax({
    url: config.api.collectStore,
    data: opt.data
  })
  .done(function (data) {
    switch (data.info.code) {
      case 'E04';
        opt.onTimeout(data);
        break;

      case 'S01':
        opt.onSuccess(data);
        break;

      case 'E01':
        opt.onFail(data);
        break;

      case 'E03':
        opt.onNotLogin(data);
        break;
    }
  });
};
util.collect.defaults = {
  data: {},
  onTimeout: util.noop,   // 超时
  onSuccess: util.noop,   // 收藏成功
  onFail: util.noop,      // 收藏失败
  onNotLogin: util.noop   // 未登录
};

/**
 * 打开礼盒
 * arg {Object} 设置回调和参数
 **/
util.openGift = function (arg) {
  var opt = $.extend({}, util.openGift.defaults, arg);

  $.ajax({
    url: config.api.openGift,
    data: opt.data
  })
  .done(function (data) {
    switch (data.info.code) {
      case 'E04':
        opt.onTimeout(data);
        break;

      case 'S01':
        opt.onSuccess(data);
        break;

      case 'E02':
        opt.onOpened(data);
        break;

      case 'E03':
        opt.onNotLogin(data);
        break;
    }
  });
};
util.openGift.defaults = {
  data: {},
  onTimeout: util.noop,   // 超时
  onSuccess: util.noop,   // 获得奖品
  onOpened: util.noop,    // 已领取
  onNotLogin: util.noop   // 未登录
};


var ctrl = {};

var app = {};

app.init = function () {
  $('.gift').on('click', function (e) {
    util.checkCollect({
      onNotCollect: function () {
        util.collect({
          onSuccess: function () {
            util.openGift({
              onSuccess: function (data) {
                console.log(data.data.awardName);
              }
            });
          }
        });
      },

      onCollect: function () {
        util.openGift({
          onSuccess: function (data) {
            console.log(data.data.awardName);
          }
        });
      }
    });
  });
};
```

上面的代码将基本的ajax请求根据接口逻辑进行了一层封装，提高代码复用度，结构也更清晰。基本上可以满足需求了。

但是设置了大量异步回调似乎还有可以优化的地方。

## EventEmitter增加逻辑事件

EventEmitter使用还不是很熟悉，之后会使用进来进一步优化


[1]: http://7xio0w.com1.z0.glb.clouddn.com/QQ20150622-2@2x.png
