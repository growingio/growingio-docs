# 圈选常见问题

## 圈选指标

### 1. 什么时候应该重新圈选已经圈选过的元素？

简单说在元素的 **XPath** 改变的时候，大多数情况是在您上次圈选后，页面经历过重构后，需要重新圈选。

圈选一个元素，我们使用的是元素的 **XPxpath** 来唯一定位一个元素的，所以保存圈选的指标就是这个元素的 **XPath** 为主的一些信息。

**什么是 XPath 呢？**

引用维基百科介绍：

> The XPath language is based on a tree representation of the XML document, and provides the ability to navigate around the tree, selecting nodes by a variety of criteria.In popular use \(though not in the official specification\), an XPath expression is often referred to simply as "an XPath".

大致译为：

XPath 其实是描述基于 XML 文档这种树型结构的语言，它以一些信息就可以定位这种树形结构的每一个元素。

**XPath 中都有什么内容呢？**

我们使用一条伪 XPath 来举个例子，下图中 **“帮助文档”** 区域的 XPath 为：

```text
//*[@id="root"]/div/div/div[3]/div/div/div[3]/div/div[1]/div[1]/div[1]/div/div[1]
```

XPath 的规则是： 

```text
/          满足该绝对路径的元素
//         所有满足其后面的规则的元素
*          所有元素
[表达式]    限定表达式
[数字]：    第几个元素
[last()]:  最后一个
[@属性]：   满足该属性
|          逻辑或，将多个路径合并到一起
```

![XPath &#x56FE;&#x793A;](../.gitbook/assets/image%20%28278%29.png)

如果您依然半知半解，请遵循以下原则也可： 

> 您圈选的元素和上次圈选时不一样了、位置变化、文字变化、页面变化等，都需要重新圈选。

### **2. 同一个元素出现在 web 端和 WAP 端，可以只圈选一次吗？**

* 如果 URL 完全一样，是可以的;
* 如果 URL 不一样，圈选时就需要分别圈选。

### **3. 能否一次圈选多组页面？**

GrowingIO 可以让用户方便的圈选定义一组多个页面。例如,在定义整页时,如果关闭路径,那么就可以直接定义整个域名下的所有页面。同时,我们也可以利用通配符 「\*」 来定义同一域名下一组路径相似的页面。

例如，GrowingIO 博客文章的地址如下： [`https://blog.growingio.com/post/`](https://blog.growingio.com/post/) 具体路径,那么我们在路径中输入 /posts/\* 就会圈选出所有的博客单篇文章的页面。注意此时的路径开关需要打开。

具体使用可以参考[圈选-定义页面]()。

### **4. 圈选模式里的高亮实线和高亮虚线分别表示什么？**

高亮实线代表已经被圈选的元素，高亮虚线代表已经被圈选的同类元素。

### **5. 圈选时有数，保存后指标管理里面暂无数据？**

可能是圈选该元素的所在页面带查询条件 query，保存带 query 的元素不会回溯过去不带 query 的数据，所以数据会从保存后开始匹配 query 的数据（iOS 用户圈选 H5 时是关不了query 的）。

### **6. 为什么我页面的 PV 会小于页面上元素的浏览量 \(impression\)？**

1. 元素有可能有跨页面的展示，您又进行了跨页面的圈选定义;
2. 页面上的元素被多次展示，浏览量增加而页面没有再次加载;
3. 定义了同类元素，元素 impression 是多个元素的和。

### 7. **今天创建的指标，我点进去选过去 7 天和过去 14 天，为什么数据是一样的？**

如果是新创建的指标，指标需要回溯，而且只会回溯创建过去 7 天的数据，因此过去 14 天到过去 7 天都没数。



## web 圈选

### **1. 为什么 web 圈选的元素只有点击量，没有浏览量？**

* 如果被圈选元素是 a, button, input，img，并且在倒数两层以下，只会统计点击量，不会统计浏览量。
* 如果用户使用的是 IE8 及以下的 IE 浏览器版本，GrowingIO 无法统计元素浏览量 ，只会统计点击量。

### **2. 如果提示 iframe 被禁用而无法进行圈选怎们办？**

GrowingIO 是使用 iframe 来加载目标网页进行可视化定义的。如果目标网站禁止了 iframe 加载，就无法正常定义标签，当点击某个按钮的时候，页面无法发生跳转且命令行显示：

```text
Refused to display '**' in a frame because it set 'X-Frame-Options' to 'SAMEORIGIN'.
```

此时只允许同个顶级域名加载，所以需要设置http响应头允许 iframe 加载。

如果你的网站使用https协议，需向响应头添加配置

```javascript
Content-Security-Policy: frame-ancestors 'self' https://www.growingio.com
X-Frame-Options: Allow-From https://www.growingio.com
```

如果你的网站使用http协议，需向响应头添加配置

```javascript
Content-Security-Policy: frame-ancestors 'self' http://www.growingio.com
X-Frame-Options: Allow-From http://www.growingio.com
```

由于 Chrome 浏览器已经不再支持 X-Frame-Options 配置项，如果你只需在 Chrome 浏览器中进行圈选，建议通过浏览器检查后，只给 Chrome 请求的响应头添加配置

```javascript
Content-Security-Policy: frame-ancestors 'self' http://www.growingio.com https://www.growingio.com
```

### **3. 如何对带 hashtag 的 URL 进行各页面浏览量统计？**

我们默认不会把 hashtag 识别成页面 URL 的一部分。

对于使用 hashtag 作为单页应用页面切换的网站来说，可以使用 enableHT 来监听 hashtag 的变化，并区分页面来收集页面数据，每次 hashtag 改变都会触发一次 PV，hashtag 的信息也会记录在页面 URL 中。

这部分需要在[ SDK 集成](../sdk-integration/web-js-sdk/#22-hashtag-xi-tong-bian-liang)时开启设置。

### **4. web 搜索框里的内容可以采集吗？**

可以。但是需要集成 SDK 时设置开启内容采集。

### **5. web 圈选时候页面加载不完全？错位？**

部分浏览器存在不兼容的情况，推荐使用 Chrome 浏览器。

### **6. web 端已经装了 SDK，但是仍不能进行圈选。**

1. 有可能是工程师没有成功加载 JS； 
2. 可能是该网站禁止了 iframe 的加载，请联系工程师修改配置（参见第 3 点）； 
3. 可能是工程师加载 JS 代码时的项目 ID 填写有误（项目 ID 没有空格）。

## 移动端圈选

### **1. App 上页面的定义是什么？**

* Android：一个 Activity 或者 Fragment 是一个页面；
* iOS：一个 ViewController 是一个页面。

### **2. 用 GrowingIO 的 APP 圈选 APP 时提示 url Scheme 不正确？**

**请参考我们的 APP SDK 接入部分。**

注意安卓在添加 URL Scheme 时要加一整个 intent-filter 区块，并确保其中只有一个 data 字段。

### **3. 扫描圈选二维码，但是无法正常圈选**

#### 扫描二维码完毕后，点击落地页按钮是否正常唤起了APP呢？如果没有唤起APP， 有以下两种情况：

**Android：**

URL Scheme 填写错误， 请检查您的 manifest.xml 文件中 Url Scheme 是否和您官网的应用管理中的一致，示例代码如下：

{% code-tabs %}
{% code-tabs-item title="AndroidManifest.xml" %}
```markup
<activity
    android:name=".LauncherActivity"
    android:theme="@style/AppTheme"
    tools:replace="screenOrientation">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />

    </intent-filter>
    <!-- GrowingIO Url Scheme 配置，请从官网的应用管理中获取，正确填写 -->
    <intent-filter>
        <data android:scheme="growing.您的 URL Scheme" /> 
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
    </intent-filter>
    <!-- 即使含有自定义 scheme 也能正常唤醒，不互斥，不相互影响 -->
    <intent-filter>
        <data
            android:host="share"
            android:scheme="will" />
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
    </intent-filter>
</activity>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**iOS：**

[请参照文档正确配置 URL Scheme](../sdk-integration/ios-sdk-1/ios-sdk.md#2-she-zhi-url-scheme)，并检查确认和官网中的应用管理中的一致。

#### **如果能够唤起APP，但是看不到小红点：**

**Android：**

（1）安卓移动端圈选在小米开发者版本和 MIUI8 稳定版下调不起小红点，是因为这两个系统版本禁止了悬浮框权限，您可以尝试授予 App 的悬浮框权限，或者换台手机圈选。

步骤：安全中心 - 授权管理 - 应用权限管理 - 找到您要设置权限的 App - 找到悬浮框权限 - 进行授权信任。当然也可以换用其他安卓手机进行圈选。

（2）请找开发确认以下事项：

         i.  是否将`<intent-filter/>` 代码块配置在了 “闪屏页面”，并且在短时间内`finish`了这个 Activity ？ 

解答： 请将闪屏页面停留时间稍微延长，Gio 会在唤醒页面中检测是否登录了圈选，这时被 finish 将无法正常唤醒圈选。或者如果您不需要圈选闪屏页面， 请将`<intent-filter/>` 代码块放在主页的 `Activity` 中。

        ii. 是否在配置 `<intent-filter/>` 代码块中的 Activity 中使用了 `getIntent` 呢？

解答： 请在您的逻辑中过滤掉 Gio 的 Intent ，我们的 Intent 大致格式是这样：

`growing.xxxxxxxxxxx://growing/oauth2/token?loginToken=xxxxx.........`

请您在代码逻辑中判断如果是以 `growing` 开头的 `scheme` 不处理它，代码示例：

```java
// 请注意自己判断 intent 是否为空
Uri data = intent.getData();
if (data.getScheme().startsWith("growing.")){
    Log.d(TAG, "GrowingIO url scheme, not process");
}
```

**iOS：**

[是否添加了圈选激活代码](../sdk-integration/ios-sdk-1/ios-sdk.md#23-zai-appdelegate-zhong-tian-jia-ji-huo-quan-xuan-de-dai-ma)。

