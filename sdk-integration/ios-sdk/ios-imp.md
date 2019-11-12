---
description: 半自动浏览事件能够携带用户添加的自定义事件和变量，当视图从屏幕中出现则自动发送事件。
---

# iOS 半自动采集浏览事件

## 更新日志

{% hint style="info" %}
React Native 无埋点 SDK 目前还不支持。
{% endhint %}

| 无埋点 SDK 版本   | ReleaseNote |
| :--- | :--- |
| 2.8.1 | 支持 imp 半自动埋点 |
| 2.8.4 | 支持自动采集元素内容 |
| 2.8.5 | 添加配置当 View 可见的比例则自动触发埋点事件 |

请根据需求，更新对应版本的 SDK。

## 旧版全自动浏览事件介绍

集成无埋点 SDK 后，GIO 使用事件类型 imp（impression）采集用户浏览事件。

它的采集逻辑为 View 采集初始化完毕后在当前页面上可见，将发送 imp 事件。

注：

1. SDK标记的可见与否是针对于 View 对象的可见，所以对于列表的元素复用 iew 对象的情况，不会再次发送 imp 。
2. 标记的是对象是否可见，没有将 iew 是否滚动出屏幕再次出现在屏幕上的情况计算在内。**一个可见的 View 对象，只会发送一次 imp 事件，滚动出屏幕再次滚动出现，不会重复发送。**
3. View 对象和页面强关联，如果两个页面共享一个 View 对象，会认为是两个元素，发送两次 imp 事件。

综上， 虽然 SDK 能够自动采集每个 View 对象的浏览事件，但是有以下弊端：

* 在实际用户场景中并不能真正代表用户对于某个 View 的浏览次数
* 想自定义添加一些自定义变量，无法添加。

## **新版半自动浏览事件介绍**

新版方案：**用户标记一个 View 并提供自定义事件和变量，SDK 负责监控，当此 View 对象可见时发送用户配置的自定义事件和变量。**

新版用户浏览事件半自动究竟指什么？ 

1. 不再全量采集所有 View 的浏览事件，改为只采集用户主动标记 view 。事件类型使用自定义事件类型 **cstm** （custom），不再使用  **imp** （impression）。**需要用户在代码中埋点并且在官网配置自定义事件和变量。**
2. SDK 将 View 对象在当前屏幕是否可见，是否滚动出屏幕又再次出现纳入采集发送策略，并取消浏览事件与页面的关联。即：共享 View 对象的不同页面不会重复发送，并且 view 对象不在当前屏幕又再次滑入会再次发送。后文详细举例说明，[见：场景1](ios-imp.md#chang-jing-1-bu-tong-de-ye-mian-gong-xiang-tong-yi-ge-yuan-su)。
3. **半自动指**：**用户提供 View 的自定义事件和变量内容，SDK 根据当前 view 对象是否在屏幕上可见，自动发送一个自定义事件。**即：需要用户标记 View 并且提供自定义事件和变量，SDK 在 View 对象出现在屏幕上时自动发送，不同于 track 接口发送的 cstm 事件，调用即发送。

下面我们将从三个方面介绍此方案。

### **1.** **标记View并设置自定义事件和变量**

在旧版无埋点 SDK  中，如果想查看某个广告位的具体某个商品的曝光次数，通过圈选广告位的 view 元素的 imp 事件是无法做到的，仍需要您埋点自定义事件。

在埋点元素曝光时，开发人员需要对 View 在当前屏幕上的可见性做逻辑判断，需要监控 View 对象是否在当前可见或者是否再次出现在屏幕上，有一定代码实施难度。

此方案将监控 View 的可见和埋点的触发时机交给 GIO SDK ， 开发者只需要将自定义事件的变量传递给 SDK 即可。

### **2.** **元素的浏览事件相对于旧版更准**

对于用户主动标记 imp 采集的元素，相对于旧版本自动采集会更加准确，我们举例以下两个场景来说明。

#### **场景1** **：** **不同的页面共享同一个元素**

![imp &#x573A;&#x666F; 1](../../.gitbook/assets/image%20%28389%29.png)

底部的导航栏在 Tab 页面中是共享元素，即切换 Tab ，底部的导航栏持续可见，在不同的 Tab 页面中是共享元素。

**旧版本：**

导航栏的 imp 事件会分别在对应的四个页面中发送，切换 Tab 页面时虽然底部导航栏持续可见，但是会重复发送。

为什么旧版本会这么统计呢？ 因为 imp 事件的归因和 page 事件强关联，切换了页面，就认为导航栏是这个页面中的元素，不支持多个页面共享元素的情况。

**新版本：**

在用户标记导航栏需要采集后，无论切换 ab 多少次，我们只会在用户第一次可见导航栏，发一次元素浏览事件。

可以认为新版本方案只关心当前元素对于用户是否可见，和页面访问事件 page 无关联。更贴近实际场景，相对旧版本更准。



#### **场景2** **：** **列表元素的展现统计**

![imp &#x573A;&#x666F; 2](../../.gitbook/assets/image%20%28215%29.png)

**旧版本：**

GIO 推荐广告位在 `ScrollView` 的最底部，需要用户滑动才能看到，但是页面初始化的时候绘制，只是当前屏幕不可见。

旧版本中只会发送一次 imp 事件，无论随着用户的滑动元素是否再次可见，只有在页面初始化的时候发送此元素的 imp 事件。

**新版本：**

随着用户的滑动手势，元素进入屏幕则触发自动埋点事件，滑动出屏幕后，再滚动回来 View 再次可见依然会发送自动埋点事件。

即为元素对于用户可见几次则发送几次事件。

新版本浏览事件采集的列表，内部的元素展现次数的统计更加准确，更符合业务场景。

### **3.** **提升用户浏览事件采集的性能**

旧版本中，无埋点 SDK 将自动采集所有 View 的信息，最终分析时不会分析所有 APP 中的 View，在APP的性能、用户流量上都是一种浪费。

新版本中，SDK 非全量采集所有列表中的元素，只采集用户配置的元素，性能有较大提升。



## **API 文档**

{% hint style="info" %}
* 注意参数是否合法，和埋点 API 一样，eventID 和事件级变量 JSONObject 都有参数限制要求。
* 在元素展示前调用GIO API，GIO 负责监听元素展示并触发事件，半自动化 IMP 方案 SDK 上传的数据类型为 cstm ，和自定义事件是同种类型，所以**需要您在官网新建对应的事件类型和变量，并且强烈建议使用数据校验工具验证埋点事件。**
* 触发 SDK 自动采集时机： 元素从当前屏幕上不可见到可见。
* 对于被追踪元素上方有其它元素遮挡的情况 ，GrowingIO 仍可能发送该元素的展示事件 （适配这种case会消耗巨大性能，暂时不兼容）。
{% endhint %}

### **标记半自动化采集View元素**

**growingImpTrack**

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>&#x53C2;&#x6570;</b>
      </th>
      <th style="text-align:left">&#x662F;&#x5426;&#x5FC5;&#x586B;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">eventId</td>
      <td style="text-align:left">&#x662F;</td>
      <td style="text-align:left">
        <p><b>NSString:</b> &#x81EA;&#x5B9A;&#x4E49;&#x4E8B;&#x4EF6;&#x6807;&#x8BC6;&#x7B26;&#xFF1B;</p>
        <p>&#x975E;&#x7A7A;&#xFF0C;&#x957F;&#x5EA6;&#x9650;&#x5236;&#x5C0F;&#x4E8E;&#x7B49;&#x4E8E;50&#xFF1B;</p>
        <p>SDK 2.4.0&#x4EE5;&#x4E0B;&#x7248;&#x672C;&#x4E0D;&#x652F;&#x6301;&#x4E2D;&#x6587;&#xFF0C;&#x4EC5;&#x652F;&#x6301;
          0 &#x5230; 9&#x3001;a &#x5230; z &#x4EE5;&#x53CA;&#x4E0B;&#x5212;&#x7EBF;&#xFF0C;&#x5E76;&#x4E14;&#x4E0D;&#x80FD;&#x4EE5;&#x6570;&#x5B57;&#x5F00;&#x5934;&#x3002;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><del>number</del>
      </td>
      <td style="text-align:left"><del>&#x5426;</del>
      </td>
      <td style="text-align:left">
        <p><b>&#x5DF2;&#x5E9F;&#x5F03;&#xFF0C;GIO &#x5DF2;&#x7ECF;&#x4E0D;&#x652F;&#x6301;&#x5206;&#x6790;&#x6B64;&#x7C7B;&#x578B;&#x7684;&#x6570;&#x636E;&#x3002;</b><del><b>NSNumber:</b>&#x4E8B;&#x4EF6;&#x7684;&#x6570;&#x503C;&#xFF0C;&#x6CA1;&#x6709;number&#x53C2;&#x6570;&#x65F6;&#xFF0C;&#x4E8B;&#x4EF6;&#x9ED8;&#x8BA4;&#x52A0;&#x4E00;&#xFF1B;</del>
        </p>
        <p><del>&#x5F53;&#x51FA;&#x73B0;number&#x53C2;&#x6570;&#x65F6;&#xFF0C;&#x4E8B;&#x4EF6;&#x81EA;&#x589E;number&#x7684;&#x6570;&#x503C;&#x3002;</del>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">variable</td>
      <td style="text-align:left">&#x5426;</td>
      <td style="text-align:left">
        <p><b>NSDictionary: </b>&#x4E8B;&#x4EF6;&#x53D1;&#x751F;&#x65F6;&#x6240;&#x4F34;&#x968F;&#x7684;&#x7EF4;&#x5EA6;&#xFF08;&#x53D8;&#x91CF;&#xFF09;&#x4FE1;&#x606F;&#x3002;</p>
        <p>&#x975E;&#x7A7A;&#xFF0C;&#x957F;&#x5EA6;&#x9650;&#x5236;&#x5C0F;&#x4E8E;&#x7B49;&#x4E8E;100&#xFF08;<code>eventLevelVariable.length()&lt;=100</code>&#xFF09;&#xFF1B;</p>
        <p><code>eventLevelVariable</code> &#x5185;&#x90E8;&#x4E0D;&#x5141;&#x8BB8;&#x542B;&#x6709;<code>JSONObject</code>&#x6216;&#x8005;<code>JSONArray&#xFF1B;</code>
        </p>
        <p><code>key</code> &#x957F;&#x5EA6;&#x9650;&#x5236;&#x5C0F;&#x4E8E;&#x7B49;&#x4E8E;50&#xFF0C;<code>value</code> &#x957F;&#x5EA6;&#x9650;&#x5236;&#x5C0F;&#x7B49;&#x4E8E;1000&#xFF0C;&#x503C;&#x4E0D;&#x80FD;&#x4E3A;&#x7A7A;&#x4E32;&#xFF0C;&#x4E5F;&#x5C31;&#x662F;&quot;&quot;&#x3002;</p>
      </td>
    </tr>
  </tbody>
</table>

### 停止半自动采集View的浏览事件

**growingStopImpTrack**

已经标记的View对象，通知GIO停止标记跟踪View的可见性。通常应用于列表中的重用元素。

例如您只想追踪列表中的第一行元素的展示,但当第四行出现时重用了第一行的元素,此时您可调用此函数避免事件触发。



### 自动采集元素内容

{% hint style="info" %}
iOS 无埋点 **SDK 2.8.4** 及以上支持。
{% endhint %}



### 设置半自动浏览View的可见比例

{% hint style="info" %}
iOS 无埋点 **SDK 2.8.5** 及以上支持。             ``
{% endhint %}

#### 对单独元素设置可见比例

`@interface UIView(GrowingImpression)  
  
// 设置该节点被认定为可见的比例  
// 节点在屏幕中展示的面积 >= 节点面积 * scale 则判定该节点可见,反之不可见  
// scale 比例因子,范围[0-1];默认值0,这里0的意义可以理解为无限接近于0  
// 如需要指定scale 请在API growingImpTrack 调用之前调用  
@property (nonatomic, assign) double growingImpScale;  
  
@end`

#### 全局设置可见比例

`@interface Growing(AutoTrackKit)  
/**  
 imp半自动打点scale全局配置项  
 设置全局节点被认定为可见的比例  
 设置后对所有节点的imp scale生效,但优先级低于单独对节点设置imp scale  
 节点在屏幕中展示的面积 >= 节点面积 * scale 则判定该节点可见,反之不可见  
 !!! 请在main函数之前调用 !!!  
 @param scale 比例因子,范围[0-1];默认值为0,这里0的意义可理解为无限接近于0  
*/  
+ (void)setGlobalImpScale:(double)scale;  
+ (double)globalImpScale;`

**代码示例**

```objectivec
// 对单独元素设置可见比例
// 比例为0.5
UILabel *label = [[UILable alloc] init];label.growingImpScale = 0.5;
// 全局设置可见比例
// 全局比例为0.5
int main(int argc, char * argv[]) {
    @autoreleasepool {
        [Growing setGlobalImpScale:0.5];
        return UIApplicationMain(argc, argv, nil, NSStringFromClass([AppDelegate class]));
    }
}
```

## 常用控件代码示例

### **1. 追踪实例 testLabel 的元素展示事件：**

```objectivec
- (void)viewDidLoad {
    [super viewDidLoad];
    self.testLabel = [[UILabel alloc] initWithFrame:CGRectMake(0, 0, 50, 100)];
    self.testLabel.text = @"test";
    [self.testLabel growingImpTrack:@"您的eventId"];
    [self.view addSubview:self.testLabel];
    // Do any additional setup after loading the view.
}
```

### **2.追踪列表元素的展示事件，只追踪第一行：**

```objectivec
- (UICollectionViewCell *)collectionView:(UICollectionView *)collectionView cellForItemAtIndexPath:(NSIndexPath *)indexPath {
    TestCell *cell = [collectionView dequeueReusableCellWithReuseIdentifier:@"testCell" forIndexPath:indexPath];
    
    if (indexPath.item == 0) {
        [cell growingImpTrack:@"您的eventId"];
    } else {
        [cell growingStopImpTrack];
    }
    return cell;
}
```

### **3.追踪列表元素的展示事件，追踪全部行：**

```objectivec
- (UICollectionViewCell *)collectionView:(UICollectionView *)collectionView cellForItemAtIndexPath:(NSIndexPath *)indexPath {
    TestCell *cell = [collectionView dequeueReusableCellWithReuseIdentifier:@"testCell" forIndexPath:indexPath];
    
    [cell growingImpTrack:@"您的eventId" withVariable:@{由您来指定}];
    
    return cell;
}
```

