---
description: 目前处于灰度期间，想要体验请先联系 GrowingIO 对接人。
---

# 电脑端圈选 App - Android SDK 接入文档

**2.7.0 已经正式发布，想要试用 web 圈 app ，申请灰度权限后，只需要将想要用来圈选的 App 的 SDK 升级到 2.7.0 就可以。**

### 第一步 修改依赖中的版本号 

Gradle 编译环境（AndroidStudio）

（1）在 project 级别的 build.gradle 文件中将 **`vds-gradle-plugin`** 依赖的版本号改为 **autotrack-2.7.0** 

（2）在 module 级别的 build.gradle 文件中**的** **`com.growingio.android`** 插件的版本号改为 **autotrack-2.7.0**

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

