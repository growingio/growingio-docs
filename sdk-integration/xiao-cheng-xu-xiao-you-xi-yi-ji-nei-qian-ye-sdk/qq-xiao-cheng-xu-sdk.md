# QQ 小程序 SDK

## QQ小程序SDK标准接入指南

进入集成页面，选择QQ小程序，填写应用名称和 AppID，点击「下一步」

### 1 根据 QQ 小程序框架选择 SDK 并添加跟踪代码

参照小程序的开发框架，下载相应的SDK，并添加跟踪代码。

* QQ 原生框架
* Taro 框架
* uni-app 框架

#### QQ 原生框架

1.下载 gio-qq-minp.js 文件，把文件放在QQ小程序项目里，比如 utils 目录下。

```text
curl --compressed https://assets.giocdn.com/sdk/gio-qq-minp.js -o gio-qq-minp.js
```

2、添加跟踪代码

方式一：

在根目录 app.js 文件的顶部添加跟踪代码

```javascript
var gio = require("utils/gio-qq-minp.js").default;gio('init', '你的 GrowingIO 项目ID', '你的小程序AppID', { version: '小程序版本' });
```

方式二：

步骤一：新建一个 gioConfig.js 文件，并且配置 gioConfig.js 文件中的 必要 配置参数

```javascript
export default {projectId: '你的 GrowingIO 项目ID',appId: '你的小程序AppID',version: '小程序版本'...}
```

步骤二：在根目录 app.js文件的顶部添加跟踪代码

```javascript
var gio = require("utils/gio-qq-minp.js").default;var gioConfig = require("你的 gioConfig.js 文件地址").default;gio('setConfig', gioConfig);
```

#### Taro 框架

1.下载 gio-qq-minp.js 文件，把文件放在QQ小程序项目里，比如 utils 目录下。

```text
curl --compressed https://assets.giocdn.com/sdk/gio-qq-minp.js -o gio-qq-minp.js
```

2、添加跟踪代码

方式一：

在根目录 app.js 文件的顶部添加跟踪代码

```javascript
import Taro from '@tarojs/taro';var gio = require("utils/gio-qq-minp.js").default;gio('init', '你的 GrowingIO 项目ID', '你的小程序AppID', { version: '小程序版本', taro: Taro });
```

方式二：

步骤一：新建一个 gioConfig.js 文件，并且配置 gioConfig.js 文件中的 必要 配置参数

```javascript
import Taro from '@tarojs/taro';export default {projectId: '你的 GrowingIO 项目ID',appId: '你的小程序AppID',version: '小程序版本',taro: Taro,...}
```

步骤二：在根目录 app.js文件的顶部添加跟踪代码

```javascript
var gio = require("utils/gio-qq-minp.js").default;var gioConfig = require("你的 gioConfig.js 文件地址").default;gio('setConfig', gioConfig);
```

#### uni-app 框架

1.下载 gio-qq-minp.js 文件，把文件放在QQ小程序项目里，比如 utils 目录下。

```text
curl --compressed https://assets.giocdn.com/sdk/gio-qq-minp.esm.js -o gio-qq-minp.js
```

2、添加跟踪代码

方式一：

在根目录 app.js 文件的顶部添加跟踪代码

```javascript
import Vue from 'vue';import App from './App';App.mpType = 'app';var gio = require("utils/gio-qq-minp.js").default;gio('init', '你的 GrowingIO 项目ID', '你的小程序AppID', { version: '小程序版本', vue: Vue });
```

方式二：

步骤一：新建一个 gioConfig.js 文件，并且配置 gioConfig.js 文件中的 必要 配置参数

```javascript
import Vue from 'vue';export default {projectId: '你的 GrowingIO 项目ID',appId: '你的小程序AppID',version: '小程序版本',vue: Vue...}
```

步骤二：在根目录 app.js文件的顶部添加跟踪代码

```javascript
var gio = require("utils/gio-qq-minp.js").default;var gioConfig = require("你的 gioConfig.js 文件地址").default;gio('setConfig', gioConfig);
```



建议每次发布小程序新版本的时候，更新一下版本号 version，可以在 GrowingIO 分析不同版本的数据。除了 version 之外，还有以下额外参数可以使用。  


| 参数 | 值 | 解释 |
| :--- | :--- | :--- |
| version | string | 你的小程序的版本号 |
| getLocation autoGet | true \| false | 是否自动获取用户的地理位置信息。默认false |
| getLocation type | wgs84 \| gcj02 | gcj02 为火星坐标系 |
| followShare | true \| false | 详细跟踪分享数据，开启后可使用分享分析功能。默认true |
| forceLogin | true \| false | 你的 QQ 小程序是否强制要求用户登陆微信获取 openid。默认 false |
| debug | true \| false | 是否开启调试模式，可以看到采集的数据。默认 false |

forceLogin 是一个需要特别注意的参数。GrowingIO 默认会在小程序里面设置用户标识符，存储在QQ Storage 里面。这个用户标识符潜在可能会被 `clearStorage` 清除掉，所以有可能不同的用户标识符对应同一个QQ里的 openid。如果你的小程序在用户打开后会去做登陆并且获取 `openid` 和/或 `unionid`，可以设置 `forceLogin` 为 true。当 forceLogin 为 true 的时候，用户标识符会使用 openid，潜在风险是如果没有设置 openid，数据不会发送，**所以请特别注意这个参数的设置**

```text
如需配置 请将 forceLogin 设为 true
```

### 2 添加请求服务器域名

要正常采集QQ小程序的数据并发送给 GrowingIO，需要在QQ小程序里事先设置一个通讯域名，允许跟 GrowingIO API 服务器进行网络通信。具体步骤如下：

1. 登录QQ小程序后台，进入配置
2. 打开开发设置，到服务器域名配置部分
3. 在request合法域名中添加：https://wxapi.growingio.com

![&#x5C0F;&#x7A0B;&#x5E8F;&#x5C55;&#x793A;](http://k8s-nmetrics-statics.growingio.com/img-2HEAVwc.png)

### 3 SDK QQ 用户属性设置

作为用户行为数据分析工具，用户信息的完善会给后续的分析带来很大的帮助。在小程序中，QQ用户属性是非常重要的设置，只有完善了QQ用户属性信息，QQ的访问用户变量（如下表）才可以在分析工具中使用，交互数据定义、数据校验功能才会方便通过用户QQ相关的信息（QQ姓名和头像）定位用户。  


下面是专门针对用户的三个接口。  


#### 绑定QQ用户ID

当用户在你的小程序上登陆获取到 openid 后，可以用过 identify 接口绑定QQ用户ID，后续在 GrowingIO 中获取更准确的QQ访问用户量。示例代码如下：

```text
qq.request({  url: 'https://YOUR_HOST_NAME/wechat/code2key',  method: 'GET',  data: { code: res.code },  success: function(res) {    var openid = res.data.openid;    var unionid = res.data.unionid;    ...    gio('identify', openid, unionid);  }})
```

#### 设置QQ用户信息

当用户在你的小程序上绑定QQ信息后，可以通过 setVisitor 接口设置QQ用户信息，后续在 GrowingIO 中分析这个数据。示例代码如下，

```text
qq.getUserInfo({  success: function(res) {    ...    gio('setVisitor', res.userInfo);  }})
```

QQ信息包含**QQ昵称**、**QQ头像**、**性别**、**QQ所填国家**、**QQ所填省份**、**QQ所填城市**。  


\*注：用户画像中的部分数据，只有在设置QQ用户信息后，才可以统计。  


#### 设置注册用户ID

当用户在你的小程序上注册以后，你的产品应用服务端会在用户数据库里添加一条记录并且分配一个 ID，可以通过 setUserId 接口设置注册用户ID，后续在 GrowingIO 中分析登录用户这个数据。示例代码如下，

```text
gio('setUserId', YOUR_USER_ID); 
```

#### 设置注册用户信息

当用户在你的小程序上传了注册用户ID后，可以通过 setUser 接口设置注册用户信息，后续在 GrowingIO 中分析这个数据。示例代码如下，  
 `gio('setUserId', user.id); gio('setUser', { id: user.id, name: user.name });`  


### 4 检测数据 <a id="jian-ce-shu-ju"></a>

当集成成功后，需要回到 GrowingIO SDK 集成页面，点击右下角“**检测数据”**。请在添加了跟踪代码的QQ小程序重新启动几次，发送数据给 GrowingIO，完成安装最后一步。

![](../../.gitbook/assets/image%20%28359%29.png)

![](../../.gitbook/assets/image%20%28197%29.png)



### 5 进入数据校验模块，查看数据发送情况

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
// 假设初始化后把 gio 对象放在 App 的 globalData 里面// 在 Page 的 clickBanner 函数里添加以下代码Page({  clickBanner(e) {    getApp().globalData.gio('track', 'clickBanner', {       id: movie.id,       title: movie.title,       index: e.currentTarget.dataset.index     });  }})
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
// 假设初始化后把 gio 对象放在 App 的 globalData 里面// 比如在针对不同的用户做某个 Campaign 的 A/B 测试getApp().globalData.gio('setVisitor', {   campaign_id: 3,   campaign_group: 'A 组用户'});
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
// 假设初始化后把 gio 对象放在 App 的 globalData 里面getApp().globalData.gio('setUser', {   age: 30,   level: '高级用户',   company: 'GrowingIO',   title: '工程师'});
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
// 假设初始化后把 gio 对象放在 App 的 globalData 里面// 推荐在 Page#onShow 处理这个事件// 下面假设我在 GrowingIO 后台已经配置了两个页面级变量 pageName 和 typePage({  onShow() {    getApp().globalData.gio('setPage', {       pageName: '电影列表页',       type: this.data.type    });  }}
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
// 假设初始化后把 gio 对象放在 App 的 globalData 里面getApp().globalData.gio('setEvar', {   campaign: '活动A'});
```





