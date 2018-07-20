# 名词解释

* [事件和指标](terminology.md#shi-jian-he-zhi-biao)
  * [事件和指标的关系](terminology.md#shi-jian-he-zhi-biao-de-guan-xi)
  * [事件和指标在 GrowingIO 中的使用](terminology.md#shi-jian-he-zhi-biao-zai-growingio-zhong-de-shi-yong)
* [维度和变量](terminology.md#wei-du-he-bian-liang)
  * [维度和变量的关系](terminology.md#wei-du-he-bian-liang-de-guan-xi)
  * [维度和变量在 GrowingIO 中的使用](terminology.md#wei-du-he-bian-liang-zai-growingio-zhong-de-shi-yong)

GrowingIO 默认提供了一些预定义的[指标](events-metrics/predefined-metrics.md)和[维度](dimensions/predefined-dimensions.md)，帮助你更好地进行数据分析。

## 事件和指标

### 事件和指标的关系

**事件，是指用户在产品中做了什么事情。**转义成描述性语言就是 “操作+对象”，直白语言来说就是做了什么事情。我们当前可以提供的事件类型包括：

* 浏览 页面
* 点击 元素
* 浏览 元素
* 修改 文本框
* 自定义打点事件 \(非常灵活\)

**“指标” 是对事件的度量，**就是 “事件+ 度量方式”，比如：

**事件：**加入购物车

**指标**：加入购物车次数、加入购物车金额、人均购物车金额，等等

一个事件有多种不同的度量方式，可以衍生出很多指标，包含了**事件触发次数、事件在某个变量的统计（求和、求平均等其他运算）、事件触发人数、人均事件触发次数**等等。  


### 事件和指标在 GrowingIO 中的使用

您在使用 GrowingIO 后台分析平台时，会在各个不同的工具中使用到 "事件" 和 "指标"。常见的分析工具如：事件分析、漏斗分析、留存分析和用户分群，发起新建时都要选择对应的事件或指标作为分析对象。

GrowingIO 提供了通过 SDK 默认采集到的很多[预定义指标](events-metrics/predefined-metrics.md)，也提供了自定义事件和指标的能力，具体可以参考[事件和指标](terminology.md#shi-jian-he-zhi-biao)章节。

#### 事件分析

![&#x4E8B;&#x4EF6;&#x5206;&#x6790;-&#x9009;&#x62E9;&#x4E8B;&#x4EF6;&#x6216;&#x6307;&#x6807;&#x4F5C;&#x4E3A;&#x5206;&#x6790;&#x5BF9;&#x8C61;](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LHWKwpBRNve3A-7qf1a%2F-LHWLm47JpOUzS2CaIAs%2Fimage.png?alt=media&token=79fe39fc-b90c-4061-b421-31c7e13a448d)

#### 漏斗分析

![&#x6F0F;&#x6597;&#x5206;&#x6790;-&#x9009;&#x62E9;&#x4E8B;&#x4EF6;&#x4F5C;&#x4E3A;&#x6F0F;&#x6597;&#x6B65;&#x9AA4;](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LHWKwpBRNve3A-7qf1a%2F-LHWM7aWAVL3YSUyWN9x%2Fimage.png?alt=media&token=94dcec8d-9c72-4362-ab5c-8a81e55c412e)

#### 留存分析

![&#x7559;&#x5B58;&#x5206;&#x6790;-&#x9009;&#x62E9;&#x4E8B;&#x4EF6;&#x4F5C;&#x4E3A;&#x8D77;&#x59CB;&#x548C;&#x7559;&#x5B58;&#x884C;&#x4E3A;](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LHWKwpBRNve3A-7qf1a%2F-LHWMYEF8rAYWcol_Vs2%2Fimage.png?alt=media&token=572f218c-a309-46a3-a330-15df5b58dade)

#### 用户分群

![&#x7528;&#x6237;&#x5206;&#x7FA4;-&#x9009;&#x62E9;&#x4E8B;&#x4EF6;&#x4F5C;&#x4E3A;&#x5206;&#x7FA4;&#x6761;&#x4EF6;](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LHWKwpBRNve3A-7qf1a%2F-LHWMlfcf343LUlAct70%2Fimage.png?alt=media&token=42f6cb46-4518-4a7b-9230-35af8683a0a6)

## 维度和变量

### 维度和变量的关系

"变量"，是在自定义数据中引入的概念，具体可以参考[维度](dimensions/predefined-dimensions.md#zi-ding-yi-da-dian-wei-du-bian-liang)章节。在进行数据定义时，事件和变量是构成事件完整描述的2个概念。

维度和指标是数据分析中两个基础概览。指标是一个具体的度量，维度对指标进行细分以便于进行对比发现指标的异常。

举例来说，当我们看到昨天的 "跳出率" 指标是 45% 时，我们并不能直接得到任何的结论或者观点判断这个数据是高是低。这个时候，我们可以用 "落地页" 维度对  "跳出率" 指标进行细分，发现手机端 H5 落地页跳出率达到了 80% ，而 PC 网页跳出率只有 20% ，就会明显发现异常。

在进行自定义数据配置和上传时，字符串类型的变量都会被作为 "维度"来使用，而 "数值型"变量会被 GrowingIO 处理成1个指标。举例来说，对于电商在分析用户下单情况时，用户的下单量，下单金额就是我们需要量化的 “指标”，而每个订单所含具体商品、商品分类、优惠券信息等就是“维度”。 那么对于下单这件事，我们就可以这样设计“指标+维度”：  


指标：

* 订单总量
* 订单总金额

维度：

* 商品ID/名称
* 商品SKU
* 优惠券名称
* 收货地址
* ...

将 "订单金额" 设置成数值型变量，GrowingIO 会处理成2个指标： "订单金额求和" 和"订单金额平均" 两个指标。 "商品 ID/名称"，"商品SKU" 等设置成字符串型变量，GrowingIO 会将其作为维度。

### 维度和变量在 GrowingIO 中的使用

GrowingIO 通过 SDK 默认采集很多[预定义维度](dimensions/predefined-dimensions.md#yu-ding-yi-wei-du)用以分析；同事也支持自定义变量配置得到自定以维度，请参考[维度文档](dimensions/predefined-dimensions.md#zi-ding-yi-da-dian-wei-du-bian-liang)。处理成指标的变量，使用方法与上文（[事件和指标](terminology.md#shi-jian-he-zhi-biao)）中提到的使用一致。处理成维度的变量，GrowingIO 中作为 "维度" 在不同的分析工具中使用。预定义和自定义维度的使用，举例说明： 

#### 事件分析

![&#x4E8B;&#x4EF6;&#x5206;&#x6790;-&#x9009;&#x62E9;&#x7EF4;&#x5EA6;&#x62C6;&#x5206;&#x6307;&#x6807;](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LHWdQehi-dq3IywOtVW%2F-LHWfW8KxFrh0wX_UKeN%2Fimage.png?alt=media&token=7e68f7ad-5b16-4d05-a0c5-bd09e4d280be)

#### 漏斗分析

![&#x6F0F;&#x6597;&#x5206;&#x6790;-&#x901A;&#x8FC7;&#x7EF4;&#x5EA6;&#x5BF9;&#x6F0F;&#x6597;&#x6B65;&#x9AA4;&#x8FDB;&#x884C;&#x7EC6;&#x5206;&#x6216;&#x8FC7;&#x6EE4;](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LHWdQehi-dq3IywOtVW%2F-LHWgDnrvDSVb1JMTh90%2Fimage.png?alt=media&token=728eb5c4-5548-4430-8ee3-f71e2db5604a)

#### 留存分析

![&#x7559;&#x5B58;&#x5206;&#x6790;-&#x901A;&#x8FC7;&#x7EF4;&#x5EA6;&#x5BF9;&#x8D77;&#x59CB;&#x6216;&#x7559;&#x5B58;&#x884C;&#x4E3A;&#x8FDB;&#x884C;&#x7EC6;&#x5206;&#x6216;&#x8FC7;&#x6EE4;](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LHWdQehi-dq3IywOtVW%2F-LHWgUGN2OSkxLE4PL66%2Fimage.png?alt=media&token=d63e2cc0-3f7b-4025-9aa5-c254af5238e6)

#### 用户分群

![&#x7528;&#x6237;&#x5206;&#x7FA4;-&#x9009;&#x62E9;&#x7EF4;&#x5EA6;&#x4F5C;&#x4E3A;&#x5206;&#x7FA4;&#x6761;&#x4EF6;](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LHWdQehi-dq3IywOtVW%2F-LHWge_wOn0MNsTdAdyK%2Fimage.png?alt=media&token=7d55084b-bdae-4908-bc90-48e8c65c70f3)

![&#x7528;&#x6237;&#x5206;&#x7FA4;-&#x901A;&#x8FC7;&#x7EF4;&#x5EA6;&#x5BF9;&#x67D0;&#x4E2A;&#x6B65;&#x9AA4;&#x4E8B;&#x4EF6;&#x8FDB;&#x884C;&#x8FC7;&#x6EE4;](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LHWdQehi-dq3IywOtVW%2F-LHWgj49aclorV16l3kH%2Fimage.png?alt=media&token=4989ca17-ad4a-4339-830b-9226faa0c996)



  




