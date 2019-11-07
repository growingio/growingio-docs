---
description: GrowingIO 埋点 SDK 仅自动采集设备信息和您埋点内容数据
---

# Flutter 埋点 SDK

## Flutter插件集成

### 1. Flutter插件获取安装

        根据[dart pub](https://pub.dartlang.org/packages/flutter_growingio_track#-installing-tab-)文档获取安装

### 2. Android集成\(Native部分\)

此Flutter插件运行在Android手机上时依赖于GrowingIO Android SDK \(可以是无埋 点SDK也可以是埋点SDK\)2.6.0及以上, 原生部分请参考：

    [Android官方文档集成SDK部分\(无埋点\)](https://docs.growingio.com/docs/sdk-integration/android-sdk/#ji-cheng-sdk) 或[Android官方文档集成SDK部分\(埋点\)](https://docs.growingio.com/docs/sdk-integration/android-sdk/android-maidian-sdk)

  接入集成，可以参考仓库中的[example](https://github.com/growingio/flutter-growingio-track/tree/develop/example)项目.

### 3. iOS集成\(Native部分\)

#### 添加iOS埋点SDK依赖

Flutter埋点插件是在 iOS 原生 SDK 上的扩展，请参照 [iOS 埋点 SDK 集成步骤 1~3 ]()，操作完全一致

### 4. 常见问题

#### **4.1  IOS: App Store 提交应用**

如果您添加了库AdSupport.framework, GrowingIO则会启用IDFA，所以在向App Store 提交应用时，需要：

* 对于问题Does this app use the Advertising Identifier \(IDFA\)，选择YES。
* 对于选项Attribute this app installation to a previously served advertisement，打勾。
* 对于选项Attribute an action taken within this app to a previously served advertisement，打勾。

#### **4.2   IOS: 为什么GrowingIO使用IDFA**

GrowingIO 使用IDFA 来做来源管理激活设备的精确匹配，让你更好的衡量广告效果。如果你不希望跟踪这个信息，可以选择不引入AdSupport.framework

#### **4.3  Android: 初始化Android SDK时, GrowingIO类可能会报红色**

这个应该是Flutter项目结构的问题，并不影响运行，可以放心编译. 不过需要手动import-\_-\|

#### **4.4  为什么不在flutter中单独初始化**

* 因为GrowingIO需要获取Android的Activity生命周期，为了数据的准确性，需要在Activity出现前就初始化完成
* 开发者相信很多用户都会使用flutter + native形式的进行开发，为了同时服务flutter于native

## API说明

### 1. track

#### 说明：发送自定义事件, 对应于cstm事件

| **参数** | **是否必填** | **说明** |
| :--- | :--- | :--- |
| eventId | 是 | 事件Id |
| num | 否 | 数值, double型 |
| variable | 否 | 变量, Map型 |

调用示例:

```dart
import 'package:growingioflutter/growingio_track.dart';
```

```dart
GrowingIO.track('eventId');GrowingIO.track('testEventId', num: 23.0, variable: {'testKey': 'testValue', 'testNumKey': 233});GrowingIO.track('eventId', num: 23.0);GrowingIO.track('eventId', variable: {'testkey': 'testValue', 'testNumKey': 2333});
```

### 2. setEvar

#### 说明：发送转化变量, 对应于evar事件

   函数原型为: setEvar\(Map&lt;String, dynamic&gt; variable\), 调用示例:

```dart
import 'package:growingioflutter/growingio_track.dart';
```

```dart
GrowingIO.setEvar({  'testKey': 'testValue', 'testNumKey': 2333.0});
```

### 3. setPeopleVariable

#### 说明：发送用户变量, 对应于ppl事件

  函数原型为: setPeopleVariable\(Map&lt;String, dynamic&gt; variable\)

  调用示例:

```dart
import 'package:growingioflutter/growingio_track.dart';
```

```dart
GrowingIO.setPeopleVariable({  'testKey': 'testValue', 'testNumKey': 2333.0});
```

### 4. setUserId

#### 说明：设置登录用户Id, 对应于cs1字段

| **参数** | **类型** | **描述** |
| :--- | :--- | :--- |
| userId | String | 登录用户Id |

函数原型: setUserId\(String userId\)

调用示例:

```dart
import 'package:growingioflutter/growingio_track.dart';
```

```dart
GrowingIO.setUserId("testUserId");
```

### 5. clearUserId

#### 说明：清除登录用户Id

函数原型: clearUserId\(\)

调用示例:

```dart
import 'package:growingioflutter/growingio_track.dart';
```

```dart
GrowingIO.clearUserId();
```

### 6. setVisitor

#### 说明：设置访问用户变量, 对应于vstr事件

函数原型: setVisitor\(Map&lt;String, dynamic&gt; variable\)

调用示例:

```dart
import 'package:growingioflutter/growingio_track.dart';
```

```dart
GrowingIO.setVisitor({	  "visitorKey": 'key', "visitorValue": 34});
```



