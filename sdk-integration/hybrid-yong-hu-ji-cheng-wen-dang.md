# Hybrid 用户集成文档

{% hint style="info" %}
```
1.Hybrid项目只需集成原生SDK即可，原生SDK会自动在webview加载的页面中注入
Hybrid JS SDK
2.如果h5页面已经集成过 Web JS SDK ，但不想进行Web JS SDK 的采集时，
请将window.webViewRequestSend的值为 false。 
3. hybrid 已经支持 tap 等基于touch事件 实现的点击的采集, SDK默认不采集tap ,
如果用户需要采集需要在初始化时配置 window.hybridEnableTouch的值为true
```
{% endhint %}

##  **打点事件（原生 SDK 2.2 开始支持）**

### 1，设置自定义事件和事件级变量（track）

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

### 2，设置页面级变量（page.set）

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

### 3,设置转化变量（evar.set）

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

### 4,设置用户级变量（people.set）

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

### 5,设置用户id（hybridSetUserId）

设置用户id 。

| 参数名称    | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| customerVariables | String | 是 | 长度不可以大于1000,并且不可为Null |

```javascript
//调用示例
gio('hybridSetUserId', '1234567890');

```

### 6，清除用户id（hybridClearUserId）

```javascript
//调用示例
gio('hybridClearUserId');

```

### 7，设置访问用户变量

```javascript
//调用示例
gio('hybridSetVisitor',{'testkey': 'testValue', 'testNumKey': 2333});
```

{% hint style="warning" %}
注：如果无法进行H5页面与原生应用联调的情况下可手动在head标签中加入以下代码，上线时删除即可

 &lt;script src="https://assets.growingio.com/sdk/hybrid/2.0/gio\_hybrid.min.js"&gt;&lt;/script&gt; 
{% endhint %}

## 

