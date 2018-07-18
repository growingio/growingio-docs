# 打点事件

* [第一步：从数据需求到具体 “指标+维度”](manual-metrics.md#di-yi-bu-cong-shu-ju-xu-qiu-dao-ju-ti-zhi-biao-wei-du)
* [第二步：在 ”打点管理“ 中完成配置](manual-metrics.md#di-er-bu-zai-da-dian-guan-li-zhong-wan-cheng-pei-zhi)
* [第三步：代码部署](manual-metrics.md#di-san-bu-dai-ma-bu-shu)
* [第四步：数据校验](manual-metrics.md#di-si-bu-shu-ju-xiao-yan)

在 GrowingIO 的一个项目内，自定义事件的个数无限制。

## 第一步：从数据需求到具体 “指标+维度”

在GrowingIO上着手进行任何分析之前，首先要确定的一个问题是：如何设计“指标+维度”的体系？对于无埋点数据，我们通过圈选确定“指标”，而“维度”则由 GrowingIO 提供了数个[预定义的维度](https://growingio.gitbook.io/docs/~/drafts/-LGyNfnU9qfd7AXzFkhu/primary/data-defination/dimensions#yu-ding-yi-wei-du)。对于自定义数据，我们可以相对更自由地选择“指标+维度”的体系。

更具体的说，从一个实际场景出发，我们需要确定在分析中需要用到哪些量化的值，然后用什么样的维度来分解这些值。例如，对于电商在分析用户下单情况时，用户的下单量、下单金额就是我们需要量化的“指标”，而每个订单所含具体商品、商品分类、优惠券信息等就是“维度”。那么对于下单这件事，我们就可以这样设计 ”指标+维度“：

* 指标：
  * 订单总量
  * 订单总金额
* 维度：
  * 商品ID/名称
  * 商品SKU
  * 优惠券名称
  * 收货地址

    ...

那么现在很明显了，订单总量与订单总金额这两个指标，需要用 ”自定义事件“ 来实现；商品ID/名称、优惠券名称等维度，则可以通过[事件级变量](https://growingio.gitbook.io/docs/~/drafts/-LGyNfnU9qfd7AXzFkhu/primary/data-defination/events-metrics#shi-jian-ji-bian-liang-pei-zhi)来实现。

在设计“指标+维度”体系时可能需要注意：

* Android、iOS 以及 Web 端不需要区分：对于这三端，可以只定义一个指标，而不需要分别对三端定义三个指标。举例来说，“订单总量”这个指标可以包含三端的总量，在 GrowingIO 平台分析时用预定义维度“网站/手机应用”或者“操作系统”就可以拆分三端分别的量。考虑到自定义事件和事件级变量均有最大个数上限，不建议拆分三端而占用事件数。

## 第二步：在 ”打点管理“ 中完成配置

当我们完成 ”指标+维度“ 的设计之后，请勿直接开始代码的部署，需要先到 GrowingIO 后台找到【数据管理】-【事件与变量】功能，在其中完成对应的配置。

#### **自定义事件配置：** {#zi-ding-yi-shi-jian-pei-zhi}

对于每一个自定义事件，建议您在配置之前，先按下表列出配置细项，其中

* 名称 ==&gt; GrowingIO 后台指标名称
* 描述 ==&gt; 可选，仅作备注使用
* 标识符 ==&gt; 此事件在代码中的标识，可以为英文、下划线和数字，大小写敏感
* 事件级变量 ==&gt; 此事件下属的事件级变量
* 类型 ==&gt; 计数器（递增1）或数值（递增任意值）。当您的事件是计“次数”时，请选用“计数器”类型；当您的事件是计“金额”、“数量/个数”等任意累加值时，请选用“数值”类型。 例如“订单金额”这个自定义事件，就需要选用“数值”类型。

![&#x6253;&#x70B9;&#x4E8B;&#x4EF6;](https://docs.growingio.com/.gitbook/assets/3%20%286%29.png)

填入自定义事件配置界面，如下图：打点事件配置

![](https://docs.growingio.com/.gitbook/assets/4%20%281%29.png)

#### **事件级变量配置：** {#shi-jian-ji-bian-liang-pei-zhi}

从第一步讲到的需求拆解得知，满足一个分析监控需求往往需要同时定义自定义事件和自定义维度。伴随打点事件最常用的是 “事件级变量”，此处一并说明配置方法。更多其他的打点维度请参考[打点维度](https://growingio.gitbook.io/docs/~/drafts/-LGyNfnU9qfd7AXzFkhu/primary/data-defination/dimensions#zi-ding-yi-da-dian-wei-du-bian-liang)文档。

对每一个事件级变量，建议您在配置之前，先按下表列出配置细项，其中

* 变量名称 ==&gt; GrowingIO 后台维度名称
* 变量描述 ==&gt; 可选，仅作备注使用
* 变量标识符 ==&gt; 此变量在代码中的标识，可以为英文、下划线和数字，大小写敏感
* 类型 ==&gt; 可以是字符串或数值（注意：只有在您希望对此事件级变量进行运算时，才需选择数值类型。其他情况建议您优先选择字符串类型。因为一旦选择数值类型，此变量便会作为一个运算项，而无法再图表中展示）

![&#x4E8B;&#x4EF6;&#x7EA7;&#x53D8;&#x91CF;](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LHVXegeSvwqaEGUCEvI%2F-LHVdYIvsplehyn77GJg%2Fimage.png?alt=media&token=a41adaaa-4d0c-4225-b51a-a3052aed65f2)

将上面列好的配置，填入事件级变量配置界面，如下图：事件级变量配置

![](https://docs.growingio.com/.gitbook/assets/ping-mu-kuai-zhao-20180108-xia-wu-5.40.48.png)

点击确定，即完成了事件级变量的配置。

## 第三步：代码部署

在完成了配置后，即可在代码中完成以上设计的 “自定义事件和变量” 的部署。具体的说，就是调用 GrowingIO 提供的API接口，上传数据。

* ​[JS 接口文档](https://growingio.gitbook.io/docs/~/drafts/-LGyNfnU9qfd7AXzFkhu/primary/sdk-integration/web-js-sdk#3-web-js-sdk-2-1-api)​
* ​[Android 接口文档](https://growingio.gitbook.io/docs/~/drafts/-LGyNfnU9qfd7AXzFkhu/primary/sdk-integration/android-sdk#2-android-sdk-api)​
* ​[iOS 接口文档](https://growingio.gitbook.io/docs/~/drafts/-LGyNfnU9qfd7AXzFkhu/primary/sdk-integration/ios-sdk#ios-sdk-api)​
* ​[小程序接口文档](https://growingio.gitbook.io/miniprogram/tag-management/sdk-logic/custom-event)​

API中给出了打点事件和事件级变量的上传方式，以及[其他打点维度（用户级变量、页面级变量、转化变量）](https://growingio.gitbook.io/docs/~/drafts/-LGyNfnU9qfd7AXzFkhu/primary/data-defination/dimensions#zi-ding-yi-da-dian-wei-du-bian-liang)的配置方法。

## 第四步：数据校验

在完成了【数据管理】-【事件与变量】的配置，以及代码实施后，我们当然需要对数据是否成功上传进行校验。校验工作分为两步完成。

**数据校验第一步：本地开发环境校验**

GrowingIO 提供了 SDK debug 模式以及 [debug 工具](https://growingio.gitbook.io/docs/~/drafts/-LGyNfnU9qfd7AXzFkhu/primary/sdk-integration/growingio-debugger)，来帮助您完成数据的校验。

**数据校验第二步：GrowingIO 后台图表验证**

在 GrowingIO 分析后台，找到 “单图” - “新建事件分析”，然后在图表中选择您设计好的 “指标+维度”，查看是否有数据。当然，您需要首先确保打点事件或维度确实有被触发。

详细文档：[打点数据验证方法](https://s.growingio.com/ZpbXBY)

至此，您已经完成了 “打点事件” 的上传，如您在配置或添加代码中有任何疑问，请联系您的客户成功经理咨询，或在工单系统中反馈问题。谢谢。

