---
description: 自动采集用户行为数据、页面元素信息和设备信息等数据，和原生无埋点 SDK 的采集内容和功能一致。
---

# React Native 无埋点 SDK

GitHub Demo ： [https://github.com/growingio/ReactNativeDemo](https://github.com/growingio/ReactNativeDemo) 。

## 支持版本

* 兼容react native版本:0.46-0.56
* 兼容组件react-navigation 版本在2.6.2及以上
* 兼容组件react-native-navigation 版本在1.1.486及以上

## Android 

### 1. 预处理 JS 文件

RN无埋点的实现原理是修改用户`React Native`，`React-navigation`， `React-Native-Navigation`的源码。所以需要预先处理`js`文件，[GitHub 开源 JS 脚本](https://github.com/growingio/GIORNHook)。

#### （1）使用命令行进入项目根目录，执行下面操作： 

```bash
#二者选择其一即可
npm install --save react-native-autotrack-growingio
npm install --save https://github.com/growingio/GIORNHook.git#0.0.5
```

#### （2） 配置 `package.json` 文件

考虑到了`hook.js`每次`npm install`之后都需要执行， 建议直接配在项目的`package.json`中，  
在原有文件中，添加如下代码，保存后执行**`npm install`**。

```bash
"scripts": {
	  "postinstall": "node node_modules/react-native-autotrack-growingio/hook.js -run"
}
```

### 3. 添加 Android 原生依赖

React  Native 无埋点 SDK 是在 Android 原生 SDK 上的扩展，参照 [Android 无埋点 SDK](../android-sdk/#ji-cheng-wu-mai-dian-sdk)，集成步骤的 1~5，注意将 SDK 版本号替换成 RN 版本`RN-autotrack-2.6.0` 。

集成步骤中，只有版本号不同，适配 RN 与原生混合开发场景。

### 4. Android 重要配置项

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



## iOS

### 1. 选择集成方式

#### （1）使用 CocoaPods 快速集成

1. 添加`pod 'GrowingReactNativeKit'`到Podfile中
2. 执行`pod update`,不要用`--no-repo-update`选项
3. **\(可选\)** GrowingIO推荐您添加**AdSupport.framework**依赖库,用于来源管理激活匹配,有利于您更好的分析的数据
4. 添加项目依赖库的位置在项目设置target -&gt; 选项卡General -&gt; Linked Frameworks and Libraries
5. 集成`react native`打点`sdk` 

```text
npm install --save https://github.com/growingio/react-native-growingio.git#develop

react-native link react-native-growingio 
```

   6. 集成`js`脚本并执行命令 ，二者任选其一即可

```text
# 通过npmjs
npm install --save react-native-autotrack-growingio

# 直接使用git
npm install --save https://github.com/growingio/GIORNHook.git#0.0.5
```

{% hint style="danger" %}
**如果 Android 执行过过这个步骤，可以省略**
{% endhint %}

#### （2）手动安装

* 获取sdk zip包
* 解压iOS SDK压缩文件
* 将`Growing.h` 、`module.modulemap`、`GrowingCoreKit`、`GrowingAutoTrackKit`、`GrowingReactNativeKit`添加到iOS工程

| 库名称 | 描述 | 下载 |
| :--- | :--- | :--- |
| `Growing.h` 、`module.modulemap` | 公共头文件 | [下载](https://assets.growingio.com/sdk/ios/GrowingIO-iOS-PublicHeader-2.6.0-20181106162738.zip) |
| `GrowingCoreKit` | 基础功能库 | [下载](https://assets.growingio.com/sdk/ios/GrowingIO-iOS-CoreKit-2.6.0-20181106162738.zip) |
| `GrowingAutoTrackKit` | 无埋点库 | [下载](https://assets.growingio.com/sdk/ios/GrowingIO-iOS-CoreKit-2.6.0-20181106162738.zip) |
| `GrowingReactNativeKit` | RN 库 | [下载](https://assets.growingio.com/sdk/ios/GrowingIO-iOS-ReactNativeKit-2.6.0-20181106162738.zip) |

* 添加依赖, 在项目中添加以下库文件

| 库名称 | 类型 |
| :--- | :--- |
| Foundation.framework | 基础依赖库 |
| Security.framework | 用于SSL连接 |
| CoreTelephony.framework | 用于读取运营商名称 |
| SystemConfiguration.framework | 用于判断网络状态 |
| AdSupport.framework | 用于来源管理激活匹配 |
| libicucore.tbd | 用于WebSocket |
| libsqlite3.tbd | 存储日志 |
| CoreLocation.framework | 用于读取地理位置信息（如果您的app有权限） |

**添加完成以后, 库的引用如下:**

 **提醒：**添加项目依赖库的位置在项目设置target -&gt; 选项卡General -&gt; Linked Frameworks and Libraries

1. 添加编译参数
2. 集成`js`脚本并执行命令（如果 Android 做过这个步骤，可以省略）
3. 集成`react native`打点`sdk` 

```text
npm install --save https://github.com/growingio/react-native-growingio.git#develop

react-native link react-native-growingio 
```

   6. 集成`js`脚本并执行命令 ，二者任选其一即可

```text
# 通过npmjs
npm install --save react-native-autotrack-growingio

# 直接使用git
npm install --save https://github.com/growingio/GIORNHook.git#0.0.5
```

{% hint style="info" %}
如果`react-native link react-native-growingio`失败\(成功则忽略此步骤\),即发现Libraries中没有GrowingIORNPlugin.xcodeproj,则可手动配置：

1. 打开XCode's工程中, 右键点击Libraries文件夹 ➜ Add Files to &lt;...&gt; 
2. 去node\_modules ➜ react-native-growingio ➜ ios ➜ 选择 GrowingIORNPlugin.xcodeproj 
3. 在工程Build Phases ➜ Link Binary With Libraries中添加libGrowingIORNPlugin.a
{% endhint %}

### 2. [**设置URL Scheme**](../ios-sdk/#2-she-zhi-url-scheme)\*\*\*\*

### **3.**[**添加初始化函数**](../ios-sdk/#3-chu-shi-hua)\*\*\*\*

### **4.重要配置**

与原生混合开发的开发者可详细查看[ iOS 无埋点 重要配置](../ios-sdk/#zhong-yao-pei-zhi)文档，如果原生控件使用不多，只需关注如下配置即可：

* \*\*\*\*[**App Store 提交应用注意事项**](../ios-sdk/#zai-app-store-ti-jiao-ying-yong)\*\*\*\*

\*\*\*\*

## 手动采集页面

由于RN应用的页面切换并不遵循原生的生命周期， 需要单独适配， 目前在 React Native 页面中我们只支持`react-navigation`， `react-native-navigation`作为导航器, 并且为了拓展性， 留下了手动的page接口， 开发者可自行适配（[直接更改 hook.js 脚本](https://github.com/growingio/GIORNHook)）。

### 1. **react-navigation**

如果使用react-navigation， 我们的hook脚本进行了自动适配, 默认的page名称为其key值。 用户可以自行设置， 如下代码：

```text
this.props.navigation.setParams({growingPagePath: 'xx'});
```

### 2.  **react-native-navigation**

* 当前支持`react-native-navigation`版本为 1.1.486， 1.1.406。 理论上兼容1.1版本。
* `react-native-navigation`其标题默认取`title`， 如果没有`title`则取`screenId`，作为唯一标记。

用户可以设置自定义`title`, 只需要设置`growingPagePath`字段， 该字段与`title`同级即可.

### **3 其他**

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
      <th style="text-align:left">参数名称</th>
      <th style="text-align:left">限制条件</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">eventId</td>
      <td style="text-align:left">非空，长度限制小于等于50；</td>
    </tr>
    <tr>
      <td style="text-align:left">number</td>
      <td style="text-align:left">非空。</td>
    </tr>
    <tr>
      <td style="text-align:left">eventLevelVariable</td>
      <td style="text-align:left">
        <p>不可使用嵌套的<code>JSONObject</code>对象，即为JSONObject中不可以放入<code>JSONObject</code>或者<code>JSONArray</code>；</p>
        <p>key 长度限制小于等于50，value长度限制小等于1000。</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">conversionVariables</td>
      <td style="text-align:left">同上</td>
    </tr>
    <tr>
      <td style="text-align:left">peopleVariables</td>
      <td style="text-align:left">同上</td>
    </tr>
    <tr>
      <td style="text-align:left">visitor</td>
      <td style="text-align:left">同上</td>
    </tr>
    <tr>
      <td style="text-align:left">userId</td>
      <td style="text-align:left">
        <p>长度限制小于等于1000；</p>
        <p>如果值为空则清空了登录用户变量，不建议这么用，</p>
        <p>请使用 clearUserId 清除登录用户变量。</p>
      </td>
    </tr>
  </tbody>
</table>#### 代码示例：

GrowingIOPackage 向 RN 提供了一个 NativeModule ， 所有打点接口都是由其实现，使用方法如下：

```javascript
import {
    NativeModules
  } from 'react-native';

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
| ignore | 仅接受`"true"` | 忽略对应的元素，不采集点击事件和浏览事件 | [Android](../android-sdk/#hu-lve-yuan-su) 文档解释，iOS 意义相同 |
| track | 仅接受`"true"` | 采集输入框内容，默认采集输入框内容变化次数，不采集内容 | [Android](../android-sdk/#cai-ji-shu-ru-kuang-shu-ju) 文档解释，iOS 意义相同 |
| id |  | 设置界面元素ID | [Android](../android-sdk/#she-zhi-jie-mian-yuan-su-id) 文档解释，iOS 意义相同 |
| content |  | 设置界面元素内容 | [Android](../android-sdk/#she-zhi-jie-mian-yuan-su-nei-rong) 文档解释，iOS 意义相同 |
| info |  | 设置元素对象 | [Android](../android-sdk/#she-zhi-yuan-su-dui-xiang) 文档解释，iOS 意义相同 |

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
5. [原生验证内容](../ios-sdk/#3-quan-xuan-he-re-tu-gong-neng-yan-zheng)（iOS）

#### 验证工具：

1. [Mobile Debugger](../growingio-debugger/)
2. [查看日志： ](../android-sdk/#she-zhi-debug-mo-shi)Android 打开 TestMode  和 Debug Mode
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



