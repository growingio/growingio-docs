# 埋点事件级变量

我们在[埋点事件](./)文档中已经讲解了埋点事件的配置，本文将展开讲解埋点事件级变量的定义和配置细节。

* [第一步：从数据需求到具体 “指标 + 维度”](event-variable.md#di-yi-bu-cong-shu-ju-xu-qiu-dao-ju-ti-zhi-biao-wei-du)
* [第二步：在 ”打点管理“ 中完成配置](event-variable.md#di-er-bu-zai-da-dian-guan-li-zhong-wan-cheng-pei-zhi)
* [第三步：代码部署](event-variable.md#di-san-bu-dai-ma-bu-shu)
* [第四步：数据校验](event-variable.md#di-si-bu-shu-ju-xiao-yan)

在 GrowingIO 的一个项目中，埋点事件级变量的个数上限为 100 个，暂时不支持扩展。如果您所创建的事件级变量已达上限，请您删除无用的变量后再尝试创建。

## 第一步：从数据需求到具体 “指标 + 维度”

从一个实际场景出发，我们需要确定在分析中需要用到哪些量化的值，然后用什么样的维度来分解这些值。例如，对于电商在分析用户下单情况时，用户的下单量、下单金额就是我们需要量化的“指标”，而每个订单所含具体商品、商品分类、优惠券信息等就是“维度”。那么对于下单这件事，我们就可以这样设计 ”指标 + 维度“：

* 指标：
  * 订单总量
  * 订单总金额
* 维度：
  * 商品 ID/名称
  * 商品 SKU
  * 优惠券名称
  * 收货地址

    ...

上文中讲到，"订单总量" 这个指标需要用 ”埋点事件“ 来实现；而商品 ID/名称、优惠券名称等维度，则可以通过 **"埋点事件级变量"**来实现。

## 第二步：在 ”埋点管理“ 中完成配置

当我们完成 ”指标+维度“ 的设计之后，请勿直接开始代码的部署，需要先到 GrowingIO 后台找到【数据管理】-【事件与变量】-【事件级变量】功能，在界面中完成对应的配置。

对每一个事件级变量，建议您在配置之前，先按下表列出配置细项，其中

* 变量名称 ==&gt; GrowingIO 后台维度名称；
* 变量描述 ==&gt; 可选，仅作备注使用；
* 变量标识符 ==&gt; 此变量在代码中的标识，可以为英文、下划线和数字，大小写敏感；
* 类型 ==&gt; 可以是字符串或数值（**注意：只有在您希望对此事件级变量进行运算时，才需选择数值类型。其他情况建议您优先选择字符串类型。**因为一旦选择数值类型，此变量便会作为一个运算项，而无法在图表中作为维度展示）；

![&#x57CB;&#x70B9;&#x4E8B;&#x4EF6;&#x53D8;&#x91CF;](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LHVXegeSvwqaEGUCEvI%2F-LHVdYIvsplehyn77GJg%2Fimage.png?alt=media&token=a41adaaa-4d0c-4225-b51a-a3052aed65f2)

将上面列好的配置，填入事件级变量配置界面，如下图：事件级变量配置

![](https://docs.growingio.com/.gitbook/assets/ping-mu-kuai-zhao-20180108-xia-wu-5.40.48.png)

点击确定，即完成了事件级变量的配置。

## 第三步：代码部署

在完成了配置后，即可在代码中完成以上设计的 “自定义事件和变量” 的部署。具体的说，就是调用 GrowingIO 提供的 API 接口，上传数据。

* [​JS 接口文档​](../../sdk-integration/web-js-sdk/#track)
* [​Android 接口文档​](../../sdk-integration/android-sdk/android-sdk.md#2-zi-ding-yi-shi-jian-he-bian-liang-api)
* [​iOS 接口文档​](../../sdk-integration/ios-sdk-1/ios-sdk.md#ios-sdk-api)
* ​[小程序接口文档​](../../sdk-integration/mina-sdk/#zi-ding-yi-shi-jian-he-bian-liang)

API 中给出了埋点事件和埋点事件变量的上传方式。

## 第四步：数据校验

在完成了【数据管理】-【事件与变量】的配置，以及代码实施后，我们接下来需要对数据是否成功上传进行校验。校验工作分为两步完成。

**数据校验第一步：本地开发环境校验**

GrowingIO 提供了 SDK debug 模式以及 debug 工具，来帮助您完成数据的校验。具体请参考 [Debugger最佳实践](../../sdk-integration/growingio-debugger/best-practice.md#cstm-shi-jian-yi-ji-guan-lian-de-shi-jian-ji-bian-liang-shi-jian)。

**数据校验第二步：GrowingIO 后台图表验证**

在 GrowingIO 分析后台，找到 “单图” - “新建事件分析”，然后在图表中选择您设计好的 “指标+维度”，查看是否有数据。当然，您需要首先确保打点事件或维度确实有被触发。

至此，您已经完成了 “打点事件” 的上传，如您在配置或添加代码中有任何疑问，请联系您的客户成功经理咨询，或在工单系统中反馈问题。谢谢。

