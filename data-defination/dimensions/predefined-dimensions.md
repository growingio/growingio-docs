# 预定义维度

在 GrowingIO 数据模型中，提供了 36 个预定义维度，支持丰富的分析场景，同时也可以通过[创建自定义变量](manual-dimensions.md)的方式添加其他变量。

* [第一部分：用户来源维度](predefined-dimensions.md#01)
  * [1.访问来源（web/app）](predefined-dimensions.md#11)
  * [2.一级访问来源（web/app）](predefined-dimensions.md#12)
  * [3.搜索词（web/app）](predefined-dimensions.md#13)
  * [4.App 版本（app）](predefined-dimensions.md#14)
  * [5.自定义 App 渠道（app）](predefined-dimensions.md#15)
* [第二部分：地域信息维度](predefined-dimensions.md#02)
  * [1.城市名称（web/app）](predefined-dimensions.md#21)
  * [2.地区名称（web/app）](predefined-dimensions.md#22)
  * [3.国家代码（web/app）](predefined-dimensions.md#23)
  * [4.国家名称（web/app）](predefined-dimensions.md#24)
* [第三部分：设备信息维度](predefined-dimensions.md#03)
  * [1.网站/手机应用（web/app）](predefined-dimensions.md#31)
  * [2.屏幕大小（web/app）](predefined-dimensions.md#32)
  * [3.操作系统（web/app）](predefined-dimensions.md#33)
  * [4.操作系统版本（web/app）](predefined-dimensions.md#34)
  * [5.操作系统语言（web/app）](predefined-dimensions.md#35)
  * [6.浏览器（web）](predefined-dimensions.md#36)
  * [7.浏览器版本（web）](predefined-dimensions.md#37)
  * [8.设备品牌（app）](predefined-dimensions.md#38)
  * [9.设备型号（web/app）](predefined-dimensions.md#39)
  * [10.设备类型（web/app）](predefined-dimensions.md#310)
* [第四部分：其他维度](predefined-dimensions.md#04)
  * [1.域名（web）](predefined-dimensions.md#41)
  * [2.页面（web/app）](predefined-dimensions.md#42)
  * [3.页面来源（web/app）](predefined-dimensions.md#43)
  * [4.设备方向（app）](predefined-dimensions.md#44)
  * [5.元素内容（web/app）](predefined-dimensions.md#45)
  * [6.元素位置（web/app）](predefined-dimensions.md#46)
  * [7.时间（web/app）](predefined-dimensions.md#47)
* [常见问题](predefined-dimensions.md#05)
  * [“超出维度行数上限”是什么意思？](predefined-dimensions.md#51)

### 第一部分：用户来源维度 {#01}

> **通过用户来源了解用户是怎样来到你的网站上的。**

#### 1.访问来源（web/app） {#11}

网站的流量来源，可以是百度，谷歌，优酷等站外渠道，也可能是直接访问\*该网站。可以通过设置 UTM 参数来确定流量具体是哪个广告带来的。

访问来源中微信类来源: 微信目前是一个非常重要的流量来源。一般不考虑广点通等付费推广的话，微信渠道上很多H5\WEB等有非常成熟的使用和分享、获客场景，根据微信环境中打开的数据参数，GrowingIO 识别了几种微信环境访问 H5/PC 的分类，常见的用例有这样几种：

| 场景 | 访问来源 | 解释 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 扫描二维码在微信中打开H5页面查看 | 微信-其他 | 如果不使用GrowingIO生成具体的追踪链接，仅可以识别微信环境打开；如果想要识别具体的二维码的信息，例如那篇文章的，哪个公众号等，详情可以了解广告监测链接的相关功能。 |
| 公众号中查看图文消息-点击“阅读原文” | 微信-公众号 | GroiwngIO会识别到微信的环境，微信会返回“公众号”访问来源链接（例如mp.weixinbridge.com\)，即可以判断出公众号的来源。 |
| 公众号直接给文字/图片链接，用户打开 | 微信-其他 | 如果不使用GrowingIO生成具体的追踪链接，仅可以识别微信环境打开；如果想要识别具体的二维码的信息，例如那篇文章的，哪个公众号等，详情可以了解广告监测链接的相关功能。 |
| 朋友圈里查看分享的页面类链接 | 微信-朋友圈 | GroiwngIO会识别到微信的环境，同时微信会给出相应的技术参数，即可以判断为朋友圈打开。 |
| 查看分享给群组的页面链接 | 微信-群聊 | GroiwngIO会识别到微信的环境，同时微信会给出相应的技术参数，即可以判断为群组信息中打开。 |
| 查看分享给单个好友的页面链接 | 微信-单聊 | GroiwngIO会识别到微信的环境，同时微信会给出相应的技术参数，即可以判断为好友信息中打开。 |
| 打开分享到朋友圈、群聊、单聊消息中的URL链接 | 微信-其他 | 如果仅是URL，只会追踪具体的URL，但可以识别在微信环境中打开。 |

#### 2.一级访问来源（web/app） {#12}

为了便于分析，我们将访问来源进行了归类：直接访问，搜索引擎，社交媒体，外部链接四大部分。

**直接访问：**   
可能是用户直接在浏览器中输入了一个域名或使用书签进行访问；   
也有可能是   
a.从邮件中点击链接访问网站取决于电子邮件的提供商/程序）；   
b.从 Microsoft Office 或 PDF 文件中点击链接访问网站；   
c.通过点击由原 url 生成的短链接访问网站；   
d.通过 App 点击链接访问网站（比如今日头条、微博、微信中的链接）；   
e.通过点击一个 https 类型的 url 访问一个 http 类型的 url（比如如果点击 [https://example.com/1](https://example.com/1) 转到 [http://example.com/2](http://example.com/2), 对于 example.com/2 的分析会认为是直接访问）；   
f.部分浏览器（特别是移动端浏览器）会把搜索跳转当成直接访问；

**搜索引擎：**   
www.baidu.com，m.baidu.com，bzclk.baidu.com，so.com, sogou.com, bing.com, youdao.com，zhongsou.com，google.xx.xx，sm.cn，yahoo.com。

**社交媒体：**   
weibo.com，t.cn, weibo.cn，zhihu.com，linkedin.com，renren.com，facebook.com，twitter.com，mp.weixin.qq.com 等。   
由于目前微信已经作为一个较重要的访问来源，所以 GrowingIO 目前专门针对微信环境做了判定：如果 PC 网页或 H5 的访问是发生在 wechat 环境中，也会被归入社交媒体，而根据不同的微信后置参数，进一步区分访问来源，详情请看访问来源中微信来源的维度解释。

**外部链接：**   
除了社交媒体，搜索引擎和直接访问之外的来源。

#### 3.搜索词（web） {#13}

用户从搜索引擎进入网站所使用的搜索词；一般情况下付费搜索的搜索词可以被解析，而百度谷歌渠道自然搜索词无法获取。

#### 4.App 版本（app/小程序） {#14}

App 和小程序 的版本号

#### 5.自定义 App 渠道（app/小程序） {#15}

自定义 App 渠道：

在APP集成SDK的时候，针对安卓用户，设置分包渠道。

在小程序中，则自动获取小程序打开的来源。目前支持微信中60多种统计场景，例如：二维码扫码，群聊中点击分享的小程序卡片等。详情请见[小程序概览-获客场景](../../data-analytics/mina-overview.md#huo-ke-chang-jing)。

**案例：**在「事件分析」横向柱图中添加不同的「维度」，可以查看不同访问来源下的页面浏览量情况。

​

![&#x6B64;&#x5904;&#x8F93;&#x5165;&#x56FE;&#x7247;&#x7684;&#x63CF;&#x8FF0;](http://growing.cn-bj.ufileos.com/ccc10.png)

> **通过「广告监测」设置 App 推广的维度值**

通过顶部导航栏进入「广告监测」功能，设置 App 推广的字段：

![&#x6B64;&#x5904;&#x8F93;&#x5165;&#x56FE;&#x7247;&#x7684;&#x63CF;&#x8FF0;](http://growing.cn-bj.ufileos.com/ccc11.png)

> **通过「广告监测」设置网站推广的维度值**

通过顶部导航栏进入「广告监测」功能，设置网站推广的字段：

![&#x6B64;&#x5904;&#x8F93;&#x5165;&#x56FE;&#x7247;&#x7684;&#x63CF;&#x8FF0;](http://growing.cn-bj.ufileos.com/ccc12.png)

> **支持自定义 UTM 参数**

| UTM 参数 | 使用方式 | 示意 |
| --- | --- | --- | --- | --- | --- |
| 广告来源 utm\_source | 标识投放的网站，例如：google、baidu、toutiao | utm\_source=Google |
| 广告媒介 utm\_medium | 广告媒介或营销媒介，例如：CPC、Banner 和 EDM | utm\_medium=cpc |
| 广告名称 utm\_campaign | 产品的具体广告系列名称、标语、促销代码等 | utm\_campaign=spring\_sale |
| 广告关键词 utm\_term | 标识付费搜索关键字 | utm\_term=running+shoes |
| 广告内容 utm\_content | 用于区分相似内容或同一广告内的链接。如果在同一封 EDM 中使用了两个不同的链接，就可以使用 utm\_content 设置不同的值，以便判断哪个版本的效果更好 | utm\_content=logolink or utm\_content=textlink |

UTM 渠道归因模式为非直接访问的最后一次访问。

**案例：**可以通过设置 utm 参数，选择相应的维度「广告来源」、「广告内容」、「广告名称」、「广告关键词」、「广告媒介」等，来监控投放的推广内容效果。

![&#x6B64;&#x5904;&#x8F93;&#x5165;&#x56FE;&#x7247;&#x7684;&#x63CF;&#x8FF0;](http://growing.cn-bj.ufileos.com/ccc13.png)

### 第二部分：地域信息维度 {#02}

> **了解用户在哪里访问你的网站**

#### 1.城市名称（web/app/小程序） {#21}

web 基于 IP 地址，以城市作为维度值，目前只支持国内城市。   
app 和小程序 基于 IP 地址和 GPS 。优先判断GPS值，GPS值缺失时，使用IP地址。

#### 2.地区名称（web/app/小程序）

web 基于IP地址，该维度包含国内省级以上行政区，以及国外地区。   
app 和小程序 基于 IP 地址和 GPS 。优先判断GPS值，GPS值缺失时，使用IP地址。

#### 3.国家代码（web/app/小程序） {#23}

用户所在的国家的英文缩写，常见的维度值有：CN，US，JP，SG等。

#### 4.国家名称（web/app/小程序） {#24}

用户所在的国家的名称，常见的有：中国，美国，英国，新加坡等。

> 技术说明：维度指为「未知」的原因：可能是用户使用的是移动网络，或开了代理。

#### 5.微信用户所在城市（小程序）

小程序特有维度。表示用户在微信端设置的所在城市。使用前提是小程序SDK获取到微信访问用户属性。详情请见 [小程序SDK-微信用户属性设置](../../sdk-integration/mina-sdk.md#sdk-wei-xin-yong-hu-shu-xing-she-zhi)。

#### 6.微信用户所在省（小程序）

小程序特有维度。表示用户在微信端设置的所在省。使用前提是小程序SDK获取到微信访问用户属性。详情请见 [小程序SDK-微信用户属性设置](../../sdk-integration/mina-sdk.md#sdk-wei-xin-yong-hu-shu-xing-she-zhi)。

#### 7.微信用户所在国家（小程序）

小程序特有维度。表示用户在微信端设置的所在国家。使用前提是小程序SDK获取到微信访问用户属性。详情请见 [小程序SDK-微信用户属性设置](../../sdk-integration/mina-sdk.md#sdk-wei-xin-yong-hu-shu-xing-she-zhi)。

### 第三部分：设备信息维度 {#03}

> **了解用户使用什么设备访问你的网站**

#### 1.网站/手机应用（web/app/小程序） {#31}

用于区分该设备是接入了 GrowingIO 的 JS SDK 还是 iOS SDK、Android SDK 等。

#### 2.屏幕大小（web/app/小程序） {#32}

web 端是窗口大小，移动端是屏幕大小。

#### 3.操作系统（web/app/小程序） {#33}

用户所使用的操作系统，比如 Windows 8 ，Windows 7 ，Mac OS X ，Android，weixin-iOS，weixin-Android。

#### 4.操作系统版本（web/app/小程序） {#34}

同「浏览器」，但是会按照不同的版本进行区分，比如 Android 4.0 等。

#### 5.操作系统语言（web/app/小程序） {#35}

统计不同的操作系统语言的使用情况，比如简体中文等。

#### 6.浏览器（web） {#36}

用户所用浏览器的类型，比如 Chrome，Chrome Mobile，Safari，IE 等。

#### 7.浏览器版本（web） {#37}

同「浏览器」，但是会按照不同的版本进行区分，比如 Chrome 47.0.2526 等。

#### 8.设备品牌（app/小程序） {#38}

将不同设备的品牌作为维度值。

#### 9.设备型号（web/app/小程序） {#39}

用于区分具体的机型。

#### 10.设备类型（web/app/小程序） {#310}

设备的类型，平板和手机。

### 第四部分：其他维度 {#04}

> **页面级维度**

对于 web 端可以这样理解：   
用户先访问了 [https://www.growingio.com/](https://www.growingio.com/)   
再访问了 [https://www.growingio.com/circle](https://www.growingio.com/circle)

#### 1.域名（web/小程序） {#41}

www.growingio.com 是这两个页面的域名。

小程序为appID.

#### 2.页面（web/app/小程序） {#42}

对于 web 端可以这样理解：   
/ 是 [https://www.growingio.com/](https://www.growingio.com/) 的页面。   
/circle 是 [https://www.growingio.com/circle](https://www.growingio.com/circle) 的页面。

对于 app 端可以这样理解：   
android : activity + fragment   
iOS : UIViewController

小程序：页面URL（不包括query）

#### 3.页面来源（web/app/小程序） {#43}

这次访问中 [https://www.growingio.com/circle](https://www.growingio.com/circle) 这个页面的页面来源是 [https://www.growingio.com/](https://www.growingio.com/) 。

> **事件级维度：当次事件发生时，对应的维度**

#### 4.设备方向（app） {#44}

可以理解为用户的手机时横屏还是竖屏。

#### 5.元素内容（web/app/小程序） {#45}

通过圈选可定义定义的维度包括“元素内容“ 和 “元素位置“。

圈选某个元素时，如果该元素本身有 "内容"信息，即可用 "元素内容" 来统计该元素对应的指标。

#### 6.元素位置（web/app/小程序） {#46}

如果该元素本身有 "位置"信息，即可用 "元素位置"来统计该元素对应的指标。

**案例：**我们在圈选时只能看到一种元素，但是可能用户看到的网站内容不一样，元素的内容也不一样，因此，当我们在定义一个元素时，要思考我们想要统计的是「现在这个位置的按钮点击情况」还是「这个位置上这个内容的按钮的点击情况」。常见于商品位、AB测试等使用场景。

在下图中，有一部分用户看到的按钮是「立即注册」，还有一部分用户看到的按钮是「免费试用」，因此，如果我想定义这个按钮的点击情况，不管是什么内容，那么就只「限定顺序」，不限定其他条件：

![](https://docs.growingio.com/.gitbook/assets/21_32_36__04_25_2018%20%281%29.jpg)

如果想看这个位置上不同内容的点击情况，可以在「事件分析」中，选择「柱图」，用「元素内容」作为维度，来进行分析。使用「元素位置」作为维度来分析时，可以看到「元素位置」的值都是 2（因为在圈选时限定了元素的位置）。

#### 7.时间（web/app/小程序） {#47}

以「小时」粒度切分时，代表当前小时的开始到结束；   
以「天」粒度切分时，代表北京时间的 0:00-24:00。

### 第五部分：用户性别

#### 1. 微信用户性别 （小程序）

小程序特有维度。表示用户在微信端设置的性别。通常为男、女、未知（表示未设置性别信息）。使用前提是小程序SDK获取到微信访问用户属性。详情请见 [小程序SDK-微信用户属性设置](../../sdk-integration/mina-sdk.md#sdk-wei-xin-yong-hu-shu-xing-she-zhi)。

### 常见问题 {#05}

#### “超出维度行数上限”是什么意思？ {#51}

我们使用维度和指标来进行数据分析。维度用来决定一个分析的角度，指标用来展现这个角度的数据。例如，我们可以使用“网站/手机应用”作为维度，页面浏览量作为指标，在GrowingIO中制作出下面这张表：

![&#x6B64;&#x5904;&#x8F93;&#x5165;&#x56FE;&#x7247;&#x7684;&#x63CF;&#x8FF0;](http://growing.cn-bj.ufileos.com/ccc1.png)

“网站/手机应用”维度一共有三个值，分别是：Web、iOS、Android、minP。那么右边的这几行数值分别代表这四个平台获得的页面浏览量指标。

维度就像一个集合，其中存放的是从某个角度分析的描述性字符串。集合论中有一个重要的概念，叫做“基数”，描述的就是集合中元素的个数。在上面的这个例子中，我们使用的“网站/手机应用”维度有三个值，分别是“Web、iOS、Android”。在这个情况下，我们可以称这个维度的“基数”为3。不难想到，在GrowingIO系统中，有些维度的基数是很大的，例如页面维度。很多电子商务类型的网站，产品详情页URL会是类似于：[http://item.ecommerce.com/](http://item.ecommerce.com/){productId}.html。那么如果这个网站有N个商品有页面浏览（至少一次页面浏览），页面这个维度的基数就是N，可见这个时候N的值是非常巨大的。 GrowingIO系统为了更加专注资源在用户关注的页面或者事件上，引入了处理“巨大基数维度“（High Cardinality Dimensions）”算法来区别对待产生巨大流量页面、事件和长尾的页面、事件。

以页面维度为例，算法细节如下：   
以天作为一个计算单位，天级别的数据中包含的不同的值如果低于2000，则正常计算所有的页面，这些所有的页面都会作为“页面”维度的维度值展示在报表中。

当天级别的数据中页面维度的不同的值超过2000时，系统自动进入“巨大基数维度”的整理模式。

在整理模式中，每当添加一个新的值到页面维度中，系统会自动计算出当前页面浏览量总数的前95%的维度的值按照从大到小排序的位置编号值，假设为x。如果x小于5000，则页面维度的不同的值保留到该位置为止，剩下的维度值和相应的指标数值合并到一个称为“超出维度行数上限”的行中。如果x大于5000，则按照页面浏览量数值大小，取前5000行，剩下的维度值和相应的指标数值合并到一个称为“超出维度行数上限”的行中。

其他常见问题详见[这里。](../../faq/faq-metrics-dimensions.md)



