# iOS SDK 常见问题

### 1. GrowingIO SDK 无埋点采集原理是什么？

    答：实现原理：[Method Swizzling](http://nshipster.cn/method-swizzling/) \( method swizzling 作用为替换或者修改系统方法\)，可理解为以下 5 个步骤。

> 1. 在系统方法 M1 中插入 GIO 代码 GIO\_B1
> 2. 系统方法 M1 被执行，此时也会执行 GIO\_B1
> 3. 在 GIO\_B1 内遍历 View Tree / DOM Tree 以及 UIViewController 的层次关系以获取必要的数据
> 4. 生成待发送事件并缓存
> 5. 发送事件到服务器，完成采集

![](https://growingio.atlassian.net/wiki/download/thumbnails/180519296/hook.png?version=1&modificationDate=1524817973586&cacheVersion=1&api=v2&width=265&height=250)

### 2. GrowingIO SDK 支持哪些 iOS 系统？

    答：目前支持 iOS 7 ～ iOS 12。

### 3. 如果动态添加 UIView、删除 UIView 或者 修改 UIView 在父视图中的位置，会有什么影响？

    答：SDK 依赖 subviews 里面的元素次序。如果有动态的需求，建议在 viewDidLoad 里加载所有可能的 UIView 节点，添加或删除可以通过设置 hidden 属性来实现，把不需要显示的设置 hidden 属性为 YES，把需要显示的设置 hidden 属性为 NO。在 UIView 已经显示之后，不要调用 -bringSubviewToFront:方法， -sendSubviewToBack:方法或 -insertSubview: 方法。

### 4. 子线程操作 UI 引起的问题如何处理？

    答：GIO SDK 会在主线程中遍历找寻某个subView，如果此时在子线程中删除了该subView，就会造成错乱甚至 crash。此外，Apple 不建议用户在子线程更新 UI。建议客户在 Xcode 中打开 "Main Thread Checker" 检测线程使用是否合理。参考：[Main Thread Checker](https://developer.apple.com/documentation/code_diagnostics/main_thread_checker?language=objc)

### 5. 圈选时，来自不同 VC 的元素显示的 VC 名称相同？

    答：这种情况，一般是因为父 VC 添加子 VC 方式不正确引起的，请客户排查子VC的生命周期是否完整。

### 6. 是否允许设置 view 的 growingAttributesValue 为单个的数字或者字母？

    答：不允许这种设置。出于安全考虑，金融类 App 会自定义键盘（默认键盘容易被黑），如果 SDK 允许采集 V 值为单个数字或字母的点击事件，则有可能会记录用户输入的账号或密码。由于上述原因， SDK 不会发送V 值为单个字母或数字的点击事件，如果用户违反约定，则会导致某个元素只有浏览量而没有点击量。

### 7. 打点为什么也要在主线程调用？

    答：打点数据涉及到 UI 元素，凡是涉及 UI 的我们都建议在主线程调用。

### 8. 是否支持在开启热图的时候圈选？

    答：不支持，开启热图可能会导致被圈选元素的 x 值发生变化。

### 9. 是否支持用 UITouch 实现的点击事件？

    答：不支持，建议使用 UITapGestureRecognizer

### 10. GIO SDK 是否支持 Swift 项目？

    答：支持

### 11. App 做了 Button 防重复点击，集成 SDK 后发现按钮无法点击？

    答： 请使用 GrowingIO 提供的防重复点击解决方式：[DJRepeatClickFilter](https://github.com/sishen/DJRepeatClickFilter/blob/master/TestClickQuickly/UIView%2BDJRepeatClickFilter.m)。

### 12. 如果项目中使用了Firebase SDK，需要注意什么？

    答：如果您的 iOS 项目中集成了 Firebase SDK，请确保使用的 Firebase SDK 版本在 4.8.1 及以上，否则会造成数据采集不到的情况。

### **13.关于苹果隐私政策相关事宜的公告**

亲爱的客户：

您好！

   从2018年10月3日开始，App Store Connect将要求所有新应用和应用更新版本时提供**隐私政策**，添加后才可以在App Store上提交或通过TestFlight外部测试进行分发。

【苹果通知：As a reminder, in June the App Store Review Guidelines were updated to require a privacy policy for all new apps and app updates as part of the app review process. Starting October 3, 2018, App Store Connect will require a privacy policy for all new apps and app updates before they can be submitted for distribution on the App Store or through TestFlight external testing. In addition, your app’s privacy policy link or text will only be editable when you submit a new version of your app.（详情可参见：[https://developer.apple.com/news/?id=08312018a）。](https://developer.apple.com/news/?id=08312018a%EF%BC%89%E3%80%82) 】

所以，在此提醒各位开发者：**提交App Store 审核前一定要准备自己的隐私权政策，并在app SafariViewContoller中弹出，否则会无法通过审核哦！如需要专业的法律意见，还请各位开发者小伙伴咨询您的律师或法律顾问哦！**

### **14.无法圈选**

请根据[这篇文档](../../faq/faq-circle.md#3-sao-miao-quan-xuan-er-wei-ma-dan-shi-wu-fa-zheng-chang-quan-xuan)自行排查，如果仍有问题，可以联系技术支持。

### **15.**[**如何查看当前 APP SDK 版本**](../android-sdk/android-chang-jian-wen-ti.md#ru-he-cha-kan-dang-qian-app-sdk-ban-ben)\*\*\*\*

### **16.**[**为何不建议自定义设备ID**](../android-sdk/android-chang-jian-wen-ti.md#wei-he-bu-jian-yi-zi-ding-yi-she-bei-id)\*\*\*\*



