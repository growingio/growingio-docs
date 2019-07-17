---
description: 目前微信内嵌页 SDK 属于内测beta版本，如有需求，请联系GrowingIO和您对接的客户成功经理或销售，申请灰度试用。
---

# 微信内嵌页 SDK \(beta\)

如果您的H5应用是应用在公众号、微信环境的H5应用、或者小程序中，但是希望能够独立统计数据的话，可以使用微信内嵌页的SDK。

## 微信内嵌页SDK集成前工作 <a id="wei-xin-nei-qian-ye-sdk-ji-cheng-qian-gong-zuo"></a>

### 创建新的GrowingIO项目 <a id="chuang-jian-xin-de-growingio-xiang-mu"></a>

如果你还未注册 GrowingIO，请[点击链接访问注册页面](https://accounts.growingio.com/signup?utm_source=docs&utm_content=minp)，完成注册后你会看到使用引导，点击“**添加跟踪代码**“即可开始。

如果你已有在使用的项目，要在一个新项目中集成微信内嵌页应用，在你的 GrowingIO 项目页面点击右上角项目切换控件，在下拉框点击“**项目管理”**，在弹出的列表中选择“**项目概览**“。在项目概览页面，点击“**新建项目**“来创建一个新项目。在创建好的新项目里，你会看到使用引导，点击“**添加跟踪代码**“即可开始。

​在代码集成页面，选择“微信内嵌页“平台，输入**应用名称**，点击下一步。

### 在项目中接入一个新的微信内嵌页应用 <a id="zai-xiang-mu-zhong-jie-ru-yi-ge-xin-de-wei-xin-nei-qian-ye-ying-yong"></a>

在你的 GrowingIO 项目页面点击右上角点击小齿轮，在弹出的列表中选择“应用管理“。在应用管理页面，点击“**新建应用**“来创建一个新应用。

​![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LH3J3bJm8y879FZ1E7G%2F-LH3KSslW7JKLy4bb98K%2Fimage.png?alt=media&token=04f949af-bbb1-49f5-98e2-c212022a9c1d)​

然后会到达SDK集成页面，填写微信内嵌页的应用名称，点击“下一步”，即可以到达微信内嵌页SDK接入的页面。

目前暂不支持微信内嵌页产品和web\iOS\Android端产品集成在同一项目下，微信端的产品可以集成在同一项目下。

如果需要集成其他端产品，或者需要在已有web/iOS/Andoird产品集成的情况下，集成微信内嵌页产品，请创建新的项目。

请按照以下步骤进行 WeChat SDK 安装。

## 微信内嵌页 SDK标准接入 <a id="wei-xin-nei-qian-ye-sdk-biao-zhun-jie-ru"></a>

将以下深色区域内的 JS 代码复制到您所需分析页面中的 **&lt;head&gt;** 和 **&lt;/head&gt;** 标签之间即可。安装成功后，除 localhost 和 IP 地址外，所有网址下的行为数据都将会被收集。

```javascript
<script type="text/javascript">
      !function(e,t,n,g,i){e[i]=e[i]||function(){(e[i].q=e[i].q||[]).push(arguments)},n=t.createElement("script"),tag=t.getElementsByTagName("script")[0],n.async=1,n.src=('https:'==document.location.protocol?'https://':'http://')+g,tag.parentNode.insertBefore(n,tag)}(window,document,"script","assets.growingio.com/gio-wxwv.js","gio");
      // ‘你的appid’为选填项，如果你的微信内嵌页应用有微信分配的appid，建议填写；如果没有，可以留空。
      gio('init', '你的项目ID', '你的 appid', { debug: false });
      gio('send');
</script>
```

微信内嵌页 SDK 还有以下额外参数可以使用：

| 参数 | 值 | 解释 |
| :--- | :--- | :--- |
| hashtag | true \| false | GrowingIO默认不会把 hashtag 识别成页面 URL 的一部分。对于使用 hashtag 进行页面跳转的单页面网站应用来说，可以启用 hashtag 作为标识页面的一部分，将hashtag设置为true，默认为false。 |
| debug | true \| false | 开启debug可以进行数据的实时调试，默认为false，调试方式为打开开发者工具，在console中查看。 |
| touch | true \| false | 设置是否支持touch事件，如果为true则会采集touch事件，否则采集click事件。sdk中会判断当前是否支持touch事件设置默认值。 |

## 微信内嵌页 SDK高级设置 <a id="wei-xin-nei-qian-ye-sdk-gao-ji-she-zhi"></a>

### hashtag参数 <a id="hashtag-can-shu"></a>

GrowingIO默认不会把 hashtag 识别成页面 URL 的一部分。对于使用 hashtag 进行页面跳转的单页面网站应用来说，可以启用 hashtag 作为标识页面的一部分，将hashtag设置为true

| 参数 | 值 | 解释 |
| :--- | :--- | :--- |
| hashtag | true \| false | GrowingIO默认不会把 hashtag 识别成页面 URL 的一部分。对于使用 hashtag 进行页面跳转的单页面网站应用来说，可以启用 hashtag 作为标识页面的一部分，将hashtag设置为true，默认为false |

即微信小程序项目根目录的 app.js 文件设置参数如下：

```text
//如果内嵌页存在微信App_id，建议您填写相应的微信App_id,如果没有，就不用填写
gio('init', '你的项目ID'[,'微信App_id'], { setImp:false, hashtag: true });
```

### SDK 微信用户ID 和 用户属性 <a id="sdk-wei-xin-yong-hu-shu-xing-she-zhi"></a>

作为用户行为数据分析工具，用户信息的完善会给后续的分析带来很大的帮助。在微信内嵌页中，微信用户属性是非常重要的设置，只有完善了微信用户属性信息，微信的访问用户变量（如下表）才可以在分析工具中使用，交互数据定义、数据校验功能才会方便通过用户微信相关的信息（微信姓名和头像）定位用户。

下面是专门针对用户的两个个接口。

#### 绑定微信用户ID <a id="bang-ding-wei-xin-yong-hu-id"></a>

当用户在你的微信内嵌页上授权获取到 openid 后，可以用过 `identify` 接口绑定微信用户ID，后续在 GrowingIO 中使用微信ID创建用户分群。示例代码如下，

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

#### 设置微信用户信息 <a id="she-zhi-wei-xin-yong-hu-xin-xi"></a>

当用户在你的微信内嵌页上绑定微信信息后，可以通过 `setVisitor` 接口设置微信用户信息，后续在 GrowingIO 中，使用访问用户变量分析这个数据。示例代码如下，

```text
wx.getUserInfo({ 
  success: res => 
    ...
    gio('setVisitor', res.userInfo);
})
```

微信信息包含**微信昵称**、**微信头像**、**性别、微信所填国家、微信所填省份、微信所填城市**。

![](../.gitbook/assets/image%20%28268%29.png)

### 登录用户ID <a id="deng-lu-yong-hu-id"></a>

设置登录用户ID，可以将用户行为和您业务系统中的用户ID打通，有助于您在分析用户时，能够进一步了解业务价值上的用户核心行为。

#### 设置登录用户 ID（setUserId） <a id="she-zhi-deng-lu-yong-hu-idsetuserid"></a>

当用户登录之后调用 setUserId API ，设置登录用户 ID 。

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| userId | String | 是 | 用户的登录ID |

```text
//setUserId API原型gio('setUserId', userId);
```

```text
//setuserId API调用示例gio('setUserId', '1234567890');
```

#### 清除登录用户 ID（clearUserId） <a id="qing-chu-deng-lu-yong-hu-idclearuserid"></a>

当用户登出之后调用 clearUserId ，清除已经设置的登录用户 ID 。

```text
//clearUserId API原型和调用示例gio('clearUserId');
```

## 微信内嵌页自定义事件和变量 <a id="wei-xin-nei-qian-ye-zi-ding-yi-shi-jian-he-bian-liang"></a>

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

### 登录用户变量 <a id="deng-lu-yong-hu-bian-liang"></a>

给登录用户附上额外的信息，便于后续做用户信息相关分析。在添加所需要设置的登录用户变量的代码之前，需要在 GrowingIO 产品的`自定义事件和变量`管理页面配置注册用户级变量。

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

​  


