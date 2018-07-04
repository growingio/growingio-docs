# 自定义数据上传和配置

* [四种 “自定义数据”](custom-data-implement-guide.md#四种-自定义数据-上传方式)
* [“自定义数据” 上传步骤](custom-data-implement-guide.md#自定义数据-上传步骤)
  * [第一步：从数据需求到具体 “指标+维度”](custom-data-implement-guide.md#di-yi-bu-cong-shu-ju-xu-qiu-dao-ju-ti-zhi-biao-wei-du)
  * [第二步：在 ”打点管理“ 中完成配置](custom-data-implement-guide.md#第二步：在-打点管理-中完成配置)
  * [第三步：代码部署](custom-data-implement-guide.md#第三步：代码部署)
  * [第四步：数据校验](custom-data-implement-guide.md#第四步：数据校验)

**重要：**如果您正在使用的1.x版本的SDK，请参考[此文档](https://docs.growingio.com/SDK/zidingyi_config_1.x/%E8%87%AA%E5%AE%9A%E4%B9%89%E6%95%B0%E6%8D%AE%E7%9A%84%E4%B8%8A%E4%BC%A0%E4%B8%8E%E9%85%8D%E7%BD%AE1.x.html)进行自定义数据的上传和配置。

### 四种 “自定义数据” {#四种-自定义数据-上传方式}

如您所知，您的APP或网页在集成了 GrowingIO 的 SDK 之后，它将会自动地为您采集一系列用户行为数据，并在 GrowingIO 分析后台供您制成数据分析报表。除上述的用户行为数据（或称为无埋点数据）之外，GrowingIO 还提供了多种 API 接口，供您上传一些自定义的数据指标及维度，他们包括：

* 自定义事件 + 事件级变量
* [页面级变量](https://docs.growingio.com/implementation/event-variable/custom-event/custom-variables-introduction/page-level-variable.html)
* [转化变量](https://docs.growingio.com/implementation/event-variable/custom-event/custom-variables-introduction/conversion-variable.html)
* [用户变量](https://docs.growingio.com/implementation/event-variable/custom-event/custom-variables-introduction/user-variable.html)

上述的 “自定义事件” 在GrowingIO分析后台体现为一个 “指标”，而 ”事件级变量“、”页面级变量“、”转化变量“ 和 ”用户变量“ 均为 ”维度“。

如您需要，请参考本页内容，完成此类自定义数据的上传。

请注意，自定义事件的个数上限为500个，事件级变量的个数上限为100个（并非每个事件可附带100个变量，而是整个项目中只可以有100个事件级变量），页面级变量的个数上限为50个，转化变量的个数上限为10个，用户变量的个数上限为50个。

### “自定义数据” 上传步骤 {#自定义数据-上传步骤}

#### 总体流程 {#总体流程}

1. 从数据需求出发，梳理指标、维度
2. 在【管理】-【自定义事件与变量】中完成配置
3. 在代码中完成 API 调用
4. 数据校验

以下针对上述 4 个步骤进行详细说明。

#### 第一步：从数据需求到具体 “指标+维度”

在GrowingIO上着手进行任何分析之前，首先要确定的一个问题是：如何设计“指标+维度”的体系？对于无埋点数据，我们通过圈选确定“指标”，而“维度”则由 GrowingIO 提供了数个[预定义的维度](https://docs.growingio.com/growingio-shu-ju-mo-xing/yu-ding-yi-wei-du.html)。对于自定义数据，我们可以相对更自由地选择“指标+维度”的体系。

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

那么现在很明显了，订单总量与订单总金额这两个指标，需要用 ”自定义事件“ 来实现；商品ID/名称、优惠券名称等维度，则用 ”事件级变量“ 来实现。对于如何选用哪种自定义数据上传方式，请您进入本文第一节的几个链接了解他们的使用场景。

在设计“指标+维度”体系时可能需要注意：

* Android、iOS 以及 Web 端不需要区分：对于这三端，可以只定义一个指标，而不需要分别对三端定义三个指标。举例来说，“订单总量”这个指标可以包含三端的总量，在 GrowingIO 平台分析时用预定义维度“网站/手机应用”或者“操作系统”就可以拆分三端分别的量。考虑到自定义事件和事件级变量均有最大个数上限，不建议拆分三端而占用事件数。
* “自定义事件”无法用于创建复合指标，也无法被“页面级变量”创建的维度分解。
* “转化变量”创建的维度，只能够分解用“自定义事件”创建的指标，不能够分解无埋点指标（圈选得到的指标）。

#### 第二步：在 ”打点管理“ 中完成配置 {#第二步：在-打点管理-中完成配置}

当我们完成 ”指标+维度“ 的设计之后，请勿直接开始代码的部署，需要先到 GrowingIO 后台找到【管理】-【自定义事件与变量】功能，在其中完成对应的配置。

**事件级变量配置：**

对每一个事件级变量，建议您在配置之前，先按下表列出配置细项，其中

* 变量名称 ==&gt; GrowingIO 后台维度名称
* 变量描述 ==&gt; 可选，仅作备注使用
* 变量标识符 ==&gt; 此变量在代码中的标识，可以为英文、下划线和数字，大小写敏感
* 类型 ==&gt; 可以是字符串或数值（注意：只有在您希望对此事件级变量进行运算时，才需选择数值类型。其他情况建议您优先选择字符串类型。因为一旦选择数值类型，此变量便会作为一个运算项，而无法再图表中展示）

![](https://docs.growingio.com/.gitbook/assets/ping-mu-kuai-zhao-20180108-xia-wu-5.41.08%20%281%29.png)

将上面列好的配置，填入事件级变量配置界面，如下图：

![](https://docs.growingio.com/.gitbook/assets/ping-mu-kuai-zhao-20180108-xia-wu-5.40.48.png)

点击确定，即完成了事件级变量的配置。

**自定义事件配置：**

对于每一个自定义事件，建议您在配置之前，先按下表列出配置细项，其中

* 名称 ==&gt; GrowingIO 后台指标名称
* 描述 ==&gt; 可选，仅作备注使用
* 标识符 ==&gt; 此事件在代码中的标识，可以为英文、下划线和数字，大小写敏感
* 事件级变量 ==&gt; 此事件下属的事件级变量
* 类型 ==&gt; 计数器（递增1）或数值（递增任意值）。当您的事件是计“次数”时，请选用“计数器”类型；当您的事件是计“金额”、“数量/个数”等任意累加值时，请选用“数值”类型。 例如“订单金额”这个自定义事件，就需要选用“数值”类型。

![](https://docs.growingio.com/.gitbook/assets/3%20%286%29.png)

将上面列好的配置，填入自定义事件配置界面，如下图：

![](https://docs.growingio.com/.gitbook/assets/4%20%281%29.png)

点击保存配置，即完成了自定义事件的配置。

**页面级变量配置：**

对于每一个页面级变量，建议您在配置之前，先按下表列出配置细项，其中

* 变量名称 ==&gt; GrowingIO 后台维度名称
* 变量描述 ==&gt; 可选，仅作备注使用
* 变量标识符 ==&gt; 此变量在代码中的标识，可以为英文、下划线和数字，大小写敏感

![](https://docs.growingio.com/.gitbook/assets/5%20%281%29.png)

将上面列好的配置，填入页面级变量配置界面，如下图：

![](https://docs.growingio.com/.gitbook/assets/6%20%281%29.png)

点击确定，即完成了页面级变量的配置。

**转化变量配置：**

对于每一个转化变量，建议您在配置之前，先按下表列出配置细项，其中

* 变量名称 ==&gt; GrowingIO 后台维度名称
* 变量描述 ==&gt; 可选，仅作备注使用
* 变量标识符 ==&gt; 此变量在代码中的标识，可以为英文、下划线和数字，大小写敏感
* 归因方式 ==&gt; 转化归因的算法，可为最初、最近或线性（具体解释请参考[转化变量](https://docs.growingio.com/implementation/event-variable/custom-event/custom-variables-introduction/conversion-variable.html#gui-yin-fang-shi)介绍）；归因方式选定后，不建议修改，因为修改归因方式会导致修改前的所有数据失效
* 失效 ==&gt; 转化变量的失效时间（具体解释请参考[转化变量](https://docs.growingio.com/implementation/event-variable/custom-event/custom-variables-introduction/conversion-variable.html#shi-xiao-shi-jian)介绍）

![](https://docs.growingio.com/.gitbook/assets/7%20%281%29.png)

将上面列好的配置，填入转化变量配置界面，如下图：

![](https://docs.growingio.com/.gitbook/assets/8.png)

点击确定，即完成了转化变量的配置。

**用户变量配置：**

对于每一个用户变量，建议您在配置之前，先按下表列出配置细项，其中

* 变量名称 ==&gt; GrowingIO 后台维度名称
* 变量描述 ==&gt; 可选，仅作备注使用
* 变量标识符 ==&gt; 此变量在代码中的标识，可以为英文、下划线和数字，大小写敏感
* 归因方式 ==&gt; 用户变量归因的算法（具体解释请参考[用户变量](https://docs.growingio.com/implementation/event-variable/custom-event/custom-variables-introduction/user-variable.html#gui-yin-mo-xing)介绍）

![](https://docs.growingio.com/.gitbook/assets/9%20%281%29.png)

将上面列好的配置，填入用户变量配置界面，如下图：

![](https://docs.growingio.com/.gitbook/assets/10.png)

点击确定，即完成了用户变量的配置。

**注意： 除登录用户ID外其他的用户变量需要按以上截图形式配置，登录用户ID已为您配置好，您只需按下图开启即可。此处一定需要开启，否则用户变量即便上传了也是无效的。**

![](https://docs.growingio.com/.gitbook/assets/11%20%281%29.png)

#### 第三步：代码部署 {#第三步：代码部署}

在完成了配置后，即可在代码中完成以上设计的 “自定义事件和变量” 的部署。具体的说，就是调用 GrowingIO 提供的API接口，上传数据。

* [JS 接口文档](https://docs.growingio.com/sdk-20/sdk-20-api-wen-dang/js-sdk-api-wen-dang.html)
* [Android 接口文档](https://docs.growingio.com/sdk-20/sdk-20-api-wen-dang/android-sdk-api-wen-dang.html)
* [iOS 接口文档](https://docs.growingio.com/sdk-20/sdk-20-api-wen-dang/ios-sdk-api-wen-dang.html)

#### 第四步：数据校验 {#第四步：数据校验}

在完成了【管理】-【自定义事件与变量】的配置，以及代码实施后，我们当然需要对数据是否成功上传进行校验。校验工作分为两步完成。

**数据校验第一步：本地开发环境校验**

GrowingIO 提供了 SDK debug 模式以及 [debug 工具](growingio-debugger.md)，来帮助您完成数据的校验。

**log 中自定义数据的关键字**

* 在 cstm 条目中，可以看到上传的 “自定义事件+事件级变量” 数据
* 在 pvar 条目中，可以看到上传的 “页面级变量” 数据
* 在 evar 条目中，可以看到上传的 “转化变量” 数据
* 在 ppl 条目中，可以找到 “用户变量” 对应的数据

**1. Web 端**

对 Web 端的开发者，GrowingIO 提供了 Chrome 浏览器插件形式的 debug 工具，请在[这里](https://docs.growingio.com/sdk-integration/web-debugger/web-debugger-install.html)下载安装。

debug 工具的工作界面如下图：

![](https://docs.growingio.com/assets/WebDebuggerInstall5.png)

**2. 移动端**

对移动端的开发者，GrowingIO 的 SDK 提供了 debug 模式，在 SDK 初始化代码中可以找到。如下图：

Android：

![](https://docs.growingio.com/.gitbook/assets/12%20%282%29.jpeg)

**注：如果您 Android 的包添加了截图中代码仍无法打印出 GrowingIO 相关 log 的话，请在**`GrowingIO.startWithConfiguration()`**方法后，添加:**`GConfig.DEBUG = true;`

iOS：

![](https://docs.growingio.com/.gitbook/assets/13.jpeg)

开启 debug 模式后，您需要在app上触发一下打点事件，在打出的log里搜索上述关键字就能找到对应自定义事件&变量上传的数据。

**数据校验第二步：GrowingIO 后台图表验证**

在 GrowingIO 分析后台，找到 “分析” - “新建事件分析”，然后在图表中选择您设计好的 “指标+维度”，查看是否有数据。当然，您需要首先确保您的自定义事件或变量确实有被触发。

至此，您已经完成了 “自定义数据” 的上传，如您在配置或添加代码中有任何疑问，请联系您的客户成功经理咨询，或在工单系统中反馈问题。谢谢。

#### 注意：自定义事件及变量的分析应用范围 {#注意：自定义事件及变量的分析应用范围}

自定义事件与变量能够帮助我们收集到更多行为及业务数据，在 GrowingIO 的可视化平台中，它们也拥有不同的应用范围。

* **“自定义事件”创建的指标**：与无埋点指标应用无太大区别，可用于事件分析、漏斗分析、留存分析、用户分群等分析工具。但无法用于创建复合指标。
* **“事件级变量”创建的维度**：只可以分解其附属的自定义事件的指标。
* **“页面级变量”创建的维度**：可用事件分析、漏斗分析、留存分析和用户分群。只可以分解此变量所在页面上的无埋点指标，不可以分解任何其它指标。
* **“转化变量”创建的维度**：可用于事件分析。只可以分解用自定义事件创建的指标，不可以分解无埋点指标。并且由于转化归因按自然天计算归因，因此时间粒度只可以选择天或者大于天的粒度。
* **“用户变量”创建的维度**：可用于事件分析、漏斗分析、留存分析、用户分群等分析工具。

