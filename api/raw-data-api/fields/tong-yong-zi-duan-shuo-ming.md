# 通用字段说明

### 时间字段

所有原始数据导出接口中的时间字段，一般情况会包含下面两类：

#### 事件时间：1.0接口中字段名为eventTime，2.0接口中字段名为time

取自客户端的系统时间，time字段中出现过去或者未来的时间，很大的可能是用户的系统时间是错的。  
对于移动端来说，如果App异常退出，或者突然关闭网络，会导致数据晚发，从而造成time字段早于当前时间的情况。

#### 发送时间：1.0和2.0接口中皆为sendTime

取自GIO接收到数据的时间，GIO所有小时、天数据全部使用此字段进行统计

### 用户字段

所有原始数据导出接口中会包含三类用户标示，用于您将GIO数据与您自有数据进行关联：

#### 访问用户ID

参考：[https://docs.growingio.com/docs/data-model/user-model/visitor](https://docs.growingio.com/docs/data-model/user-model/visitor)

#### 登录用户ID

参考：[https://docs.growingio.com/docs/data-model/user-model/loginuser](https://docs.growingio.com/docs/data-model/user-model/loginuser)

#### 其他身份标示

GrowingIO在导出的访问事件中包含了移动端用于标示用户身份常用的三个字段：IDFA、AndroidID、IMEI

参考：[https://docs.growingio.com/docs/api/raw-data-api/fields/autotrack-event\#vst-fang-wen-shi-jian](https://docs.growingio.com/docs/api/raw-data-api/fields/autotrack-event#vst-fang-wen-shi-jian)



