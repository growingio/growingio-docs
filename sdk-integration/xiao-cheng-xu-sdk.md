# 小程序 SDK

## SDK集成前工作

### 集成SDK的阶段和流程

![](../.gitbook/assets/image%20%288%29.png)

如果您已经注册GrowingIO,并且已经有创建或者集成SDK的项目了，您可以进行如下选择：

1. 使用已有项目的ID，添加新的应用，集成SDK。
2. 创建一个新的项目，然后集成SDK。

`*注：如果已有项目且集成了其他应用，新建的项目，数据不能和原有项目中的数据合并。例如：拥有小程序A，iOS端产品，如果各个应用集成在同一个项目ID中，且都上传登录用户ID，登录用户计算时会去重，并且在使用高级分析功能时，可以同时选择跨平台的行为和登录用户。但是如果小程序和APP不在同一个项目ID下，登录用户计算分别在两个项目中，不会去重计算；高级分析功能中，也不能跨平台选择登录用户ID和行为。`

如果你还未注册 GrowingIO，请[点击链接访问注册页面](https://accounts.growingio.com/signup?utm_source=docs&utm_content=minp)，完成注册后你会看到使用引导，点击“**添加追踪代码**“即可开始。

### 创建应用或项目

{% tabs %}
{% tab title="在已有项目中添加小程序应用" %}
在你的 GrowingIO 项目页面点击右上角项目切换控件，在下拉框点击“**项目管理”**，在弹出的列表中选择“应用管理“。在项目概览页面，点击“**新建应用**“来创建一个新应用。

![](../.gitbook/assets/image%20%2811%29.png)

![](../.gitbook/assets/image%20%282%29.png)

然后会到达SDK集成页面，填写小程序的应用名称，和小程序的AppID ,点击“下一步”，即可以到达小程序SDK接入的页面。

![](../.gitbook/assets/image%20%286%29.png)
{% endtab %}

{% tab title="创建新的GrowingIO项目" %}
如果你还未注册 GrowingIO，请[点击链接访问注册页面](https://accounts.growingio.com/signup?utm_source=docs&utm_content=minp)，完成注册后你会看到使用引导，点击“**添加追踪代码**“即可开始。

如果你已经注册 GrowingIO，使用小程序分析功能需要用一个全新的项目的话，在你的 GrowingIO 项目页面点击右上角项目切换控件，在下拉框点击“**项目管理”**，在弹出的列表中选择“**项目概览**“。在项目概览页面，点击“**新建项目**“来创建一个新项目。在创建好的新项目里，你会看到使用引导，点击“**添加追踪代码**“即可开始。

![](../.gitbook/assets/image%20%284%29.png)

![](../.gitbook/assets/image%20%2810%29.png)

在代码集成页面，选择“**小程序**“平台，输入**应用名称**和**小程序 AppID**，点击“下一步”。
{% endtab %}
{% endtabs %}

## SDK标准接入指南

### 下载小程序采集 SDK

下载 gio-minp.js 文件

```text
$ curl https://assets.growingio.com/gio-minp.js -o gio-minp.js
```

当下载到 gio-minp.js 文件以后，把文件放在微信小程序项目里，比如 utils 目录下。下面会假设 SDK 文件放在 utils 目录下。

### 添加追踪代码

在微信小程序项目根目录的 app.js 文件的顶部添加以下 JS 代码，请注意一定要放在 App\(\) 之前：

```text
var gio = require("utils/gio-minp.js");// version 是你的小程序的版本号gio('init', '你的 GrowingIO 项目ID', '你的微信小程序的 AppID', { version: '1.0' });
```

其中GrowingIO 项目ID、微信小程序的 AppID，即为**SDK安装页面** 第②部分 **代码框中生成的代码。**

建议每次发布小程序新版本的时候，更新一下版本号 version，可以在 GrowingIO 分析不同版本的数据。除了 `version` 之外，还有以下额外参数可以使用。

| 参数 | 值 | 解释 |
| --- | --- | --- | --- |
| forceLogin | true \| false | 你的小程序是否强制要求用户登陆微信获取 openid，默认 false |
| debug | true \| false | 是否开启调试模式，可以看到采集的数据，默认 false |
| version | string | 你的小程序的版本号 |

{% hint style="info" %}
forceLogin 是一个需要特别注意的参数。GrowingIO 默认会在小程序里面设置用户标识符，存储在微信 Storage 里面。这个用户标识符潜在可能会被`clearStorage` 清除掉，所以有可能不同的用户标识符对应同一个微信里的 `openid`。如果你的微信小程序在用户打开后会去做登陆并且获取 `openid` 和/或 `unionid`，强烈建议设置 `forceLogin` 为 true。当 forceLogin 为 true 的时候，用户标识符会使用 openid。具体集成示例：



```text
gio('init', '你的 GrowingIO 项目ID', '你的微信小程序的 AppID', { version: '1.0', forceLogin: true });
...
// 当获取到 openid 后，调用以下方法
gio("identify", openid, unionid);
```
{% endhint %}

### 特别注意

如果你使用 mpvue 来开发小程序的应用，为了让 GrowingIO SDK 能自动采集到真实行为事件，需要把 Vue 作为初始化参数传入。比如在 main.js 这样子集成，

```text
import gio from './utils/gio-minp'
import Vue from 'vue'
import App from './App'
​
gio(‘init‘, '你的 GrowingIO 项目ID', '你的微信小程序的 AppID', { vue: Vue, version: '1.0' });
```

gio-minp 默认是用 module.exports, 对于 import 不支持，可以修改 gio-minp.js 最后部分代码，把 `, module.exports = gio`修改成`; export default gio`。

### 添加请求服务器域名

要正常采集微信小程序的数据并发送给 GrowingIO，需要在微信小程序里事先设置一个通讯域名，允许跟 GrowingIO API 服务器进行网络通信。具体步骤如下：

1. 登陆微信小程序后台，进入配置
2. 打开开发设置，到服务器域名配置部分
3. 在`request合法域名`中添加：https://wxapi.growingio.com

![SDK &#x6DFB;&#x52A0;&#x670D;&#x52A1;&#x5668;&#x57DF;&#x540D;](../.gitbook/assets/image%20%2812%29.png)

### 检测数据

当集成成功后，需要回到 GrowingIO SDK 集成页面检测数据。请在添加了追踪代码的小程序重新启动几次，发送数据给 GrowingIO，完成安装最后一步。详情可见[小程序Debugger](https://growingio.gitbook.io/docs/~/drafts/-LH61Ni6lwUWAkWLDYpB/primary/sdk-integration/growingio-debugger#growingio-xiao-cheng-xu-debugger)。

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

### 无埋点采集事件

#### 访问数据

每次用户打开小程序的时候，发送一条`应用打开`消息，对应的数据指标是打开次数。发送数据包含但不限于以下信息：进入页面、进入时间、场景值、来源小程序或 App、设备信息、微信信息、应用版本号。

每次用户关闭小程序的时候，关闭动作包括退出小程序回到微信、退出微信，会发送一条`应用关闭`消息，会根据这个消息来计算应用访问时长。发送数据包含但不限于以下信息：退出页面、退出时间。

#### 页面数据

每次用户访问一个新的页面，发送一条`页面打开`消息，对应的数据意义是页面分析。发送数据包含但不限于以下信息：页面信息、打开时间、页面来源、页面标题、页面级变量。

每次当用户点击转发按钮时，会弹出转发框，这时会发送一条`要转发消息`消息。这是一个自定义事件，数据包含以下信息：事件时间、所在页面、转发动作来源、转发页面标题和转发页面路径。注意这个事件不代表用户真正转发了消息到聊天里面，而是用户触发了转发动作。如果要跟踪成功转发消息事件，建议在 onShareAppMessage 的 success callback 里面发送一个自定义事件。

#### 行为数据

对于用户在页面发生的行为，如果这个行为有绑定事件比如 bindtap，并且在你的小程序里面进行处理，那么 GrowingIO 的小程序 SDK 会自动采集这些事件，发送一条行为消息。目前，我们支持的事件有 tap、longpress、confirm 和 change 事件。

#### tap {#tap}

tap 事件是手指触摸后马上离开时触发的事件。当 wxml 中的 view 绑定了 bindtap 事件以后，在事件处理函数执行的时候，SDK 会自动采集 tap 事件，发送数据包含但不限于以下信息：点击事件时间、事件发生所在页面、点击控件相关信息。比如如下，

```text
<view data-title='复仇者联盟3' data-index='1' bindtap='clickMovie'>  <image src='IMAGE—URL' mode='aspectFill'/>  <text>复仇者联盟3</text></view>
```

注意这里的 `data-title` 和 `data-index` 属性，因为微信小程序的限制，无法采集到控件的内容和结构数据，所以在小程序 SDK 里面我们采取的是声明式编程，通过在 wxml 文件里面设置 data- 属性，可以给 view 控件添加额外的`内容`和`位置`属性，方便后续在分析时可以按照元素内容和元素位置做分析，对于列表式的组件特别有用和方便。

**建议在 View 的控件里面多使用 data-title 和 data-index 属性这种声明式编程方式。**

#### longpress {#longpress}

longpress 事件是手指触摸后，超过350ms再离开时触发的事件。当 wxml 的 view 绑定了 bindlongpress 事件以后，在事件处理函数执行的时候，SDK 会自动采集 longpress 事件，发送数据包含但不限于以下信息：点击事件时间、事件发生所在页面、点击控件相关信息。比如如下，

```text
<view data-title='复仇者联盟3' data-index='1' bindtap='clickMovie'>
  <image src='IMAGE—URL' mode='aspectFill'/>
  <text>复仇者联盟3</text>
</view>
```

注意这里的 `data-title` 和 `data-index` 属性，因为微信小程序的限制，无法采集到控件的内容和结构数据，所以在小程序 SDK 里面我们采取的是声明式编程，通过在 wxml 文件里面设置 data- 属性，可以给 view 控件添加额外的`内容`和`位置`属性，方便后续在分析时可以按照元素内容和元素位置做分析，对于列表式的组件特别有用和方便。

**建议在 View 的控件里面多使用 data-title 和 data-index 属性这种声明式编程方式。**

#### change {#change}

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

#### confirm {#confirm}

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

