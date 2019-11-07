# 项目管理

## 简介

GrowingIO提供了强大而全面的管理功能，您可以根据您的具体需求来对您的组织和数据进行管理。让您的成员轻松的协作，并且充分保障数据的安全性和私密性。**您可以在任意界面点击右上角的头像，进入项目管理模块。**



## 组织-项目-应用的关系

当您注册 GrowingIO 时，需要填写公司信息，注册完成时，我们会根据您填写的信息生成一个新的组织。后续您创建的所有项目，都会自动关联到这个组织。组织是您所在公司与 GrowingIO 对接的基本方式，包括对应的销售、技术支持、客户成功。我们的服务、合同和计费也会按照组织的方式。

一个组织下可以创建多个项目，GrowingIO 数据统计按照项目的方式进行，不支持跨项目的数据分析。例如访问用户量、登录用户量等，在同一个项目下不同产品内的同一个用户会被合并。

一个项目下可以创建多个应用，Web端对应不同的域名，移动端对应不同的平台或包名，小程序则对应不同的 AppID。一个项目下的多个应用数据会被合并统计，并在概览中通过平台、AppID 的方式区分。在其他分析模块，您可以使用 域名（对应移动端的包名和小程序的 AppID）或 网站/手机应用 拆分不同应用数据。



## 项目概览

项目概览为您呈现项目成员的项目使用情况。

![](https://docs.growingio.com/.gitbook/assets/1%20%285%29.png)

* 本周项目访问量：您的项目每周的页面打开次数，帮助您宏观的了解整个项目的使用情况。
* 7日访问趋势：帮助您了解您的项目在最近7日的使用情况。
* 成员使用情况排名：帮助您了解您的每个成员在项目中的活跃程度。
* 最热单图/最热看板：在您建立的看板/单图中，最常用最受欢迎的看板。
* 活动日志：您的成员在项目中的协作日志，帮助您了解您的分群，事件的新建和改动情况。



## 应用管理

{% hint style="info" %}
进入项目管理页面需要管理员权限，请您联系您公司项目的管理员为您开通权限。
{% endhint %}

在应用管理中，您可以创建多个产品。这些产品可以是不同平台的产品，例如 GrowingIO Android、GrowingIO iOS ，您可以对不同产品的数据进行统一的管理和分析，统计不同产品之间的数据差异，分析其中的共性和原因，进行深度的透视。

![](https://docs.growingio.com/.gitbook/assets/2%20%284%29.png)

#### 

### 配置 Universal Links（iOS）

* 在应用管理页面，配置 Universal Links，应用宝微下载（可选项）。
* 配置 Universal Links 将会需要您公司的 iOS 工程师协同配合，在 Xcode 客户端进行操作。

![&#x627E;&#x5230;&#x5E94;&#x7528;&#x7BA1;&#x7406;&#x9875;&#x9762;](../.gitbook/assets/image%20%2859%29.png)

![](../.gitbook/assets/ying-yong-guan-li.png)

* 您需要按照如下提示完成配置 Universal Links 的配置

#### 1.在您的 Xcode 中勾选如下功能

![](../.gitbook/assets/image%20%28233%29.png)

#### 2.添加GIO域名到 Xcode

![](../.gitbook/assets/image%20%2829%29.png)

注：添加至Domain的链接为：

```text
applinks:gio.ren
```

（请注意该 Domain 链接内容的正确填写）

#### 3.在苹果开发者网站中找到 Team ID 与 Bundle ID

![](../.gitbook/assets/image%20%28166%29.png)

#### 4.将 Team ID / Bundle ID 到 GrowingIO 后台,并勾选“我已完成Xcode配置，开启Universal Link跳转”

![](../.gitbook/assets/unilink.png)



### 配置 App Links（Android）

GrowingIO 支持 Android 系统提供的原生方案 App Links 实现 DeepLink 跳转 ，利用该技术，在某些场景下将实现一步唤起 App 并打开 APP 指定页面的功能。

App Links 技术只支持 Android 6.0 以上的机型，但是不用担心，6.0 以下机型将会通过 URL Scheme 的方式实现 DeepLink 跳转。

![](../.gitbook/assets/image%20%28186%29.png)

#### 1. 获取应用签名 SHA256 指纹证书

1. 使用命令行进入你的证书目录，一般签名分为 debug keystore 和 release keystore ，开发期间建议先配置为 debug keystore ，上线前一定要更新为 release keystore 。如果担心忘记，建议新建应用。 
2. 执行以下命令 ：

   ```text
   keytool -list -v -keystore my-release-key.keystore
   ```

3. 执行后你将看到类似下面这样的结果，请复制下来并填写进 GrowingIO 对应的应用配置中。

![](../.gitbook/assets/image%20%28172%29.png)

#### 2. 在 Manifest.xml 中配置 Intent Filter 

1. 点击「复制代码片段」
2. 进入您的安卓应用源码中的 manifest.xml 文件中，找到您的主页面，建议复制在主页，即为 Launcher Activity 中。
3. 复制完成后，您的 manifest.xml 文件将类似这样：

```markup
...
<activity
    android:name=".LauncherActivity"
    android:launchMode="singleTop"
    android:theme="@style/AppTheme">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
    <!-- GIO DeepLink -->
    <intent-filter>
        <data android:scheme="growing.856a6ce98c31281d" />
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
    </intent-filter>
    <!-- GIO App Links -->
    <intent-filter android:autoVerify="true">
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data
            android:host="gio.ren"
            android:pathPattern="/v8aud.*"
            android:scheme="https" />
    </intent-filter>
    <intent-filter android:autoVerify="true">
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data
            android:host="gio.ren"
            android:pathPattern="/v8a.*id.*"
            android:scheme="https" />
    </intent-filter>
    <intent-filter android:autoVerify="true">
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data
            android:host="gio.ren"
            android:pathPattern="/v8a.*td.*"
            android:scheme="https" />
    </intent-filter>
</activity>
...
```

{% hint style="danger" %}
* GrowingIO 暂不支持自定义 App Links 的 host，请不要修改复制的代码块中的 host；
* 不要尝试修改或者合并 GIO 的 intent filter ，[Google 官方解释](https://developer.android.com/training/app-links/deep-linking#adding-filters)。
{% endhint %}

#### 3. 测试并验证您的 App Links 

1. 完成上述配置后，安装在手机上
2. 执行以下命令：

   ```text
   adb shell dumpsys package d
   ```

3. 上述命令执行后的结果中，查找您应用的包名，只有 Status 为 always 才通过了系统校验。示例如下：

```text
  Package: com.growingio.android.test
  Domains: gio.ren
  Status:  always
```

{% hint style="info" %}
Domains 为 manifest.xml 文件中配置 Intent filter 中的 host , GIO 可能后续会更改广告的域名，一切以您复制得到的代码片段为准即可。
{% endhint %}

#### 4. 在 Callback 中接收自定义参数并跳转页面

在上文中，建议各位开发者将 GIO Intent Filter 代码块配置在 Launcher Activity 下，在用户点击短链后打开 App ，系统将自动跳转到 Launcher Activity ，此时 GIO DeepLink Callback 则会返回您在 GIO 官网广告监测中配置的自定义参数，此时您需要接收您的自定义参数，跳转到指定页面。

详见 [Android DeepLink CallBack 接收参数](../sdk-integration/android-sdk/android-sdk.md#deep-link-hui-tiao-can-shu-huo-qu)文档。



### 配置应用宝微下载

GrowingIO 提供跳转到应用宝微下载的功能，应用宝微下载为腾讯应用宝体系下的微下载链接。使用应用宝微下载，在微信等腾讯旗下软件中将转至微下载逻辑。

腾讯微下载介绍：[https://wiki.open.qq.com/index.php?title=mobile/应用宝微下载](https://wiki.open.qq.com/index.php?title=mobile/应用宝微下载)

![](../.gitbook/assets/tu-pian%20%281%29.png)

{% hint style="warning" %}
在确认开启应用宝微下载前，请确认您已经达到腾讯微下载服务的量级标准，并且审核通过，否则直接开启将导致用户使用体验下降。
{% endhint %}

