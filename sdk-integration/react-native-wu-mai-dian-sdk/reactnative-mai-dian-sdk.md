---
description: GrowingIO 埋点 SDK 仅自动采集设备信息和您埋点内容数据
---

# ReactNative 埋点 SDK

## Android 

### 1. 添加 Android 原生依赖

React  Native 无埋点 SDK 是在 Android 原生 SDK 上的扩展，参照[ Android 埋点 SDK](../android-sdk/android-mai-dian-sdk.md#ji-cheng-mai-dian-sdk)，集成步骤的 1~4，注意将 SDK 版本号替换成 RN 版本`RN-track-2.6.0` 。

集成步骤中，只有版本号不同，适配 RN 与原生混合开发场景。

### 2. 重要配置项

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

* 添加`pod 'GrowingCoreKit'`到Podfile中
* 执行`pod update`,不要用`--no-repo-update`选项
* **\(可选\)** GrowingIO推荐您添加**AdSupport.framework**依赖库,用于来源管理激活匹配,有利于您更好的分析的数据
* 添加项目依赖库的位置在项目设置target -&gt; 选项卡General -&gt; Linked Frameworks and Libraries

#### \(2）手动安装

* 获取sdk zip包
* 解压iOS SDK压缩文件
* 将`Growing.h` 、`module.modulemap`、`GrowingCoreKit`

| 库名称 | 描述 | 下载 |
| :--- | :--- | :--- |
| `Growing.h` 、`module.modulemap` | 公共头文件 | [下载](https://assets.growingio.com/sdk/ios/GrowingIO-iOS-PublicHeader-2.6.0-20181106162738.zip) |
| `GrowingCoreKit` | 基础功能库 | [下载](https://assets.growingio.com/sdk/ios/GrowingIO-iOS-CoreKit-2.6.0-20181106162738.zip) |

### 2. [**设置URL Scheme**](../ios-sdk/#2-she-zhi-url-scheme)\*\*\*\*

### **3.**[**添加初始化函数**](../ios-sdk/#3-chu-shi-hua)\*\*\*\*

### **4.重要配置**

与原生混合开发的开发者可详细查看[ iOS 无埋点 重要配置](../ios-sdk/#zhong-yao-pei-zhi)文档，如果原生控件使用不多，只需关注如下配置即可：

* \*\*\*\*[**App Store 提交应用注意事项**](../ios-sdk/#zai-app-store-ti-jiao-ying-yong)\*\*\*\*



## 自定义事件和变量API

[同无埋点版本的 API 。](./#zi-ding-yi-shi-jian-he-bian-liang-api)



## 验证 SDK 是否正常工作

#### 验证内容：

1. 验证打点事件是否发送

#### 验证工具：

1. [Mobile Debugger](../growingio-debugger/)
2. [查看日志： ](../android-sdk/#she-zhi-debug-mo-shi)Android 打开 TestMode  和 Debug Mode
3. 查看日志：iOS



