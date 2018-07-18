# 事件和指标

* ​[事件和指标的关系](https://growingio.gitbook.io/docs/~/drafts/-LGyNfnU9qfd7AXzFkhu/primary/data-model/yu-ding-yi-shu-ju#shi-jian-he-zhi-biao-de-guan-xi)​
  * ​[什么是 “事件”](https://growingio.gitbook.io/docs/~/edit/drafts/-LGyNfnU9qfd7AXzFkhu/data-model/yu-ding-yi-shu-ju#shen-me-shi-shi-jian)​
  * ​[“指标” 是对事件的度量](https://growingio.gitbook.io/docs/~/edit/drafts/-LGyNfnU9qfd7AXzFkhu/data-model/yu-ding-yi-shu-ju#zhi-biao-shi-dui-shi-jian-de-du-liang)​

## 事件和指标的关系 {#shi-jian-he-zhi-biao-de-guan-xi}

### **什么是 “事件”** {#shen-me-shi-shi-jian}

事件，是指用户在产品中做了什么事情。转义成描述性语言就是 “操作+对象”，直白语言来说就是做了什么事情。我们当前可以提供的事件类型包括：

* 浏览 页面
* 点击 元素
* 浏览 元素
* 修改 文本框
* 自定义打点事件 \(非常灵活\)

### **“指标” 是对事件的度量** {#zhi-biao-shi-dui-shi-jian-de-du-liang}

指标就是 “事件 + 度量方式”，比如：

**事件：**加入购物车

**指标**：加入购物车次数、加入购物车金额、人均购物车金额，等等

一个事件有多种不同的度量方式，可以衍生出很多指标，包含了**事件触发次数、事件在某个变量的统计（求和、求平均等其他运算）、事件触发人数、人均事件触发次数**等等。

在 GrowingIO 数据模型中，提供了 21 个预定义指标，支持丰富的分析场景，同时也可以通过[圈选](circle-metrics/)或[打点](manual-metrics.md)创建自定义事件。

