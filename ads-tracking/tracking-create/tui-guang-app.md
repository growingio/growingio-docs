# 推广 App

* [1 增加 App 下载量](tui-guang-app.md#1-zeng-jia-app-xia-zai-liang)
  * [1.1 场景适用](tui-guang-app.md#11-chang-jing-shi-yong)
  * [1.2 链接创建](tui-guang-app.md#12-lian-jie-chuang-jian)
* [2 吸引用户直接打开 App（DeepLink）](tui-guang-app.md#2-xi-yin-yong-hu-zhi-jie-da-kai-app)
* * [2.1 场景适用](tui-guang-app.md#21-chang-jing-shi-yong)
  * [2.2 链接创建](tui-guang-app.md#22-lian-jie-chuang-jian)
  * [2.3 相关配置](tui-guang-app.md#23-xiang-guan-pei-zhi)
* [3 SDK 版本支持](tui-guang-app.md#3-sdk-xiang-guan-xin-xi)
  * [3.1 App 下载追踪](tui-guang-app.md#31-app-xia-zai-zhui-zong)
  * [3.2 DeepLink 唤起 App](tui-guang-app.md#32-deeplink-huan-qi-app)

### 1 增加 App 下载量

#### 1.1 场景适用

适用于拉新场景，对于常见的 App 下载行为追踪，可选择该方式创建链接来追踪用户 App下载行为，方便您衡量广告推广效果。

![](../../.gitbook/assets/image%20%28136%29.png)

#### 1.2 链接创建

在“广告监测”模块下，点击“新建监测链接”，在推广 App 中选择“增加 App 下载量”，并填写监测链接创建所需的基本信息：

* 监测链接命名，按照系统提示填写内容，示例：百度 618 拉新投放；
* 选择您要推广的 App ，如果您需要在一条链接中同时推广您的 iOS 与 Android App，请勾选复选框；
* 选择该链接所属的推广活动；
* 选择您要投放的渠道，如果系统已经存在建议目标投放渠道，建议优先选择该渠道；
* 填写推广目标页地址，根据实际活动推广情况，填入目标页 URL 地址；
* 在一条链接推广一个平台 App 的情况下，支持批量创建监测链接，选择“批量创建”，输入链接数量即可批量生成一组监测链接；
*  完成链接创建，准备投放，可获取监测链接或下载二维码。
* 
### 2 吸引用户直接打开 App （DeepLink）

#### 2.1 场景适用

适用于促活、唤回场景，对于 App 已安装，希望通过广告推广促进用户打开 App 或是召回老客户，可通过该方式来创建深度链接（DeepLink）来促使用户回到您的 App 中。

![](../../.gitbook/assets/image%20%28342%29.png)

#### 2.2 链接创建

在“广告监测”模块下，点击“新建监测链接”，在推广 App 中选择“吸引用户直接打开 App ”，并填写监测链接创建所需的基本信息：

* 监测链接命名，按照系统提示填写内容，示例：百度 618 拉新投放；
* 选择您要推广的 App ，如果您需要在一条链接中同时推广您的 iOS 与 Android App，请勾选复选框；
* 可设定“直达 App 内落地页”参数，添加该参数后可实现直达 App 内的某一页面；
* 选择该链接所属的推广活动
* 选择您要投放的渠道
* 完成链接创建，准备投放，可获取监测链接或下载二维码 

{% hint style="info" %}
直达 App 内落地页的技术原理：

在移动端 App 中我们使用 URI Scheme 来定位一个应用甚至应用里的某个具体的功能或页面，就像定位一个网页一样。

例如：在 App 中我们要定位某个功能页面 ID 为 1234 的某个具体页面，就可以通过 [myapp://com.gio.function?page=1234](myapp://com.gio.function?page=1234) 这样的 URI Scheme 来实现。其中，page=1234 即为当前活动页的 URI ，其中 key=page ，value=1234 。

如果要使用该技术，请与活动页的开发人员确认页面 URI 参数信息以及 Key/Value 值，确认无误后请填入直达落地页参数输入框，该条深度链接将会跳转到 App 内的具体页面。 
{% endhint %}

#### 2.3 DeepLink 相关配置

在正确集成 GrowingIO 提供的 SDK ，以及 URL Scheme 的配置后，即可开始使用 GrowingIO 提供的 DeepLink 深度链接功能。

SDK 端配置：[iOS 端](https://docs.growingio.com/docs/sdk-integration/ios-sdk-1/ios-sdk#deeplink-hui-tiao-can-shu-huo-qu)、[Android 端](https://docs.growingio.com/docs/sdk-integration/android-sdk/android-sdk#deep-link-hui-tiao-can-shu-huo-qu)

为获得更好的用户使用体验，GrowingIO 同时建议您在 iOS 下开启 Universal Links 、在 Android 下开启 App Links ，该两项技术为 Apple 与 Google 提供的原生方案，使用该技术将在其系统生态中将获得更流畅的跳转体验。

Universal Links 配置：[配置方法](../qi-ta-gong-neng/shen-du-lian-jie-pei-zhi.md#universal-links)

App Links 配置：[配置方法](../qi-ta-gong-neng/shen-du-lian-jie-pei-zhi.md#applinks)

### 3 SDK 版本支持

#### 3.1 App 下载追踪

如果您需要跟踪 App（ iOS & Android）由推广带来的 App 下载，可以使用此功能。使用之前请确保您的 App 中加载了GrowingIO 的 SDK1.0.3 以上。

#### 3.2 DeepLink 唤起 App

| DeepLink功能 | App  SDK版本 |
| :--- | :--- |
|  DeepLink 基础功能（Scheme 唤起 App） | 2.3.0 |
| DeepLink 直达 App 内落地页（Scheme打开至活动页） | 2.3.2 |
| Universal Links / 应宝微下载支持 | 2.4.1 |
| Universal Links / App Links 一步跳入 App 支持 | 2.8.4 |

注：使用 DeepLink 功能需要 SDK 升级到 2.3.0 以上，SDK 版本功能向下兼容。



