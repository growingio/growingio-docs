# 小程序 SDK

* [小程序SDK集成前工作​](mina-sdk.md#xiao-cheng-xu-sdk-ji-cheng-qian-gong-zuo)
  * ​[1. ](/docs/~/drafts/-LH8-yUMU-sgLDUogqkP/primary/sdk-integration/ios-sdk#1-sdk-dai-ma-an-zhuang)[确定集成SDK的项目](mina-sdk.md#que-ding-ji-cheng-sdk-de-xiang-mu)
  * ​[2. ](/docs/~/drafts/-LH8-yUMU-sgLDUogqkP/primary/sdk-integration/ios-sdk#ios-sdk-api)[在已有项目中添加小程序应用](mina-sdk.md#zai-yi-you-xiang-mu-zhong-tian-jia-xiao-cheng-xu-ying-yong) \| [创建新的GrowingIO项目](mina-sdk.md#chuang-jian-xin-de-growingio-xiang-mu)
* ​[小程序SDK标准接入指南](mina-sdk.md#xiao-cheng-xu-sdk-biao-zhun-jie-ru-zhi-nan)
  * [1. 下载小程序采集SDK](mina-sdk.md#xia-zai-xiao-cheng-xu-cai-ji-sdk)
  * ​[2. 添加跟踪代码​](mina-sdk.md#tian-jia-gen-zong-dai-ma)
  * ​[3. ](/docs/~/drafts/-LH8-yUMU-sgLDUogqkP/primary/sdk-integration/ios-sdk#3-qian-yi-ye-mian-shu-xing-zi-duan-ps-zi-duan)[添加请求服务器域名](mina-sdk.md#tian-jia-qing-qiu-fu-wu-qi-yu-ming)
  * ​[4. ](/docs/~/drafts/-LH8-yUMU-sgLDUogqkP/primary/sdk-integration/ios-sdk#4-qian-yi-zi-ding-yi-shi-jian-mai-dian-shi-jian)[检测数据](mina-sdk.md#jian-ce-shu-ju)
* [SDK高级设置](mina-sdk.md#sdk-gao-ji-she-zhi-shu-ju-cai-ji-pei-zhi)
  * ​[小程序SDK 微信用户属性设置](mina-sdk.md#sdk-wei-xin-yong-hu-shu-xing-she-zhi)
  * [无埋点采集事件逻辑和高级配置](mina-sdk.md#wu-mai-dian-cai-ji-shi-jian-luo-ji-he-gao-ji-pei-zhi)
  * [自定义事件和变量](mina-sdk.md#zi-ding-yi-shi-jian-he-bian-liang)

## 小程序SDK集成前工作

### 确定集成SDK的项目

![&#x96C6;&#x6210;SDK&#x7684;&#x9636;&#x6BB5;&#x548C;&#x6D41;&#x7A0B;](../.gitbook/assets/image%20%2848%29.png)

如果您已经注册GrowingIO,并且已经有创建或者集成SDK的项目了，您可以进行如下选择：

1. 使用已有项目的ID，添加新的应用，集成SDK。
2. 创建一个新的项目，然后集成SDK。

`*注：如果已有项目且集成了其他应用，新建的项目，数据不能和原有项目中的数据合并。例如：拥有小程序A，iOS端产品，如果各个应用集成在同一个项目ID中，且都上传登录用户ID，登录用户计算时会去重，并且在使用高级分析功能时，可以同时选择跨平台的行为和登录用户。但是如果小程序和APP不在同一个项目ID下，登录用户计算分别在两个项目中，不会去重计算；高级分析功能中，也不能跨平台选择登录用户ID和行为。`

如果你还未注册 GrowingIO，请[点击链接访问注册页面](https://accounts.growingio.com/signup?utm_source=docs&utm_content=minp)，完成注册后你会看到使用引导，点击“**添加跟踪代码**“即可开始。

### 在已有项目中添加小程序应用

在你的 GrowingIO 项目页面点击右上角项目切换控件，在下拉框点击“**项目管理”**，在弹出的列表中选择“应用管理“。在项目概览页面，点击“**新建应用**“来创建一个新应用。

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LH3J3bJm8y879FZ1E7G%2F-LH3Jrrg6GYh7ccjZGaG%2Fimage.png?alt=media&token=a8744aff-4d74-451b-9adf-ec773e5cbb5c)

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LH3J3bJm8y879FZ1E7G%2F-LH3KSslW7JKLy4bb98K%2Fimage.png?alt=media&token=04f949af-bbb1-49f5-98e2-c212022a9c1d)

然后会到达SDK集成页面，填写小程序的应用名称，和小程序的AppID ,点击“下一步”，即可以到达小程序SDK接入的页面。

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LH3J3bJm8y879FZ1E7G%2F-LH3JTZYNHN07NUun-S6%2Fimage.png?alt=media&token=ea6a9dc2-5bc9-408d-b50f-f9c734f5cf0d)

### 创建新的GrowingIO项目

如果你还未注册 GrowingIO，请[点击链接访问注册页面](https://accounts.growingio.com/signup?utm_source=docs&utm_content=minp)，完成注册后你会看到使用引导，点击“**添加跟踪代码**“即可开始。

如果你已经注册 GrowingIO，使用小程序分析功能需要用一个全新的项目的话，在你的 GrowingIO 项目页面点击右上角项目切换控件，在下拉框点击“**项目管理”**，在弹出的列表中选择“**项目概览**“。在项目概览页面，点击“**新建项目**“来创建一个新项目。在创建好的新项目里，你会看到使用引导，点击“**添加跟踪代码**“即可开始。![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LGyRLnN1UW6BL8O3mEr%2F-LGySEePLI1xZvcmo0KX%2Fimage.png?alt=media&token=c9960fdc-81b9-42a1-8604-069d0ad6bdaf)![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LGyRLnN1UW6BL8O3mEr%2F-LGySTNxnseL1EsH7kN8%2Fimage.png?alt=media&token=91d05ea5-95d4-4104-b228-0f1837d5201b)

在代码集成页面，选择“**小程序**“平台，输入**应用名称**和**小程序 AppID**，点击下一步。

## 小程序SDK标准接入指南

### 下载小程序采集 SDK

下载 gio-minp.js 文件

```text
$ curl --compressed https://assets.growingio.com/gio-minp.js -o gio-minp.js
```

当下载到 gio-minp.js 文件以后，把文件放在微信小程序项目里，比如 utils 目录下。下面会假设 SDK 文件放在 utils 目录下。

### 添加跟踪代码

在微信小程序项目根目录的 app.js 文件的顶部添加以下 JS 代码，**请注意一定要放在 App\(\) 之前**：

```text
var gio = require("utils/gio-minp.js");
// version 是你的小程序的版本号，发版时请调整
gio('init', '你的 GrowingIO 项目ID', '你的微信小程序的 AppID', { version: '1.0' });
```

其中GrowingIO 项目ID、微信小程序的 AppID，即为**SDK安装页面** 第②部分 **代码框中生成的代码。**

建议每次发布小程序新版本的时候，更新一下版本号 version，可以在 GrowingIO 分析不同版本的数据。除了 `version` 之外，还有以下额外参数可以使用。

| 参数 | 值 | 解释 |
| --- | --- | --- | --- |
| forceLogin | true \| false | 你的小程序是否强制要求用户登陆微信获取 openid，默认 false |
| debug | true \| false | 是否开启调试模式，可以看到采集的数据，默认 false |
| version | string | 你的小程序的版本号 |

{% hint style="info" %}
forceLogin 是一个需要特别注意的参数。GrowingIO 默认会在小程序里面设置用户标识符，存储在微信 Storage 里面。这个用户标识符潜在可能会被`clearStorage` 清除掉，所以有可能不同的用户标识符对应同一个微信里的 `openid`。如果你的微信小程序在用户打开后会去做登录并且获取 `openid` 和/或 `unionid`，可以设置 `forceLogin` 为 true。当 forceLogin 为 true 的时候，用户标识符会使用 openid，潜在风险是如果用户没有登录，数据不会发送。具体集成示例：



```text
gio('init', '你的 GrowingIO 项目ID', '你的微信小程序的 AppID', { version: '1.0', forceLogin: true });
...
// 当获取到 openid 后，调用以下方法
gio("identify", openid, unionid);
```
{% endhint %}

**注意：**如果你的微信小程序在用户打开后不要求用户授权获取openid和/或 unionid，但是设置了forceLogin为True，那么GroiwngIO不能采集到用户的数据，所以请特别注意这个参数的设置。

### 特别注意

如果你使用 mpvue 来开发小程序的应用，为了让 GrowingIO SDK 能自动采集到真实行为事件，需要把 Vue 作为初始化参数传入。比如在 main.js 这样子集成，

```text
import gio from './utils/gio-minp'
import Vue from 'vue'
import App from './App'
​
gio('init', '你的 GrowingIO 项目ID', '你的微信小程序的 AppID', { vue: Vue, version: '1.0' });
```

gio-minp 默认是用 module.exports 对于 import 不支持，可以修改 gio-minp.js 最后部分代码，把 `, module.exports = gio`修改成`; export default gio`。

### 添加请求服务器域名

要正常采集微信小程序的数据并发送给 GrowingIO，需要在微信小程序里事先设置一个通讯域名，允许跟 GrowingIO API 服务器进行网络通信。具体步骤如下：

1. 登陆微信小程序后台，进入配置
2. 打开开发设置，到服务器域名配置部分
3. 在`request合法域名`中添加：https://wxapi.growingio.com

![SDK &#x6DFB;&#x52A0;&#x670D;&#x52A1;&#x5668;&#x57DF;&#x540D;](../.gitbook/assets/image%20%2858%29.png)

### 检测数据

当集成成功后，需要回到 GrowingIO SDK 集成页面检测数据。请在添加了跟踪代码的小程序重新启动几次，发送数据给 GrowingIO，完成安装最后一步。详情可见小程序Debugger。

## SDK高级设置&数据采集配置

### SDK 微信用户属性设置

作为用户行为数据分析工具，用户信息的完善会给后续的分析带来很大的帮助。下面是专门针对用户的三个接口。

#### 绑定微信用户ID

当用户在你的小程序上登陆获取到 openid 后，可以用过 `identify` 接口绑定微信用户ID，后续在 GrowingIO 中获取更准确的微信访问用户量。示例代码如下，

```text
wx.request({ 
  url: 'https://YOUR_HOST_NAME/wechat/code2key',
  method: 'GET',
  data: { code: res.code }
  success: res => 
    var openid = res.data.openid;
    var unionid = res.data.unionid;
    ...
    gio('identify', res.data.openid, res.data.unionid)
})
```

如果你希望直接用 openid 来替换 GrowingIO 自行设置的用户标识符，请在初始化的时候指定 forceLogin 参数为 true。详见[SDK添加标准代码](https://growingio.gitbook.io/docs/~/edit/drafts/-LH63IC_CQDsX8RsyU_H/sdk-integration/xiao-cheng-xu-sdk#tian-jia-zhui-zong-dai-ma)。

#### 设置微信用户信息

当用户在你的小程序上绑定微信信息后，可以通过 `setVisitor` 接口设置微信用户信息，后续在 GrowingIO 中分析这个数据。示例代码如下，

```text
wx.getUserInfo({ 
  success: res => 
    ...
    gio('setVisitor', res.userInfo);
})
```

微信信息包含**微信昵称**、**微信头像**、**性别、微信所填国家、微信所填省份、微信所填城市**。

\*注：用户画像中的部分数据，只有在设置微信用户信息后，才可以统计。

#### 设置注册用户ID

当用户在你的小程序上注册以后，你的产品应用服务端会在用户数据库里添加一条记录并且分配一个 ID，可以通过 setUserId 接口设置注册用户ID，后续在 GrowingIO 中分析登录用户这个数据。示例代码如下，

```text
gio('setUserId', YOUR_USER_ID); 
```

### 无埋点采集事件逻辑和高级配置

#### 访问数据

每次用户打开小程序的时候，发送一条`应用打开`消息，对应的数据指标是打开次数。发送数据包含但不限于以下信息：进入页面、进入时间、场景值、来源小程序或 App、设备信息、微信信息、应用版本号。

每次用户关闭小程序的时候，关闭动作包括退出小程序回到微信、退出微信，会发送一条`应用关闭`消息，会根据这个消息来计算应用访问时长。发送数据包含但不限于以下信息：退出页面、退出时间。

#### 页面数据

每次用户访问一个新的页面，发送一条`页面打开`消息，对应的数据意义是页面分析。发送数据包含但不限于以下信息：页面信息、打开时间、页面来源、页面标题、页面级变量。

每次当用户点击转发按钮时，会弹出转发框，这时会发送一条`要转发消息`消息。这是一个自定义事件，数据包含以下信息：事件时间、所在页面、转发动作来源、转发页面标题和转发页面路径。注意这个事件不代表用户真正转发了消息到聊天里面，而是用户触发了转发动作。如果要跟踪成功转发消息事件，建议在 onShareAppMessage 的 success callback 里面发送一个自定义事件。

#### 行为数据

对于用户在页面发生的行为，如果这个行为有绑定事件比如 bindtap，并且在你的小程序里面进行处理，那么 GrowingIO 的小程序 SDK 会自动采集这些事件，发送一条行为消息。目前，我们支持的事件有 tap、longpress、confirm 和 change 事件。

#### tap

tap 事件是手指触摸后马上离开时触发的事件。当 wxml 中的 view 绑定了 bindtap 事件以后，在事件处理函数执行的时候，SDK 会自动采集 tap 事件，发送数据包含但不限于以下信息：点击事件时间、事件发生所在页面、点击控件相关信息。比如如下，

```text
<view data-title='复仇者联盟3' data-index='1' bindtap='clickMovie'>  <image src='IMAGE—URL' mode='aspectFill'/>  <text>复仇者联盟3</text></view>
```

注意这里的 `data-title` 和 `data-index` 属性，因为微信小程序的限制，无法采集到控件的内容和结构数据，所以在小程序 SDK 里面我们采取的是声明式编程，通过在 wxml 文件里面设置 data- 属性，可以给 view 控件添加额外的`内容`和`位置`属性，方便后续在分析时可以按照元素内容和元素位置做分析，对于列表式的组件特别有用和方便。

**建议在 View 的控件里面多使用 data-title 和 data-index 属性这种声明式编程方式。**

#### longpress

longpress 事件是手指触摸后，超过350ms再离开时触发的事件。当 wxml 的 view 绑定了 bindlongpress 事件以后，在事件处理函数执行的时候，SDK 会自动采集 longpress 事件，发送数据包含但不限于以下信息：点击事件时间、事件发生所在页面、点击控件相关信息。比如如下，

```text
<view data-title='复仇者联盟3' data-index='1' bindtap='clickMovie'>
  <image src='IMAGE—URL' mode='aspectFill'/>
  <text>复仇者联盟3</text>
</view>
```

注意这里的 `data-title` 和 `data-index` 属性，因为微信小程序的限制，无法采集到控件的内容和结构数据，所以在小程序 SDK 里面我们采取的是声明式编程，通过在 wxml 文件里面设置 data- 属性，可以给 view 控件添加额外的`内容`和`位置`属性，方便后续在分析时可以按照元素内容和元素位置做分析，对于列表式的组件特别有用和方便。

**建议在 View 的控件里面多使用 data-title 和 data-index 属性这种声明式编程方式。**

#### change

change 事件是针对 checkbox, radio, picker-view 这些控件，当选择项发生改变时触发的事件。当 wxml 的 view 绑定了 bingchange 事件以后，在事件处理函数执行的时候，SDK 会自动采集 change 事件，发送数据包含但不限于以下信息：选择事件的发生时间、事件发生所在页面。如果设置了要采集内容，则也会包含选择项的内容信息。比如如下，

```text
<checkbox-group bindchange='checkboxChange' data-growing-track>
  <label class='checkbox'>
    <checkbox value='GrowingIO' checked='true' /> GrowingIO
  </label>
  <label class='checkbox'>
    <checkbox value='Tencent' checked='false' /> 腾讯小程序分析工具
  </label>
  <label class='checkbox'>
    <checkbox value='Google' checked='false' /> Google Analytics
  </label>
</checkbox-group>
```

当用户选择了某项以后，因为这里设置了 `data-growing-track` 属性，所以会采集到这一项对应的 value 值。

#### confirm

confirm 事件是对于 input 和 textarea 控件，当输入完成后触发的事件。当 wxml 的 view 绑定了 bindconfirm 事件以后，在事件处理函数执行的时候，SDK 会自动采集 confirm 事件，发送数据包含但不限于以下信息：输入事件的发生时间、事件发生所在页面。如果设置了要采集内容，则也会包含输入的内容。比如如下，

```text
<input class='new-todo'
       value='{{ input }}'
       placeholder='Anything here...'
       data-growing-track='true'
       bindinput='inputChangeHandle'
       bindconfirm='addTodoHandle'
/>
```

当用户输入完成后，因为这里指定了 `data-growing-track` 属性，所以会采集到输入的内容。

## 自定义事件和变量

### 预置自定义事件

GrowingIO 预置了两个小程序的标准自定义事件：分享到群聊或好友信息和程序错误，接入SDK即可以使用。

**微信小程序分享到好友或群聊信息**

![](../.gitbook/assets/image%20%2825%29.png)

**程序错误**

![](../.gitbook/assets/image%20%284%29.png)

### 自定义事件配置

手动发送一个自定义事件。在添加所需要发送的事件代码之前，需要在 GrowingIO 产品的`自定义事件和变量`管理页面配置事件以及事件级变量。

接口定义：

```text
gio('track', eventName: string, properties: object)
```

参数说明：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- |
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
| --- | --- |
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



给注册用户附上额外的信息，便于后续做用户信息相关分析。在添加所需要设置的注册用户变量的代码之前，需要在 GrowingIO 产品的`自定义事件和变量`管理页面配置注册用户级变量。

接口定义：

```text
gio('setUser', properties: object)
```

参数说明：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| --- | --- |
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
| --- | --- |
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
| --- | --- |
| properties | object | 是 | 转化级变量，转化信息 |

示例：

```text
// 假设初始化后把 gio 对象放在 App 的 globalData 里面
getApp().globalData.gio('setEvar', { 
  campaign: '活动A'
});
```





