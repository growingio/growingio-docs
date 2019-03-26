# 微信小程序 SDK

* [小程序SDK集成前工作​](./#xiao-cheng-xu-sdk-ji-cheng-qian-gong-zuo)
  * [​创建新的GrowingIO项目 ](./#chuang-jian-xin-de-growingio-xiang-mu)或 [在项目中集成一个新的小程序应用](./#zai-tong-yi-xiang-mu-xia-jie-ru-yi-ge-xin-de-xiao-cheng-xu)
* [小程序SDK标准接入指南](./#xiao-cheng-xu-sdk-biao-zhun-jie-ru-zhi-nan)
* [SDK高级设置](./#sdk-gao-ji-she-zhi-shu-ju-cai-ji-pei-zhi)
  * [无埋点采集事件逻辑和高级配置](./#wu-mai-dian-cai-ji-shi-jian-luo-ji-he-gao-ji-pei-zhi)
  * [自定义事件和变量](./#zi-ding-yi-shi-jian-he-bian-liang)

## 小程序SDK集成前工作

### 创建新的GrowingIO项目

如果你还未注册 GrowingIO，请[点击链接访问注册页面](https://accounts.growingio.com/signup?utm_source=docs&utm_content=minp)，完成注册后你会看到使用引导，点击“**添加跟踪代码**“即可开始。

如果你已经注册 GrowingIO，使用小程序分析功能需要用一个全新的项目，在你的 GrowingIO 项目页面点击右上角项目切换控件，在下拉框点击“**项目管理”**，在弹出的列表中选择“**项目概览**“。在项目概览页面，点击“**新建项目**“来创建一个新项目。在创建好的新项目里，你会看到使用引导，点击“**添加跟踪代码**“即可开始。

![&#x9879;&#x76EE;&#x6982;&#x89C8;](../../.gitbook/assets/image%20%28213%29.png)

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LGyRLnN1UW6BL8O3mEr%2F-LGySTNxnseL1EsH7kN8%2Fimage.png?alt=media&token=91d05ea5-95d4-4104-b228-0f1837d5201b)



在代码集成页面，选择“**小程序**“平台，输入**应用名称**和**小程序 AppID**，点击下一步。

### 在项目中接入一个新的小程序应用

在你的 GrowingIO 项目页面点击右上角点击小齿轮，在弹出的列表中选择“应用管理“。在应用管理页面，点击“**新建应用**“来创建一个新应用。

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LH3J3bJm8y879FZ1E7G%2F-LH3KSslW7JKLy4bb98K%2Fimage.png?alt=media&token=04f949af-bbb1-49f5-98e2-c212022a9c1d)

然后会到达SDK集成页面，填写小程序的应用名称，和小程序的AppID ,点击“下一步”，即可以到达小程序SDK接入的页面。

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LH3J3bJm8y879FZ1E7G%2F-LH3JTZYNHN07NUun-S6%2Fimage.png?alt=media&token=ea6a9dc2-5bc9-408d-b50f-f9c734f5cf0d)

目前仅支持小程序应用集成在同一个项目下，暂不支持小程序产品和web\iOS\Android端产品集成在同一项目下。

如果需要集成其他端产品，或者需要在已有web/iOS/Andoird产品集成的情况下，集成小程序产品，请创建新的项目。

## 微信小程序SDK标准接入指南

### 1、根据小程序框架选择SDK文件并添加跟踪代码

参照小程序的开发框架，下载相应的SDK，并添加跟踪代码。

* [微信小程序原生框架](./#wei-xin-yuan-sheng-kuang-jia)
* [微信小程序原生 + 第三方插件](./#wei-xin-yuan-sheng-kuang-jia-di-san-fang-cha-jian)
* [taro框架](./#taro-kuang-jia)
* [Wepy框架](./#wepy-kuang-jia)
* [mpvue框架/ uni-app框架](./#mpvue-kuang-jia)
* [mpvue + 第三方插件](./#mpvue-di-san-fang-cha-jian)
* [Chameleon框架](./#chameleon-kuang-jia)

{% hint style="danger" %}
**在微信小程序项目根目录的 app.js  文件的顶部添加代码，请注意一定要放在 App\(\) 之前, 否则可能导致采集的数据不完整。**
{% endhint %}

#### **微信原生框架：**

1.下载 gio-minp.js 文件，把文件放在微信小程序项目里，比如 utils 目录下。

```text
$ curl --compressed https://assets.growingio.com/gio-minp.js -o gio-minp.js
```

2.在根目录 app.js文件的顶部添加跟踪代码

```javascript
var gio = require ("utils/gio-minp.js").default;
// version 是你的小程序的版本号，发版时请调整
gio('init', '你的 GrowingIO 项目ID', '你的微信小程序的 AppID', { version: '1.0' });
```

#### **微信原生框架+第三方插件**

1.下载 gio-minp.js 文件，把文件放在微信小程序项目里，比如 utils 目录下。

```text
$ curl --compressed https://assets.growingio.com/gio-minp.js -o gio-minp.js
```

2.在根目录 app.js文件的顶部添加跟踪代码

```javascript
var gio = require ("utils/gio-minp.js").default;
// version 是你的小程序的版本号，发版时请调整; usePlugin 设置是否使用插件
gio('init', '你的 GrowingIO 项目ID', '你的微信小程序的 AppID', { version: '1.0', usePlugin: true });
// app.js 文件，在文件顶部 （其他代码之前）添加如下代码： 
const App = global.GioApp
```

3.在**每个page**页面**（新增页面也需要添加）**的 .js 文件顶部添加如下代码

```javascript
//在每个Page页面的 .js 文件顶部（其他代码之前）添加如下代码。（请注意是每个页面都要引入）
const Page = global.GioPage;
```

#### **taro 框架**

1.下载 gio-minp.js 文件，把文件放在微信小程序项目里，比如 utils 目录下。

```text
$ curl --compressed https://assets.growingio.com/gio-minp.js -o gio-minp.js
```

2.在根目录 app.js文件的顶部添加跟踪代码

```javascript
import Taro from '@tarojs/taro'
import gio from './utils/gio-minp'
gio('init', '你的 GrowingIO 项目ID', '你的微信小程序的 AppID', {
  version: '1.0',
  taro: Taro
});
```

#### **wepy 框架**

1.下载 **gio-minp.esm.js** 文件，把文件放在微信小程序项目里，比如 utils 目录下。

```text
$ curl --compressed https://assets.growingio.com/gio-minp.esm.js -o gio-minp.js
```

2.在根目录 app.wpy文件的顶部添加跟踪代码

```javascript
import gio from './utils/gio-minp';
gio('init', '你的 GrowingIO 项目ID', '你的微信小程序的 AppID', { version: '1.0' })
```

#### mpvue框架 / uni-app 框架

1.如果小程序使用以上框架，下载 **gio-minp.esm.js** 文件，把文件放在微信小程序项目里，比如 utils 目录下。

```text
$ curl --compressed https://assets.growingio.com/gio-minp.esm.js -o gio-minp.js
```

2.在根目录 app.js文件的顶部添加跟踪代码

```javascript
import gio from './utils/gio-minp'
import Vue from 'vue'
import App from './App'
gio('init', '你的 GrowingIO 项目ID', '你的微信小程序的 AppID', { vue: Vue, version: '1.0' });
```

####  mpvue + 第三方插件

mpvue + 第三方插件 设置代码较为复杂，请点击如下链接进行查看。

{% page-ref page="mpvue+-di-san-fang-cha-jian-tian-jia-dai-ma.md" %}

#### Chameleon框架

1.下载 gio-minp.js 文件，把文件放在微信小程序项目里，比如 utils 目录下。

```text
$ curl --compressed https://assets.growingio.com/gio-minp.js -o gio-minp.js
```

2.在根目录 app.js文件的顶部添加跟踪代码

```javascript
import Cml from 'chameleon-runtime'
import gio from '../utils/gio-minp'
gio("init","9c76fe4756c3404d", "wx51cba5e78d4ef4d8", { debug: true, version: "0.7.0", cml: Cml})
```

### **2、进行SDK的配置设置**

SDK参数配置

微信用户信息配置

用户访问的地理坐标信息

**SDK中提供了以下几个参数可以用来进行配置**

| 参数 | 值 | 解释 |
| :--- | :--- | :--- |
| version | string | 你的小程序的版本号 |
| getLocation | true \| false | 是否自动获取用户的地理位置信息。默认false |
| followShare | true \| false | 详细跟踪分享数据，开启后可使用分享分析功能。默认false |
| forceLogin | true \| false | 你的小程序是否强制要求用户登陆微信获取 openid。默认 false |
| debug | true \| false | 是否开启调试模式，可以看到采集的数据。默认 false |
| usePlugin | true \| false | 你的小程序中是否使用了第三方插件。默认false。 |

#### Version 参数

每次发布小程序新版本的时候，需要更新一下版本号 version, 与线上发布小程序保持一致; 在使用使，可以用来分析不同版本的数据。

#### followShare 分享分析参数

转发分享小程序是小程序获客的重要场景，想要详细的进行转发分享的统计，需要在SDK参数中，设置如下参数，值为true

| 参数 | 值 | 解释 |
| :--- | :--- | :--- |
| followShare | true \| false | 详细跟踪分享数据，开启后可使用分享分析功能。默认false |

小程序项目根目录的 app.js 文件设置参数示例如下：

```javascript
var gio = require("utils/gio-minp.js").default;
// version 是你的小程序的版本号，发版时请调整
gio('init', '你的 GrowingIO 项目ID', '你的微信小程序的 AppID', { version: '1.0', followShare: true });
```

对于 mpvue 用户，使用下面这种方式：

```javascript
import gio from './utils/gio-minp'
import Vue from 'vue'
import App from './App'
gio('init', '你的 GrowingIO 项目ID', '你的微信小程序的 AppID', { vue: Vue, version: '1.0', followShare: true });
```

#### 

#### getLocation 参数

根据微信最新的用户地理位置获取的规则，GrowingIO 小程序SDK 默认不会在小程序启动时获取用户的坐标信息。 

* 如果您的小程序在打开时就需要获取用户地理信息，就可以将这个参数配置为true。
* 如果您的小程序在用户点击某些按钮时，才触发获取位置，则可以按照配置方式，进行[用户位置的补发](./#huo-qu-yong-hu-de-di-li-xin-xi)，从而增强用户地理位置的分析能力。

| 参数 | 值 | 解释 |
| :--- | :--- | :--- |
| getLocation | true \| false | 是否自动获取用户的地理位置信息。默认false |



#### forceLogin 用户标识参数

{% hint style="info" %}
forceLogin 是一个需要特别注意的参数。GrowingIO 默认会在小程序里面设置用户标识符，存储在微信 Storage 里面。这个用户标识符潜在可能会被`clearStorage` 清除掉，所以有可能不同的用户标识符对应同一个微信里的 `openid`。如果你的微信小程序在用户打开后会去做登录并且获取 `openid` 和/或 `unionid`，可以设置 `forceLogin` 为 true。当 forceLogin 为 true 的时候，用户标识符会使用 openid，潜在风险是如果用户没有登录，数据不会发送。具体集成示例：



```text
gio('init', '你的 GrowingIO 项目ID', '你的微信小程序的 AppID', { version: '1.0', forceLogin: true });
...
// 当获取到 openid 后，调用以下方法
gio("identify", openid, unionid);
```
{% endhint %}

**注意：如果你的微信小程序在用户打开后不要求用户授权获取openid和/或 unionid，但是设置了forceLogin为True，那么GroiwngIO不能采集到用户的数据，采集到的用户会偏少，所以请特别注意这个参数的设置。如果您不能确定是否要设置这个参数，请先咨询我们。**

#### SDK 微信用户属性设置

作为用户行为数据分析工具，用户信息的完善会给后续的分析带来很大的帮助。在小程序中，微信用户属性是非常重要的设置，只有完善了微信用户属性信息，微信的访问用户变量（如下表）才可以在分析工具中使用，交互数据定义、数据校验功能才会方便通过用户微信相关的信息（微信姓名和头像）定位用户。

![&#x5FAE;&#x4FE1;&#x8BBF;&#x95EE;&#x7528;&#x6237;&#x53D8;&#x91CF;](../../.gitbook/assets/image%20%28114%29.png)

下面是专门针对用户的三个接口。

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

#### 获取用户的地理信息

GrowingIO SDK 默认不会在小程序启动时获取用户的坐标信息。当用户访问到某一功能时需要位置信息时，可以调用以下位置接口，补发vst，采集位置信息，提升用户地域分布的分析准确性。

```text
gio('getLocation')
```

### \*\*小程序中有**Webview**

采集数据需要额外添加如下代码。目前**Webview的数据采集**目前暂时仅支持采用‘'[**自定义事件和变量**](./#zi-ding-yi-shi-jian-he-bian-liang)“的方式进行采集。

**1、使用如上提供的最新版 SDK**

#### 2、添加如下代码

**在 WebView 内网站中集成 WebView SDK**

```text
<!-- GrowingIO Analytics code version 2.1 -->
<!-- Copyright 2015-2017 GrowingIO, Inc. More info available at http://www.growingio.com -->
<script type='text/javascript'>
  !function(e,t,n,g,i){e[i]=e[i]||function(){(e[i].q=e[i].q||[]).push(arguments)},n=t.createElement("script"),tag=t.getElementsByTagName("script")[0],n.async=1,n.src=('https:'==document.location.protocol?'https://':'http://')+g,tag.parentNode.insertBefore(n,tag)}(window,document,"script","assets.growingio.com/gio-minp-h5.js","gio");
  gio('init', '你的 GrowingIO 项目ID', '你的微信小程序的 AppID', { });
  gio('send');
</script>
```

**在小程序里面 WebView 加载时 URL 添加额外属性**

为了支持 WebView 和小程序用户打通，需要在 WebView 加载的时候额外在 url 里面添加 giou 和 giocs1 属性，giou 是用来标识访问用户的，giocs1 是用来标识登录用户的。

1. gio\('getVisitorId'\) 获取访问用户ID
2. gio\('getUserID'\) 获取登录用户ID

举个例子，

```text
# webview.js
Page({
  data: {
    webUrl: `https://example.org/demo.html?giou=${getApp().globalData.gio('getVisitorId')}&giocs1=${getApp().globalData.gio('getUserId')}`
  } 
});
# webview.wxml
<web-view src="{{ webUrl }}"></web-view>
```



### 3、添加请求服务器域名

要正常采集微信小程序的数据并发送给 GrowingIO，需要在微信小程序里事先设置一个通讯域名，允许跟 GrowingIO API 服务器进行网络通信。具体步骤如下：

1. 登陆微信小程序后台，进入配置
2. 打开开发设置，到服务器域名配置部分
3. 在`request合法域名`中添加：https://wxapi.growingio.com

![SDK &#x6DFB;&#x52A0;&#x670D;&#x52A1;&#x5668;&#x57DF;&#x540D;](../../.gitbook/assets/image%20%28238%29.png)

### 4、检测数据

当集成成功后，需要回到 GrowingIO SDK 集成页面检测数据。请在添加了跟踪代码的小程序重新启动几次，发送数据给 GrowingIO，完成安装最后一步。详情可见小程序Debugger。

## 微信小程序SDK高级设置&数据采集配置

### 设置**CRM 用户**ID

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

#### 使用了navigator组件

如果您的小程序使用了navigator组件，需要您手动绑定一个空的点击事GrowingIO才能实现跳转点击的采集。

```text
<navigator ...>
  <view bindtap="nameForThisClickButton">
     ...
  </view>
</navigator>
```

## 自定义事件和变量

{% hint style="info" %}
小程序自定义事件和变量的埋点代码，建议放在 onShow 的生命周期函数中
{% endhint %}

### 预置自定义事件

GrowingIO 预置了两个小程序的标准自定义事件：分享到群聊或好友信息和程序错误，接入SDK即可以使用。

**微信小程序分享到好友或群聊信息**

![](../../.gitbook/assets/image%20%28101%29.png)

**程序错误**

![](../../.gitbook/assets/image%20%2821%29.png)

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





