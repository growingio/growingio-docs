---
description: GrowingIO 埋点 SDK 仅自动采集设备信息和您埋点内容数据。
---

# ReactNative 埋点 SDK

## Android 集成

### 1. 添加 Android 埋点 SDK 依赖

* 建议使用 Android Studio 打开项目中的`android`文件夹
* React  Native 埋点 SDK 是在 Android 原生 SDK 上的扩展，参照[ Android 埋点 SDK](../android-sdk/android-mai-dian-sdk.md#ji-cheng-mai-dian-sdk)，集成步骤的 1~4，注意将 SDK 版本号替换成 RN 版本`RN-track-2.6.0` 

集成步骤中，只有版本号不同，适配 RN 与原生混合开发场景。

#### 配置示例：

将版本号更改为`RN-track-2.6.0`

```groovy
apply plugin: 'com.android.application'

android {
    defaultConfig {
        resValue("string", "growingio_project_id", "您的项目ID")
        resValue("string", "growingio_url_scheme", "您的URL Scheme")
    }
}
dependencies {
    //GrowingIO RN 埋点 SDK
    implementation 'com.growingio.android:vds-android-agent:RN-track-2.6.0@aar'
}
```

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



## iOS 集成

### 1. **添加 iOS 埋点 SDK 依赖**

React  Native 埋点 SDK 是在 iOS 原生 SDK 上的扩展，请参照 [iOS 埋点 SDK 集成步骤 1~3 ](../ios-sdk/mai-dian-sdk-ji-cheng.md#mai-dian-sdk-ji-cheng)，操作完全一致。

### **2.重要配置**

与原生混合开发的开发者可详细查看[ iOS 无埋点 重要配置](../ios-sdk/#zhong-yao-pei-zhi)文档，如果原生控件使用不多，只需关注如下配置即可：

* \*\*\*\*[**App Store 提交应用注意事项**](../ios-sdk/#zai-app-store-ti-jiao-ying-yong)\*\*\*\*



## 自定义事件和变量API

[同 React Native 无埋点版本的 API 。](./#zi-ding-yi-shi-jian-he-bian-liang-api)



## 验证 SDK 是否正常工作

#### 验证内容：

1. 验证打点事件是否发送

#### 验证工具：

1. [Mobile Debugger](../growingio-debugger/)
2. Android [查看日志： ](../android-sdk/#she-zhi-debug-mo-shi)设置 TestMode  和 Debug Mode ：

```java
GrowingIO.startWithConfiguration(this,new Configuration()
    //BuildConfig.DEBUG 这样配置就不会上线忘记关闭
    .setDebugMode(BuildConfig.DEBUG)
    .setTestMode(true)
    ...
    );
```

    3. iOS 查看日志：iOS 在 AppDelegate 文件中配置：

```objectivec
[Growing setEnableLog:YES];
```





