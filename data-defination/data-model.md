# 数据模型

* [数据模型简介](data-model.md#shu-ju-mo-xing-jian-jie)
  * [四层模型：用户 - 访问 - 页面浏览 - 事件](data-model.md#si-ceng-mo-xing-yong-hu-fang-wen-ye-mian-lan-shi-jian)
  * [访问用户 和 登录用户](data-model.md#fang-wen-yong-hu-he-deng-lu-yong-hu)
    * [访问用户](data-model.md#fang-wen-yong-hu)
    * [登录用户](data-model.md#deng-lu-yong-hu)
* [无埋点（圈选）和打点数据模型一致性说明](data-model.md#wu-mai-dian-quan-xuan-he-da-dian-shu-ju-mo-xing-yi-zhi-xing-shuo-ming)

## 数据模型简介

GrowingIO支持 Web，App，小程序 等多个用户平台，这给 GrowingIO 的数据模型提出了较高的要求，既要适配用户在网站上的操作行为模式，又要适配用户在 App 上的操作行为逻辑。需要满足2个核心要求：

* 数据模型是适配各个平台的
* 能够支持跨平台的用户行为分析

举个例子来说，小明在刷朋友圈时看到了某个电商平台的广告，他点击这个广告，进入了一个引导注册的 H5页面，小明填写自己的手机号通过验证，注册成功，并在成功页面看到提示下载 App 即送5涨满减优惠券，小明下载了 App 并登录，选择了自己心仪的产品后使用优惠券结算，完成了购买流程。

在上面的这个过程中，涉及到了**3个平台**：微信（广告投放媒体）、H5、App 三端；**2个 ID**：小明在完成注册之前，对于电商平台来说是匿名用户，假设这个时候他的 匿名 ID 是 xxxx，完成注册后获得了注册用户 ID: xiaoming 。我们希望通过 GrowingIO 能够完成小明在这3端的转化是如何完成的。为了满足以上诉求，

1. GrowingIO 采用四个可数实体（Countable Entity）层级的数据模型：**用户 - 访问 - 页面浏览 - 事件**
2. GrowingIO 提供了 "访问用户 ID" 和 "登录用户 ID" 两种用户 ID 体系

### **四层模型：用户 - 访问 - 页面浏览 - 事件**

需要说明的是，这四层数据模型是树形属性。在实际用户浏览网站或使用 App 时，一个用户可以发起多个访问，一个访问中会有多个页面浏览，同一个1个页面上会触发多个不同事件，如下图所示：

![&#x56DB;&#x5C42;&#x6811;&#x5F62;&#x7ED3;&#x6784;](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LHWllLuJXiaZOdbWcp5%2F-LHWygFc97JBAqidzyKH%2Fimage.png?alt=media&token=f202ecc4-98c0-4dba-86a2-097dbf015996)

在这四个层级上，每一个层级都有一些[维度和指标。](terminology.md)

我们使用D：代表维度，M：代表指标。斜体表示数量可扩展的自定义维度或者指标。

GrowingIO的数据模型可以表示如下：

1. **用户（Visitor（User））**
   1. D：网站/手机应用（WebSite/mApp）
   2. D：屏幕大小（Screen Size）
   3. D：操作系统（Operation System）
   4. D：设备品牌（Device Brand）
   5. D：设备型号（Device Model）
   6. D：设备类型（Device Type）
   7. D：设备制造商（Device Manufacture）
   8. D：浏览器（Browser）\[Web \]
   9. D：移动应用-推广活动（Mobile App Campaign）\[mApp\]
   10. D：移动应用-广告目标渠道（Mobile App Channel）\[mApp\]
   11. D：移动应用-监测链接（Mobile App Tracking Link）\[mApp\]
   12. M：访问用户量（Visitor）
   13. M：新访问用户量（New Visitor）
2. **访问（Visit（mApp Open））**
   1. D：访问来源（Visit Referral）\[Web\]
   2. D：一级访问来源（First Level Visit Referral）\[Web\]
   3. D：搜索词（Search Keyword）\[Web\]
   4. D：网页监测链接（Web Tracking Link）\[Web\]
   5. D：广告来源（UTM\_Source）\[Web\]
   6. D：广告名称（UTM\_Campaign）\[Web\]
   7. D：广告内容（UTM\_Content）\[Web\]
   8. D：广告关键词（UTM\_Keyword）\[Web\]
   9. D：广告媒介（UTM\_Media）\[Web\]
   10. D：浏览器版本（Browser Version）\[Web\]
   11. D：操作系统版本（Operating System Version ）
   12. D：操作系统语言（Operating System Language ）
   13. D：国家代码（Country Code）
   14. D：国家名称（Country Name）
   15. D：地区名称（District Name）
   16. D：城市名称（City Name）
   17. D：App版本（App Version）\[mApp\]
   18. D：设备方向（Device Landscape or Portrait）\[mApp\]
   19. D：自定义App渠道（Custom App Channel）\[mApp\]
   20. M：访问量（Visit）
   21. M：访问用户人均访问次数（Average Visit per Visitor）
   22. M：总访问时长（分钟）（Total Time Spent per Visit）
   23. M：平均访问时长（分钟）（Average Time Spent per Visit）
   24. M：每次访问页面浏览量（Average Page View per Visit）
   25. M：进入量（Entry）\[Web\]
   26. M：访问用户人均进入次数（Average Entry per Visitor）\[Web\]
   27. M：总进入时长（Total Time Spent on Entry）\[Web\]
   28. M：平均进入时长（Average Time Spent per Entry）\[Web\]
   29. M：每次进入页面浏览量（Average Page View Per Entry）\[Web\]
   30. M：总页面停留时长（Total Time Spent on Page）\[Web\]
   31. M：平均页面停留时长（Average Time Spent on Page）\[Web\]
   32. M：跳出次数（Bounce）\[Web\]
   33. M：跳出率（Bounce Rate）\[Web\]
   34. M：退出（Exit）\[Web\]
   35. M：退出率（Exit Rate）\[Web\]
3. **页面浏览（PageView）**
   1. D：域名（Domain）\[Web\]
   2. D：页面（Page）\[Web\]
   3. D：页面来源（Page Referral）\[Web\]
   4. _D：自定义页面级变量（Custom Page Level Variable）_
   5. M：页面浏览量（Page View）
   6. _M：圈选页面的页面浏览量（Circled Page Page View）_
4. **事件（Event）**
   1. D：元素内容（Element Content）
   2. D：元素位置（Element Location）
   3. _D：自定义事件级变量（Custom Event Level Variable）_
   4. _M：自定义事件（Custom Event）_
   5. M：激活（First Launch）\[mApp\]
   6. _M：圈选事件（Circled Event）_

### 访问用户 和 登录用户

#### 访问用户

访问用户ID是GrowingIO随时生成的唯一ID，如果你想要分的是析产品所有访客，可以选择“访问用户”；上面的例子中，小明访问 H5注册页面时，GrowingIO 会获取浏览器 cookie 会据此生成1个唯一的 ID 作为小明在该 H5站点的唯一 ID

####  登录用户

登录用户 ID：也就是注册用户 ID，当用户访问您的产品并发生注册/登录行为时，您可以将该用户的注册 ID（或与之对应的唯一标识，可以加密处理）上传给 GrowingIO，这个 ID 会被作为 "登录用户" 类型来处理。在上面的例子中，如果将小明的注册 ID 在小明发生 H5 注册时和 App 登录时通过 API 上传到 GrowingIO，即可进行跨终端分析。

除了 ID 之外，您还可以将登录用户的性别，年龄等信息上传。GrowingIO 通过您上传的登录用户ID 和用户的其他属性信息和 GrowingIO 采集的用户行为数据进行匹配，方便您对自己的用户做更深入的分析。

[如何上传登录用户 ID 及其他属性信息？](dimensions/predefined-dimensions.md#yong-hu-bian-liang)

## 无埋点（圈选）和打点数据模型一致性说明

GrowingIO 支持无埋点采集+圈选定义数据，以及通过打点方式自定义数据。这两种方式的数据都符合上述的四层数据模型。这四层模型进一步总结成 GrowingIO 后台可用来分析的数据，包含了以下两大模块：

