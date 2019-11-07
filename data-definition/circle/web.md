# Web 圈选

GrowingIO 全量采集用户行为数据，你可以通过「圈选」来定义元素和页面，作为数据分析的基础指标。

在没有定义过的情况下，GrowingIO 保留和回溯元素过去 7 天的点击量，页面过去 7 天的浏览量。

通过导航栏进入「圈选」，需要确定你要定义的是元素还是页面，可以在「浏览模式」找到想要分析的元素和页面。

![](https://docs.growingio.com/.gitbook/assets/9412f46a-d87c-41ef-9ee6-9e6e408f4c6a-12626-00000bcf696b73c5_tmp.jpg)

## 1.开始圈选

### **1.1 定义页面**

**在定义页面之前需要理解的基本规则：**

URL**示意：**www.xxx.com **/** 12345/678/123 **?** id=1&ig=2

**拆分：**域名 **/** 路径 **?** 查询条件

**即** www.xxx.com **为域名，**/12345/678/123 **为路径，**?id=1&ig=2 **为查询条件**

如果页面中有 **\#**，请先设置后再进行数据采集和分析：我们默认不会把 hashtag 识别成页面 URL 的一部分。对于使用 hashtag 作为单页应用页面切换的网站来说，请在工程师的帮助下，添加下面的代码，使用`enableHT`来监听 hashtag 的变化，并区分页面来收集页面数据，每次 hashtag 改变都会触发一次PV，hashtag 的信息也会记录在页面 URL 中。

如果你的[ SDK 是最新版本](../../sdk-integration/web-js-sdk/)，请添加这段代码：

```text
gio('config', {'hashtag':true});
```

如果你 [SDK 是旧版本](../../sdk-integration/web-js-sdk/#2-1)，请添加：

```text
_vds.push(['enableHT', true])
```



接下来开始定义页面：

![](https://docs.growingio.com/.gitbook/assets/2.-ding-yi-ye-mian.gif)

点击「定义页面」

![](https://docs.growingio.com/.gitbook/assets/ping-mu-kuai-zhao-20180313-shang-wu-11.16.28.png)

左侧是选择区，显示的是与当前页面 url 相同的页面，或包含当前页面的页面组。

我们将与当前页面 url 域名和路径相同的页面打上了 **\[当前页\]** 的tag，便于查找和识别。如果需要的页面已经被定义过，可以直接使用。

右侧是编辑区，按照页面结构，我们把一个完整的 url 拆解成了：**域名、路径和查询条件**（如有则显示），可以分别编辑。

> **！如何定义一组页面？**

a.「路径」最右侧的开关开启时，可以通过在「路径」中使用「\*」来做通配符，达到圈选多个类似页面的目的：

例如，GrowingIO 博客文章内容的地址都是这样的 **https://blog.growingio.com/posts/123456** 、 **https://blog.growingio.com/posts/14562**、 **https://blog.growingio.com/posts/1264**......

那么我们在路径中输入 /posts/\* 就会圈选出所有的博客单篇文章的页面。

b.「路径」右侧的开关关闭时，意味着要圈选出所有符合域名为 xxx.xxxxx.com 的一组页面，不管后面的路径是什么。&lt;/font&gt;

**案例**

**基础步骤 \| 监控首页和全站的流量情况**

**1.定义首页：**

对于每个公司来说，首页都是比较重要的页面，通常会分析首页的流量从哪里来，以及首页的转化能力。首先，我要定义 GrowingIO 的首页。

第一步：在「圈选导航栏」中输入 url ，回车或点击跳转；

第二步：点击「定义页面」；

第三步：确认现在定义的页面 url 是否正确，修改「页面的名称」，保存。

![](https://docs.growingio.com/.gitbook/assets/20_40_02__04_25_2018.jpg)

定义完页面后，可以去「事件分析」中选择这个指标，就可以看到这个页面的基本数据情况了。

可以在「表格」中使用「访问来源」的维度来查看这个页面的流量都是从哪里来，也可以在案例2中学习到转化率的分析。

**2.定义全站页面组：**

如果你想看到全站的总体流量情况，可以定义全站页面组。

在上面的第二步，点击「页面定义」后：

第一步：关闭「路径」开关；

第二步：确认现在定义的页面是否是域名下的所有页面；

![](https://docs.growingio.com/.gitbook/assets/21_04_22__04_25_2018.jpg)

**案例1：注册转化率分析（定义每一个转化节点）**

注册流是很常见的转化流程了，注册数往往关乎核心的业务指标，因此，通常我们会有这样的分析需求。

首先，我们需要清楚自己的注册流程是什么样的，比如是否分页，每一个按钮点击后是否一定会触达跳转，在这个转化流程中的关键节点有哪些。

接下来，我们来看一个注册流程：

![](https://docs.growingio.com/.gitbook/assets/image003.png)

这是一个分页的注册流程，用户填写完上一页的内容才能进入到下一页，因此我可以将这三个页面作为漏斗的三个步骤，按照案例1的方式定义每一个页面。

然后，在漏斗分析中进行分析：

![](https://docs.growingio.com/.gitbook/assets/image005.png)

**案例2：一组页面的分析（定义一组有相关业务意义的页面）**

很多网站的页面都是有规律的，层级结构清晰：

GrowingIO 解决方案首页 **https://www.growingio.com/solution/**​

GrowingIO 在线旅游解决方案落地页 **https://www.growingio.com/solution/online-travel**

GrowingIO 互联网金融解决方案落地页 **https://www.growingio.com**​**/solution/internet-finance**

我们发现所有的解决方案落地页都是 **https://www.growingio.com/solution/xxx** ，那么如果我想统计所有解决方案页的页面情况，就可以通过通配符「\*」来定义一组页面 **https://www.growingio.com/solution/\***，即：​

![](https://docs.growingio.com/.gitbook/assets/20_59_35__04_25_2018.jpg)

**案例3：特殊页面定义——页面 url 中带有 「?」**

如果你发现你的页面 url 中带有「?」，除了用于监测广告投放的 utm 之外，需要在定义页面时手动打开「查询条件」的开关，这种情况是不支持回溯数据的，将从你定义页面的时候开始统计数据：

![](https://docs.growingio.com/.gitbook/assets/21_17_01__04_25_2018.jpg)

### **1.2 定义元素**

在**「圈选」**模式下找到你想要定义的元素。

当一些元素需要鼠标 hover 等交互行为才能显示的时候也可以按住 shift 快捷键，方便地显示出该元素，并且进行圈选。

在右上角我们提供了屏幕尺寸的选项，可以选择不同的设备的屏幕，对自适应元素进行圈选。

圈选导航栏上显示默认开启了**「高亮已定义元素」**，你可以看到哪些元素被定义过，高亮模式下，粉色实线代表已经被圈选的元素，粉色虚线代表已经被圈选的同类元素，点击元素后可以看到最近一次被定义的规则。

![](https://docs.growingio.com/.gitbook/assets/ping-mu-kuai-zhao-20180310-shang-wu-11.07.50.png)

进入「圈选」模式，点击一个元素，弹出圈选的弹窗：

![](https://docs.growingio.com/.gitbook/assets/1.-jin-ru-quan-xuan.gif)

**第一步：确认元素所在的页面是什么。**

如果这个元素出现在多个页面同样的位置，想要统计所有的数据，可以通过上面的「定义页面」，先定义元素所在的页面组，再在这里选择该页面组。

**第二步：确定元素统计的规则。**

设置元素当前的文本 / 顺序和跳转链接等限定条件，确定元素统计的规则。

**第三步：通过动态展示区确认是不是自己想要定义的元素和规则。**

![](https://docs.growingio.com/.gitbook/assets/4.-ding-yi-yuan-su-dong-tai-zhan-shi-qu.gif)

**第四步，保存指标，并使用指标去分析。**

为指标命名，最好包含元素所属页面和限定的条件的信息，保存定义的元素后，就可以去做分析了。如果数据量过大，导致数据还没有回溯完，可以先保存图表，过几分钟再回来看。

在「最近更新列表」中查看刚刚定义过的元素。

![](https://docs.growingio.com/.gitbook/assets/6.-ding-yi-yuan-su-hou-zui-jin-geng-xin.gif)

**使用案例**

**基本步骤 \| 监控重要的按钮数据（定义元素）**

注册按钮在首页的点击量是一个重要的数据，因此，我们需要定义这个元素。

首先，在「圈选」模式下点击这个按钮：

第一步：确认元素所在页面是否正确，默认的是「当前页面」；

第二步：根据下图中第二个蓝色框中的文字，确认统计规则是否符合需求；

第三步：填写一个容易理解的名称，然后就可以保存了。

![](https://docs.growingio.com/.gitbook/assets/21_20_14__04_25_2018.jpg)

接下来，可以在「事件分析」等分析工具中使用。

**案例1：分析一个商品位、测试按钮等（定义一个位置）**

我们在圈选时只能看到一种元素，但是可能用户看到的网站内容不一样，元素的内容也不一样，因此，当我们在定义一个元素时，要思考我们想要统计的是「现在这个位置的按钮点击情况」还是「这个位置上这个内容的按钮的点击情况」。常见于商品位、AB测试等使用场景。

在下图中，有一部分用户看到的按钮是「立即注册」，还有一部分用户看到的按钮是「免费试用」，因此，如果我想定义这个按钮的点击情况，不管是什么内容，那么就只「限定顺序」，不限定其他条件：![](https://docs.growingio.com/.gitbook/assets/21_32_36__04_25_2018%20%281%29.jpg)

如果想看这个位置上不同内容的点击情况，可以在「事件分析」中，选择「柱图」，用「元素内容」作为维度，来进行分析。

**案例2：导航栏分析（定义一个跨页面的元素）**

如果想要分析导航栏的点击情况，我们就会发现这是一类特殊的元素，因为他们出现在网站的多个页面中。

因此，在定义这类元素时，要注意选择全站页面组，如果没有定义全站页面组，需要先去「定义页面」，详见「定义页面部分」的「基本步骤」。

![](https://docs.growingio.com/.gitbook/assets/21_29_04__04_25_2018.jpg)

**案例3：比较一些长得差不多的元素（定义一组元素）**

当我们想要定义一组列表，或者一组元素时，会发现他们通常结构很像，圈选提供了一种便捷的方式，通过定义「同类元素」的方式来定义一组元素。选中这样的元素后，依然是先确认它的所属页面是否正确，然后「不限定任何限定条件」，这时可以注意第二个蓝框中的话：现在定义的是所属页面中，所有同类元素的数据之和。我们已经将这组同类元素用虚线框标记出来。这时，你就成功定义了这个元素所在的列表。

![](https://docs.growingio.com/.gitbook/assets/21_32_36__04_25_2018.jpg)

然后在「事件分析」中使用「元素内容」的维度进行分析。

### 1.3 插件圈选

GrowingIO 提供支持 eb 圈选的 Chrome 扩展程序。

**使用场景**

* window.name、window.top等被复写，导致圈选不了；
* 客户网站 block了外站加载 iframe，需要客户把我们加入白名单；
* https/http 切换问题；
* 客户网站的实现方式是新开一个tab显示新页面，现在会跳出 GIO；
* 客户的header里面设置了Allow-orign 为他们自己的网站 导致没法在我们的官网里面被圈选。

**插件安装**

> Chrome网上应用店一键安装（自动更新）：[https://chrome.google.com/webstore/detail/growingio-web%E5%9C%88%E9%80%89/iapmppbkobkiijkhndbnkncfaklmbhck](https://chrome.google.com/webstore/detail/growingio-web%E5%9C%88%E9%80%89/iapmppbkobkiijkhndbnkncfaklmbhck?authuser=3)
>
> 插件（最新版）手动下载地址： [https://s.growingio.com/5EoKZl](https://s.growingio.com/5EoKZl)，请参考[手动安装插件方法](https://s.growingio.com/2Z4mBB)。

**圈选步骤**

1. **圈**选插件安装成功后，**直接在 Chrome 浏览器中打开待圈选网站**。
2. 单机插件图标登录你的账号。

![](../../.gitbook/assets/image%20%28293%29.png)

3. 将插件模式切换到圈选模式，就可以开始圈选了，插件圈选方式与页面圈选功能相同。

![](../../.gitbook/assets/image%20%2833%29.png)

## 2.常见问题 FAQ

### **2.1 如何定义「一组相似元素之和」？**

如果该元素有同类元素，不限定所有的限定条件，即是统计一组同类元素之和的数据。

### **2.2 对于已定义过的元素，更改定义规则后，应该如何保存？**

更改新的规则后，如果原有数据仍然想继续统计，请选择“另存为”来定义新的规则。

### **2.3 如何进行文本「内容」的模糊匹配？**

「限定内容」的情况下，将鼠标移动到「内容」的右边，可以看到一个小铅笔，点击小铅笔后，便打开了文本和模式编辑弹窗。默认为精准匹配，即「限定内容为xxx」，若改成模糊匹配，则意义是「限定内容包含xxx」。 保存元素规则后，将按照新的规则进行统计，如果原有数据仍然想继续统计，请选择「另存为」。

![](https://docs.growingio.com/.gitbook/assets/223435.jpg)

### **2.4 3月21日迭代后，新旧版本规则对应关系**

![](https://docs.growingio.com/.gitbook/assets/ping-mu-kuai-zhao-20180319-shang-wu-11.24.51.png)

## 3.不能圈选的可能原因以及对应方法（web）

### **3.1 不能圈选的原因**

不能圈选的原因包含了，但不仅限于：

* 元素包含属性：data-growing-ignore， 因此不可以被圈选。如果需要圈选该元素，请去除该属性。 
* 密码框不支持被圈选。 
* 元素已经被圈选，因此不能被重复圈选。 
* 元素是叶子节点，无文本内容，且元素的占屏幕面积超过 50% ，因此不能被圈选。如果需要圈选该元素，请添加 data-growing-circle 属性。
*  元素所在的 Dom 嵌套层数过多，不在倒数后两层；或者层数符合但是没有实际内容，因此不能被圈选。

### **3.2 data-growing-circle 属性的使用帮助：**

元素是叶子节点，无文本内容，且元素的占屏幕面积超过 50% ，因此不能被圈选。如果需要圈选该元素，请添加 data-growing-circle 属性。例如 ：

```text
 <div id= "d1" style="border: 1px solid black; width: 80%; height: 80%"></div>
```

本来不可以圈选。在添加 data-growing-circle 属性后可以圈选：

```text
<div  id= "d1" style="border: 1px solid black; width: 80%; height: 80%" data-growing-circle></div>
```

## 4.常见问题（web）

**1.如果我的页面改版，现在标记的指标是不是需要重新定义？**

改版后，变化的元素需要重新圈选。

**2.什么时候选择手动圈选，什么时候选择自动圈选？**

使用自动模式可以很方便快捷地圈选某些元素。但是有些时候由于页面框架的实现方式，自动圈选会圈选相似的模块进来，从而造成数据误差，所以必要时可以切换手动圈选来解决问题。

**3. H5 怎么圈选？**

GrowingIO 可以统计原生应用中的 H5 页面和 H5 做成的应用。

* 原生应用中的 H5 页面，以及嵌在应用中的 H5，

  请参考 [iOS/Android圈选指南](app.md)​

* 用 H5 做的应用请参考 [web圈选指南](web.md)​

**4.为什么圈选的元素只有点击量，没有浏览量？**

1. 如果被圈选元素是 a, button, input，img，并且在倒数两层以下，只统计点击量，不统计浏览量。
2. 如果用户使用的是 IE8 及以下的 IE 浏览器版本，无法统计浏览量 ，只统计点击量。

**5.为什么圈选的元素只有浏览量，没有点击量？**

1. 可能真的是没有点击量。
2. 可能选中的是纯文本等没有带超链接可点击的元素。

**6.当页面带查询条件的时候，开关查询条件，数据为什么没有变化？**

当圈选元素所在的页面带查询条件的时候，此时圈选框内展示的数据是不带查询条件的数据。保存带查询条件指标后，指标不会回溯数据，会从保存后开始统计数据。

**7.web 圈选时候页面加载不完全？错位？**

部分浏览器会有兼容性问题，推荐使用 Chrome 浏览器。

**8.web 端已经装了 SDK，现在不能进行圈选。**

1. 有可能是工程师没有成功加载 JS；
2. 可能是该网站禁止了 iframe 的加载，请联系工程师修改配置；
3. 可能是工程师加载 js 代码时的项目 ID 填写有误（项目 ID 没有空格）；
4. 有可能复写 window 对象：可视化圈选时候，必须要保证您的网站与 GrowingIO 平台之间的通信。如果 window.top, window.parent, window.name, window.location 被复写，将导致无法圈选。

如果出于某些原因你不能改变 iframe 或 window 相关设置，建议[下载 GrowingIO Chrome 圈选插件](https://assets.growingio.com/webcircle/extension.zip)进行圈选。

**9.能否在 iframe 中进行圈选？**

如果 iFrame 中的内容集成了 GrowingIO 的代码就可以圈选，否则无法圈选。 GrowingIO 是使用 iframe 来加载目标网页进行可视化定义的。如果目标网站禁止了 iframe 加载，就无法正常定义标签，当点击某个按钮的时候，页面无法发生跳转且命令行显示：

```text
Refused to display '**' in a frame because it set 'X-Frame-Options' to 'SAMEORIGIN'.
```

此时只允许同个顶级域名加载，所以需要设置http响应头允许 iframe 加载。

如果你的网站使用https协议，需向响应头添加配置

```javascript
Content-Security-Policy: frame-ancestors 'self' https://www.growingio.comX-Frame-Options: Allow-From https://www.growingio.com
```

如果你的网站使用http协议，需向响应头添加配置

```javascript
Content-Security-Policy: frame-ancestors 'self' http://www.growingio.comX-Frame-Options: Allow-From http://www.growingio.com
```

由于 Chrome 浏览器已经不再支持 X-Frame-Options 配置项，如果你只需在 Chrome 浏览器中进行圈选，建议通过浏览器检查后，只给 Chrome 请求的响应头添加配置

```javascript
Content-Security-Policy: frame-ancestors 'self' http://www.growingio.com https://www.growingio.com
```

**10.在 web 圈选的时候，为什么有时候会一下圈出一组同类元素，有办法区分开么？**

GrowingIO 根据您网站 HTML 结构识别和定义页面上的元素。有的时候网站上的 HTML 标签写法完全相同，呈现在页面上的几个同类元素，可能 HTML 代码完全相同。此时 GrowingIO 采集、圈选数据时无法区分开。我们通过 HTML 标签的 id 和 class 来区分元素，这种情况下您可以在需要区分的标签 class 中添加一些字符串用于区分。

**仍有疑问？请参考**[**常见问题－圈选部分**](../../faq/faq-circle.md)​

