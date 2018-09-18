# Web JS SDK

* [集成最新 Web JS SDK（2.x）说明](./#ji-cheng-zui-xin-sdk-shuo-ming)
  * [1.集成 SDK](./#1-sdk)
  * [2.Web JS SDK 系统变量](./#12)
  * [3.Web JS SDK 高级设置](./#13)
    * [3.1 设置采集容器 \(data-growing-container\)​](./#31container)
    * [​3.2 设置采集文本信息 \(data-growing-title\)](./#32title)
    * [3.3 设置采集位置信息 \(data-growing-idx\)​](./#33idx)
    * [​3.4 设置数据采集黑名单 \(growing-ignore\)](./#34ignore)
    * [3.5 开启输入文本框内容采集 \(growing-track\)](./#35track)
  * [4.自定义数据上传 & 配置](./#4-zi-ding-yi-shu-ju-shang-chuan)
  * [5.注意事项](./#35)
* [旧版本 1.x 版本 SDK 升级到 2.x 指导](./#2)
  * [1. 重新集成 SDK](./#21)
  * [2. 迁移用户属性字段（CS 字段）](./#22)
  * [3. 迁移页面属性字段（PS 字段）](./#23)
  * [4. 迁移自定义事件（埋点事件）](./#24)
  * [5. 数据校验](./#25)
* [旧版本 1.x 版本 SDK 使用说明](./#2-1)
  * [1.获取 JS SDK](./#31)
  * [2.整合 JS SDK 至 web 环境](./#32)
  * [3.用户自定义维度](./#33)
  * [4.JS SDK 高级功能说明](./#34)
  * [5.注意事项](./#35-1)

2018 年 GrowingIO 发布了数据采集能力更强大，接口更丰富，兼容性更强的新一代 2.x 版本 SDK 。

我们的 JS SDK 支持 IE6 以上的 IE 浏览器、360 浏览器、谷歌浏览器、搜狗浏览器、火狐浏览器、QQ 浏览器、Safari 浏览器、Maxthon、Mobile 端浏览器，并且全面支持 H5，覆盖市面上的主流浏览器。

## 集成最新 Web JS SDK（2.x）说明

### 1.集成 SDK  {#1-sdk}

#### 1.1 跟踪**代码**

请将以下的页面代码放置到需要分析的页面中的 &lt;head&gt; 和 &lt;/head&gt; 标签之间，即可完成 Web JS SDK 2.1页面代码的安装。请注意使用**具体的项目 ID** 替换代码中的 **your projectId** 。

```markup
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

#### 1.2 GrowingIO Web Debugger

完成页面代码安装并重新部署您的网站应用后，建议使用[ GrowingIO Web Debugger ](../growingio-debugger/#growingio-web-debugger)验证数据发送是否正常。Web Debugger可以通过可视化界面让您一睹 GrowingIO 强大的数据采集能力。

### 2.Web JS SDK 系统变量 {#12}

设置系统变量的代码请插入上述追踪代码的 custom page code 部分，在 init 和 send 中间。

#### 2.1 hashtag 系统变量

GrowingIO默认不会把 hashtag 识别成页面 URL 的一部分。对于使用 hashtag 进行页面跳转的单页面网站应用来说，可以启用 hashtag 作为标识页面的一部分:

```javascript
gio('config', {'hashtag':true});
```

以此监听 hashtag 的变化，并采集不同页面的数据。每次 hashtag 的改变都会触发一次PV，hashtag 的信息也会记录在页面URL中。

#### 2.2 imp 系统变量

除点击、修改、提交等用户行为数据的采集，GrowingIO 还默认开启元素浏览量\(简称imp\)的无埋点采集。对于内容基本固定的网站，可以直接禁用元素浏览量采集。

```javascript
gio('config', {'imp':false});
```

也可以直接在init进行配置，如下：

```text
gio('init', 'your projectId', {
  'imp':false，
  'hashtag':true
});
```

### 3.Web JS SDK 高级设置 {#13}

高级设置可以帮助您更自如地进行圈选操作，请将高级设置插入您web应用的html代码中。

#### 3.1 设置采集容器 \(data-growing-container\) {#31container}

默认视button或a标签为可圈选容器。同时，默认可以圈选页面dom上所有叶子结点和叶子结点的上一级父节点。比如，

```markup
<div id="1" data-growing-container>
  <div id="2">
    <h1 id="3">商品名称</h1>
  </div>
  <div id="4">
    <div id="5">商品图片</div>
  </div>
  <div id="6">
    <div id="7">商品描述</div>
  </div>
</div>
```

由于数据采集准确性的原因，默认不支持以id="1"的div作为容器进行圈选。您可以为期望的容器元素添加data-growing-container 属性，圈选时即可圈到这个容器。

#### 3.2 设置采集文本信息 \(data-growing-title\) {#32title}

对于一些图片或者区块，可以通过设置 title 或者 data-growing-title 属性来设置采集点文本。比如，

```markup
<li data-growing-title="上一页" class="ant-pagination-disabled ant-pagination-prev">
  <a></a>
</li>
```

这时，采集到的 li 结点的内容就是_"上一页"_。

更多的文本信息规则，可以参考[第4节：What\(内容\)](https://sishen.gitbooks.io/gio-js-book/dom/4what.html)和[第1节：内容规则](https://sishen.gitbooks.io/gio-js-book/5/1.html)

#### 3.3 设置采集位置信息 \(data-growing-idx\)​​ {#33idx}

除了内容以外，元素在列表里所在位置在某些场景下也是非常重要的信息，比如对于推荐广告位而言，我们是希望知道哪个位置的点击率最高。GrowingIO SDK 会自动识别列表元素，并附带上元素在列表里的位置。

LI 标签、TR 标签、DL 标签，会被自动识别为列表元素，列表内所有元素结点都会附带上位置信息。其他标签默认并不会带有位置信息，比如一些用 DIV 标签做的平铺容器。对于这种情况，可以使用 data-growing-idx。当在容器 DOM 结点上设置 data-growing-idx 属性，容器内的所有 DOM 元素同样，都会继承该属性值。比如

```markup
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

更多的位置信息规则，可以参考[第2节：位置规则](https://sishen.gitbooks.io/gio-js-book/5/2.html)

#### ​​3.4 设置数据采集黑名单 \(growing-ignore\) {#34ignore}

如果您希望过滤一些内容，可以在网站 DOM 结点上设置 growing-ignore 属性，这样这个容器里所有的元素的浏览量和点击量都不会被采集。

```markup
<div growing-ignore='true'>
 …
</div>
```

#### 3.5 开启输入文本框内容采集 \(growing-track\) {#35track}

由于输入文本框可能涉及一些隐私信息，比如账号、密码等，GrowingIO在采集数据的时候默认不采集输入文本框的数据。如果您希望采集某些文本框输入内容，比如搜索词，可以在input标签中设置growing-track属性，这样该文本框中的输入内容就会被采集到。如果input类型是password，即使开启内容采集，也不会采集该文本框的输入内容。

```markup
<input type='text' growing-track='true' />
```

JS代码请以售前人员提供的为主，进行正确添加。

至此您的 SDK 整合已经完成，我们就可以接收您的数据，之后您可以登录 www.growingio.com 的网站即可开始进行圈选。

### 4.自定义数据上传

您的 APP 或网页在集成了 GrowingIO 的 SDK 之后，它将会自动地为您采集一系列用户行为数据，并在 GrowingIO 分析后台供您制成数据分析报表。

除上述的用户行为数据（或称为无埋点数据）之外，GrowingIO 还提供了多种 API 接口，供您[上传一些自定义的数据指标及维度](web-js-sdk-api.md)。90% 以上的用户都会[上传登录用户 ID](web-js-sdk-api.md#137)，以便分析登录用户的数据情况。

### 5.[Web JS SDK 注意事项](js-sdk-zhu-yi-shi-xiang.md) {#35}

## 旧版本 1.x 版本 SDK 升级指导 {#2}

**如果您目前希望从 1.x 版本升级至 2.x 版本，请务必联系 GrowingIO 对接人，我们需要在后台为你开启 2.x 版本所对应的功能权限。如果直接集成 2.x 版本，而后台对应功能权限未开启的话，可能会造成数据丢失的问题。**

### **1. 重新集成 SDK（请确保你已经联系 GrowingIO 对接人，并已开启后台对应功能权限）** {#21}

请参考[添加 Web JS SDK 跟踪代码](./#1-sdk)

Tips：建议您在开发中，使用 debug mode 校验 GrowingIO SDK 的数据是否正常上传。开启 debug mode 的方式请见上述文档中初始化方法部分。

### **2. 迁移用户属性字段（CS字段）（如果您未做用户属性字段上传，请忽略此部分）** {#22}

用户属性字段（简称CS字段）是 1.x 版本的概念，升级至 2.x 版本后：

* CS1字段，会强制命名为“登陆用户ID”，并且上传接口与其他变量不同。
* CS2-10字段，会迁移至“应用级变量”，应用级变量与CS字段的使用方式无任何区别。
* CS11-20字段，会迁移至“用户变量“。两者的区别主要在于：用户变量支持自定义的归因方式。

#### **2.1 上传接口**

2.x 版本中的上传用户变量方法有较大改动，不再将 `'setCSn'` 这个字段作为参数，方法中只需写入用户变量的 key - value 对。

**Web 端**

* 1.x 版本方法格式：

```javascript
_vds.push(['setCS1', 'CS1的key', 'CS1的value']);
_vds.push(['setCS2', 'CS2的key', 'CS2的value']);
_vds.push(['setCS3', 'CS3的key', 'CS3的value']);
...
_vds.push(['setCS10', 'CS10的key', 'CS10的value']);
```

* 2.x 版本方法格式：

对于 CS1 字段，也就是登陆用户ID，请使用以下方法：

```javascript
// 设置登录用户ID
gio('setUserId', userId);

// 清除登录用户ID
gio('clearUserId');
```

对于应用级变量，也就是 1.x 版本中的 CS2 - CS10，请使用以下方法：

```javascript
gio(‘app.set’, key, value) // 单个变量
gio('app.set', appLevelVariables) // 多个变量，可组合为一个JSON对象appLevelVariables传入
```

对于用户变量，也就是 1.x 版本中的 CS11 - CS20，请使用以下方法：

```javascript
gio('people.set', key, value); // 单个变量
gio('people.set', peopleVariables); // 多个变量，可组合为一个JSON对象peopleVariables传入
```

#### **2.2 GrowingIO 后台配置**

在 GrowingIO 后台进行用户属性字段配置，是在 “项目配置” - “CS字段配置” 页面。升级至 2.x 版本后，取消了上述配置方式。您可以在 **“管理” - “自定义事件和变量” 页面中的 “应用级变量” 和 “用户变量” Tab 页**分别找到自动为您迁移过去的两种变量的配置。配置方式请参考[相关帮助文档]()。

### **3. 迁移页面属性字段（PS字段）（如果您未做页面属性字段上传，请忽略此部分）** {#23}

类似于用户属性字段，在 2.x 版本中，页面属性字段被迁移到了“页面级变量”。与页面属性字段不同的是，**页面级变量相当于过去的 PS 字段，不再存在过去的 PG 字段**。

#### **3.1 上传接口**

* 1.x 版本方法格式：

```javascript
_vds.push([’setPageGroup‘, ‘PageGroup 的名称’];
_vds.push([‘setPS1’, ‘PS1 的值’]);
_vds.push([‘setPS2’, ‘PS2 的值’]);
_vds.push([‘setPS3’, ‘PS1 的值’]);
```

* 2.x 版本方法格式：

```javascript
gio('page.set', key, value);
gio('page.set', pageLevelVariables); //多个变量，可组合为一个对象传入
```

#### **3.2 GrowingIO 后台配置**

您需要在 **“管理” - “自定义事件和变量” 页面中的 “页面级变量” Tab 页**进行配置。配置方式请参考[相关帮助文档]()。

### 4. 迁移自定义事件（打点事件）（**如果您未做自定义事件的上传，请忽略此部分）** {#24}

2.x 版本的自定义事件，在概念上与 1.x 版本无任何区别，但上传接口和配置方式上有以下变更。

#### **4.1 上传接口**

**Web 端**

* 1.x 版本方法格式：

```javascript
window._vds.track(event_name, properties)
```

* 2.x 版本方法格式：

```javascript
gio('track', eventId);
gio('track', eventId, number);
gio('track', eventId, eventLevelVariables);
gio('track', eventId, number, eventLevelVariables);
```

#### **4.2 GrowingIO 后台配置**

自定义事件的配置，同 1.x 版本一样，也是在**“管理” - “自定义事件和变量”** 页面中的 **“自定义事件”** Tab 页。但您会发现，除了 “自定义事件” Tab 页外，现在还提供了 “事件级变量” Tab 页来专门管理事件级变量的配置。您只需在 “事件级变量” Tab 页完成事件级变量的配置，然后在新建自定义事件时，从已经建好的事件级变量中选择即可。

### 5. 数据校验 {#25}

在完成了上述代码实施和配置后，我们当然需要对数据是否成功上传进行校验。校验工作分为两步完成。GrowingIO 提供了 SDK debug 模式以及 debug 工具，来帮助您完成数据的校验。[点击查看 GrowingIO Web Debugger 的安装和使用](../growingio-debugger/#growingio-web-debugger)。

**数据校验第一步：本地开发环境校验**

对 Web 端的开发者，GrowingIO 提供了 Chrome 浏览器插件形式的 debug 工具，请在[这里](../growingio-debugger/#web-debugger-an-zhuang)下载安装。

debug 工具的工作界面如下图：

![](https://docs.growingio.com/assets/WebDebuggerInstall5.png)

**log 中自定义数据的关键字**

* 在 cstm 条目中，可以看到上传的 “自定义事件+事件级变量” 数据
* 在 pvar 条目中，可以看到上传的 “页面级变量” 数据
* 在 evar 条目中，可以看到上传的 “转换变量” 数据
* 在各条目中，都可以找到 “用户变量” 对应的数据

**数据校验第二步：GrowingIO 后台图表验证**

在 GrowingIO 分析后台，找到 “单图” - “新建事件分析”，然后在图表中选择您设计好的 “指标+维度”，查看是否有数据。当然，您需要首先确保您的自定义事件或变量确实有被触发。

## 旧版本 1.x 版本 SDK 使用说明 {#2}

如果您正在使用上一代1.x版本SDK且暂时不愿[进行迁移升级](./#2)，以下我们为您提供上一代1.x版本 SDK 使用说明。

### 1.获取 JS SDK {#31}

您可以通过如下的简单步骤，新建网站产品，并且获取到 GrowingIO JS SDK：

1. 登录 GrowingIO
2. 点击立即安装SDK

   ![](https://docs.growingio.com/.gitbook/assets/install_sdk_non_app.png)

3. 点击左上角的新建按钮
4. 点击添加新产品
5. 选择平台「网站」

如果您已经安装了产品，可以在应用管理中找到添加应用的按钮。

### 2.整合 JS SDK 至 web 环境 {#32}

将我们提供给您的 JS SDK 加入到您所需要分析的页面，请将我们给您提供的 JS SDK 复制到 `<head>` 和 `</head>` 标签之间即可, 例如：

```markup
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

### 3.[用户自定义维度](web-js-sdk-api.md#2-dui-ji-cheng-le-web-js-sdk-1-x-de-chan-pin-shang-chuan-pei-zhi-zi-ding-yi-wei-du) {#33}

### 4.JS SDK高级功能说明 {#34}

JS SDK 有一些高级功能，可以通过配置的方式开启，具体是在集成代码里面添加。

#### 4.1 **开启采样采集**

比如按照 1/20 的比例采样。采样原则默认按照用户 ID 来采样。

```javascript
    _vds.push(['setSampling', 20])
```

#### 4.2 **过滤爬虫数据**

[请看这里](../../configuration/bot-rule.md)

#### 4.3 **禁用元素浏览量采集**

GrowingIO 提供两种采集，元素浏览和元素点击/修改等交互行为。对于内容基本固定的网站来说，可以直接禁用元素浏览量采集。

```javascript
    _vds.push(['setImp', false])
```

#### 4.4 **启用 hashtag 作为页面收集**

我们默认不会把 hashtag 识别成页面 URL 的一部分。对于使用 hashtag 作为单页应用页面切换的网站来说，您可以使用`enableHT`来监听 hashtag 的变化，并区分页面来收集页面数据，每次 hashtag 改变都会触发一次 PV，hashtag 的信息也会记录在页面URL中。

```javascript
    _vds.push(['enableHT', true])
```

#### 4.5 **设置数据采集黑名单**

如果您希望过滤一些内容，可以在网站 DOM 结点上设置 growing-ignore 属性，这样这个容器里所有的元素的浏览量和点击量都不会被采集。

```javascript
    <div growing-ignore='true'>
      …
    </div>
```

#### 4.6 **开启输入文本框内容采集**

由于输入文本框可能涉及一些隐私信息，比如账号、密码等，GrowingIO在采集数据的时候默认不采集输入文本框的数据。如果您希望采集某些文本框输入内容，比如搜索词，可以在input标签中设置growing-track属性，这样该文本框中的输入内容就会被采集到。如果input类型是password，即使开启内容采集，也不会采集该文本框的输入内容。

```javascript
    <input type='text' growing-track='true' />
```

JS代码请以售前人员提供的为主，进行正确添加。

至此您的 SDK 整合已经完成，我们就可以接收您的数据，之后您可以登录 www.growingio.com 的网站即可开始进行圈选。

#### 4.7 **手动设置采集文本信息**

对于一些图片或者区块，可以通过设置 title 或者 data-growing-title 属性来设置采集点点文本。比如，

```javascript
<li data-growing-title="上一页" 
                 class="ant-pagination-disabled ant-pagination-prev">
  <a></a>
</li>
```

这时，采集到的 li 结点的内容就是_"上一页"_。

更多的文本信息规则，可以参考[第4节：What\(内容\)](https://sishen.gitbooks.io/gio-js-book/dom/4what.html)和[第1节：内容规则](https://sishen.gitbooks.io/gio-js-book/5/1.html)。

#### 4.8 **手动设置采集位置信息**

除了内容以外，元素在列表里所在位置在某些场景下也是非常重要的信息，比如对于推荐广告位而言，我们是希望知道哪个位置的点击率最高。GrowingIO SDK 会自动识别列表元素，并附带上元素在列表里的位置。

LI 标签、TR 标签、DL 标签，会被自动识别为列表元素，列表内所有元素结点都会附带上位置信息。其他标签默认并不会带有位置信息，比如一些用 DIV 标签做的平铺容器。对于这种情况，可以使用 data-growing-idx。当在容器 DOM 结点上设置 data-growing-idx 属性，容器内的所有 DOM 元素同样，都会继承该属性值。比如

```javascript
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

#### 4.9 **手动发送页面浏览事件（PV）**

以下情况 GrowingIO 会采集页面浏览数据：

* 页面被渲染一次
* 页面URL发生变化

当页面局部刷新，URL没有发生变化，GIO无法自动统计页面浏览量时，您可以手动发送页面浏览事件。

```javascript
_vds.trackPV();
```

### 5.[Web JS SDK 注意事项](js-sdk-zhu-yi-shi-xiang.md) {#35}

