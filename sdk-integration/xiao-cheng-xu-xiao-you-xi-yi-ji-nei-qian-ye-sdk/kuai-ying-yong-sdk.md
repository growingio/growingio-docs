# 快应用 SDK

## 快应用 SDK标准接入指南

进入集成页面，选择快应用，填写应用名称和 AppID，点击「下一步」

### 1 添加跟踪代码

1.下载 gio-quickapp.js 文件，把文件放在快应用应用项目里，比如 utils 目录下。

```bash
curl --compressed https://assets.giocdn.com/sdk/gio-quickapp.js -o gio-quickapp.js
```

2、在根目录 app.ux文件的顶部添加跟踪代码

```javascript
var gio = require("utils/gio-quickapp").default;
gio('init', '你的 GrowingIO 项目ID', '你的快应用ID(包名)', { version: '小程序版本' });

// 添加trackApp 和 trackPage 代码，如下：
// app.ux 中改写如下：
export default {
  ...
})
// 改为：
export default trackApp({
  ...
})

// 所有的Page页面的index.ux改写如下：
export default {
  ...
})
// 改为：
export default trackPage({
  ...
})
```

建议每次发布小程序新版本的时候，更新一下版本号 version，可以在 GrowingIO 分析不同版本的数据。除了 version 之外，还有以下额外参数可以使用。  


| 参数 | 值 | 解释 |
| :--- | :--- | :--- |
| version | string | 你的小程序的版本号 |
| forceLogin | true \| false | 你的快应用是否获取用户唯一标识，默认 false |
| debug | true \| false | 是否开启调试模式，可以看到采集的数据。默认 false |

### 2 添加接口权限

在您项目中的 manifest.json 文件中的 features 属性中添加权限声明代码。

```javascript
"features": [
  {"name": "system.app"},
  {"name": "system.storage"},
  {"name": "system.device"},
  {"name": "system.network"},
  {"name": "service.router"},
  {"name": "system.fetch"}
  {"name": "system.geolocation"}
];
```

### 3 SDK 快应用 用户属性设置

#### 绑定快应用用户ID

当用户在你的应用上登陆获取到 用户唯一id 后，可以用过 identify 接口绑定快应用用户ID，后续在 GrowingIO 中获取更准确的快应用访问用户量。示例代码如下：

```javascript
device.getUserId({
  success: function(res) {
    var userId = res.data.userId;
    // ...
    gio('identify', userId);
  }
})
```

#### 

#### 设置注册用户ID

当用户在你的快应用上注册以后，你的产品应用服务端会在用户数据库里添加一条记录并且分配一个 ID，可以通过 setUserId 接口设置注册用户ID，后续在 GrowingIO 中分析登录用户这个数据。示例代码如下，

```javascript
gio('setUserId', YOUR_USER_ID); 
```

#### 设置注册用户信息

当用户在你的快应用上传了注册用户ID后，可以通过 setUser 接口设置注册用户信息，后续在 GrowingIO 中分析这个数据。示例代码如下，

```javascript
gio('setUser', { id: user.id, name: user.name });
```

### 4 检测数据 <a id="jian-ce-shu-ju"></a>

当集成成功后，需要回到 GrowingIO SDK 集成页面，点击右下角“**检测数据”**。请在添加了跟踪代码的快应用重新启动几次，发送数据给 GrowingIO，完成安装最后一步。

![](../../.gitbook/assets/image%20%28343%29.png)

![](../../.gitbook/assets/image%20%28188%29.png)

### 5 进入数据校验模块，查看数据发送情况

检测成功后，可以使用[数据验证功能](../growingio-debugger/#growingio-minidebugger)，实时查看数据发送情况。

## 自定义事件和变量

### 自定义事件配置

手动发送一个自定义事件。在添加所需要发送的事件代码之前，需要在 GrowingIO 产品的`自定义事件和变量`管理页面配置事件以及事件级变量。

接口定义：

```javascript
gio('track', eventName: string, properties: object)
```

参数说明：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| eventName | string | 是 | 事件标识符 |
| properties | object | 否 | 事件级变量，即事件发生时所伴随的维度信息参数 |

示例：

```javascript
// 假设初始化后把 gio 对象放在 App 的 globalData 里面
// 在 Page 的 clickBanner 函数里添加以下代码
gio('track', 'clickBanner', { 
    id: movie.id, 
    title: movie.title, 
    index: e.currentTarget.dataset.index 
});
```

### 访问用户变量

给访问用户\(未注册你的服务的账号的用户\)附上额外的信息，便于后续做用户信息相关分析。在添加所需要设置的访问用户变量的代码之前，需要在 GrowingIO 产品的`自定义事件和变量`管理页面配置访问用户级变量。

接口定义：

```javascript
gio('setVisitor', properties: object)
```

参数说明：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| properties | object | 是 | 用户级变量，用户额外的信息参数 |

示例：

```javascript
// 比如在针对不同的用户做某个 Campaign 的 A/B 测试
gio('setVisitor', { 
  campaign_id: 3, 
  campaign_group: 'A 组用户'
});
```

### 注册用户变量

给注册用户（crm 用户ID）附上额外的信息，便于后续做用户信息相关分析。在添加所需要设置的注册用户变量的代码之前，需要在 GrowingIO 产品的`自定义事件和变量`管理页面配置注册用户级变量。

接口定义：

```javascript
gio('setUser', properties: object)
```

参数说明：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| properties | object | 是 | 用户级变量，用户额外的信息参数 |

示例：

```javascript
gio('setUser', { 
  age: 30, 
  level: '高级用户', 
  company: 'GrowingIO', 
  title: '工程师'
});
```

### 页面级变量

给当前页面附上更多的页面信息，可以作为维度拆分数据做分析。设置了页面级变量以后，这个页面的指标以及这个页面的行为指标，都可以继承使用这些维度信息做分析。在添加所需要设置的页面变量的代码之前，需要在 GrowingIO 产品的`自定义事件和变量`管理页面配置页面级变量。

接口定义：

```javascript
gio('setPage', properties: object)
```

参数说明：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| properties | object | 是 | 页面级变量，页面额外的信息参数 |

示例：

```javascript
// 下面假设我在 GrowingIO 后台已经配置了两个页面级变量 pageName 和 type
gio('setPage', { 
    pageName: '电影列表页', 
    type: this.data.type
});
```

###  转化变量

高级功能，设置一个转化信息用于高级归因分析，目前支持归因方式有最初归因、最终归因和线下归因。举个例子，如果一个用户是先后通过`活动A`、`活动B`、`活动C`来访问小程序，最后在某次后续几天后的访问购买了某个商品。如果把活动A/B/C分别设置为转化变量`campaign`的值，那么如果采用了最初归因，那么这个购买行为是由 A 贡献的；如果是最终归因，那么这次购买行为是 C 贡献的；如果是线性归因，那么这次购买行为是 A/B/C 各占 1/3 贡献。在添加所需要设置的转化变量的代码之前，需要在 GrowingIO 产品的`自定义事件和变量`管理页面配置转化级变量。

接口定义：

```javascript
gio('setEvar', properties: object)
```

参数说明：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| properties | object | 是 | 转化级变量，转化信息 |

示例：

```javascript
gio('setEvar', {
  campaign: '活动A'
});
```





