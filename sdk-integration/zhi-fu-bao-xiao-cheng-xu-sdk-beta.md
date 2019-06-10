---
description: 支付宝小程序SDK目前属于灰度内测功能，如有需求，请联系您的客户成功经理或商务对接人，申请进行灰度内测。
---

# 支付宝小程序 SDK （beta\)

## 接入前准备

### 在项目中接入一个新的小程序应用

在你的 GrowingIO 项目页面点击右上角点击小齿轮，在弹出的列表中选择“应用管理“。在应用管理页面，点击“**新建应用**“来创建一个新应用。

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LH3J3bJm8y879FZ1E7G%2F-LH3KSslW7JKLy4bb98K%2Fimage.png?alt=media&token=04f949af-bbb1-49f5-98e2-c212022a9c1d)

然后会到达SDK集成页面，**平台**选择支付宝小程序，填写支付宝小程序的应用名称，和支付宝小程序的AppID ,点击“下一步”，即可以到达支付宝小程序SDK接入的页面。

![](../.gitbook/assets/image%20%28231%29.png)

目前仅支持小程序类应用集成在同一个项目下，暂不支持小程序产品和web\iOS\Android端产品集成在同一项目下。

如果需要集成其他端产品，或者需要在已有web/iOS/Andoird产品集成的情况下，集成小程序产品，请创建新的项目。

## 支付宝小程序SDK标准接入指南

选择”支付宝小程序“平台，填写”应用名称“ 和 "AppID"，点击"**下一步**"。

![](../.gitbook/assets/image%20%28140%29.png)

### 1、根据支付宝小程序框架选择SDK文件并添加跟踪代码

参照小程序的开发框架，下载相应的SDK，并添加跟踪代码。

支付宝小程序原生框架

* 支付宝小程序原生框架
* 支付宝小程序 Taro 框架

#### 支付宝小程序原生框架

1 根据下载小程序采集SDK

下载 gio-alip.js 文件

```text
curl --compressed https://assets.growingio.com/gio-alip.js -o gio-alip.js
```

当下载到 gio-alip.js 文件以后，把文件放在支付宝小程序项目里，比如 utils 目录下。下面会假设 SDK 文件放在 utils 目录下。

2 添加跟踪代码

在支付宝小程序项目根目录的 app.js 文件的顶部添加以下 JS 代码，请注意一定要放在 App\(\) 之前：

```javascript
var gio = require("utils/gio-alip.js");
// version 是你的小程序的版本号
gio('init', '你的项目ID', '你的支付宝小程序AppID', { version: '1.0' });
修改项目中的App和Page，如下：
App({})改写为:
App($global.trackApp({
}))
所有的Page({})改写为：
$global.GioPage({
})
```

支付宝小程序 Taro 框架

下载 gio-alip.js 文件

```text
curl --compressed https://assets.growingio.com/gio-alip.js -o gio-alip.js
```

当下载到 gio-alip.js 文件以后，把文件放在支付宝小程序项目里，比如 utils 目录下。下面会假设 SDK 文件放在 utils 目录下。

2 添加跟踪代码

在支付宝小程序项目根目录的 app.js 文件的顶部添加以下 JS 代码，请注意一定要放在 App\(\) 之前：

```javascript
import Taro from '@tarojs/taro'
import gio from './utils/gio-alip'
// version 是你的小程序的版本号
gio('init', '你的项目ID', '你的支付宝小程序AppID', { version: '1.0', taro: Taro });
```

### **2、进行SDK的配置设置**

每次发布小程序新版本的时候，更新一下版本号 **version**，可以在 GrowingIO 分析不同版本的数据。除了 version 之外，还有以下额外参数可以使用。

| 参数 | 值 | 解释 |
| :--- | :--- | :--- |
| getLocation | true \| false | 是否自动获取用户的地理位置信息。默认false |
| forceLogin | true \| false | 是否要用用户登陆支付宝获取 userid作为访问用户ID标识采集，建议你的小程序在打开强制要求用户登陆支付宝获取 userid时，才进行开启。默认 false |
| debug | true \| false | 是否开启调试模式，可以在调试体验版时看到采集的数据，默认 false |
| version | string | 你的小程序的版本号 |
| followShare | true \| false | 详细跟踪分享数据，开启后可使用分享分析功能统计分享带来的用户量。默认 false |

#### followShare参数

转发分享小程序是小程序获客的重要场景，想要详细的进行转发分享的统计，需要在SDK参数中，设置如下参数，值为true

| 参数 | 值 | 解释 |
| :--- | :--- | :--- |
| followShare | true \| false | 详细跟踪分享数据，开启后可使用分享分析功能统计分享带来的用户量。默认 false |

即支付宝小程序项目根目录的 app.js 文件设置参数如下：

```text
var gio = require("utils/gio-alip.js");
// version 是你的小程序的版本号，发版时请调整
gio('init', '你的项目ID', '你的支付宝小程序AppID', { version: '1.0', followShare: true });
```

#### getLocation 参数

根据微信最新的用户地理位置获取的规则，GrowingIO 小程序SDK 默认不会在小程序启动时获取用户的坐标信息。 

* 如果您的小程序在打开时就需要获取用户地理信息，就可以将这个参数配置为true。
* 如果您的小程序在用户点击某些按钮时，才触发获取位置，则可以按照配置方式，进行[用户位置的补发](wei-xin-xiao-cheng-xu-sdk/mina-sdk/#huo-qu-yong-hu-de-di-li-xin-xi)，从而增强用户地理位置的分析能力。

| 参数 | 值 | 解释 |
| :--- | :--- | :--- |
| getLocation | true \| false | 是否自动获取用户的地理位置信息。默认false |

GrowingIO SDK 默认不会在小程序启动时获取用户的坐标信息。当用户访问到某一功能时需要位置信息时，可以调用以下位置接口，补发vst，采集位置信息，提升用户地域分布的分析准确性。

```text
gio('getLocation')
```

#### forceLogin 参数

设置forceLogin参数后，访问用户ID会被支付宝userid替换，但是风险是，如果小程序打开时并不要求立即授权上报支付宝userid，则在上报支付宝userid前的操作数据不会发送；如果在上报支付宝userid前，用户就退出了小程序，用户数据不会上报。

{% hint style="warning" %}
forceLogin 是一个需要特别注意的参数。GrowingIO 默认会在小程序里面设置用户标识符，存储在 Storage 里面。这个用户标识符潜在可能会被 `clearStorage`清除掉，所以有可能不同的用户标识符对应同一个支付宝用户的 userid。如果你的支付宝小程序在用户打开后会去做登陆并且获取 `userid` ，可以设置 `forceLogin` 为 true。当 forceLogin 为 true 的时候，用户标识符会使用 userid，潜在风险是如果用户没有授权，数据不会发送，**所以请特别注意这个参数的设置**，具体集成示例：



```javascript
gio('init', '你的项目ID', '你的支付宝小程序AppID', { version: '1.0', forceLogin: true });
...
// 当获取到 userid 后，调用以下方法
gio("identify", userid);
```
{% endhint %}

### 3 添加请求服务器域名

要正常采集小程序的数据并发送给 GrowingIO，需要在支付宝小程序里事先设置一个通讯域名，允许跟 GrowingIO API 服务器进行网络通信。具体步骤如下：

1. 登录支付宝小程序后台，进入配置
2. 打开小程序详情/设置/开发设置
3. 配置httpRequest接口请求域名白名单：https://wxapi.growingio.com

![](../.gitbook/assets/image%20%28214%29.png)

### 4 SDK支付宝用户属性设置

作为用户行为数据分析工具，用户信息的完善会给后续的分析带来很大的帮助。在小程序中，支付宝用户属性是非常重要的设置，只有完善了支付宝用户属性信息，支付宝的访问用户变量（如下表）才可以在分析工具中使用，交互数据定义、数据校验功能才会方便通过用支付宝相关的信息（支付宝姓名和头像）定位用户。

下面是专门针对用户的三个接口：

#### 绑定支付宝用户ID

当用户在你的小程序上登陆获取到 userid 后，可以用过 identify 接口绑定支付宝用户ID，后续在 GrowingIO 中获取更准确的支付宝访问用户量。示例代码如下：

```text
my.httpRequest({
    url: 'http://isv.com/auth', // 该url是自己的服务地址，实现的功能是服务端拿到authcode去开放平台进行token验证
    data: {
      authcode: res.authCode
    },
    success: (res) => {
      gio('identify', res.uid)
    }
  });
```

#### 设置支付宝用户信息

当用户在你的小程序上绑定支付宝信息后，可以通过 setVisitor 接口设置支付宝用户信息，后续在 GrowingIO 中分析这个数据。示例代码如下：

```text
my.getAuthUserInfo({
    success: (userInfo) => {
        gio('setVisitor', userInfo);
    }
    });
```

支付宝信息包含**用户昵称**、**用户头像**、**性别**、**支付宝所填国家**、**支付宝所填省份**、**支付宝所填城市**、**用户类型**、**用户状态**、**是否通过实名认证**、**学生认证**。

\*注：用户画像中的部分数据，只有在设置支付宝用户信息后，才可以统计。

#### 设置注册用户ID

当用户在你的小程序上注册以后，你的产品应用服务端会在CRM 用户数据库里添加一条记录并且分配一个 ID，可以通过 setUserId 接口设置注册用户ID，后续在 GrowingIO 中分析登录用户这个数据。示例代码如下：

```text
gio('setUserId', YOUR_USER_ID); 
```

#### 设置注册用户信息

当用户在你的小程序上传了注册用户ID后，可以通过 setUser 接口设置注册用户信息,例如用户会员等级，后续在 GrowingIO 中分析这个数据。示例代码如下，

```text
    gio('setUserId', user.id);
    gio('setUser', {
        id: user.id,
        level: user.level
    });
```

### 5 检测数据 <a id="jian-ce-shu-ju"></a>

当集成成功后，需要回到 GrowingIO SDK 集成页面，点击右下角“**检测数据”**。请在添加了跟踪代码的支付宝小程序重新启动几次，发送数据给 GrowingIO，完成安装最后一步。

![](../.gitbook/assets/image%20%28298%29.png)

![](../.gitbook/assets/image%20%28162%29.png)



### 6 进入数据校验模块，查看数据发送情况

检测成功后，可以使用[数据验证功能](growingio-debugger/#growingio-minidebugger)，实时查看数据发送情况。

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





