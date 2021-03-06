---
description: 使用 GrowiongIO 的「合并事件」功能将多个事件合并成为一个，进行跨平台转化分析与多入口转化分析
---

# 合并事件管理

* [1. 简介](data-management-merged-events.md#1-jian-jie)
* [2. 合并事件功能使用](data-management-merged-events.md#2-he-bing-shi-jian-gong-neng-shi-yong)
  * [2.1 创建合并事件](data-management-merged-events.md#21-chuang-jian-he-bing-shi-jian)
  * [2.2 管理合并事件](data-management-merged-events.md#22-guan-li-he-bing-shi-jian)
* [3. 合并事件计算口径说明](data-management-merged-events.md#3-he-bing-shi-jian-ji-suan-kou-jing-shuo-ming)
* [4. 常见问题](data-management-merged-events.md#4-chang-jian-wen-ti)

## **1. 简介**

在现实场景中，我们时常会有将多个事件合并成一个事件的需求，如进行跨平台转化分析、多入口转化分析，或是将改版前后的事件合并为一个等，这时我们可以使用 GrowiongIO 的「合并事件」功能将多个事件合并成为一个。常见的使用场景如下：

1. **多入口转化：**当转化流中有多个功能入口，在衡量转化漏斗时，您可以将多个分别圈选的入口事件合并为一个在漏斗功能中使用，分析整体转化情况。
2. **跨平台分析：**如果您想要对跨平台的数据进行汇总分析，如iOS和Andorid两个APP的总体注册转化率，您可以分别将两个平台圈选的无埋点事件合并为一个在漏斗功能中使用，分析整体转化情况。
3. **包含多个功能的功能模块：**当产品中有一组相互关联的功能，希望分析这一组功能的整体点击率、留存率等指标，您可以将这一组功能合并起来，使用事件分析、留存等分析模块来分析该 "功能模块"的表现。
4. **改版导致指标重定义：**注册页改版之前圈选的注册按钮，定义成事件 A；后来网站注册流改版，导致注册按钮的定义发生变化，这时需要重新圈选新的注册按钮并定义成事件 B。如果想要分析注册按钮在改版前后的点击和转化情况，可以将 A 和 B 进行合并，合并成事件 C，观察 C 的数据即可。

在新的数据管理上线后，我们将原先的「合并指标」功能全面升级为「合并事件」功能，新的合并事件功能不仅支持了使用埋点事件进行创建合并事件，更支持使用支持跨平台、跨类型的事件任意组合。您原先创建的合并事件也会全数迁移到新的计合并事件功能当中。

## **2. 合并事件功能使用**

您可以在合并事件管理界面创建合并事件，并对产品内所有的合并事件进行统一的维护与管理。您可以通过点击导航栏【数据中心】-【数据管理】按钮，进入数据管理界面，点击左侧【合并事件】菜单，进入合并事件管理界面。

### **2.1 创建合并事件**

点击合并事件管理界面左上角【创建合并事件】按钮，开始创建合并事件。

![&#x5408;&#x5E76;&#x4E8B;&#x4EF6;&#x914D;&#x7F6E;&#x754C;&#x9762;](../.gitbook/assets/wechatimg70.jpeg)

新的合并事件功能支持使用无埋点事件与埋点事件创建合并事件，以及使用跨平台（ex. iOS 注册成功事件与 Android 注册成功事件合并）、跨类型（ex. 埋点事件与无埋点事件合并、无埋点点击事件与无埋点页面浏览事件合并）的事件任意组合，您可以在创建合并事件时根据您的分析场景选择欲合并的事件。

### **2.2 管理合并事件**

合并事件一经保存后，便可以在合并事件管理界面中对合并事件进行管理。合并事件列表呈现了您项目中所有的合并事件以及各合并事件的创建人、创建时间、更新时间等属性。您可以在合并事件列表界面根据您的需求搜索或是筛选您需要的合并事件。

点击任一合并事件，您可以在右侧弹出的事件详情中，查看合并事件的定义条件与统计趋势。您也可以通过点击右上角编辑按钮，修改合并事件的定义。另外，您也可以在事件详情当中为合并事件添加描述说明或是业务标签。

此外，新创建的合并事件资源权限默认【所有人可见，不可编辑】，所有人都能够看到以及使用改事件，但只有该事件的创建者和项目的超级管理员拥有权限对该事件进行编辑。您可以通过在【事件详情】-【其他信息】中点击【权限】方框，更改合并事件的资源权限。

## **3. 合并事件计算口径说明**

1. 合并事件功能仅支持使用“事件”进行合并（包含无埋点事件、埋点事件），不支持使用“指标”（计算指标、预定义指标）进行合并；
2. 当您将 A、B、C 三个事件进行合并时，合并事件的次数为 A、B、C 三个事件的次数加和。合并事件的用户量是 A、B、C 三个事件的用户相加去重后的总量，也就是完成过 A 或 B 或 C 的用户量总和。

## **4. 常见问题**

**1. 合并事件和计算指标有什么区别呢？**

合并事件，常用来表征同样业务含义的事件相加，比如多个注册入口按钮，不同平台的相同事件等。计算指标，常常用来支持更加复杂的业务逻辑，比如用户活跃度（带有权重设置）、点击率、转化率产品渗透率等带有除法运算的指标等。合并事件可以在漏斗、留存分析等各个模块中使用；计算指标不可以被用在漏斗、留存分析、实时等分析和监控模块中。

**2. 为什么在新的合并事件中只能看到“次”，看不到是“点击”还是“浏览”了？**

我们在这次数据管理迭代中全面升级了合并事件（原先的「合并指标」）的数据模型和计算能力。新的合并事件功能不仅支持了使用埋点事件创建合并事件，更支持使用支持跨平台（ex. iOS 注册成功事件与 Android 注册成功事件合并）、跨类型（ex. 埋点事件与无埋点事件合并、无埋点点击事件与无埋点页面浏览事件合并）的事件任意组合。而由于原先我们只支持同类型的无埋点事件进行合并（元素只能与元素进行合并，页面只能与页面进行合并），因此在新的合并事件支持跨类型的事件合并后，合并事件将不在区分事件类型，而统一为“次”，但您可以通过事件预览查看合并事件的定义，来判断合并事件的事件类型，并通过命名或是添加描述来加以区分。

