---
description: 百度小程序SDK目前属于灰度内测功能，如有需求，请联系您的客户成功经理或商务对接人，申请进行灰度内测。
---

# 百度小程序 SDK

## 百度小程序SDK标准接入指南

进入集成页面，选择百度小程序，填写应用名称和 AppID，点击「下一步」

![](../../.gitbook/assets/image%20%28257%29.png)

### 1 下载百度小程序 SDK

下载 gio-baidup.js 文件

```text
curl --compressed https://assets.giocdn.com/gio-baidup.js -o gio-baidup.js
```

当下载到 gio-baidup.js 文件以后，把文件放在百度小程序项目里，比如 utils 目录下。下面会假设 SDK 文件放在 utils 目录下。

### 2 添加跟踪代码

在百度小程序项目根目录的 app.js 文件的顶部添加以下 JS 代码：

```text
var gio = require("utils/gio-baidup.js");
// version 是你的小程序的版本号
gio('init', '你的项目 ID', '你的百度小程序 AppID', { version: '1.0' });
```

建议每次发布小程序新版本的时候，更新一下版本号 version，可以在 GrowingIO 分析不同版本的数据。除了 version 之外，还有以下额外参数可以使用。  


| 参数 | 值 | 解释 |
| :--- | :--- | :--- |
| forceLogin | true \| false | 您的小程序是否强制要求用户登陆百度获取 swanid，默认 false |
| debug | true \| false | 是否开启调试模式，可以看到采集的数据，默认 false |
| version | string | 您的小程序的版本号 |
| followShare | true \| false | 详细跟踪分享数据，开启后可使用分享分析功能。默认 false |

forceLogin 是一个需要特别注意的参数。GrowingIO 默认会在小程序里面设置用户标识符，存储在百度 Storage 里面。这个用户标识符潜在可能会被 `clearStorage` 清除掉，所以有可能不同的用户标识符对应同一个百度里的 swanid。如果建议您获取用户的`swanid` ，然后设置 `forceLogin` 为 true。当 forceLogin 为 true 的时候，用户标识符会使用 swanid，潜在风险是如果没有设置 swanid，数据不会发送，**所以请特别注意这个参数的设置**，具体集成示例：  


```text
gio('init', '你的项目 ID', '你的百度小程序 AppID', { version: '1.0', forceLogin: true });
...
// 当获取到 swanid 后，调用以下方法
gio("identify", swanid);
```

### 3 添加请求服务器域名

要正常采集百度小程序的数据并发送给 GrowingIO，需要在百度小程序里事先设置一个通讯域名，允许跟 GrowingIO API 服务器进行网络通信。具体步骤如下：

1. 登陆百度小程序后台，进入配置
2. 打开开发设置，到服务器域名配置部分
3. 在request合法域名中添加：https://wxapi.growingio.com

![&#x5C0F;&#x7A0B;&#x5E8F;&#x5C55;&#x793A;](https://assets.giocdn.com/webapp/20190827/img-Gj32BF0.png)4添加分享参数

转发分享小程序是小程序获客的重要场景，想要详细的进行转发分享的统计，需要在SDK参数中，设置如下参数，值为true  


| 参数 | 值 | 解释 |
| :--- | :--- | :--- |
| followShare | true \| false | 详细跟踪分享数据，开启后可使用分享分析功能。默认 false |

即百度小程序项目根目录的 app.js 文件设置参数如下：

```text
var gio = require("utils/gio-baidup.js");
// version 是你的小程序的版本号，发版时请调整
gio('init', '你的项目 ID', '你的百度小程序 AppID', { version: '1.0', followShare: true });
```

### 4 SDK 百度用户属性设置

作为用户行为数据分析工具，用户信息的完善会给后续的分析带来很大的帮助。在小程序中，百度用户属性是非常重要的设置，只有完善了百度用户属性信息，百度的访问用户变量（如下表）才可以在分析工具中使用，交互数据定义、数据校验功能才会方便通过用百度相关的信息（百度姓名和头像）定位用户。  


下面是专门针对用户的三个接口。  


#### 绑定百度用户ID

当用户在你的小程序上登陆获取到 swanid 后，可以用过 identify 接口绑定百度用户ID，后续在 GrowingIO 中获取更准确的百度访问用户量。示例代码如下，  
`swan.getSwanId({ success: (res) => { gio('identify', res.data.swanid) } });`  


#### 设置百度用户信息

当用户在你的小程序上绑定百度信息后，可以通过 setVisitor 接口设置百度用户信息，后续在 GrowingIO 中分析这个数据。示例代码如下，  
`swan..getUserInfo({ success: function(res) { ... gio('setVisitor', res.userInfo); } });`  


百度信息包含**用户昵称**、**用户头像**、**性别**。  


\*注：用户画像中的部分数据，只有在设置百度用户信息后，才可以统计。  


#### 设置注册用户ID

当用户在你的小程序上注册以后，你的产品应用服务端会在用户数据库里添加一条记录并且分配一个 ID，可以通过 setUserId 接口设置注册用户ID，后续在 GrowingIO 中分析登录用户这个数据。示例代码如下，  
`gio('setUserId', YOUR_USER_ID);`  


#### 设置注册用户信息

当用户在你的小程序上传了注册用户ID后，可以通过 setUser 接口设置注册用户信息，后续在 GrowingIO 中分析这个数据。示例代码如下，  
 `gio('setUserId', user.id); gio('setUser', { id: user.id, name: user.name });`  


### 5 检测数据 <a id="jian-ce-shu-ju"></a>

当集成成功后，需要回到 GrowingIO SDK 集成页面，点击右下角“**检测数据”**。请在添加了跟踪代码的百度小程序重新启动几次，发送数据给 GrowingIO，完成安装最后一步。

![](../../.gitbook/assets/image%20%28339%29.png)

![](../../.gitbook/assets/image%20%28186%29.png)



### 6 进入数据校验模块，查看数据发送情况

检测成功后，可以使用[数据验证功能](../growingio-debugger/#growingio-minidebugger)，实时查看数据发送情况。

## 自定义事件和变量

### 自定义事件配置

手动发送一个自定义事件。在添加所需要发送的事件代码之前，需要在 GrowingIO 产品的`自定义事件和变量`管理页面配置事件以及事件级变量。

接口定义：

```text
gio('track', eventName: string, properties: object)
```

参数说明：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| eventName | string | 是 | 事件标识符 |
| properties | object | 否 | 事件级变量，即事件发生时所伴随的维度信息参数 |

示例：

```text
// 假设初始化后把 gio 对象放在 App 的 globalData 里面
// 在 Page 的 clickBanner 函数里添加以下代码
Page({
  clickBanner(e) {
    getApp().globalData.gio('track', 'clickBanner', { 
      id: movie.id, 
      title: movie.title, 
      index: e.currentTarget.dataset.index 
    });
  }
})
```

### 访问用户变量

给访问用户\(未注册你的服务的账号的用户\)附上额外的信息，便于后续做用户信息相关分析。在添加所需要设置的访问用户变量的代码之前，需要在 GrowingIO 产品的`自定义事件和变量`管理页面配置访问用户级变量。

接口定义：

```text
gio('setVisitor', properties: object)
```

参数说明：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| properties | object | 是 | 用户级变量，用户额外的信息参数 |

示例：

```text
// 假设初始化后把 gio 对象放在 App 的 globalData 里面
// 比如在针对不同的用户做某个 Campaign 的 A/B 测试
getApp().globalData.gio('setVisitor', { 
  campaign_id: 3, 
  campaign_group: 'A 组用户'
});
```

### 注册用户变量

给注册用户（crm 用户ID）附上额外的信息，便于后续做用户信息相关分析。在添加所需要设置的注册用户变量的代码之前，需要在 GrowingIO 产品的`自定义事件和变量`管理页面配置注册用户级变量。

接口定义：

```text
gio('setUser', properties: object)
```

参数说明：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| properties | object | 是 | 用户级变量，用户额外的信息参数 |

示例：

```text
// 假设初始化后把 gio 对象放在 App 的 globalData 里面
getApp().globalData.gio('setUser', { 
  age: 30, 
  level: '高级用户', 
  company: 'GrowingIO', 
  title: '工程师'
});
```

### 页面级变量

给当前页面附上更多的页面信息，可以作为维度拆分数据做分析。设置了页面级变量以后，这个页面的指标以及这个页面的行为指标，都可以继承使用这些维度信息做分析。在添加所需要设置的页面变量的代码之前，需要在 GrowingIO 产品的`自定义事件和变量`管理页面配置页面级变量。

接口定义：

```text
gio('setPage', properties: object)
```

参数说明：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| properties | object | 是 | 页面级变量，页面额外的信息参数 |

示例：

```text
// 假设初始化后把 gio 对象放在 App 的 globalData 里面
// 推荐在 Page#onShow 处理这个事件
// 下面假设我在 GrowingIO 后台已经配置了两个页面级变量 pageName 和 type
Page({
  onShow() {
    getApp().globalData.gio('setPage', { 
      pageName: '电影列表页', 
      type: this.data.type
    });
  }
}
```

###  转化变量

高级功能，设置一个转化信息用于高级归因分析，目前支持归因方式有最初归因、最终归因和线下归因。举个例子，如果一个用户是先后通过`活动A`、`活动B`、`活动C`来访问小程序，最后在某次后续几天后的访问购买了某个商品。如果把活动A/B/C分别设置为转化变量`campaign`的值，那么如果采用了最初归因，那么这个购买行为是由 A 贡献的；如果是最终归因，那么这次购买行为是 C 贡献的；如果是线性归因，那么这次购买行为是 A/B/C 各占 1/3 贡献。在添加所需要设置的转化变量的代码之前，需要在 GrowingIO 产品的`自定义事件和变量`管理页面配置转化级变量。

接口定义：

```text
gio('setEvar', properties: object)
```

参数说明：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| properties | object | 是 | 转化级变量，转化信息 |

示例：

```text
// 假设初始化后把 gio 对象放在 App 的 globalData 里面
getApp().globalData.gio('setEvar', { 
  campaign: '活动A'
});
```





