# 自定义-无埋点圈选事件实施和管理

* [Web圈选](./#web-quan-xuan)
* [App圈选](./#app-quan-xuan)
* [小程序圈选](./#xiao-cheng-xu-quan-xuan)
* [圈选命名规范](./#quan-xuan-ming-ming-gui-fan)

圈选，是利用GrowingIO进行数据分析之前的数据定义过程。您需要根据业务需求，将需要分析的关键事件通过可视化地方式在您的产品界面中定义出来，这个过程，就是圈选。

**例如：** 你关心首页banner的浏览/点击情况，那你就需要对首页banner这个元素进行圈选

GrowingIO提供了丰富的数据定义方式，您可以对您的网页、iOS App、Android App、小程序进行圈选。进入圈选模式后，需要点击相应的元素并根据界面提示做相应的选择进行保存，就能在GrowingIO后台 “指标管理“ 中看到圈选的事件。

## Web圈选

GrowingIO 全量采集用户行为数据，你可以通过「圈选」来定义元素和页面，作为数据分析的基础指标。

在没有定义过的情况下，GrowingIO 保留和回溯元素过去七天的点击量，页面过去七天的浏览量。

通过导航栏进入「圈选」，需要确定你要定义的是元素还是页面，可以在「浏览模式」找到想要分析的元素和页面。

![](https://docs.growingio.com/.gitbook/assets/9412f46a-d87c-41ef-9ee6-9e6e408f4c6a-12626-00000bcf696b73c5_tmp.jpg)

#### 1.开始圈选 {#1-kai-shi-quan-xuan}

**1.1 定义页面**

**在定义页面之前需要理解的基本规则：**

URL**示意：**www.xxx.com **/** 12345/678/123 **?** id=1&ig=2

**拆分：**域名 **/** 路径 **?** 查询条件

**即** www.xxx.com **为域名，**/12345/678/123 **为路径，**?id=1&ig=2 **为查询条件**

如果页面中有 **\#**，请先设置后再进行数据采集和分析：我们默认不会把 hashtag 识别成页面 URL 的一部分。对于使用 hashtag 作为单页应用页面切换的网站来说，请在工程师的帮助下，添加下面的代码，使用`enableHT`来监听 hashtag 的变化，并区分页面来收集页面数据，每次 hashtag 改变都会触发一次PV，hashtag 的信息也会记录在页面 URL 中。

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

例如，GrowingIO 博客文章内容的地址都是这样的 [https://blog.growingio.com/posts/123456](https://blog.growingio.com/posts/123456) 、 [https://blog.growingio.com/posts/14562、](https://blog.growingio.com/posts/14562%E3%80%81) [https://blog.growingio.com/posts/1264](https://blog.growingio.com/posts/1264)......

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

GrowingIO 解决方案首页 [**https://www.growingio.com/solution/**](https://www.growingio.com/solution/)​

GrowingIO 在线旅游解决方案落地页 [**https://www.growingio.com/solution/**](https://www.growingio.com/solution/)online-travel

GrowingIO 互联网金融解决方案落地页 [**https://www.growingio.com**](https://www.growingio.com/)​[**/**](https://www.growingio.com/online-travel)**solution/**internet-finance

我们发现所有的解决方案落地页都是 [**https://www.growingio.com/solution/xxx**](https://www.growingio.com/solution/xxx) ，那么如果我想统计所有解决方案页的页面情况，就可以通过通配符「\*」来定义一组页面 [**https://www.growingio.com/solution\\*，即：**](https://www.growingio.com/solution/*%EF%BC%8C%E5%8D%B3%EF%BC%9A)​

![](https://docs.growingio.com/.gitbook/assets/20_59_35__04_25_2018.jpg)

**案例3：特殊页面定义——页面 url 中带有 “?”**

如果你发现你的页面 url 中带有“?”，除了用于监测广告投放的 utm 之外，需要在定义页面时手动打开「查询条件」的开关，这种情况是不支持回溯数据的，将从你定义页面的时候开始统计数据：

![](https://docs.growingio.com/.gitbook/assets/21_17_01__04_25_2018.jpg)

**1.2 定义元素**

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

第一步：确认元素所在页面是否正确，默认的是“当前页面”；

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

因此，在定义这类元素时，要注意选择全站页面组，如果没有定义全站页面组，需要先去「定义页面」，详见“定义页面部分”的“基本步骤”。

![](https://docs.growingio.com/.gitbook/assets/21_29_04__04_25_2018.jpg)

**案例3：比较一些长得差不多的元素（定义一组元素）**

当我们想要定义一组列表，或者一组元素时，会发现他们通常结构很像，圈选提供了一种便捷的方式，通过定义“同类元素”的方式来定义一组元素。选中这样的元素后，依然是先确认它的所属页面是否正确，然后「不限定任何限定条件」，这时可以注意第二个蓝框中的话：现在定义的是所属页面中，所有同类元素的数据之和。我们已经将这组同类元素用虚线框标记出来。这时，你就成功定义了这个元素所在的列表。

![](https://docs.growingio.com/.gitbook/assets/21_32_36__04_25_2018.jpg)

然后在「事件分析」中使用「元素内容」的维度进行分析。

#### 2.FAQ {#2-faq}

**2.1 如何定义“一组同类元素之和”？**

如果该元素有同类元素，不限定所有的限定条件，即是统计一组同类元素之和的数据。

**2.2 对于已定义过的元素，更改定义规则后，应该如何保存？**

更改新的规则后，如果原有数据仍然想继续统计，请选择“另存为”来定义新的规则。

**2.3 如何进行文本“内容”的模糊匹配？**

限定“内容”的情况下，将鼠标移动到“内容”的右边，可以看到一个小铅笔，点击小铅笔后，便打开了文本和模式编辑弹窗。默认为精准匹配，即“限定内容为xxx”，若改成模糊匹配，则意义是“限定内容包含xxx”。 保存元素规则后，将按照新的规则进行统计，如果原有数据仍然想继续统计，请选择“另存为”。

![](https://docs.growingio.com/.gitbook/assets/223435.jpg)

**2.4 3月21日迭代后，新旧版本规则对应关系**

![](https://docs.growingio.com/.gitbook/assets/ping-mu-kuai-zhao-20180319-shang-wu-11.24.51.png)

#### 3.圈选元素的新增文案（web） {#3-quan-xuan-yuan-su-de-xin-zeng-wen-an-web}

**3.1 元素标签**

元素类型是通过 HTML 标签进行标记的，标签是 HTML 语言中最基本的单位。圈选弹出框里会告诉用户圈到的元素类型。

具体可能圈选到的标签可以参考：[https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element)​

**3.2 圈选到容器的情况**

我们认为的“容器”是嵌套了一个或多个最底层元素的元素。当所圈选的元素是一个有嵌套关系的容器时，为了方便您的圈选，我们对这些最底层元素也同时做了高亮处理，高亮的元素都是支持圈选的。

我们对于容器关系的元素圈选，容器内的元素点击数计算会有以下两种情况： 1. 当前选中容器是类型为 A 的超链接元素，该超链接元素的点击数包含其嵌套区域内部所有元素的点击数总和。 2. 在其他情况下，容器本身的点击数不包含内部所嵌套其他元素的点击数。如果需要整个容器内的点击数总和，请分别圈选该容器内部的各个元素后使用合并简单指标进行计算点击数。

#### 4.不能圈选的可能原因以及对应方法（web） {#4-bu-neng-quan-xuan-de-ke-neng-yuan-yin-yi-ji-dui-ying-fang-fa-web}

**4.1 不能圈选的原因**

不能圈选的原因包含了，但不仅限于： 1. 元素包含属性：data-growing-ignore， 因此不可以被圈选。如果需要圈选该元素，请去除该属性。 2. 密码框不支持被圈选。 3. 元素已经被圈选，因此不能被重复圈选。 4. 元素是叶子节点，无文本内容，且元素的占屏幕面积超过50%，因此不能被圈选。如果需要圈选该元素，请添加data-growing-circle属性。 5. 元素所在的Dom嵌套层数过多，不在倒数后两层；或者层数符合但是没有实际内容，因此不能被圈选。

**4.2 data-growing-circle属性的使用帮助：**

元素是叶子节点，无文本内容，且元素的占屏幕面积超过50%，因此不能被圈选。如果需要圈选该元素，请添加data-growing-circle属性。例如 ：

```text
 <div id= "d1" style="border: 1px solid black; width: 80%; height: 80%"></div>
```

本来不可以圈选。在添加data-growing-circle 属性后，既可以圈选：

```text
<div  id= "d1" style="border: 1px solid black; width: 80%; height: 80%" data-growing-circle></div>
```

#### 常见问题（web） {#chang-jian-wen-ti-web}

**1.如果我的页面改版，现在标记的指标是不是需要重新定义？**

改版后，变化的元素需要重新圈选。

**2.什么时候选择手动圈选，什么时候选择自动圈选？**

使用自动模式可以很方便快捷地圈选某些元素。但是有些时候由于页面框架的实现方式，自动圈选会圈选相似的模块进来，从而造成数据误差，所以必要时可以切换手动圈选来解决问题。

**3. H5 怎么圈选？**

GrowingIO 可以统计原生应用中的 H5 页面和 H5 做成的应用。

* 原生应用中的H5页面，以及嵌在应用中的h5，

  请参考[iOS/Android圈选指南](https://docs.growingio.com/Features/circle/iOSorAndroid.html)​

* 用H5做的应用请参考[web圈选指南](https://docs.growingio.com/Features/circle/Web.html)​

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

**9.能否在 iframe 中进行圈选？**

如果 iFrame 中的内容集成了 GrowingIO 的代码就可以圈选，否则无法圈选。 GrowingIO 是使用 iframe 来加载目标网页进行可视化定义的。如果目标网站禁止了 iframe 加载，就无法正常定义标签，当点击某个按钮的时候，页面无法发生跳转且命令行显示：

```text
Refused to display '**' in a frame because it set 'X-Frame-Options' to 'SAMEORIGIN'.
```

此时只允许同个顶级域名加载，因此需要设置运行 iframe 加载，如果您的网站使用 https 协议，需将配置修改成：

```text
X-Frame-Options: Allow-From https://www.growingio.com
```

**10.在web圈选的时候，为什么有时候会一下圈出一组同类元素，有办法区分开么？\*\***

GrowingIO根据您网站HTML结构识别和定义页面上的元素。有的时候网站上的HTML标签写法完全相同，呈现在页面上的几个同类元素，可能HTML代码完全相同。此时GrowingIO采集、圈选数据时无法区分开。我们通过HTML标签的id和class来区分元素，这种情况下您可以在需要区分的标签class中添加一些字符串用于区分。

**仍有疑问？请参考**[**常见问题－圈选部分**](https://growingio.gitbook.io/docs/~/drafts/-LGyNfnU9qfd7AXzFkhu/primary/faq/faq-circle)​

## App圈选

SDK安卓大于等于 0.9.96，iOS 大于等于 0.9.37 为新版。新版 与旧版在圈选的操作上有所区别，旧版操作将在文末进行介绍 。

#### 准备工作 {#zhun-bei-gong-zuo}

请接入SDK，并把集成SDK的App安装到您的手机。 请确保使用已发布的最新版App进行圈选，以防因为版本不同，出现数据量偏差。

注意：安卓移动端圈选在小米开发者版本和 MIUI8稳定版下调不起小红点，是因为这两个版本系统禁止了悬浮框权限，您可以先尝试信任下。步骤：安全中心-授权管理-应用权限管理-找到您要设置权限的app-找到悬浮框权限-进行授权信任。当然也可以换用其他安卓手机进行圈选。

#### 开始圈选 {#kai-shi-quan-xuan}

**一、登录状态下进入圈选，选择嵌有SDK的目标应用（安卓/iOS）， 开始圈选。**

![](https://docs.growingio.com/.gitbook/assets/quan-xuan-1.png)

**二、手机扫码（确保安装了嵌有SDK的目标应用）， 微信扫码需要在浏览器中打开，浏览器扫码后打开以下页面，点击在手机上圈选可直接唤起应用。**

![](https://docs.growingio.com/.gitbook/assets/app-quan-app-qi-dong-jie-mian-1.png)

**三、初始界面 ：启动圈选后，会看到如下界面，将红点拖动到任一界面元素， 松开即对该元素进行圈选。**

![](https://docs.growingio.com/.gitbook/assets/yi-dong-xin-quan-xuan-di-yi-bu.png)

**四、把页面定义为指标**

拖动红点到指定位置并松开后，进入”选择内容”界面 。中间的蓝色区域，上方代表当前整个页面，下方是刚才红点儿覆盖的元素， 被标记为“按钮”。他们都带有对应内容的截图。

![](https://docs.growingio.com/.gitbook/assets/yi-dong-duan-quan-xuan-di-san-bu.png)

点击代表”页面”的蓝色区域，就进入了页面指标定义界面。

![](https://docs.growingio.com/.gitbook/assets/yi-dong-xin-quan-xuan-di-4-bu.png)

（1）请在最上方的“名称”区域，给当前的页面指标命名。默认的名称是当前页面的标题。输入框下方的“原始名称”，是个当前页面的技术标识。

（2）对于安卓应用，页面技术标识是Activity或者Fragment的名称；对于iOS应用，页面技术标识是UIViewController的名称。

（3）中间的图片区域是当前页面的截图。

（4）最下方的图表代表了当前页面的近期浏览数据。

（5）点击“保存”按钮，就会把当前页面保存成指标，然后您可以在GrowingIO网站中创建图表时引用它。

**五、把界面元素定义为指标（整个按钮/按钮中的文字）**

在圈选调出的蓝色区域中，下方代表按钮的部分，也包含了按钮中所有文字。点击下拉箭头可以看到如下界面。

![](https://docs.growingio.com/.gitbook/assets/yi-dong-xin-quan-xuan-di-san-bu.png)

（1）选择“按钮”部分，表示选中整个按钮。当您对于按钮内可能的文字变化不关心时，请选择此项，定义的指标将只统计按钮的数据，忽略内部文字变化。选择后进入按钮定义界面：

![](https://docs.growingio.com/.gitbook/assets/yi-dong-xin-quan-xuan-di-wu-bu.png)

在这个界面中，除了通用的指标命名之外，下方紧挨着的是这个元素所在页面的描述。“Coding”是我们在程序中为页面定义的标题，"ProjectOtherFragment”则是这个页面的技术标识。

界面下方分为两个Tab，分别是“当前位置”和“同类元素”。出现这样的Tab时，意味着这个按钮在一个按顺序排列的列表中。

a.选择“当前位置”，这样定义的指标，将只统计刚才红点儿覆盖的位置，而不把整个列表中其他元素算进来。运营人员查看不同位置对于点击量的影响时，可以使用这个Tab来定义指标。

b.选择“同类元素”，则表示，定义的指标会把整个列表中每个位置都统计进来，包括所有用户看到的所有内容。

（2）选择“所有文字项”中的任意一个文字，意味着您关心文字的内容。不管是只显示当前的文字时候才统计，还是想把这个文字项所有可能的值都拿来做横向柱图，都请选择此项。

![](https://docs.growingio.com/.gitbook/assets/yi-dong-duan-xin-quan-xuan-di-liu-bu.png)

a.如果选择“当前元素”的Tab，定义了指标。那么只有选中的文字不改变时，才进行统计。

b.如果选择“当前位置”的Tab，定义了指标。那么只有红点覆盖的那个按钮里的文字才统计，不管选的文字有没有发生变化。

c.如果选择“同类元素”，定义了指标。那么整个列表中每个按钮里那个文字都会被统计下来。这样的指标在横向柱状图中使用，会自动把每个文字作为一项列出来。如果运营人员要查看发布的内容哪个最受欢迎，可以选择按钮中的标题文字，然后使用“同类元素”定义指标。这样，就能做出每个标题对应点击量的横向柱状图。

**六、旧版SDK圈选操作**

![](https://docs.growingio.com/.gitbook/assets/yi-dong-quan-xuan-jiu-ban-1.png)

（1）把页面定义为指标：“点击红圈”即进入页面定义页 ；

（2）把界面元素定义为指标： “拖拽红圈”至想要定义的元素位置 即可进入指标定义页面 。

![](https://docs.growingio.com/.gitbook/assets/yi-dong-duan-quan-xuan-jiu-ban-2.png)

## 小程序圈选

#### 页面指标定义 {#ye-mian-zhi-biao-ding-yi}

进入圈选页面后，显示的是所有自动采集到的页面。在这个页面，你可以看到以下信息：

1. 当前要圈选的小程序的产品名
2. 当前要圈选的小程序的 AppID
3. 当前要圈选的小程序的过去 7 天数据表现
4. 当前要圈选的小程序自动采集到的页面列表及其数据

![&#x5C0F;&#x7A0B;&#x5E8F;&#x9875;&#x9762;&#x5708;&#x9009;](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LFBrLMQ6Br7wdTsoufB%2F-LFBwCOk9pJuFNhJ7T5u%2F%E5%B0%8F%E7%A8%8B%E5%BA%8F%E9%A1%B5%E9%9D%A2%E5%9C%88%E9%80%89%E4%BB%8B%E7%BB%8D.png?alt=media&token=5b1c37ea-48a4-4ddc-a4cd-650c6df009dc)

从上图的样例可以看到，GrowingIO 小程序自动采集到 6 个页面，每个页面的具体访问趋势显示在列表内。如果要定义某个页面为指标用于后续分析，点击“**定义页面**“按钮，然后可以看到弹出框，如下图。

![&#x5C0F;&#x7A0B;&#x5E8F;&#x5B9A;&#x4E49;&#x9875;&#x9762;](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LFBrLMQ6Br7wdTsoufB%2F-LFCIlgMuUd8kMMlyBIc%2F%E5%9C%88%E9%80%89%E9%A1%B5%E9%9D%A2%E7%BB%86%E8%8A%82.png?alt=media&token=5b245215-0878-4209-bef8-77e2ba4a9864)

输入页面名称，点击保存，就定义好了一个页面指标。之后，就可以在分析工具里面使用这个指标了。

这里值得注意的是元素点击分布，显示的数据是在这个页面，行为被点击/输入的次数，可以理解成为简易的交互热图分布。

点击具体页面的容器区域，就会进入到该页面的行为指标定义页面，具体见下一节。

#### 行为指标定义 {#hang-wei-zhi-biao-ding-yi}

进入页面的行为圈选页面后，显示的是该页面所有自动采集到的行为页面。在这个页面，你可以看到以下信息：

1. 当前小程序页面的页面名称
2. 当前小程序页面的页面路径
3. 当前小程序页面的过去 7 天数据表现
4. 当前小程序页面自动采集到的行为列表及其数据

![&#x5C0F;&#x7A0B;&#x5E8F;&#x9875;&#x9762;&#x5185;&#x884C;&#x4E3A;&#x5708;&#x9009;](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LFCqNxsL3IBFp-oAtuG%2F-LFD2JJj8RC0hIy797uZ%2F%E5%B0%8F%E7%A8%8B%E5%BA%8F%E8%A1%8C%E4%B8%BA%E5%9C%88%E9%80%89.png?alt=media&token=ff416bc8-3192-40df-aabb-9599d663834b)

 从上图的样例可以看到，GrowingIO 小程序在榜单页面自动采集到 2 个行为，每个行为的具体点击趋势显示在列表内。如果要定义某个行为为指标用于后续分析，点击“**定义行为**“按钮，然后可以看到弹出框，如下图。

![&#x5C0F;&#x7A0B;&#x5E8F;&#x5B9A;&#x4E49;&#x884C;&#x4E3A;](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LFCqNxsL3IBFp-oAtuG%2F-LFD59ZE5It-ZMFZ3yJs%2F%E5%9C%88%E9%80%89%E8%A1%8C%E4%B8%BA%E7%BB%86%E8%8A%82.png?alt=media&token=b8b3dc50-6138-4ff7-8b93-96ff8d4b05cf)

输入行为名称，点击保存，就定义好了一个行为指标。

**这里值得注意**的是元素内容分布和元素位置分布，显示的数据是在这个页面，行为被点击/输入时，对应的内容和位置的交互热图分布。在[无埋点采集事件](https://growingio.gitbook.io/miniprogram/tag-management/sdk-logic/autotrack#tap)里介绍了，如果在视图里使用了 data-title 和 data-index 这种声明式编程，就能在行为数据里看到这些数据。

之后，就可以在分析工具里面使用这个指标了，同时可以在**数据管理-圈选指标**中管理这个指标。

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LGyovQeg-F1vyUV53N6%2F-LGyp73W8d2ZjpMXS8hJ%2Fimage.png?alt=media&token=ca5e17d1-fd60-42ba-ae96-0daa5c314f09)

## 圈选命名规范

#### 1.数据定义 {#1-shu-ju-ding-yi}

在数据定义中，我们已经为您预设了相应的名称。您只需要对名称进行简单的修改，就可以保存了

推荐格式为：

\[`定义类型_名称`\]

* **定义类型**是指您定义对象的类型，常见的有`页面_XX`,`元素_XX`,`列表_XX`等等
* **名称**是方便您回忆的，推荐您按照定义对象来命名，常见的有`XX_首页LOGO`，`XX_登录按钮`，`XX_广告栏`

当您关闭限制条件进行高级定义时。请您在命名时进行标注，方便您自己和您的同事进行理解和回忆

例如

`跨页面_元素_XX`,`元素_所有XX`

#### 2.指标名称 {#2-zhi-biao-ming-cheng}

在创建指标时，我们希望您按照如下的格式进行命名，方便您和您的同事进行进一步的数据分析。

推荐格式为： \[`平台_指标名字_统计含义`\]

例子： `iOS_登录按钮_点击率`，`Web_加入我们_浏览量`，`Android_付款流程_漏斗转化` 等等

如果您的产品版本迭代很频繁，且每次迭代后需要重新圈选指标的话，建议您在名称最后加上版本号，以便在重新圈选后与历史版本的指标作区分。

#### 3.图表名称 {#3-tu-biao-ming-cheng}

在创建图表时，我们推荐您在命名前加上\[`平台_XXX`\]前缀，以帮助您快速的了解图表的含义

