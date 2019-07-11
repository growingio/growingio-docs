# SDK 更新日志

## 2019年7月11日

### Android2.7.8

新特性：

* 增加接口 `setAndroidIdEnable` , `setImeiEnable`, `setGoogleAdIdEnable` 为海外上架应用涉及采集用户 `androidId`, `imei`, `googleAdId` 隐私数据的开关支持。

优化：

* 兼容点击事件发生 Activity  `onCreate` 生命周期的采集。

修复bug：

* 埋点 SDK 设置用户 ID ， 从未设置 `setUserID`到设置 userID 成功后无 page 事件发出，导致用户 ID 采集遗漏，继而导致登录用户采集遗漏；
* 修复在 Android 8.0 以上系统圈选时截图 Bitmap Config 为 HAREWARE 图片失败的问题。

## 2019年6月29日

### iOS 2.7.8

修复bug：

* 修复 iOS13`UISearchBar`崩溃问题；
* 修复 mobiledebugger 可能出现数据少显示的问题。

## 2019年6月21日

### Android 2.7.7

新特性：

* 适配 ReactNative 0.59.9 版本
* 适配  react-navigation ^3.11.0

修复bug：

* 修复编译期间用户主项目类文件过多时会触发编译期OOM

优化:

* 用户授予 `READ_PHONE_STATE` 后动态获取 imei 信息， 减少 imei 为空的统计数量。未优化前统计 imei 条件为：应用有权限后下一次冷启动采集。

### iOS 2.7.7

新特性：

* 适配 ReactNative 0.59.9 版本
* 适配  react-navigation ^3.11.0

修复bug：

* 修复地理位置相关的多线程问题
* 修复访问用户 ID 可能为 0 的问题
* 修复首次安装 app 可能会影响 `statusbar`展示方向问题
* 修复 hybrid 自定义事件匹配 page 问题

优化：

* 完善对`SDCycleScrollView`的支持

## 2019年6月4日

### Android 2.7.6

修复 bug :

* 圈选截图问题，开启硬件加速则会造成离屏缓存，导致截图不准；
* 圈选增加对内嵌 H5 页面中`chng`（输入框输入）事件的支持。

## 2019年5月13日

### iOS 2.7.6

优化：

* 兼容推送低版本 API ；
* 增强 `GrowingAspectModeSubClass` 的稳定性。

修复bug：

* 圈选增加对内嵌 H5 页面中`chng`（输入框输入）事件的支持。

## 2019年4月23日

### Android 2.7.5

新特性：

* 支持 instant run , 方便开发期间提升应用编译速度。

修复bug：

* 修复 Application onCreat 中 new WebView 可能造成的 crash ；
* 修复 setPageVariable 引起的 fragment 内存泄漏。

### iOS 2.7.5

修复bug：

* 数据库优化；
* 打点参数规则修；
* 对信鸽兼容；
* 修复对于特殊情况下react native page event发不出来的问题。

## 2019年3月29日

### Android 2.7.4

新特性：

* 支持采集 Lambda 表达式书写的点击事件。

优化：

* Web 圈选 app 中截屏速度限制；
* Mobile Debugger 下不发送 imp 事件，降低卡顿的可能性；
* 优化用户的混淆文件未更新最新版，导致的异常；
* 去除掉 EventBus 打印的 No Subscriber 日志。

修复bug：

* 用户使用 IdleHandler 更新 UI， 导致的 Mobile debugger 、 web 圈选 app 白屏。

### iOS 2.7.4

优化：

* web 圈 app 连接流畅度优化
* 优化了 mobile debugger 的流畅度

修复bug：

* 修复了`GrowingAspectModeDynamicSwizzling Mode` 下`WKWebView` crash 

### 2019年3月18日

#### IOS2.7.3

代码优化：

优化web圈app

### 2019年3月13日

#### IOS 2.7.2

兼容修复：

修复部分ReactCocoa版本crash

### 2019年3月8日

#### IOS 2.7.1

Bug fix:

1. 修复 `DDAbstractLogger`，`DDExtractFileNameWithoutExtension` 文件冲突 ，解决KVO移除异常问题。

### 2019年2月27日

#### **Android 2.7.0**

Bug fix：

1. 修复异常 `NullPointerException:Attempt to invoke virtual method 'java.lang.Class java.lang.Object.getClass()' on a null object reference` getPageName 中 fragment 为空的情况，崩溃率很低，预计在十万分之五。

Feature：

1. 增加打点验证功能 ；
2. 增加 web 圈选 APP 功能，圈选中可以查看热图 ；
3. 更改 APP 端圈选过程中提示条可拖拽提示。

#### **iOS 2.7.0**

Feature：

1. 增加打点验证功能 ；
2. 增加 web 圈选 APP 功能，圈选中可以查看热图 ；
3. 更改 APP 端圈选过程中提示条可拖拽提示。

### 2019年2月20日

#### Android 2.6.9

Bug fix：

1. 修复了多进程调用 `setVisitor` 时会出现空指针的 bug（只在极端情况下出现，crash 率预计在千分之一以内）。
2. 修复了 `AndroidRuntimeException: requestFeature() must be called before adding content`错误，该错误会出现在 Android 5.x 和 Android6.x 机型上，触发条件为：在 `onCreate` 中的 `super.onCreate(saveInstanceState)` 代码之后调用 `requestWindowFeature` 接口。

Feature：

1. 增加了错误提示，告知用户 GrowingIO Android SDK 不支持 Jack 编译器（详情：[https://developer.android.com/studio/write/java8-support\#disable\_jack](https://developer.android.com/studio/write/java8-support#disable_jack)）。
2. 加强了对 Android 水滴屏 Android P 型号的圈选的支持。

### 2019年1月8日

#### Android 2.6.8

1. 修复 Android 2.6.7 版本中圈选结果中 XPath 保存错误， 造成圈选没有数据，此问题数据采集正常，不影响用户数据的收集和统计。

### 2019年1月4日

#### Android 2.6.7

1. 修复 `View` 的 `Context` 是 `Application` 时，无法采集点击事件；
2. 修复 `Fragment` 元素不可见发送 `imp` 事件；
3. 优化元素展现事件和性能；
4. 修复在 vivo x20 plus 机型上，多进程圈选问题；
5. 自定义页面事件（`pvar`）优化，之前 `setPageVariable` 接口需要用户每次进入页面的时候设置， 现更新为`pvar`只需要设置一次，在页面销毁之前每次展示都会补发`pvar`，如果用户设置`null`则清空 `pvar`事件；
6. 提供关闭单独元素展现事件接口 —— [`ignoreViewImp`](android-sdk/api.md#growingio-yun-hang-shi-api)。

#### iOS 2.6.7

1. 提供关闭单独元素展现事件接口—— [`growingAttributesDonotTrackImp`](ios-sdk-1/ios-sdk-api.md#shu-ju-cai-ji-fa-song-api)\`\`
2. 自定义页面事件（`pvar`）优化，之前 `setPageVariable` 接口需要用户每次进入页面的时候设置， 现更新为`pvar`只需要设置一次，在页面销毁之前每次展示都会补发`pvar`，如果用户设置`null`则清空 `pvar`事件；

### 2018年12月14日

#### Android 2.6.6

1. 修复Android 4.4系统的手机上,由于`Davlik`虚拟机对`class`校验机制与`art`不同，导致的 APP 初始化 SDK 时出现 VerifyError crash。

### 2018年12月12日

#### Android 2.6.5

1. 支持运行时异步设置渠道信息 \(接口地址\)；
2. 兼容 AndroidX；
3. 增加app close事件上报,将最后一个页面的浏览时长计算入用户进入总时长；
4. 修复 Activity的背景是透明时,下边的Activity的ViewTree 变化无限回调GlobalLayout监听,导致page事件发送失败问题。

#### iOS 2.6.5

1. 增加app close事件上报,将最后一个页面的浏览时长计算入用户进入总时长。

### 2018年11月29日

#### Android 2.6.3

1. 修复客户调用`WebView`的`setWebChromeClient`， 传参为`new WebChromeClient()` 时导致内嵌 H5 页面无法圈选；
2. 支持[采集通知 Notification](android-sdk/android-sdk.md#cai-ji-tong-zhi-notification)的点击与展现。

#### iOS 2.6.3

1. 修复多个`ViewController` 滑动切换时，没有识别导致用户页面访问事件数据错误；
2. 支持[采集通知](ios-sdk-1/ios-sdk.md#push-tui-song-dian-ji-cai-ji)的点击。

### 2018年11月24日

Android 2.6.2

1. 修复在 WIFI 情况下， 数据发送时间间隔过长；
2. 华为手机 8.0 以上系统圈选时， 授权弹窗增加“已设置”按钮，使弹窗消失。

### 2018年11月14日

#### Android 2.6.1

1. 修复 SDK 2.6.0 用户 app 首次安装启动并且手机没有网络的情况下，`activate`事件发不出去，造成后续事件都不能发送，并且有可能导致 app 内存溢出。

### 2018年11月8日

#### Android，iOS 2.6.0

1. 发布 React Native 无埋点 SDK；
2. 发布 API Cloud、Cordova、Flutter、React Native 、Weex、Hybrid 、AppCan 埋点 SDK。

### 2018年10月23日

#### IOS 2.4.5

修复vstr发送地址错误

### 2018年10月13日

#### Android 2.4.5

适配 Android build gradle plugin 3.2.1

### 2018年9月21日

Android，iOS  2.4.4  

1. Hybrid SDK 支持设置登录用户变量
2. 修复若干兼容性问题

### 2018年9月8日

Web 2.1.16

1. 修复部分情况下 SDK 通信失效导致网站无法圈选的问题
2. 支持Chrome圈选插件适配 iframe 嵌套的场景
3. 采集数据实时回掉接口 addRealTimeCallback 支持默认上报所有事件类型
4. 增加 ignoreClck 配置项，开启后不采集任何点击事件

### 2018年8月21日

Android 2.4.3

1. deep link功能升级
2. 修复 bug

iOS 2.4.3

1. deep link功能升级

### 2018年8月8日

Web 2.1.15

1. 优化sdk在圈选初始化时的运行效率
2. 优化分群批量上传登录用户ID时部分ID没有取到的问题
3. 修复修改sdk的origin字段后可能无法圈选的问题
4. 修复clearUserId接口不起作用的问题
5. 修复app版本和网站/手机应用两个维度无法与自定义事件关联的问题

### 2018年8月2日

iOS 2.4.2

1. 优化iOS  Hybrid pvar事件

### 2018年7月30日

Android 2.4.1

1. Fix bug 

### 2018年7月26日 

Web 2.1.14

1. 修复设置登陆用户ID时可能会报错的问题 
2. 优化元素浏览量数据的上行数量，上行每次用户行为后60秒内的元素浏览量

### 2018年7月25日

iOS 2.4.1

1. Deeplink功能添加Universal Link/应用宝微链接支持

### 2018年7月19日 

Web 2.1.13

1. 扩大采样功能边界，自定义事件的发送也进入采样控制范围
2. 优化元素浏览量的采集方式，解决svg元素浏览量少发的问题

### 2018年7月18日

Android 2.4.0

1. 稳定性全面测试
2. 支持访问用户变量
3. 扩大SDK埋点参数限制
4. 修复若干兼容性问题

iOS 2.4.0

1. 稳定性全面测试
2. 支持访问用户变量
3. 扩大SDK埋点参数限制
4. 修复若干兼容性问题

### ​2018年7月17日 

Web 2.1.12

1. 优化cdn发版效率 
2. 优化cookie使用方式，大幅降低SDK所需占用的浏览器cookie

### 2018年7月10日

Web 2.1.11

1. 支持GrowingIO Chrome圈选插件

### 2018年7月3日

Web 2.1.10

1. 优化SDK启动效率
2. 优化数据发送效率
3. 修复了同时打开多个页面的情况下page事件少发的问题
4. 修复若干兼容性问题
5. 增加容错客户不慎重复集成SDK的情况

### 2018年6月26日 

iOS 2.3.3

1. 修复了在某些情况下影响iOS 处理器性能的问题
2. 修复了某些情况下向数组内添加nil时引起的崩溃问题
3. 修复了某些情况下tap实现的点击没有发送数据的问题
4. 增加了对一部分tags请求的超时限制功能
5. clck数据去掉无效的n字段

### 2018年6月19日  <a id="&#x66F4;&#x65B0;&#x65F6;&#x95F4;-2018&#x5E74;6&#x6708;19&#x65E5;-&#x7248;&#x672C;&#x53F7;&#xFF1A;v18111"></a>

Android 2.3.3

1. 优化SDK启动时长，提速大约5倍
2. 修复地图兼容性问题.
3. 添加 是否采集UserAgent接口
4. 修复Mobile Debugger 显示 DeepLink 的 reengage 问题
5. 修复因为 setUserId 导致 pvar 事件不发送问题
6. 修复多进程 session 同步问题
7. 删除采集 mac 地址接口
8. 修复若干兼容性问题

### 2018年6月1日

Web 2.1.9

1. 修复在iframe+jQuery情况下page事件少发的问题
2. 修复设置采样情况下的兼容性问题

### 2018年5月21日  <a id="&#x66F4;&#x65B0;&#x65F6;&#x95F4;-2018&#x5E74;5&#x6708;22&#x65E5;-&#x7248;&#x672C;&#x53F7;&#xFF1A;v1881"></a>

Web 2.1.8

1. 支持GDPR欧盟区一般数据保护条例
2. 修复若干兼容性问题

Android 2.3.2

1. 支持Deeplink直达落地页
2. 支持GDPR欧盟区一般数据保护条例
3. 支持WebView的 loadData 和 loadDataWithBaseURL 方法，修复loadUrl 混淆死循环问题
4. 兼容DSBridge
5. 使用 setAPPVariable 兼容 1.x 版本的 setCS2 ~ setCS10 方法，并支持多进程数据同步
6. 修复因动态权限请求导致丢失vst 事件的问题
7. 修复MobileDebugger中vst事件重发的问题
8. 埋点接口track取消对 p 和 ptm 的依赖，未拿到相应的值也能正常发送cstm事件
9. 优化Dialog等相关组件的hook show 方法，解决死循环问题
10. 优化热图对于控件相同 id 匹配的问题，修复热图点击次数不一致却显示一致的问题

iOS 2.3.2

1. 支持Deeplink直达落地页
2. 支持GDPR欧盟区一般数据保护条例

### 2018年4月24日 <a id="&#x66F4;&#x65B0;&#x65F6;&#x95F4;-2018&#x5E74;3&#x6708;21&#x65E5;-&#x7248;&#x672C;&#x53F7;&#xFF1A;v1840"></a>

Web 2.1.7

1. 支持圈选弹层在整个页面内拖拽

### 2018年4月17日 

Android 1.1.9

1. 修复因动态添加Fragment导致的page事件发送问题
2. 修复WebView视频播放横屏问题，屏蔽淘宝的WebView ，增加对WVJBWebView的兼容，修复其他loadUrl的问题。
3. 修复因hook 静态代码造成的崩溃
4. 优化获取mac地址的方式
5. 优化 EditText 的 chng 事件发送，处理焦点变化问题
6. 修复数据库关闭偶现崩溃异常catch
7. 实现对Robotium 测试框架兼容

### 2018年4月10日  <a id="&#x66F4;&#x65B0;&#x65F6;&#x95F4;-2018&#x5E74;3&#x6708;21&#x65E5;-&#x7248;&#x672C;&#x53F7;&#xFF1A;v1840"></a>

iOS 2.3.1

1. 支持Hybrid Mode埋点
2. 优化部分场景下Hybrid JS SDK加载失败而无法对H5页面进行数据采集、圈选的情况;
3. 优化客户端数据库的批量操作以解决部分机型出现崩溃的问题;
4. 优化因开发证书变更导致keychain数据无法获取的问题;
5. 优化部分click事件无法找到对应page事件的问题.

Android 2.3.1

1. 找回trackEditText\(EditText\)接口， 默认不采集输入框的文本值
2. 修复 webView loadUrl 报错修复，视频播放横屏问题修复，对 WVJBWebview 兼容
3. 页面访问量暴增问题修复
4. 消除无用警告日志打印
5. DeepLink 参数获取
6. Mobile Debugger 中 u 字段补充
7. 优化圈选小红点的稳定性，偶现小红点消失情况修复

### 2018年4月9日 <a id="&#x66F4;&#x65B0;&#x65F6;&#x95F4;-2018&#x5E74;3&#x6708;21&#x65E5;-&#x7248;&#x672C;&#x53F7;&#xFF1A;v1840"></a>

Web 2.1.6

1. 修复部分情况下vst事件重发的问题
2. 修复部分情况下元素无浏览量的问题
3. 增加手动发送page事件的接口以解决在单页面等场景下丢失page事件的问题

### 2018年3月27日  <a id="&#x66F4;&#x65B0;&#x65F6;&#x95F4;-2018&#x5E74;3&#x6708;21&#x65E5;-&#x7248;&#x672C;&#x53F7;&#xFF1A;v1840"></a>

iOS 1.4.2

1. 对部分场景下Hybrid JS SDK加载失败而无法对H5页面进行数据采集、圈选的情况提供可手动调用的接口
2. 优化客户端数据库的批量操作，解决小部分机型出现crash的情况
3. 优化因开发证书变更导致keychain数据无法获取的问题
4. 优化小部分clck事件无法找到对应page的问题

### 2018年3月21日  <a id="&#x66F4;&#x65B0;&#x65F6;&#x95F4;-2018&#x5E74;3&#x6708;21&#x65E5;-&#x7248;&#x672C;&#x53F7;&#xFF1A;v1840"></a>

iOS 2.3.0

1. 支持Cordova跨平台插件埋点实施
2. 支持RN（React Native）跨平台插件埋点实施
3. 支持广告监测产品DeepLink功能
4. 支持 GrowingIO Mobile Debugger

Android 2.3.0

1. 支持Cordova跨平台插件埋点实施
2. 支持RN（React Native）跨平台插件埋点实施
3. 支持广告监测产品DeepLink功能
4. Android SDK 支持 GrowingIO Mobile Debugger

### 2018年1月25日 <a id="&#x66F4;&#x65B0;&#x65F6;&#x95F4;-2018&#x5E74;3&#x6708;14&#x65E5;-&#x7248;&#x672C;&#x53F7;&#xFF1A;v1830"></a>

iOS 1.4.0

1. 修复部分情况下圈选返回后页面无法点击的问题
2. 修复App后台启动发送访问数据异常的问题git

### 2018年1月11日 <a id="&#x66F4;&#x65B0;&#x65F6;&#x95F4;&#xFF1A;2018&#x5E74;1&#x6708;11&#x65E5;"></a>

iOS v1.2.0

1. 全面提升数据采集能力
2. 优化内存占用，全面提高稳定性
3. 大幅提升控件兼容性
4. 加强了对MBProgressHUD的支持
5. 修复某些特定情况下浏览/点击事件未采集的问题
6. 修复在 iOS11环境下，应用启动时可能多发一个 Page 事件的问题

Andriod v1.1.3

1. 采集核心逻辑优化 Andriod SDK v1.1.3 问题修复：
2. 修复Spinner和RadioGroup的子元素无法正常显示热图数据的问题
3. 修复开启采样后，个别手机无法正常使用圈选功能的问题
4. 修复Android 7.1版本在某些情况下不能正常进行圈选的问题

### 2017年12月19日 <a id="&#x66F4;&#x65B0;&#x65F6;&#x95F4;&#xFF1A;2017&#x5E74;12&#x6708;19&#x65E5;"></a>

1. 优化特定环境下 H5 页面采集、圈选的情况
2. 修复部分边界条件下SDK崩溃的问题
3. 修复圈选时编辑框内文字不能复制的问题
4. 修复部分App集成RN-SDK崩溃的问题
5. 修复圈选时部分情况下page字段没有值的问题

### 2017年10月16日 <a id="2017&#x5E74;10&#x6708;16&#x65E5;"></a>

Android 1.1.1

1. 新增显示已圈选的功能
2. 新增 Hybrid 启用 HashTag 的开关
3. 增加同一个 App 对多个 UI 进程进行数据采集的支持 问题修复：
4. 修复 AppCrash 之后再进入 Session 会重算的问题
5. 修复部分情况下匹配热图数据时无法全部匹配 Spinner 和 RadioGroup 中子元素的错误的问题
6. 修复点击事件某字段可能产生的问题
7. 修复设置采样之后无法圈选的问题 优化：
8. 增加来源管理对使用 IMEI 匹配第三方检测系统的优化
9. 新增对 Android 7.1 的圈选支持 功能更新：iOS SDK 1.1.0 :
10. 增加显示已圈选的功能
11. 增加对 SDCycleScrollView 的支持
12. 增加对继承与 NSProxy 的基类的支持 问题修复：
13. 修复 AppCrash 之后再进入 Session 会重算的问题
14. 修复点击事件某字段可能产生的问题
15. 新增 SessionID 新逻辑，修复 imp 圈选数据问题
16. 修复使用 RAC 后无法正常采集数据的问题 优化：
17. 调整输入框默认采集变更次数
18. 优化与百度鹰眼 SDK 的兼容性
19. 搜集用户 IDFA 及 IDFV 信息并在 Visit 上报
20. 把 accessibilityLabel 作为文本内容采集

### 2017年8月22日 <a id="2017&#x5E74;8&#x6708;22&#x65E5;"></a>

iOS 1.0.3

1. 优化 Hybrid 采集，支持通过 hashtag 来跟踪页面切换，并与 Web 端采集规则和设置保持一致
2. 网络异常时引入 HttpDNS 发送，减少 DNS 劫持，提高数据发送的稳定性
3. 更好的支持 GPS 数据上报，城市信息分析更准确
4. 优化来源管理设备追踪的逻辑，匹配更精准
5. 优化用户识别的逻辑
6. 优化 BlocksKit 的兼容性
7. 内存优化和性能改进
8. App 内嵌 H5 支持热图功能

Android 1.0.3

1. 优化 Hybrid 采集，支持通过 hashtag 来跟踪页面切换，并与 Web 端采集规则和设置保持一致
2. 网络异常时引入 HttpDNS 发送，减少 DNS 劫持，提高数据发送的稳定性
3. 更好的支持 GPS 数据上报，城市信息分析更准确
4. 优化来源管理设备追踪的逻辑，匹配更精准
5. 优化上报接口网络请求的性能
6. 优化 Rewrite 插件，对于 Gradle 2.4 等版本有更好的兼容性
7. 内存优化和性能改进 Android 1.0.3 问题修复：修复复杂 ActivityGroup 嵌套情况下无法圈选的问题

### 2016年5月28日 <a id="&#x66F4;&#x65B0;&#x65F6;&#x95F4;&#xFF1A;2016&#x5E74;5&#x6708;28&#x65E5;"></a>

Android 0.8.83

1. 支持新版的Web圈选App功能，在电脑上可以方便地圈选App中的元素
2. 修复了许多bug

iOS 0.9.12

1. 支持新版的Web圈选App功能，在电脑上可以方便地圈选App中的元素
2. 支持设置“页面属性”，上传更多的页面数据，分析更加全面
3. 通过同一个ViewController实现的不同页面，现在可以在圈选时设置不同的名称了
4. 配合苹果公司新政策，调用的网络API改为IPv6形式
5. 修复了许多bug

### 2016年4月22日 <a id="&#x66F4;&#x65B0;&#x65F6;&#x95F4;&#xFF1A;2016&#x5E74;4&#x6708;22&#x65E5;"></a>

Android 0.8.64

1. 在圈选时可以高亮出已圈选过的元素，一次圈不完的时候可以下次接着圈，同时也方便了多个同事之间的协作圈选
2. 当元素面积较小时，会自动在手指旁出现放大镜效果，方便圈选小型元素
3. 支持圈选Hybrid App中的HTML5页面；圈选页面内的元素时还可以选择所属页面
4. SDK包精简重构，体积减小了20%

iOS 0.9.8.7

1. 在圈选时可以高亮出已圈选过的元素，一次圈不完的时候可以下次接着圈，同时也方便了多个同事之间的协作圈选
2. 改进了圈选App内HTML5页面时的一些细节
3. 修复了各种圈选可用性和准确性的问题

### 2016年3月26日 <a id="&#x66F4;&#x65B0;&#x65F6;&#x95F4;&#xFF1A;2016&#x5E74;3&#x6708;26&#x65E5;"></a>

Android 0.8.52

1. 支持在App内圈选H5页面
2. 改进App定义标签界面UI，美观度大大提高
3. 改进App圈选交互，结合使用场景判断默认参数
4. 恢复了定义页面标签
5. 对于已经集成过SDK的产品，需要重新集成最新SDK，并重新发版，才能在App内圈选H5

iOS 0.9.8.1

1. 改进App定义标签界面UI，美观度大大提高
2. 改进App圈选交互，结合使用场景判断默认参数
3. 恢复了定义页面标签

