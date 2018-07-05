# Web JS SDK

* [SDK 安装](web-js-sdk.md#sdk-安装)
* [Web JS 系统变量](web-js-sdk.md#web-js-系统变量)
* [Web JS SDK API](web-js-sdk.md#web-js-sdk-2-1-api)
* [自定义数据上传&配置指南](web-js-sdk.md#自定义数据上传配置指导)
* [1.x 版本 SDK 升级指导](web-js-sdk.md#1x-版本-sdk-升级指导)
* [GrowingIO Web Debugger](web-js-sdk.md#growingio-web-debugger)

### SDK 代码安装 {#sdk-安装}

重要：如果您目前已经在使用GrowingIO 1.x 版本的 SDK，希望升级到最新版本 \(V2.1），您需要联系您的 GrowingIO 对接人，我们需要帮您开启新版本 SDK 的功能。如果您忽略该提醒自助升级，而后台对应功能未开启的话，可能会带来数据丢失的问题。[如何升级?](web-js-sdk.md#1x-版本-sdk-升级指导)

#### GrowingIO Web JS SDK 分为两个部分 {#growingio-web-js-sdk-21-分为两个部分}

1. 页面代码（Page Code）
2. gio.js库文件（Library）

**页面代码如下所示：**

**请将以下的页面代码放置到需要分析的页面中的&lt;head&gt;和&lt;/head&gt;标签之间，即可完成Web JS SDK 2.1页面代码的安装。**

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

**gio.js文件**

可以看到页面代码中有对gio.js文件的引用，gio.js的地址为[http://assets.growingio.com/2.1/gio.js](https://github.com/growingio/help_site/tree/f4b4103b288205f6a9b13e0c4692f4d65a2ab386/assets.growingio.com/2.1/gio.js)

请注意使用具体的项目ID替换上述代码中 ‘your projectId’ 的部分。

建议在安装页面代码结束之后，建议使用GrowingIO Web Debugger验证一下服务器请求发送是否正常。

### Web JS 系统变量 {#web-js-系统变量}

Web JS SDK可以配置一些系统变量来控制 Web JS SDK 的数据发送。

有以下系统变量在实施时可以使用以下这些系统变量：

#### imp系统变量 {#imp系统变量}

作用：禁止元素浏览量采集

GrowingIO 提供两种采集，元素浏览和元素点击，文本框内容修改等交互行为。对于内容基本固定的网站来说，可以直接禁用元素浏览量采集。

```text
gio('config', {'imp':false});
```

#### hashtag系统变量 {#hashtag系统变量}

作用：启用 hashtag 作为标识页面的一部分

默认值：false

GrowingIO默认不会把 hashtag 识别成页面URL的一部分。对于使用 hashtag 作为单页应用页面切换的网站来说，可以使用

```text
gio('config', {'hashtag':true});
```

来监听 hashtag 的变化，并区分页面来收集页面数据，每次 hashtag 改变都会触发一次PV，hashtag 的信息也会记录在页面URL中。

### Web JS SDK 2.1 API

#### API简介：

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

#### init {#init}

初始化参数，设置项目ID和一些常用的配置项。

参数：

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

#### track {#track}

发送一个事件。在添加所需要发送的事件代码之前，需要在打点管理用户界面配置事件以及事件级变量。

参数：

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

#### page.set {#pageset}

发送页面级别的维度信息，在添加代码之前必须在打点管理界面上声明页面级变量。

参数：

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

#### evar.set {#evarset}

发送一个转化信息用于高级归因分析，在添加代码之前必须在打点管理界面上声明转化变量。

参数

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

#### people.set {#peopleset}

发送用户信息用于用户信息相关分析，在添加代码之前必须在打点管理界面上声明转化变量。

参数：

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

#### setUserId {#setuserid}

当用户登录之后调用setUserId API，设置登录用户ID。

参数：

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

#### clearUserId {#clearuserid}

当用户登出之后调用clearUserId，清除已经设置的登录用户ID。

```text
//clearUserId API原型和调用示例
gio('clearUserId');
```

#### sendPage {#sendpage}

在默认情况下，由于用户浏览网站的交互行为导致当前页面的URL产生变化时，GrowingIO的Web JS SDK会发送一个page类型的请求。在一些特殊的情况下，例如用户在访问单页应用（Single Page Application）类型的网站时，用户的操作会导致业务上面理解的页面产生了变化，但是当前的URL可能并没有改变。

这时，可以调用GrowingIO提供的sendPage接口手动发送页面浏览事件。这个接口的调用将会发送出一条‘page’类型的数据，GIO服务器在收到page类型的数据之后，页面浏览量这个预定义指标会加1。

```text
//sendPage API原型和调用示例
gio('sendPage');
```

### 自定义数据上传&配置指导 {#自定义数据上传配置指导}

您的APP或网页在集成了 GrowingIO 的 SDK 之后，它将会自动地为您采集一系列用户行为数据，并在 GrowingIO 分析后台供您制成数据分析报表。除上述的用户行为数据（或称为无埋点数据）之外，GrowingIO 还提供了多种 API 接口，供您上传一些自定义的数据指标及维度，具体请参考[相关文档](../data-model/event-variable/custom-event.md)。

### 1.x 版本 SDK 升级指导 {#1x-版本-sdk-升级指导}

本文旨在帮助您从 1.x 版本无缝升级至 2.x 版本，由于两个版本的诸多接口、方法、字段含义均有较大变动，因此建议您在升级前一定参照本文完成必要的实施工作。

**1. 重新集成 SDK**

* [Web JS SDK 开发文档](web-js-sdk-2.x/)

Tips：建议您在开发中，使用 debug mode 校验 GrowingIO SDK 的数据是否正常上传。开启 debug mode 的方式请见上述文档中初始化方法部分。

**2. 迁移用户属性字段（CS字段）**

如果您未做用户属性字段上传，请忽略此部分。

用户属性字段（简称CS字段）是 1.x 版本的概念，升级至 2.x 版本后：

* CS1字段，会强制命名为“登陆用户ID”，并且上传接口与其他变量不同。
* CS2-10字段，会迁移至“应用级变量”，应用级变量与CS字段的使用方式无任何区别。
* CS11-20字段，会迁移至“[用户变量”](../data-model/event-variable/custom-variable.md#用户变量)。两者的区别主要在于：用户变量支持自定义的归因方式。

**2.1 上传接口：**

2.x 版本中的上传用户变量方法有较大改动，不再将 `'setCSn'` 这个字段作为参数，方法中只需写入用户变量的 key - value 对。

**Web 端：**

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

**2.2 GrowingIO 后台配置**

在 GrowingIO 后台进行用户属性字段配置，是在 “项目配置” - “CS字段配置” 页面。升级至 2.x 版本后，取消了上述配置方式。您可以在 **“管理” - “自定义事件和变量” 页面中的 “应用级变量” 和 “用户变量” Tab 页**分别找到自动为您迁移过去的两种变量的配置。配置方式请参考[相关帮助文档](../data-model/event-variable/custom-event.md)。

**3. 迁移页面属性字段（PS字段）**

如果您未做页面属性字段上传，请忽略此部分。

类似于用户属性字段，在 2.x 版本中，页面属性字段被迁移到了“[页面级变量](../data-model/event-variable/custom-variable.md#页面级变量)”。与页面属性字段不同的是，**页面级变量相当于过去的 PS 字段，不再存在过去的 PG 字段**。

**3.1 上传接口**

**Web 端：**

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

**3.2 GrowingIO 后台配置**

您需要在 **“管理” - “自定义事件和变量” 页面中的 “页面级变量” Tab 页**进行配置。配置方式请参考[相关帮助文档](../data-model/event-variable/custom-event.md#第二步：在-打点管理-中完成配置)

#### 4. 迁移自定义事件（埋点事件） {#4-迁移自定义事件埋点事件}

如果您未做自定义事件的上传，请忽略此部分。

2.x 版本的自定义事件，在概念上与 1.x 版本无任何区别，但上传接口和配置方式上有以下变更。

**4.1 上传接口**

**Web 端：**

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

**4.2 GrowingIO 后台配置**

自定义事件的配置，同 1.x 版本一样，也是在\*\*“管理” - “自定义事件和变量” 页面中的 “自定义事件” Tab 页\*\*。但您会发现，除了 “自定义事件” Tab 页外，现在还提供了 “事件级变量” Tab 页来专门管理事件级变量的配置。您只需在 “事件级变量” Tab 页完成事件级变量的配置，然后在新建自定义事件时，从已经建好的事件级变量中选择即可。

#### 5. 数据校验 {#5-数据校验}

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

### GrowingIO Web Debugger {#growingio-web-debugger}

GrowingIO Debugger是GrowingIO推出的调试 SDK所发送数据的工具。在GrowingIO Debugger的帮助下，实施工程师可以看到在什么样的页面上，在什么时机向GrowingIO发送了什么样的服务器请求。

在数据驱动增长的实践中，数据准确性一直以来是一个必须重视的课题。数据分析行业中有一句谚语：Garbage In, Garbage Out，简称GIGO。这句谚语告诉我们：如果我们在实施阶段的方案有纰漏导致收集的数据就是错误的，那么基于错误的数据得出的结论就一定是有问题的，所以我们必须重视GrowingIO产品的实施工作，来确保实施方案的严谨正确，从而保证收集到的数据是尽最大可能准确的。

而在GrowingIO产品的实施过程中，GrowingIO Debugger能够帮助工程师、技术实施顾问清楚的看到发送出去的服务器请求是什么？发送的时机是什么？数据是不是按照实施方案中所描述的那样进行发送。有了GrowingIO Web Debugger的帮助，实施工程师可以通过高质量的实施，给分析师带来高质量高准确性的数据，这将大大有利于在企业内部养成数据驱动决策的文化和习惯。使用数据，使用高质量的数据帮助企业获得真正的增长。

[点击查看 GrowingIO Web Debugger 的安装和使用](growingio-debugger.md#growingio-web-debugger)。



