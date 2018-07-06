# 事件和变量（打点）

**重要：**如果您正在使用的 1.x 版本的 SDK ，请参考[此文档](https://docs.growingio.com/SDK/zidingyi_config_1.x/%E8%87%AA%E5%AE%9A%E4%B9%89%E6%95%B0%E6%8D%AE%E7%9A%84%E4%B8%8A%E4%BC%A0%E4%B8%8E%E9%85%8D%E7%BD%AE1.x.html)进行自定义数据的上传和配置。

如果您想要使用各种变量，建议升级至最新版本的 SDK ，请注意：您需要联系您的 GrowingIO 对接人，我们需要帮您开启后台新版本所对应功能。如果您直接集成新版本，而后台对应功能未开启的话，可能会造成数据丢失的问题。

APP 或网页在集成了 GrowingIO 的 SDK 之后，它将会自动地为您采集一系列用户行为数据，并在 GrowingIO 分析后台供您制成数据分析报表。除上述的用户行为数据（或称为无埋点数据）之外，GrowingIO 还提供了多种 API 接口，供您上传一些自定义的数据指标及维度，他们包括：

* 自定义事件 + 事件级变量
* [页面级变量](https://docs.growingio.com/implementation/event-variable/custom-event/custom-variables-introduction/page-level-variable.html)
* [转化变量](https://docs.growingio.com/implementation/event-variable/custom-event/custom-variables-introduction/conversion-variable.html)
* [用户变量](https://docs.growingio.com/implementation/event-variable/custom-event/custom-variables-introduction/user-variable.html)

上述的 “自定义事件” 在 GrowingIO 分析后台体现为一个 “指标”，而 ”事件级变量“、”页面级变量“、”转化变量“ 和 ”用户变量“ 均为 ”维度“。

数量上限：

**SDK 最新版本 2.x （2018年）**

|  | 付费前 | 付费后 |
| --- | --- | --- | --- | --- | --- |
| 自定义事件 | 20 | 500 |
| 事件级变量 | 10 | 100 |
| 页面级变量 | 5 | 60 |
| 转化变量 | 3 | 10 |
| 用户变量 | 10 | 50 |

**SDK 1.x 旧版本**

|  | 付费前 | 付费后 |
| --- | --- | --- |
| 自定义事件 | 20 | 500 |
| 事件级变量 | 10 | 100 |

### 

