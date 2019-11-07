---
description: 自动采集 App 内嵌 H5 的用户行为数据。
---

# Hybrid SDK \(App内嵌H5\)

## Hybrid SDK 简介

集成 [Android](android-sdk/android-sdk.md) 或 [iOS](ios-sdk/ios-sdk-2.x.md) 无埋点 SDK 后，原生 SDK 会自动在 WebView 加载的页面中注入 Hybrid JS SDK，不需要手动集成此 SDK。

Hybrid SDK 负责采集用户在 App 中内嵌 H5 页面中的用户行为数据。

如果内嵌 H5 页面只在移动端的 App 中投放，不需要集成 Web JS SDK 可直接使用移动端圈选查看采集的数据。

如果内嵌 H5 页面不仅在移动端投放，还可在 Web 端浏览，需要集成 Web JS SDK 并使用 Web 圈选，可以拆分出移动端和 Web 浏览器的数据。 

{% hint style="info" %}
如果同时存在 web js sdk 和 hybrid js sdk , 埋点调用代码只用写一遍，数据会自动发两份。
{% endhint %}

## 重要配置项

### 1. 在 App 中禁用 Web JS SDK

如果 H5 页面已经集成过 Web JS SDK，但不想在 App 中进行 Web JS SDK 的采集时，请将 `window.webViewRequestSend`的值为 false。

### 2. Touch 点击事件采集

Hybrid 支持基于 touch 事件实现的点击数据采集,  
如果用户使用了类似 Zepto 等三方框架，需要采集 tap 事件时，请在初始化时配置`window.hybridEnableTouch` 的值为 true。

### 3. 埋点时机配置项

如果用户需要在hybrid界面加载过程中或者加载完成后立刻调用埋点方法，需要在该H5页面的script标签最前端添加如下代码

```javascript
(function(){
	window["gio"] = window["gio"] || function(){
		(window["gio"].q = window["gio"].q || []).push(arguments);
	}
	gio('init', 'fakeAccountID');
})()
```

为了不影响用户 H5 页面的加载速度，我们优先加载用户的页面再注入Hybrid JS SDK ，保证用户页面先加载。这样就引出一个问题：用户的界面在加载过程中或者加载完成后立刻调用埋点方法会出现gio未定义，因为这时候hybrid js sdk可能还没有完全注入成功。



## **埋点 API** 

{% hint style="info" %}
**原生无埋点 SDK 2.2 及以上支持。**
{% endhint %}

{% hint style="warning" %}
注：如果无法进行H5页面与原生应用联调的情况下可手动在head标签中加入以下代码，上线时删除即可

 &lt;script src="https://assets.giocdn.com/sdk/hybrid/2.0/gio\_hybrid.min.js" &gt;&lt;/script&gt;
{% endhint %}

### 1. 设置自定义事件和事件级变量（track）

在添加所需要发送的事件代码之前，需要在打点管理用户界面配置事件以及事件级变量。

| 参数名称   |  参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| eventId    | String | 是 | 事件标识符 |
| number | Number | 否 | 事件的数值，没有number参数时，事件默认加1；当出现number参数时，事件自增number的数值。 |
| eventLevelVariables | JSON Object | 否 | 包含事件级变量的JSON对象，暨事件发生时所伴随的维度信息。 |

```javascript
// track API原型
gio('track', eventId, eventLevelVariables);
gio('track', eventId, number, eventLevelVariables);
// track API调用示例一
gio('track', 'registerSuccess');
// track API调用示例二
gio('track', 'registerSuccess', {'gender':'male', 'age':21});
// track API调用示例三
gio('track', 'loanAmount', 800000, {'loanType':'houseMortgage','province':'Zhejiang'})
```

### 2. 设置页面级变量（page.set）

发送页面级别的维度信息，在添加代码之前必须在打点管理界面上声明页面级变量。

| 参数名称  | 参数类型  | 是否必须  | 说明 |
| :--- | :--- | :--- | :--- |
| key | String | 否 | 页面级变量的标识符 |
| value | String | 否 | 页面级变量的值 |
| pageLevelVariables | JSON Object | 否 | 包含页面级变量的JSON对象，暨页面级别的信息 |

```javascript
// page.set API原型
gio('page.set', key, value);
gio('page.set', pageLevelVariables);
// page.set API调用示例一
gio('page.set', {'pageName': 'Home Page', 'author': 'Zhang San'});
// page.set API调用示例二
gio('page.set', 'author', 'Zhang San');
```

### 3. 设置转化变量（evar.set）

发送一个转化信息用于高级归因分析，在添加代码之前必须在打点管理界面上声明转化变量。

| 参数名称    | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| key | String | 否 | 转化变量的标识符 |
| Value | String | 否 | 转化变量的值 |
| conversionVariables | JSON Object | 否 | 包含转化变量的JSON对象 |

```javascript
// evar.set API原型
gio('evar.set', key, value);
gio('evar.set', conversionVariables);
// evar.set API调用示例一
gio('evar.set', 'campaignId'，'1234567890');
// evar.set API调用示例二
gio('evar.set', {'campaignId': '1234567890', 'campaignOwner':'lisi'});
```

### 4. 设置用户级变量（people.set）

发送用户信息用于用户信息相关分析，在添加代码之前必须在打点管理界面上声明转化变量。

| 参数名称   | 参数类型 | 是否必须 |  说明 |
| :--- | :--- | :--- | :--- |
| key | String | 否 | 用户变量的标识符 |
| value | String | 否 | 用户变量的值 |
| customerVariables | JSON Object | 否 | 包含用户变量的JSON对象 |

```javascript
// people.set API原型
gio('people.set', key, value);
gio('people.set', customerVariables);
// people.set API调用示例一
gio('people.set', 'gender', 'male');
//people.set API调用示例二
gio('people.set', {'gender':'male', 'age':'25'});
```

### 5. 设置用户id（hybridSetUserId）

设置用户id 。

| 参数名称    | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| customerVariables | String | 是 | 长度不可以大于1000,并且不可为Null |

```javascript
//调用示例
gio('hybridSetUserId', '1234567890');
```

### 6. 清除用户id（hybridClearUserId）

```javascript
//调用示例
gio('hybridClearUserId');
```

### 7. 设置访问用户变量

```javascript
//调用示例
gio('hybridSetVisitor',{'testkey': 'testValue', 'testNumKey': 2333});
```

## 

