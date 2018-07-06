---
description: >-
  GrowingIO支持 Web，App 等多个用户平台，这给 GrowingIO
  的数据模型提出了较高的要求，既要适配用户在网站上的操作行为模式，又要适配用户在 App 上的操作行为逻辑。
---

# 数据模型

GrowingIO 采用四个可数实体（Countable Entity，简称Countable）层级的数据模型：

**用户 - 访问 - 页面浏览 - 事件**

在这四个层级上，每一个层级都有一些维度和指标。

我们使用C：代表可数实体，D：代表维度，M：代表指标。斜体表示数量可扩展的自定义维度或者指标。

GrowingIO的数据模型可以表示如下：

1. **C：用户（Visitor（User））**
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
2. **C：访问（Visit（mApp Open））**
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
   18. D：自定义App渠道（Custom App Channel）\[mApp\]
   19. M：访问量（Visit）
   20. M：访问用户人均访问次数（Average Visit per Visitor）
   21. M：总访问时长（分钟）（Total Time Spent per Visit）
   22. M：平均访问时长（分钟）（Average Time Spent per Visit）
   23. M：每次访问页面浏览量（Average Page View per Visit）
   24. M：进入量（Entry）\[Web\]
   25. M：访问用户人均进入次数（Average Entry per Visitor）\[Web\]
   26. M：总进入时长（Total Time Spent on Entry）\[Web\]
   27. M：平均进入时长（Average Time Spent per Entry）\[Web\]
   28. M：每次进入页面浏览量（Average Page View Per Entry）\[Web\]
   29. M：总页面停留时长（Total Time Spent on Page）\[Web\]
   30. M：平均页面停留时长（Average Time Spent on Page）\[Web\]
   31. M：跳出次数（Bounce）\[Web\]
   32. M：跳出率（Bounce Rate）\[Web\]
   33. M：退出（Exit）\[Web\]
   34. M：退出率（Exit Rate）\[Web\]
3. **C：页面浏览（PageView）**
   1. D：域名（Domain）\[Web\]
   2. D：页面（Page）\[Web\]
   3. D：页面来源（Page Referral）\[Web\]
   4. _D：自定义页面级变量（Custom Page Level Variable）_
   5. M：页面浏览量（Page View）
   6. _M：圈选页面的页面浏览量（Circled Page Page View）_
4. **C：事件（Event）**
   1. D：设备方向（Device Landscape or Portrait）\[mApp\]
   2. D：元素内容（Element Content）
   3. D：元素位置（Element Location）
   4. _D：自定义事件级变量（Custom Event Level Variable）_
   5. _M：自定义事件（Custom Event）_
   6. M：激活（First Launch）\[mApp\]
   7. _M：圈选事件（Circled Event）_

{% page-ref page="predefined-metrics.md" %}

{% page-ref page="predefined-dimensions.md" %}

{% page-ref page="../circle/" %}

{% page-ref page="../event-variable/" %}

