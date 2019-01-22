# 微信小游戏 SDK

## 小游戏SDK集成前工作 <a id="xiao-cheng-xu-sdk-ji-cheng-qian-gong-zuo"></a>

### 创建新的GrowingIO项目 <a id="chuang-jian-xin-de-growingio-xiang-mu"></a>

如果你还未注册 GrowingIO，请[点击链接访问注册页面](https://accounts.growingio.com/signup?utm_source=docs&utm_content=minp)，完成注册后你会看到使用引导，点击“**添加跟踪代码**“即可开始。

如果你已经注册 GrowingIO，使用小程序分析功能需要用一个全新的项目，在你的 GrowingIO 项目页面点击右上角项目切换控件，在下拉框点击“**项目管理”**，在弹出的列表中选择“**项目概览**“。在项目概览页面，点击“**新建项目**“来创建一个新项目。在创建好的新项目里，你会看到使用引导，点击“**添加跟踪代码**“即可开始

。![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LJx04ER9ZFR9ndFEdU4%2F-LJx1SorCCRnCk8CPCLR%2Fimage.png?alt=media&token=f0a9f63a-a9ea-4886-b172-b3edfa5d3779)项目概览![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LGyRLnN1UW6BL8O3mEr%2F-LGySTNxnseL1EsH7kN8%2Fimage.png?alt=media&token=91d05ea5-95d4-4104-b228-0f1837d5201b)​

​

在代码集成页面，选择“**微信小游戏**“平台，输入**小游戏名称**和**小游戏 AppID**，点击下一步。

### 在项目中接入一个新的小游戏应用 <a id="zai-xiang-mu-zhong-jie-ru-yi-ge-xin-de-xiao-cheng-xu-ying-yong"></a>

在你的 GrowingIO 项目页面点击右上角点击小齿轮，在弹出的列表中选择“应用管理“。在应用管理页面，点击“**新建应用**“来创建一个新应用。

​![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LH3J3bJm8y879FZ1E7G%2F-LH3KSslW7JKLy4bb98K%2Fimage.png?alt=media&token=04f949af-bbb1-49f5-98e2-c212022a9c1d)​

然后会到达SDK集成页面，填写小游戏的应用名称，和小游戏的AppID ,点击“下一步”，即可以到达小游戏SDK接入的页面。

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LPxaqNrmOfcLRyXYmaY%2F-LPxbiVUb1WwF4plB3y2%2Fimage.png?alt=media&token=3b94a3e5-d654-477f-86be-bf196a4d5c8c)

目前仅支持微信小程序应用集成在同一个项目下，暂不支持小游戏产品和web\iOS\Android端产品集成在同一项目下。

如果需要在已有web/iOS/Andoird产品集成的情况下，集成小游戏产品，请创建新的项目。

## 小游戏SDK标准接入指南 <a id="xiao-cheng-xu-sdk-biao-zhun-jie-ru-zhi-nan"></a>

### 下载小游戏采集 SDK <a id="xia-zai-xiao-you-xi-cai-ji-sdk"></a>

下载 gio-ming.js 文件

```text
curl --compressed https://assets.growingio.com/gio-ming.js -o gio-ming.js
```

当下载到 gio-ming.js 文件以后，把文件放在微信小游戏项目里，比如 utils 目录下。下面会假设 SDK 文件放在 utils 目录下。

### 添加跟踪代码 <a id="tian-jia-gen-zong-dai-ma"></a>

在微信小游戏项目根目录的 game.js 文件的顶部添加以下 JS 代码：

```javascript
var gio = require("utils/gio-ming.js");
// version 是你的小游戏的版本号
gio('init', '你的 GrowingIO 项目ID', '你的微信小游戏的 AppID', { version: '1.0' });
```

建议每次发布小游戏新版本的时候，更新一下版本号 version，可以在 GrowingIO 分析不同版本的数据。除了 version 之外，还有以下额外参数可以使用。

| 参数 | 值 | 解释 |
| :--- | :--- | :--- |
| version | string | 你的小游戏的版本号 |
| forceLogin | true \| false | 你的小游戏是否强制要求用户登陆微信获取 openid。默认 false |
| followShare | true \| false | 详细跟踪微信组件中的wx. shareAppMessage事件的转发分享数据，开启后可使用分享分析功能。默认false |
| getLocation | true \| false | 是否自动获取用户的地理位置信息。默认false |
| debug | true \| false | 是否开启调试模式，可以看到采集的数据。默认 false |

#### Version 参数

每次发布小游戏新版本的时候，需要更新一下版本号 version, 与线上发布小游戏保持一致; 在使用使，可以用来分析不同版本的数据。

#### followShare 分享分析参数

转发分享小游戏是小游戏获客的重要场景，想要详细的进行转发分享的统计，需要在SDK参数中，设置如下参数，值为true；获客可以参考更具体的设置方式。

| 参数 | 值 | 解释 |
| :--- | :--- | :--- |
| followShare | true \| false | 详细跟踪分享数据，开启后可使用分享分析功能。默认false |

#### getLocation 参数

根据微信最新的用户地理位置获取的规则，GrowingIO 小游戏SDK 默认不会在小游戏启动时获取用户的坐标信息。 

* 如果您的小游戏在打开时就需要获取用户地理信息，就可以将这个参数配置为true。
* 如果您的小游戏在用户点击某些按钮时，才触发获取位置，则可以按照配置方式，进行[用户位置的补发](mina-sdk/#huo-qu-yong-hu-de-di-li-xin-xi)，从而增强用户地理位置的分析能力。

| 参数 | 值 | 解释 |
| :--- | :--- | :--- |
| getLocation | true \| false | 是否自动获取用户的地理位置信息。默认false |

#### forceLogin 用户标识参数

{% hint style="info" %}
forceLogin 是一个需要特别注意的参数。GrowingIO 默认会在小游戏里面设置用户标识符，存储在微信 Storage 里面。这个用户标识符潜在可能会被 `clearStorage` 清除掉，所以有可能不同的用户标识符对应同一个微信里的 openid。如果你的微信小游戏在用户打开后会去做登陆并且获取 `openid` 和/或 `unionid`，可以设置 `forceLogin` 为 true。当 forceLogin 为 true 的时候，用户标识符会使用 openid，潜在风险是如果用户没有授权，数据不会发送，**所以请特别注意这个参数的设置**，具体集成示例：



```javascript
gio('init', '你的 GrowingIO 项目ID', '你的微信小程序的 AppID', { version: '1.0', forceLogin: true });
...
// 当获取到 openid 后，调用以下方法gio("identify", openid, unionid);
```
{% endhint %}

**注意：如果你的微信小游戏在用户打开后不及时要求用户授权获取openid和/或 unionid，但是设置了forceLogin为True，那么GroiwngIO不能采集到用户的数据，采集到的用户会偏少，所以请特别注意这个参数的设置。如果您不能确定是否要设置这个参数，请先咨询我们。**

### 添加请求服务器域名 <a id="tian-jia-qing-qiu-fu-wu-qi-yu-ming"></a>

要正常采集微信小游戏的数据并发送给 GrowingIO，需要在微信小游戏里事先设置一个通讯域名，允许跟 GrowingIO API 服务器进行网络通信。具体步骤如下：

1. 登陆微信小游戏后台，进入配置
2. 打开开发设置，到服务器域名配置部分
3. 在request合法域名中添加：https://wxapi.growingio.com

![](../.gitbook/assets/image%20%28125%29.png)

### 检测数据 <a id="jian-ce-shu-ju"></a>

当集成成功后，需要回到 GrowingIO SDK 集成页面检测数据。请在添加了跟踪代码的小游戏重新启动几次，发送数据给 GrowingIO，完成安装最后一步。

## 小游戏 SDK 高级设置&数据采集配置 <a id="sdk-gao-ji-she-zhi-shu-ju-cai-ji-pei-zhi"></a>

### SDK分享参数设置

转发分享小游戏是小游戏获客的重要场景，想要详细的进行转发分享的统计，需要在SDK参数中，设置如下参数，值为true

| 参数 | 值 | 解释 |
| :--- | :--- | :--- |
| followShare | true  | 详细跟踪微信组件中的wx. shareAppMessage事件的转发分享数据，开启后可使用分享分析功能。默认false |

  
即微信小游戏项目根目录的 game.js 文件设置参数如下：

```javascript
var gio = require("utils/gio-ming.js");
// version 是你的小游戏的版本号，发版时请调整
gio('init', '9c76fe4756c3404d', 'wx87c6f4a3a6cf31e7', { version: '1.0', followShare: true });
//将微信的wx.OnShareAppMessage替换成gio("gioOnShareAppMessage", function(){})
//分享，监听用户点击右上角菜单的“转发”按钮时触发的事件
gio("gioOnShareAppMessage", function(){
return {
title: "试试我的小游戏"
}
})
//将微信的wx. shareAppMessage替换成gio("gioShareAppMessage", {})
//分享，主动拉起转发，进入选择通讯录界面
gio("gioShareAppMessage", {
title: "试试我的小游戏"
}
)
```

#### \*\*如果您希望采用微信的原生接口，那么需要在分享触发事件上做这样的配置操作，这样GrowingIO才能采集到分享的数据。

```javascript
//设置follwShare
gio('init', '', '', { version: '1.0', debug: true, followShare: true});

//分享，监听用户点击右上角菜单的“转发”按钮时触发的事件
//调用 pageShareInfo 发送分享事件以及处理分享追踪信息
wx.onShareAppMessage(function () {
  var obj = {
    title: "试试我的小游戏"
  }
  obj = gio('pageShareInfo', obj);
  return obj;
})

//分享，主动拉起转发，进入选择通讯录界面
//调用 pageShareInfo 发送分享事件以及处理分享追踪信息
obj = gio('pageShareInfo', obj);
wx.shareAppMessage(obj)

```

### SDK 微信用户属性设置 <a id="sdk-wei-xin-yong-hu-shu-xing-she-zhi"></a>

作为用户行为数据分析工具，用户信息的完善会给后续的分析带来很大的帮助。在小程序中，微信用户属性是非常重要的设置，只有完善了微信用户属性信息，微信的访问用户变量（如下表）才可以在分析工具中使用，交互数据定义、数据校验功能才会方便通过用户微信相关的信息（微信姓名和头像）定位用户。

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LLhV0ClvHRSssTkMIA0%2F-LLhWZY75GvYyad8Omcf%2Fimage.png?alt=media&token=3ac8d729-8643-484a-ba16-ab306535a381)

下面是专门针对用户的三个接口。

#### 绑定微信用户ID <a id="bang-ding-wei-xin-yong-hu-id"></a>

当用户在你的小程序上登陆获取到 openid 后，可以用过 `identify` 接口绑定微信用户ID，后续在 GrowingIO 中获取更准确的微信访问用户量。示例代码如下，

```text
wx.request({   url: 'https://YOUR_HOST_NAME/wechat/code2key',  method: 'GET',  data: { code: res.code }  success: res =>     var openid = res.data.openid;    var unionid = res.data.unionid;    ...    gio('identify', res.data.openid, res.data.unionid)})
```

#### 设置微信用户信息 <a id="she-zhi-wei-xin-yong-hu-xin-xi"></a>

当用户在你的小程序上绑定微信信息后，可以通过 `setVisitor` 接口设置微信用户信息，后续在 GrowingIO 中分析这个数据。示例代码如下，

```text
wx.getUserInfo({   success: res =>     ...    gio('setVisitor', res.userInfo);})
```

微信信息包含**微信昵称**、**微信头像**、**性别、微信所填国家、微信所填省份、微信所填城市**。

\*注：用户画像中的部分数据，只有在设置微信用户信息后，才可以统计。

#### 设置注册用户ID <a id="she-zhi-zhu-ce-yong-hu-id"></a>

当用户在你的小游戏上注册以后，你的产品应用服务端会在用户数据库里添加一条记录并且分配一个 ID，可以通过 setUserId 接口设置注册用户ID，后续在 GrowingIO 中分析登录用户这个数据。示例代码如下。设置了注册用户ID后，才可以使用**登录用户变量**。

```text
gio('setUserId', YOUR_USER_ID); 
```



## 自定义事件和变量 <a id="zi-ding-yi-shi-jian-he-bian-liang"></a>

### 预置自定义事件 <a id="yu-zhi-zi-ding-yi-shi-jian"></a>

GrowingIO 预置了两个小程序的标准自定义事件：分享到群聊或好友信息和程序错误，接入SDK即可以使用。

**微信小程序分享到好友或群聊信息**

**​**![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LH66a23TIvbEtQOPKyt%2F-LH675ymMloYtj1u3JVJ%2Fimage.png?alt=media&token=025a5cfb-90eb-45ee-aa58-767c0380bae1)​

**程序错误**

**​**![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LH66a23TIvbEtQOPKyt%2F-LH67LAaMuVbR_KxSYVc%2Fimage.png?alt=media&token=0708739e-4cae-4315-b633-e9aeb2e25ca2)​

### 自定义事件配置 <a id="zi-ding-yi-shi-jian-pei-zhi"></a>

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
// 假设初始化后把 gio 对象放在 App 的 globalData 里面// 在 Page 的 clickBanner 函数里添加以下代码Page({  clickBanner(e) {    getApp().globalData.gio('track', 'clickBanner', {       id: movie.id,       title: movie.title,       index: e.currentTarget.dataset.index     });  }})
```

### 访问用户变量 <a id="fang-wen-yong-hu-bian-liang"></a>

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
// 假设初始化后把 gio 对象放在 App 的 globalData 里面// 比如在针对不同的用户做某个 Campaign 的 A/B 测试getApp().globalData.gio('setVisitor', {   campaign_id: 3,   campaign_group: 'A 组用户'});
```

### 注册用户变量 <a id="zhu-ce-yong-hu-bian-liang"></a>

​

给注册用户附上额外的信息，便于后续做用户信息相关分析。在添加所需要设置的注册用户变量的代码之前，需要在 GrowingIO 产品的`自定义事件和变量`管理页面配置注册用户级变量。**注，使用注册用户变量的前提是必须上传登录用户ID，并且在事件及变量的页面打开登录用户ID。**

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
// 假设初始化后把 gio 对象放在 App 的 globalData 里面getApp().globalData.gio('setUser', {   age: 30,   level: '高级用户',   company: 'GrowingIO',   title: '工程师'});
```

### 页面级变量 <a id="ye-mian-ji-bian-liang"></a>

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
// 假设初始化后把 gio 对象放在 App 的 globalData 里面// 推荐在 Page#onShow 处理这个事件// 下面假设我在 GrowingIO 后台已经配置了两个页面级变量 pageName 和 typePage({  onShow() {    getApp().globalData.gio('setPage', {       pageName: '电影列表页',       type: this.data.type    });  }}
```

###  转化变量 <a id="zhuan-hua-bian-liang"></a>

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
// 假设初始化后把 gio 对象放在 App 的 globalData 里面getApp().globalData.gio('setEvar', {   campaign: '活动A'});
```

