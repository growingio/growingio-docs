---
description: 目前处于灰度期间，想要体验请先联系 GrowingIO 对接人。
---

# 电脑端圈选 App - Android SDK 接入文档

请确保你在之前已经安装了 GrowingIO 无埋点 SDK。

接下来，在将用来圈选的 app 上进行如下操作，安装 2.7.0-beta 版本的 SDK，**不需要也不建议进行发版。**

### 第一步 修改依赖中的版本号 

Gradle 编译环境（AndroidStudio）

（1）在 project 级别的 build.gradle 文件中将 **`vds-gradle-plugin`** 依赖的版本号改为 **autotrack-2.7.0-beta** 

（2）在 module 级别的 build.gradle 文件中**的** **`com.growingio.android`** 插件的版本号改为 **autotrack-2.7.0-beta** 

### **第二步 代码混淆**

如果你启用了混淆，请在你的 proguard-rules.pro 中加入如下代码（如果之前做过这个操作，检查代码相同，可以忽略这一步）：

```text
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

如果您使用了 AndResGuard ,请在白名单里添加 GrowingIO ,如下：

```text
R.string.growingio*
```

