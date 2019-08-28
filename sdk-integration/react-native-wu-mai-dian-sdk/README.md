---
description: 自动采集用户行为数据、页面元素信息和设备信息等数据，和原生无埋点 SDK 的采集内容和功能一致。
---

# React Native 无埋点 SDK

GitHub Demo ： [https://github.com/growingio/ReactNativeDemo](https://github.com/growingio/ReactNativeDemo) 。

## 支持版本

* 兼容 react native 版本：0.46-0.56、0.59.9
* 兼容组件 react-navigation 版本：^2.7.4、^3.11.0
* 兼容组件 react-native-navigation 版本：^1.1.486

{% hint style="info" %}
如果没有使用react-navigation或者react-native-navigation作为导航,请仔细阅读 [Page设置API](./#3-page-she-zhi-api)。
{% endhint %}

## 集成

### 预处理 JS 文件

RN无埋点的实现原理是修改用户`React Native`，`React-navigation`， `React-Native-Navigation`的源码。所以需要预先处理`js`文件，[GitHub 开源 JS 脚本](https://github.com/growingio/GIORNHook)。

#### （1）使用命令行进入项目根目录，执行下面操作（二者任选其一即可）： 

```bash
npm install --save react-native-autotrack-growingio
```

```bash
npm install --save https://github.com/growingio/GIORNHook.git#0.0.6
```

#### （2） 配置 `package.json` 文件

考虑到了`hook.js`每次`npm install`之后都需要执行， 建议直接配在项目的`package.json`中，  
在原有文件中，添加如下代码，保存后执行**`npm install`**。

```bash
"scripts": {
	  "postinstall": "node node_modules/react-native-autotrack-growingio/hook.js -run"
}
```



## Android 集成

### 1. 添加 Android 无埋点 SDK 依赖

React  Native 无埋点 SDK 是在 Android 原生 SDK 上的扩展，参照 [Android 无埋点 SDK](../android-sdk/android-sdk.md#ji-cheng-wu-mai-dian-sdk)，集成步骤的 1~5.

{% hint style="danger" %}
**注意将 SDK 版本号替换成 RN 版本:`RN-autotrack-2.8.2 。`**
{% endhint %}

集成步骤中，只有版本号不同，适配 RN 与原生混合开发场景。

### 2. 重要配置项

#### 1.添加 GrowingIOPackage

由于 SDK 需要在`java`代码中进行初始化。

在项目的Application中，添加`GrowingIOPackage`：

```java
public class MainApplication extends Application implements ReactApplication {
    @Override
    protected List<ReactPackage> getPackages() {
      return Arrays.<ReactPackage>asList(
          new MainReactPackage(), 
          // 此处加入GrowingIOPackage
          new GrowingIOPackage()
      );
    }
}
```

#### 2.混淆文件配置

注意混淆文件比 Android 原生 SDK 多一条 RN 控件的混淆代码，请复制全部内容添加到您的 `proguard_rules.pro` 文件中，如下：

{% code-tabs %}
{% code-tabs-item title="proguard-rules.pro" %}
```text
# GIO RN 控件混淆代码，不添加则会造成点击事件采集失败
-keep class com.facebook.react.uimanager.JSTouchDispatcher{
    *;
}
-keep class com.growingio.** {
    *;
}
-dontwarn com.growingio.**
-keepnames class * extends android.view.View
-keepnames class * extends android.app.Fragment
-keepnames class * extends android.support.v4.app.Fragment
-keepnames class * extends androidx.fragment.app.Fragment
-keep class android.support.v4.view.ViewPager{
    *;
}
-keep class android.support.v4.view.ViewPager$**{
	*;
}
-keep class androidx.viewpager.widget.ViewPager{
    *;
}
-keep class androidx.viewpager.widget.ViewPager$**{
	*;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## iOS 集成

### **1. 选择SDK集成方式**

#### 注意:请保证Growing,GrowingCoreKit,GrowingAutoTrackKit,GrowingReactNativeKit版本号一致

一.使用 CocoaPods 管理依赖

1. 添加`pod 'GrowingReactNativeKit'`到Podfile中
2. 执行`pod update`,不要用`--no-repo-update`选项
3. **\(optional\)** GrowingIO推荐您添加**AdSupport.framework**依赖库,用于来源管理激活匹配,有利于您更好的分析的数据

* 添加项目依赖库的位置在项目设置target -&gt; 选项卡General -&gt; Linked Frameworks and Libraries

二.手动安装

1. 获取 [GrowingCoreKit](https://assets.growingio.com/sdk/ios/GrowingIO-iOS-CoreKit-2.8.3.zip)、[GrowingAutoTrackKit](https://assets.growingio.com/sdk/ios/GrowingIO-iOS-AutoTrackKit-2.8.3.zip)、[GrowingReactNativeKit](https://assets.growingio.com/sdk/ios/GrowingIO-iOS-ReactNativeKit-2.8.3.zip)、[GrowingHeader](https://assets.growingio.com/sdk/ios/GrowingIO-iOS-PublicHeader-2.8.3.zip)包
2. 解压上述 iOS SDK 压缩文件
3. 将 Growing.h 、module.modulemap 和 GrowingCoreKit.framework、GrowingAutoTrackKit.framework、GrowingReactNativeKit.framework

   添加到iOS工程

其余参照[ iOS 无埋点 SDK ](../ios-sdk-1/ios-sdk.md#wu-mai-dian-sdk-ji-cheng)集成，操作步骤一致。

### 2. 集成 React Native 打点 SDK

```bash
npm install --save https://github.com/growingio/react-native-growingio.git#0.0.7
```

```bash
react-native link react-native-growingio 
```

{% hint style="info" %}
如果`react-native link react-native-growingio`失败

（成功则忽略此步骤），即发现`Libraries`中没有`GrowingIORNPlugin.xcodeproj`，则可手动配置：

1. 打开 XCode 工程中, 右键点击 Libraries 文件夹 ➜ Add Files to &lt;...&gt; 
2. 去 `node_modules`  ➜ `react-native-growingio` ➜ iOS ➜ 选择 `GrowingIORNPlugin.xcodeproj` 
3. 在工程`Build Phases` ➜ `Link Binary With Libraries`中添加`libGrowingIORNPlugin.a`
{% endhint %}

{% hint style="danger" %}
如果您使用的是pod 的方式集成 ：

请添加以下内容到Podfile中，并在添加之后执行**`pod update`**：

`pod 'GrowingReactNativeTrackKit', :path => '../node_modules/react-native-growingio'`
{% endhint %}



### 3. 重要配置项

与原生混合开发的开发者可详细查看[ iOS 无埋点 重要配置](../ios-sdk-1/ios-sdk.md#zhong-yao-pei-zhi)文档，如果原生控件使用不多，只需关注如下配置即可：

* \*\*\*\*[**App Store 提交应用注意事项**](../ios-sdk-1/ios-sdk.md#zai-app-store-ti-jiao-ying-yong)\*\*\*\*



## 页面识别

由于RN应用的页面切换并不遵循原生的生命周期， 需要单独适配， 目前在 React Native 页面中我们只支持`react-navigation`， `react-native-navigation`作为导航器, 并且为了拓展性， 留下了手动的page接口， 开发者可自行适配（[直接更改 hook.js 脚本](https://github.com/growingio/GIORNHook)）。

### 1. **react-navigation**

如果使用react-navigation， 我们的hook脚本进行了自动适配, 默认的page名称为其key值。 用户可以自行设置， 如下代码：

```javascript
this.props.navigation.setParams({growingPagePath: 'xx'});
```

{% hint style="info" %}
采集原理参照 [react-navigation 的 screen tracking](https://reactnavigation.org/docs/en/screen-tracking.html)，目前仅兼容 [`Listening to State Changes`](https://reactnavigation.org/docs/en/screen-tracking.html#listening-to-state-changes)方式，如果用户使用 Redux ，请结合官网的 [`Screen tracking with Redux` ](https://reactnavigation.org/docs/en/screen-tracking.html#screen-tracking-with-redux)和 [Page 设置 API](./#3-page-she-zhi-api) 采集页面。
{% endhint %}

### 2.  **react-native-navigation**

* 当前支持`react-native-navigation`版本为 1.1.486， 1.1.406。 理论上兼容1.1版本。
* `react-native-navigation`其标题默认取`title`， 如果没有`title`则取`screenId`，作为唯一标记。

用户可以设置自定义`title`, 只需要设置`growingPagePath`字段， 该字段与`title`同级即可.

### **3. Page 设置 API**

如果SDK目前支持的路由方案不能满足您的需求， SDK留下拓展接口， 您需要在您认为页面发生切换时， 将最新的page名称告诉我们。 调用方法如下：

```javascript
import {
    NativeModules
  } from 'react-native';

// 在react native页面将要展示时调用
NativeModules.GrowingIO.onPagePrepare("pageName");
// 在react native页面已经展示时调用
NativeModules.GrowingIO.onPageShow("pageName");
```



## 自定义事件和变量API

| 方法名 | 参数类型 | 说明                                      |
| :--- | :--- | :--- |
| track | \(String eventId, Object eventLevelVariable\(optional\)\) | 自定义事件 |
| trackWithNumber | \(String eventId, Number number, Object eventLevelVariable\(optional\)\) | 自定义事件 |
| setEvar | \(Object conversionVariables\) | 设置转化变量 |
| setPeopleVariable | \(Object peopleVariables\) | 设置用户变量 |
| setUserId | \(String userId\) | 设置登录用户ID |
| clearUserId | 无参数 | 清除登录用户ID |
| setVisitor | \(Object visitor\) | 设置访问用户变量 |

{% hint style="warning" %}
埋点接口其实最终是通过 NativeModules.GrowingIO 调用的原生GrowingIO 无埋点的API，以上接口使用时，对应的参数限制条件对您很重要。请您注意遵守，并且[验证](./#yan-zheng-sdk-shi-fou-zheng-chang-gong-zuo)打点是否成功。
{% endhint %}

#### 参数限制条件

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;&#x540D;&#x79F0;</th>
      <th style="text-align:left">&#x9650;&#x5236;&#x6761;&#x4EF6;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">eventId</td>
      <td style="text-align:left">&#x975E;&#x7A7A;&#xFF0C;&#x957F;&#x5EA6;&#x9650;&#x5236;&#x5C0F;&#x4E8E;&#x7B49;&#x4E8E;50&#xFF1B;</td>
    </tr>
    <tr>
      <td style="text-align:left">number</td>
      <td style="text-align:left">&#x975E;&#x7A7A;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">eventLevelVariable</td>
      <td style="text-align:left">
        <p>&#x4E0D;&#x53EF;&#x4F7F;&#x7528;&#x5D4C;&#x5957;&#x7684;<code>JSONObject</code>&#x5BF9;&#x8C61;&#xFF0C;&#x5373;&#x4E3A;JSONObject&#x4E2D;&#x4E0D;&#x53EF;&#x4EE5;&#x653E;&#x5165;<code>JSONObject</code>&#x6216;&#x8005;<code>JSONArray</code>&#xFF1B;</p>
        <p>key &#x957F;&#x5EA6;&#x9650;&#x5236;&#x5C0F;&#x4E8E;&#x7B49;&#x4E8E;50&#xFF0C;value&#x957F;&#x5EA6;&#x9650;&#x5236;&#x5C0F;&#x7B49;&#x4E8E;1000&#x3002;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">conversionVariables</td>
      <td style="text-align:left">&#x540C;&#x4E0A;</td>
    </tr>
    <tr>
      <td style="text-align:left">peopleVariables</td>
      <td style="text-align:left">&#x540C;&#x4E0A;</td>
    </tr>
    <tr>
      <td style="text-align:left">visitor</td>
      <td style="text-align:left">&#x540C;&#x4E0A;</td>
    </tr>
    <tr>
      <td style="text-align:left">userId</td>
      <td style="text-align:left">
        <p>&#x957F;&#x5EA6;&#x9650;&#x5236;&#x5C0F;&#x4E8E;&#x7B49;&#x4E8E;1000&#xFF1B;</p>
        <p>&#x5982;&#x679C;&#x503C;&#x4E3A;&#x7A7A;&#x5219;&#x6E05;&#x7A7A;&#x4E86;&#x767B;&#x5F55;&#x7528;&#x6237;&#x53D8;&#x91CF;&#xFF0C;&#x4E0D;&#x5EFA;&#x8BAE;&#x8FD9;&#x4E48;&#x7528;&#xFF0C;</p>
        <p>&#x8BF7;&#x4F7F;&#x7528; clearUserId &#x6E05;&#x9664;&#x767B;&#x5F55;&#x7528;&#x6237;&#x53D8;&#x91CF;&#x3002;</p>
      </td>
    </tr>
  </tbody>
</table>#### 代码示例：

GrowingIOPackage 向 RN 提供了一个 NativeModule ， 所有打点接口都是由其实现，使用方法如下：

```javascript
//在使用 GrowingIO 埋点功能的文件中导入 NativeModules
import {
    NativeModules
  } from 'react-native';

//埋点方法调用示例如下：

//track 设置自定义事件
NativeModules.GrowingIO.track('testEventId', {'卖家Id': 'xxxxxx', '地点': '北京'});

//trackWithNumber 设置自定义事件
NativeModules.GrowingIO.trackWithNumber('addCart',97,{"book":"EnglishBook"});

//setPeopleVariable 设置用户变量
NativeModules.GrowingIO.setPeopleVariable({ "name": "Danny", "Age": 20 });

//setEvar 设置转化变量
NativeModules.GrowingIO.setEvar({ "registered": true, "fee": "200" });

//setUserId 设置登录用户名称
NativeModules.GrowingIO.setUserId("Gioer");

//clearUserId 清除登录用户名称
NativeModules.GrowingIO.clearUserId();

//setVisitor 设置访问用户变量
NativeModules.GrowingIO.setVisitor({ "age": 20, "gender": "male" });

```



## 高级选项设置

开发者可以为`UI`元素添加`growingParams`

####  代码示例:

```markup
<Text growingParams={{id:"test",content:"test",info:"test",ignore:"true"}}>高级选项设置</Text>
```

####  属性列表 

| 属性名称         | 参数限制                         | 描述 | 功能对应于原生接口       |
| :--- | :--- | :--- | :--- |
| ignore | 仅接受`"true"` | 忽略对应的元素，不采集点击事件和浏览事件 | [Android](../android-sdk/android-sdk.md#hu-lve-yuan-su) 文档解释，iOS 意义相同 |
| track | 仅接受`"true"` | 采集输入框内容，默认采集输入框内容变化次数，不采集内容 | [Android](../android-sdk/android-sdk.md#cai-ji-shu-ru-kuang-shu-ju) 文档解释，iOS 意义相同 |
| id |  | 设置界面元素ID | [Android](../android-sdk/android-sdk.md#she-zhi-jie-mian-yuan-su-id) 文档解释，iOS 意义相同 |
| content |  | 设置界面元素内容 | [Android](../android-sdk/android-sdk.md#she-zhi-jie-mian-yuan-su-nei-rong) 文档解释，iOS 意义相同 |
| info |  | 设置元素对象 | [Android](../android-sdk/android-sdk.md#she-zhi-yuan-su-dui-xiang) 文档解释，iOS 意义相同 |

#### 支持设置组件（Component）列表（包括但不限于）

| 支持的组件 |
| :--- |
| View |
| ScrollView |
| ListView |
| Picker |
| Text |
| TextInput |
| WebView |
| RefreshControl |
| ActivityIndicator |



## 验证 SDK 是否正常工作

#### 验证内容：

1. 验证 点击事件是否发送 （clck）
2. 验证 页面是否识别（page）
3. 验证[热图](../../data-analytics/heatmap/heatmap-app.md)和[圈选](../../data-definition/circle/app.md)功能
4. [原生验证内容](../android-sdk/android-mai-dian-sdk.md#yan-zheng-gong-ju)（Android）
5. [原生验证内容](../ios-sdk-1/ios-sdk.md#3-quan-xuan-he-re-tu-gong-neng-yan-zheng)（iOS）

#### 验证工具：

1. [Mobile Debugger](../growingio-debugger/)
2. [查看日志： ](../android-sdk/android-sdk.md#she-zhi-debug-mo-shi)Android 打开 TestMode  和 Debug Mode
3. 查看日志：iOS

## 常见问题

1.iOS 使用react native location功能时与GrowingIO有冲突么?

> 是的,但不属于growingio的原因,具体可以参考[issue](https://github.com/growingio/react-native-growingio/issues/4)中的解答,并且可以使用[rn-patch.zip](https://github.com/growingio/react-native-growingio/files/2166299/rn-patch.zip)来解决这个问题

2. 支持react native 版本?

> 0.46 ~ 0.56

3. chrome debug时是否可以校验sdk采集数据的准确性?

> 目前不支持,但不影响您chrome debug调试自己的项目, 建议您在release环境下校验

4. react native页面中的tabbar imp event不准确?click event是否受影响?

> \(1\) 原则上对于react native中的tabbar控件,我们只能保证使用原生代码UITabBarController,或者不是RCT为前缀的原生控件实现的tabbar,可以正确采集tabbar上的imp  
> \(2\) click event不受影响,可正确采集



