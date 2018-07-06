# Web JS SDK

* [集成 SDK 和 API 导入](https://growingio.gitbook.io/docs/~/drafts/-LGduzeZ7QWp537-6Hk-/primary/sdk-integration/web-js-sdk#ji-cheng-sdk-he-api-dao-ru)
  * [1.SDK 代码安装](https://growingio.gitbook.io/docs/~/drafts/-LGduzeZ7QWp537-6Hk-/primary/sdk-integration/web-js-sdk#sdk-%E5%AE%89%E8%A3%85)
  * [2.Web JS 系统变量](https://growingio.gitbook.io/docs/~/drafts/-LGduzeZ7QWp537-6Hk-/primary/sdk-integration/web-js-sdk#web-js-%E7%B3%BB%E7%BB%9F%E5%8F%98%E9%87%8F)
  * [3.Web JS SDK 2.1 API](https://growingio.gitbook.io/docs/~/drafts/-LGduzeZ7QWp537-6Hk-/primary/sdk-integration/web-js-sdk#3-web-js-sdk-2-1-api)
  * [4.自定义数据上传 & 配置指导](https://growingio.gitbook.io/docs/~/drafts/-LGduzeZ7QWp537-6Hk-/primary/sdk-integration/web-js-sdk#%E8%87%AA%E5%AE%9A%E4%B9%89%E6%95%B0%E6%8D%AE%E4%B8%8A%E4%BC%A0%E9%85%8D%E7%BD%AE%E6%8C%87%E5%AF%BC)
* [1.x 旧版本 SDK 升级指导](https://growingio.gitbook.io/docs/~/drafts/-LGduzeZ7QWp537-6Hk-/primary/sdk-integration/web-js-sdk#1x-jiu-ban-ben-sdk-sheng-ji-zhi-dao)
  * [1. 重新集成 SDK](https://growingio.gitbook.io/docs/~/drafts/-LGduzeZ7QWp537-6Hk-/primary/sdk-integration/web-js-sdk#1-zhong-xin-ji-cheng-sdk)
  * [2. 迁移用户属性字段（CS字段）](https://growingio.gitbook.io/docs/~/drafts/-LGduzeZ7QWp537-6Hk-/primary/sdk-integration/web-js-sdk#2-qian-yi-yong-hu-shu-xing-zi-duan-cs-zi-duan)
  * [3. 迁移页面属性字段（PS字段）](https://growingio.gitbook.io/docs/~/drafts/-LGduzeZ7QWp537-6Hk-/primary/sdk-integration/web-js-sdk#3-qian-yi-ye-mian-shu-xing-zi-duan-ps-zi-duan)
  * [4. 迁移自定义事件（埋点事件）](https://growingio.gitbook.io/docs/~/drafts/-LGduzeZ7QWp537-6Hk-/primary/sdk-integration/web-js-sdk#4-qian-yi-zi-ding-yi-shi-jian-mai-dian-shi-jian)
  * [5. 数据校验](https://growingio.gitbook.io/docs/~/drafts/-LGduzeZ7QWp537-6Hk-/primary/sdk-integration/web-js-sdk#5-shu-ju-xiao-yan)
* [GrowingIO Web Debugger](https://growingio.gitbook.io/docs/~/drafts/-LGduzeZ7QWp537-6Hk-/primary/sdk-integration/web-js-sdk#growingio-web-debugger)

## 集成 SDK 和 API 导入

### 1.SDK 代码安装 {#sdk-安装}

重要：如果您目前已经在使用GrowingIO 1.x 版本的 SDK，希望升级到最新版本 \(V2.1），您需要联系您的 GrowingIO 对接人，我们需要帮您开启新版本 SDK 的功能。如果您忽略该提醒自助升级，而后台对应功能未开启的话，可能会带来数据丢失的问题。[如何升级?](web-js-sdk.md#1x-版本-sdk-升级指导)

#### 1.1 **页面代码如下所示**

**请将以下的页面代码放置到需要分析的页面中的 &lt;head&gt; 和 &lt;/head&gt; 标签之间，即可完成 Web JS SDK 2.1页面代码的安装。**

```text
<!-- GrowingIO Analytics code version 2.1 -->
<!-- Copyright 2015-2017 GrowingIO, Inc. More info available at http://www.growingio.com -->
<script type='text/javascript'>
!function(e,t,n,g,i){e[i]=e[i]||function(){(e[i].q=e[i].q||[]).push(arguments)},n=t.createElement("script"),tag=t.getElementsByTagName("script")[0],n.async=1,n.src=('https:'==document.location.protocol?'https://':'http://')+g,tag.parentNode.insertBefore(n,tag)}(window,document,"script","assets.growingio.com/2.1/gio.js","gio");
  gio('init', 'your projectId', {});

  //custom page code begin here


  //custom page code end here

  gio('send');
</script>
<!-- End GrowingIO Analytics code version: 2.1 -->
```

#### 1.2 **gio.js 文件**

可以看到页面代码中有对 gio.js 文件的引用，gio.js 的地址为[http://assets.growingio.com/2.1/gio.js](https://github.com/growingio/help_site/tree/f4b4103b288205f6a9b13e0c4692f4d65a2ab386/assets.growingio.com/2.1/gio.js)

请注意使用具体的项目 ID 替换上述代码中 ‘your projectId’ 的部分。

建议在安装页面代码结束之后，建议使用 GrowingIO Web Debugger 验证一下服务器请求发送是否正常。

### 2.Web JS 系统变量 {#web-js-系统变量}

Web JS SDK 可以配置一些系统变量来控制 Web JS SDK 的数据发送。

有以下系统变量在实施时可以使用以下这些系统变量：

#### 2.1 imp 系统变量

作用：禁止元素浏览量采集

GrowingIO 提供两种采集，元素浏览和元素点击，文本框内容修改等交互行为。对于内容基本固定的网站来说，可以直接禁用元素浏览量采集。

```text
gio('config', {'imp':false});
```

#### 2.2 hashtag 系统变量

作用：启用 hashtag 作为标识页面的一部分

默认值：false

GrowingIO默认不会把 hashtag 识别成页面URL的一部分。对于使用 hashtag 作为单页应用页面切换的网站来说，可以使用

```text
gio('config', {'hashtag':true});
```

来监听 hashtag 的变化，并区分页面来收集页面数据，每次 hashtag 改变都会触发一次PV，hashtag 的信息也会记录在页面URL中。

### 3.Web JS SDK 2.1 API

#### 3.1 API 简介

```text
// 初始化参数
gio('init', projectId, options); 

// 修改系统变量API
gio('config', options);

// 发送事件API
gio('track', eventId);
gio('track', eventId, number);
gio('track', eventId, eventLevelVariables);
gio('track', eventId, number, eventLevelVariables);

// 发送页面级变量API
gio('page.set', key, value);
gio('page.set', pageLevelVariables);

// 发送转化变量API
gio('evar.set', key, value);
gio('evar.set', conversionVariables);

// 发送用户变量API
gio('people.set', key, value);
gio('people.set', customerVariables);

// 设置登录用户ID
gio('setUserId', userId); 

// 清除登录用户ID
gio('clearUserId');
```

#### 3.2 init  {#init}

初始化参数，用来设置项目ID和一些常用的配置项：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- |
| projectId | String | 是 | 项目ID |
| options | JSON Object | 否 | 系统变量配置 |

```text
//init API原型
gio('init', projectId, options);
```

```text
//init API调用示例
//配置imp类型的数据关闭发送
gio('init', '1234567890', {'imp':false});
```

#### 3.3 设置自定义事件和事件级变量（track） {#track}

在添加所需要发送的事件代码之前，需要在打点管理用户界面配置事件以及事件级变量。

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| eventId | String | 是 | 事件标识符 |
| number | Number | 否 | 事件的数值，没有number参数时，事件默认加1；当出现number参数时，事件自增number的数值。 |
| eventLevelVariables | JSON Object | 否 | 包含事件级变量的JSON对象，暨事件发生时所伴随的维度信息。 |

```text
// track API原型
gio('track', eventId, eventLevelVariables);
gio('track', eventId, number, eventLevelVariables);
```

```text
// track API调用示例一
gio('track', 'registerSuccess');
```

```text
// track API调用示例二
gio('track', 'registerSuccess', {'gender':'male', 'age':21});
```

```text
// track API调用示例三
gio('track', 'loanAmount', 800000, {'loanType':'houseMortgage','province':'Zhejiang'});
```

#### 3.4 设置页面级变量（page.set） {#pageset}

发送页面级别的维度信息，在添加代码之前必须在打点管理界面上声明页面级变量。

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| key | String | 否 | 页面级变量的标识符 |
| value | String | 否 | 页面级变量的值 |
| pageLevelVariables | JSON Object | 否 | 包含页面级变量的JSON对象，暨页面级别的信息 |

```text
// page.set API原型
gio('page.set', key, value);
gio('page.set', pageLevelVariables);
```

```text
// page.set API调用示例一
gio('page.set', {'pageName': 'Home Page', 'author': 'Zhang San'});
```

```text
// page.set API调用示例二
gio('page.set', 'author', 'Zhang San');
```

#### 3.5 设置转化变量（evar.set） {#evarset}

发送一个转化信息用于高级归因分析，在添加代码之前必须在打点管理界面上声明转化变量。

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| key | String | 否 | 转化变量的标识符 |
| Value | String | 否 | 转化变量的值 |
| conversionVariables | JSON Object | 否 | 包含转化变量的JSON对象 |

```text
// evar.set API原型
gio('evar.set', key, value);
gio('evar.set', conversionVariables);
```

```text
// evar.set API调用示例一
gio('evar.set', 'campaignId'，'1234567890');
```

```text
// evar.set API调用示例二
gio('evar.set', {'campaignId': '1234567890', 'campaignOwner':'lisi'});
```

#### 3.6 设置用户级变量（people.set） {#peopleset}

发送用户信息用于用户信息相关分析，在添加代码之前必须在打点管理界面上声明转化变量。

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| key | String | 否 | 用户变量的标识符 |
| value | String | 否 | 用户变量的值 |
| customerVariables | JSON Object | 否 | 包含用户变量的JSON对象 |

```text
// people.set API原型
gio('people.set', key, value);
gio('people.set', customerVariables);
```

```text
// people.set API调用示例一
gio('people.set', 'gender', 'male');
```

```text
//people.set API调用示例二
gio('people.set', {'gender':'male', 'age':'25'});
```

#### 3.7 设置登录用户id（setUserId） {#setuserid}

当用户登录之后调用 setUserId API ，设置登录用户 ID 。

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| --- | --- |
| userId | String | 是 | 用户的登录用户ID |

```text
//setUserId API原型
gio('setUserId', userId);
```

```text
//setuserId API调用示例
gio('setUserId', '1234567890');
```

#### 3.8 上传登录用户 ID（clearUserId） {#clearuserid}

当用户登出之后调用 clearUserId ，清除已经设置的登录用户 ID 。

```text
//clearUserId API原型和调用示例
gio('clearUserId');
```

#### 3.9 手动发送页面浏览事件 sendPage {#sendpage}

在默认情况下，由于用户浏览网站的交互行为导致当前页面的 URL 产生变化时，GrowingIO 的 Web JS SDK 会发送一个 page 类型的请求。在一些特殊的情况下，例如用户在访问单页应用（Single Page Application）类型的网站时，用户的操作会导致业务上面理解的页面产生了变化，但是当前的 URL 可能并没有改变。

这时，可以调用GrowingIO提供的 sendPage 接口手动发送页面浏览事件。这个接口的调用将会发送出一条‘page’类型的数据，GIO 服务器在收到 page 类型的数据之后，页面浏览量这个预定义指标会加 1。

```text
//sendPage API原型和调用示例
gio('sendPage');
```

### 4.自定义数据上传 & 配置指导 {#自定义数据上传配置指导}

您的 APP 或网页在集成了 GrowingIO 的 SDK 之后，它将会自动地为您采集一系列用户行为数据，并在 GrowingIO 分析后台供您制成数据分析报表。除上述的用户行为数据（或称为无埋点数据）之外，GrowingIO 还提供了多种 API 接口，供您上传一些自定义的数据指标及维度，具体请参考[相关文档](../data-model/event-variable/custom-event.md)。

## 1.x 旧版本 SDK 升级指导

如果您目前使用的是 1.x 版本的 SDK，希望升级至 2.x 版本，请注意：您需要联系您的 GrowingIO 对接人，我们需要帮您开启后台 2.x 版本所对应功能。如果您直接集成 2.x 版本，而后台对应功能未开启的话，可能会造成数据丢失的问题。

### **1. 重新集成 SDK**

* [Web JS SDK 开发文档](web-js-sdk-2.x/)

Tips：建议您在开发中，使用 debug mode 校验 GrowingIO SDK 的数据是否正常上传。开启 debug mode 的方式请见上述文档中初始化方法部分。

### **2. 迁移用户属性字段（CS字段）**

如果您未做用户属性字段上传，请忽略此部分。

用户属性字段（简称CS字段）是 1.x 版本的概念，升级至 2.x 版本后：

* CS1字段，会强制命名为“登陆用户ID”，并且上传接口与其他变量不同。
* CS2-10字段，会迁移至“应用级变量”，应用级变量与CS字段的使用方式无任何区别。
* CS11-20字段，会迁移至“[用户变量”](../data-model/event-variable/custom-variable.md#用户变量)。两者的区别主要在于：用户变量支持自定义的归因方式。

#### **2.1 上传接口**

2.x 版本中的上传用户变量方法有较大改动，不再将 `'setCSn'` 这个字段作为参数，方法中只需写入用户变量的 key - value 对。

**Web 端**

* 1.x 版本方法格式：

```text
_vds.push(['setCS1', 'CS1的key', 'CS1的value']);
_vds.push(['setCS2', 'CS2的key', 'CS2的value']);
_vds.push(['setCS3', 'CS3的key', 'CS3的value']);
...
_vds.push(['setCS10', 'CS10的key', 'CS10的value']);
```

* 2.x 版本方法格式：

对于 CS1 字段，也就是登陆用户ID，请使用以下方法：

```text
// 设置登录用户ID
gio('setUserId', userId);

// 清除登录用户ID
gio('clearUserId');
```

对于应用级变量，也就是 1.x 版本中的 CS2 - CS10，请使用以下方法：

```text
gio(‘app.set’, key, value) // 单个变量
gio('app.set', appLevelVariables) // 多个变量，可组合为一个JSON对象appLevelVariables传入
```

对于用户变量，也就是 1.x 版本中的 CS11 - CS20，请使用以下方法：

```text
gio('people.set', key, value); // 单个变量
gio('people.set', peopleVariables); // 多个变量，可组合为一个JSON对象peopleVariables传入
```

#### **2.2 GrowingIO 后台配置**

在 GrowingIO 后台进行用户属性字段配置，是在 “项目配置” - “CS字段配置” 页面。升级至 2.x 版本后，取消了上述配置方式。您可以在 **“管理” - “自定义事件和变量” 页面中的 “应用级变量” 和 “用户变量” Tab 页**分别找到自动为您迁移过去的两种变量的配置。配置方式请参考[相关帮助文档](../data-model/event-variable/custom-event.md)。

### **3. 迁移页面属性字段（PS字段）**

如果您未做页面属性字段上传，请忽略此部分。

类似于用户属性字段，在 2.x 版本中，页面属性字段被迁移到了“[页面级变量](../data-model/event-variable/custom-variable.md#页面级变量)”。与页面属性字段不同的是，**页面级变量相当于过去的 PS 字段，不再存在过去的 PG 字段**。

#### **3.1 上传接口**

* 1.x 版本方法格式：

```text
_vds.push([’setPageGroup‘, ‘PageGroup 的名称’];
_vds.push([‘setPS1’, ‘PS1 的值’]);
_vds.push([‘setPS2’, ‘PS2 的值’]);
_vds.push([‘setPS3’, ‘PS1 的值’]);
```

* 2.x 版本方法格式：

```text
gio('page.set', key, value);
gio('page.set', pageLevelVariables); //多个变量，可组合为一个对象传入
```

#### **3.2 GrowingIO 后台配置**

您需要在 **“管理” - “自定义事件和变量” 页面中的 “页面级变量” Tab 页**进行配置。配置方式请参考[相关帮助文档](../data-model/event-variable/custom-event.md#c-ye-mian-ji-bian-liang-pei-zhi)

### 4. 迁移自定义事件（埋点事件）

如果您未做自定义事件的上传，请忽略此部分。

2.x 版本的自定义事件，在概念上与 1.x 版本无任何区别，但上传接口和配置方式上有以下变更。

#### **4.1 上传接口**

**Web 端**

* 1.x 版本方法格式：

```text
window._vds.track(event_name, properties)
```

* 2.x 版本方法格式：

```text
gio('track', eventId);
gio('track', eventId, number);
gio('track', eventId, eventLevelVariables);
gio('track', eventId, number, eventLevelVariables);
```

#### **4.2 GrowingIO 后台配置**

自定义事件的配置，同 1.x 版本一样，也是在**“管理” - “自定义事件和变量”** 页面中的 **“自定义事件”** Tab 页。但您会发现，除了 “自定义事件” Tab 页外，现在还提供了 “事件级变量” Tab 页来专门管理事件级变量的配置。您只需在 “事件级变量” Tab 页完成事件级变量的配置，然后在新建自定义事件时，从已经建好的事件级变量中选择即可。

### 5. 数据校验

在完成了上述代码实施和配置后，我们当然需要对数据是否成功上传进行校验。校验工作分为两步完成。

**数据校验第一步：本地开发环境校验**

GrowingIO 提供了 SDK debug 模式以及 debug 工具，来帮助您完成数据的校验。

对 Web 端的开发者，GrowingIO 提供了 Chrome 浏览器插件形式的 debug 工具，请在[这里](growingio-debugger.md#web-debugger-an-zhuang)下载安装。

debug 工具的工作界面如下图：

![](https://docs.growingio.com/assets/WebDebuggerInstall5.png)

**log 中自定义数据的关键字**

* 在 cstm 条目中，可以看到上传的 “自定义事件+事件级变量” 数据
* 在 pvar 条目中，可以看到上传的 “页面级变量” 数据
* 在 evar 条目中，可以看到上传的 “转换变量” 数据
* 在各条目中，都可以找到 “用户变量” 对应的数据

**数据校验第二步：GrowingIO 后台图表验证**

在 GrowingIO 分析后台，找到 “单图” - “新建事件分析”，然后在图表中选择您设计好的 “指标+维度”，查看是否有数据。当然，您需要首先确保您的自定义事件或变量确实有被触发。

## GrowingIO Web Debugger

GrowingIO Debugger是GrowingIO推出的调试 SDK所发送数据的工具。在GrowingIO Debugger的帮助下，实施工程师可以看到在什么样的页面上，在什么时机向GrowingIO发送了什么样的服务器请求。

在数据驱动增长的实践中，数据准确性一直以来是一个必须重视的课题。数据分析行业中有一句谚语：Garbage In, Garbage Out，简称GIGO。这句谚语告诉我们：如果我们在实施阶段的方案有纰漏导致收集的数据就是错误的，那么基于错误的数据得出的结论就一定是有问题的，所以我们必须重视GrowingIO产品的实施工作，来确保实施方案的严谨正确，从而保证收集到的数据是尽最大可能准确的。

而在GrowingIO产品的实施过程中，GrowingIO Debugger能够帮助工程师、技术实施顾问清楚的看到发送出去的服务器请求是什么？发送的时机是什么？数据是不是按照实施方案中所描述的那样进行发送。有了GrowingIO Web Debugger的帮助，实施工程师可以通过高质量的实施，给分析师带来高质量高准确性的数据，这将大大有利于在企业内部养成数据驱动决策的文化和习惯。使用数据，使用高质量的数据帮助企业获得真正的增长。

[点击查看 GrowingIO Web Debugger 的安装和使用](growingio-debugger.md#growingio-web-debugger)。



## 如何集成 1.x 版本SDK

GrowingIO 提供 JS SDK,将代码插入到客户的网页中以后我们就可以接收用户的行为数据了。

我们的 JS SDK 支持 IE6 以上的 IE 浏览器、360 浏览器、谷歌浏览器、搜狗浏览器、火狐浏览器、QQ 浏览器、Safari 浏览器、Maxthon、Mobile 端浏览器，并且全面支持 H5，覆盖市面上主流的浏览器。

### 获取JS SDK {#获取js-sdk}

您可以通过如下的简单步骤，新建网站产品，并且获取到 GrowingIO JS SDK：

1. 登录 GrowingIO
2. 点击立即安装SDK

   ![](https://docs.growingio.com/.gitbook/assets/install_sdk_non_app.png)

3. 点击左上角的新建按钮
4. 点击添加新产品
5. 选择平台「网站」

如果您已经安装了产品，可以在应用管理中找到添加应用的按钮。

### 整合JS SDK至web 环境 {#整合js-sdk至web-环境}

将我们提供给您的 JS SDK 加入到您所需要分析的页面，请将我们给您提供的 JS SDK 复制到 `<head>` 和 `</head>` 标签之间即可, 例如：

```text
<head>
...
<script type='text/javascript'>
    var _vds = _vds || [];
    window._vds = _vds;
    (function(){
      _vds.push(['setAccountId', '您的项目ID']);
      (function() {
        var vds = document.createElement('script');
        vds.type='text/javascript';
        vds.async = true;
        vds.src = ('https:' == document.location.protocol ? 'https://' : 'http://') + 'assets.growingio.com/vds.js';
        var s = document.getElementsByTagName('script')[0];
        s.parentNode.insertBefore(vds, s);
      })();
    })();
</script>
...
</head>
```

成功加载SDK后GrowingIO会自动采集页面浏览量、元素浏览、点击量。我们还提供一些高级设置：用户属性、页面属性、采样采集、禁用元素浏览量采集、单页面应用启用hashtag、开启输入文本框内容等。具体设置方法请参考下面的详细说明。

### 用户自定义维度 {#用户自定义维度}

GrowingIO 的数据分析工具本身提供了例如 “访问来源”，“关键字”，“城市”,“操作系统"，”浏览器“等等这些维度。这些维度都可以和用户创建的指标进行多维的分析。但是往往不能满足用户对数据多维度分析的要求，因为每个公司的产品都有各自的用户维度，比如客户所服务的公司，用户正在使用的产品版本等等。GrowingIO 为了能够让数据分析变得更加的灵活，我们在 JS SDK 中提供了用户自定义维度的API接口:

```text
_vds.push(['setCS1', 'CS1的key', 'CS1的value']);
_vds.push(['setCS2', 'CS2的key', 'CS2的value']);
_vds.push(['setCS3', 'CS3的key', 'CS3的value']);
...
_vds.push(['setCS10', 'CS10的key', 'CS10的value']);
```

在 JS SDK 中，我们总计支持上传 10 个自定义维度 CS1 - CS10，**所有CS属性都必须是用户的属性，不能是订单 ID，商品ID 等和用户没有确定的关联关系的属性。**

**CS字段设置条件和限制**

1. CS 字段不能是和用户没有直接关系的属性，比如不能是订单 ID，商品 ID 等。
2. CS1 字段：在 GrowingIO 系统中用于识别注册用户的身份，因此 CS1 的 value 必须填写用户的唯一身份标示 ID。
3. CS2 字段：在 GrowingIO 系统中用于识别 SaaS 客户的租户，因此所有的 SaaS 用户必须填写租户的唯一身份标示 ID，非 SaaS 用户不做限定。
4. 对于未登录用户，不要设置任何CS字段。
5. 如果没有用到所有的CS字段，剩下的可以不设置。
6. 同一个CS字段，必须保持在各个平台意义相同。

如下例子中，总计上传 4 个用户属性，分别是：

**CS1:** user\_id:100324  
**CS2:** company\_id:943123  
**CS3:** user\_name:张溪梦  
**CS4:** company\_name:GrowingIO  
**CS5:** sales\_name:销售员小王

```text
    <script type='text/javascript'>
        var _vds = _vds || [];
        window._vds = _vds;
        (function(){
          _vds.push(['setAccountId', '您的项目ID']);

          _vds.push(['setCS1', 'user_id', '100324']);
          _vds.push(['setCS2', 'company_id', '943123']);
          _vds.push(['setCS3', 'user_name', '张溪梦']);
          _vds.push(['setCS4', 'company_name', 'GrowingIO']);
          _vds.push(['setCS5', 'sales_name', '销售员小王']);

          (function() {
            var vds = document.createElement('script');
            vds.type='text/javascript';
            vds.async = true;
            vds.src = ('https:' == document.location.protocol ? 'https://' : 'http://') + 'dn-growing.qbox.me/vds.js';
            var s = document.getElementsByTagName('script')[0];
            s.parentNode.insertBefore(vds, s);
          })();
        })();
    </script>
```

**在上传成功两小时后，您需要在「项目管理-项目配置-CS 配置中」进行字段配置和激活，配置成功后便可开始使用 CS 字段进行分析。配置过程可参考**[ **属性数据\(CS\)上传配置文档**](../data-model/event-variable/custom-event.md#e-yong-hu-bian-liang-pei-zhi)**。**

### JS SDK高级功能说明 {#js-sdk高级功能说明}

JS SDK 有一些高级功能，可以通过配置的方式开启，具体是在集成代码里面添加。

**开启采样采集**

比如按照 1/20 的比例采样。采样原则默认按照用户 ID 来采样。

```text
    _vds.push(['setSampling', 20])
```

**过滤爬虫数据（Deprecated）**

SDK中的爬虫过滤功能已经停止更新。推荐使用GrowingIO产品界面上的爬虫规则功能。

搜索引擎的爬虫爬网页会产生pv数据，可以通过trackBot过滤这部分爬虫数据。

```text
    _vds.push(['trackBot', false])
```

**禁用元素浏览量采集**

GrowingIO 提供两种采集，元素浏览和元素点击/修改等交互行为。对于内容基本固定的网站来说，可以直接禁用元素浏览量采集。

```text
    _vds.push(['setImp', false])
```

**启用 hashtag 作为页面收集**

我们默认不会把 hashtag 识别成页面URL的一部分。对于使用 hashtag 作为单页应用页面切换的网站来说，您可以使用`enableHT`来监听 hashtag 的变化，并区分页面来收集页面数据，每次 hashtag 改变都会触发一次PV，hashtag 的信息也会记录在页面URL中。

```text
    _vds.push(['enableHT', true])
```

**设置数据采集黑名单**

如果您希望过滤一些内容，可以在网站 DOM 结点上设置 growing-ignore 属性，这样这个容器里所有的元素的浏览量和点击量都不会被采集。

```text
    <div growing-ignore='true'>
      …
    </div>
```

**开启输入文本框内容采集**

由于输入文本框可能涉及一些隐私信息，比如账号、密码等，GrowingIO在采集数据的时候默认不采集输入文本框的数据。如果您希望采集某些文本框输入内容，比如搜索词，可以在input标签中设置growing-track属性，这样该文本框中的输入内容就会被采集到。如果input类型是password，即使开启内容采集，也不会采集该文本框的输入内容。

```text
    <input type='text' growing-track='true' />
```

JS代码请以售前人员提供的为主，进行正确添加。

至此您的 SDK 整合已经完成，我们就可以接收您的数据，之后您可以登录 www.growingio.com 的网站即可开始进行圈选。

**手动设置采集文本信息**

对于一些图片或者区块，可以通过设置 title 或者 data-growing-title 属性来设置采集点点文本。比如，

```text
<li data-growing-title="上一页" 
                 class="ant-pagination-disabled ant-pagination-prev">
  <a></a>
</li>
```

这时，采集到的 li 结点的内容就是_"上一页"_。

更多的文本信息规则，可以参考[第4节：What\(内容\)](https://sishen.gitbooks.io/gio-js-book/dom/4what.html)和[第1节：内容规则](https://sishen.gitbooks.io/gio-js-book/5/1.html)。

**手动设置采集位置信息**

除了内容以外，元素在列表里所在位置在某些场景下也是非常重要的信息，比如对于推荐广告位而言，我们是希望知道哪个位置的点击率最高。GrowingIO SDK 会自动识别列表元素，并附带上元素在列表里的位置。

LI 标签、TR 标签、DL 标签，会被自动识别为列表元素，列表内所有元素结点都会附带上位置信息。其他标签默认并不会带有位置信息，比如一些用 DIV 标签做的平铺容器。对于这种情况，可以使用 data-growing-idx。当在容器 DOM 结点上设置 data-growing-idx 属性，容器内的所有 DOM 元素同样，都会继承该属性值。比如

```text
<div data-growing-idx="1">
  <div class="left-container">
    <img src="" alt="图片1"/>
  </div>
  <div class="right-container">
    <h3 class="title">
      文章一标题
    </h3>
  </div>
</div>
```

更多的位置信息规则，可以参考[第2节：位置规则](https://sishen.gitbooks.io/gio-js-book/5/2.html)。

**手动发送页面浏览事件（PV）**  
以下情况GrowingIO会采集页面浏览数据：

* 页面被渲染一次
* 页面URL发生变化

当页面局部刷新，URL没有发生变化，GIO无法自动统计页面浏览量时，您可以手动发送页面浏览事件。

```text
_vds.trackPV();
```

### 注意事项 {#注意事项}

**Angular 1.4 以下版本冲突**

目前 Angular 1.4 以下版本与GrowingIO使用的MutationObserver 有冲突，这个是 Angular 的 bug，不好解决。我们建议您升级到 Angular 1.4 或者按照上述“禁用显示内容采集”方法关闭 impression 记录。

**允许iframe加载**

在GrowingIO 平台上使用可视化圈选指标功能需要使用 iframe 来加载目标页面。如果您的网站禁止了 iframe 加载，就无法正常使用圈选功能定义指标，需要设置允许iframe加载。

如果您的网站使用https协议，需将配置修改成

```text
X-Frame-Options: Allow-From https://www.growingio.com
```

如果您的网站使用http协议，需将配置修改成

```text
X-Frame-Options: Allow-From http://www.growingio.com
```

**请勿复写window对象**

可视化圈选指标功能需要保证您的网站与GrowingIO平台之间的通信。如果window.top、window.parent、window.name、window.location被复写，将导致无法圈选。

**页面内部嵌入的iframe元素如何加载SDK和圈选** iframe 元素可以将一个页面嵌入到另一个页面里，iframe 元素会创建包含另外一个文档的内联框架（即行内框架）。简单理解iframe可以将多个相互独立的页面展示在一页上。所以我们需要在iframe内部再次加载SDK代码收集iframe内部的元素浏览、点击数据。 同普通网页加载SDK方式相同，将SDK复制到iframe标签内部即可完成SDK安装。



