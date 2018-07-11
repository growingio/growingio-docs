# 小程序SDK标准接入指南

## 下载小程序采集 SDK

下载 gio-minp.js 文件

```text
$ curl https://assets.growingio.com/gio-minp.js -o gio-minp.js
```

当下载到 gio-minp.js 文件以后，把文件放在微信小程序项目里，比如 utils 目录下。下面会假设 SDK 文件放在 utils 目录下。

## 添加追踪代码

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

## 添加请求服务器域名

要正常采集微信小程序的数据并发送给 GrowingIO，需要在微信小程序里事先设置一个通讯域名，允许跟 GrowingIO API 服务器进行网络通信。具体步骤如下：

1. 登陆微信小程序后台，进入配置
2. 打开开发设置，到服务器域名配置部分
3. 在`request合法域名`中添加：https://wxapi.growingio.com

![SDK &#x6DFB;&#x52A0;&#x670D;&#x52A1;&#x5668;&#x57DF;&#x540D;](../../.gitbook/assets/image%20%2820%29.png)

## 检测数据

当集成成功后，需要回到 GrowingIO SDK 集成页面检测数据。请在添加了追踪代码的小程序重新启动几次，发送数据给 GrowingIO，完成安装最后一步。详情可见[小程序Debugger](https://growingio.gitbook.io/docs/~/drafts/-LH61Ni6lwUWAkWLDYpB/primary/sdk-integration/growingio-debugger#growingio-xiao-cheng-xu-debugger)。

