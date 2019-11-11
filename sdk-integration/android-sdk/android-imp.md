---
description: 半自动浏览事件能够携带用户添加的自定义事件和变量，当 View 从屏幕中出现则自动发送事件。
---

# Android 半自动采集浏览事件

## 更新日志

{% hint style="info" %}
React Native 无埋点 SDK 目前还不支持。
{% endhint %}

| 无埋点 SDK 版本   | ReleaseNote |
| :--- | :--- |
| 2.8.2 | 支持 imp 半自动埋点 |
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
2. SDK 将 View 对象在当前屏幕是否可见，是否滚动出屏幕又再次出现纳入采集发送策略，并取消浏览事件与页面的关联。即：共享 View 对象的不同页面不会重复发送，并且 view 对象不在当前屏幕又再次滑入会再次发送。后文详细举例说明，[见：场景1](android-imp.md#chang-jing-1-bu-tong-de-ye-mian-gong-xiang-tong-yi-ge-yuan-su)。
3. **半自动指**：**用户提供 View 的自定义事件和变量内容，SDK 根据当前 view 对象是否在屏幕上可见，自动发送一个自定义事件。**即：需要用户标记 View 并且提供自定义事件和变量，SDK 在 View 对象出现在屏幕上时自动发送，不同于 track 接口发送的 cstm 事件，调用即发送。

下面我们将从三个方面介绍此方案。

### **1.** **标记View并设置自定义事件和变量**

在旧版无埋点 SDK  中，如果想查看某个广告位的具体某个商品的曝光次数，通过圈选广告位的 view 元素的 imp 事件是无法做到的，仍需要您埋点自定义事件。

在埋点元素曝光时，开发人员需要对 View 在当前屏幕上的可见性做逻辑判断，需要监控 View 对象是否在当前可见或者是否再次出现在屏幕上，有一定代码实施难度。

此方案将监控 View 的可见和埋点的触发时机交给 GIO SDK ， 开发者只需要将自定义事件的变量传递给 SDK 即可。

### **2.** **元素的浏览事件相对于旧版更准**

对于用户主动标记 imp 采集的元素，相对于旧版本自动采集会更加准确，我们举例以下两个场景来说明。

#### **场景1** **：** **不同的页面共享同一个元素**

![imp &#x573A;&#x666F;1](../../.gitbook/assets/image%20%2895%29.png)

底部的导航栏在 Tab 页面中是共享元素，即切换 Tab ，底部的导航栏持续可见，在不同的 Tab 页面中是共享元素。

**旧版本：**

导航栏的 imp 事件会分别在对应的四个页面中发送，切换 Tab 页面时虽然底部导航栏持续可见，但是会重复发送。

为什么旧版本会这么统计呢？ 因为 imp 事件的归因和 page 事件强关联，切换了页面，就认为导航栏是这个页面中的元素，不支持多个页面共享元素的情况。

**新版本：**

在用户标记导航栏需要采集后，无论切换 ab 多少次，我们只会在用户第一次可见导航栏，发一次元素浏览事件。

可以认为新版本方案只关心当前元素对于用户是否可见，和页面访问事件 page 无关联。更贴近实际场景，相对旧版本更准。



#### **场景2** **：** **列表元素的展现统计**

![imp &#x573A;&#x666F;2](../../.gitbook/assets/image%20%2896%29.png)

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

#### **markImpression\(ImpressionMark mark\)**

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>&#x53C2;&#x6570;</b>
      </th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">mark</td>
      <td style="text-align:left">
        <p><b>ImpressionMark:</b> &#x6807;&#x8BB0; view &#x7684;&#x914D;&#x7F6E;&#x7C7B;&#xFF1B;</p>
        <p><b>&#x6CE8;&#x610F;&#xFF1A;</b>&#x8FD0;&#x884C;&#x65F6;&#x6807;&#x8BB0;
          view &#x5BF9;&#x8C61;&#xFF0C;SDK &#x4E0D;&#x5B58;&#x50A8;&#x6807;&#x8BB0;&#x5BF9;&#x8C61;&#xFF0C;&#x6240;&#x4EE5;&#x4E0D;&#x5B58;&#x5728;&#x6807;&#x8BB0;&#x4E00;&#x6B21;&#xFF0C;&#x6C38;&#x8FDC;&#x8BB0;&#x5F55;&#x5E76;&#x91C7;&#x96C6;&#x7684;&#x73B0;&#x8C61;&#x3002;</p>
      </td>
    </tr>
  </tbody>
</table>**ImpressionMark 对象说明**

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;</th>
      <th style="text-align:left">&#x4F20;&#x53C2;&#x65B9;&#x5F0F;</th>
      <th style="text-align:left">&#x662F;&#x5426;&#x5FC5;&#x586B;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">view</td>
      <td style="text-align:left">&#x6784;&#x9020;&#x51FD;&#x6570;&#x4F20;&#x53C2;</td>
      <td style="text-align:left">&#x662F;</td>
      <td style="text-align:left"><b>View&#xFF1A;</b>&#x9700;&#x8981;&#x6807;&#x8BB0;&#x91C7;&#x96C6; imp
        &#x7684; view</td>
    </tr>
    <tr>
      <td style="text-align:left">eventId</td>
      <td style="text-align:left">&#x6784;&#x9020;&#x51FD;&#x6570;&#x4F20;&#x53C2;</td>
      <td style="text-align:left">&#x662F;</td>
      <td style="text-align:left">
        <p><b>String&#xFF1A;</b>&#x81EA;&#x5B9A;&#x4E49;&#x4E8B;&#x4EF6;&#x6807;&#x8BC6;&#x7B26;&#xFF1B;</p>
        <p>&#x975E;&#x7A7A;&#xFF0C;&#x957F;&#x5EA6;&#x9650;&#x5236;&#x5C0F;&#x4E8E;&#x7B49;&#x4E8E;50&#xFF1B;</p>
        <p>SDK 2.4.0&#x4EE5;&#x4E0B;&#x7248;&#x672C;&#x4E0D;&#x652F;&#x6301;&#x4E2D;&#x6587;&#xFF0C;&#x4EC5;&#x652F;&#x6301;
          0 &#x5230; 9&#x3001;a &#x5230; z &#x4EE5;&#x53CA;&#x4E0B;&#x5212;&#x7EBF;&#xFF0C;&#x5E76;&#x4E14;&#x4E0D;&#x80FD;&#x4EE5;&#x6570;&#x5B57;&#x5F00;&#x5934;&#x3002;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><del>setNum</del>
      </td>
      <td style="text-align:left"><del>set &#x65B9;&#x6CD5;&#x4F20;&#x53C2;</del>
      </td>
      <td style="text-align:left"><del>&#x5426;</del>
      </td>
      <td style="text-align:left">
        <p><b>&#x5DF2;&#x5E9F;&#x5F03;&#xFF0C;GIO &#x5DF2;&#x7ECF;&#x4E0D;&#x652F;&#x6301;&#x5206;&#x6790;&#x6B64;&#x7C7B;&#x578B;&#x7684;&#x6570;&#x636E;&#x3002;</b>
        </p>
        <p><del><b>Number</b>&#xFF1A;&#x4E8B;&#x4EF6;&#x7684;&#x6570;&#x503C;&#xFF0C;&#x6CA1;&#x6709;number&#x53C2;&#x6570;&#x65F6;&#xFF0C;&#x4E8B;&#x4EF6;&#x9ED8;&#x8BA4;&#x52A0;&#x4E00;&#xFF1B;</del>
        </p>
        <p><del>&#x5F53;&#x51FA;&#x73B0;number&#x53C2;&#x6570;&#x65F6;&#xFF0C;&#x4E8B;&#x4EF6;&#x81EA;&#x589E;number&#x7684;&#x6570;&#x503C;&#x3002;</del>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">setVariable</td>
      <td style="text-align:left">set &#x65B9;&#x6CD5;&#x4F20;&#x53C2;</td>
      <td style="text-align:left">&#x5426;</td>
      <td style="text-align:left">
        <p><b>JSONObject&#xFF1A;</b>&#x4E8B;&#x4EF6;&#x53D1;&#x751F;&#x65F6;&#x6240;&#x4F34;&#x968F;&#x7684;&#x7EF4;&#x5EA6;&#xFF08;&#x53D8;&#x91CF;&#xFF09;&#x4FE1;&#x606F;&#x3002;</p>
        <p>&#x975E;&#x7A7A;&#xFF0C;&#x957F;&#x5EA6;&#x9650;&#x5236;&#x5C0F;&#x4E8E;&#x7B49;&#x4E8E;100&#xFF08;<code>eventLevelVariable.length()&lt;=100</code>&#xFF09;&#xFF1B;</p>
        <p><code>eventLevelVariable</code> &#x5185;&#x90E8;&#x4E0D;&#x5141;&#x8BB8;&#x542B;&#x6709;<code>JSONObject</code>&#x6216;&#x8005;<code>JSONArray&#xFF1B;</code>
        </p>
        <p><code>key</code> &#x957F;&#x5EA6;&#x9650;&#x5236;&#x5C0F;&#x4E8E;&#x7B49;&#x4E8E;50&#xFF0C;<code>value</code> &#x957F;&#x5EA6;&#x9650;&#x5236;&#x5C0F;&#x7B49;&#x4E8E;1000&#xFF0C;&#x503C;&#x4E0D;&#x80FD;&#x4E3A;&#x7A7A;&#x4E32;&#xFF0C;&#x4E5F;&#x5C31;&#x662F;&quot;&quot;&#x3002;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">setGlobalId</td>
      <td style="text-align:left">set &#x65B9;&#x6CD5;&#x4F20;&#x53C2;</td>
      <td style="text-align:left">&#x5426;</td>
      <td style="text-align:left"><b>String</b>&#xFF1A;&#x7528;&#x4E8E;&#x5728;Activity&#x8303;&#x56F4;&#x5185;&#x552F;&#x4E00;&#x6807;&#x8BB0;View&#xFF0C;&#x4E0B;&#x6587;&#x6709;&#x8BE6;&#x7EC6;&#x89E3;&#x91CA;&#x6539;&#x5B57;&#x6BB5;&#x7684;&#x7528;&#x6CD5;&#x4E0E;&#x539F;&#x56E0;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <br />setDelayTimeMills</td>
      <td style="text-align:left">set &#x65B9;&#x6CD5;&#x4F20;&#x53C2;</td>
      <td style="text-align:left">&#x5426;&#xFF0C;&#x9ED8;&#x8BA4;&#x4E3A;0</td>
      <td style="text-align:left">
        <p><b>long &#xFF1A;</b>&#x68C0;&#x6D4B;Impression&#x7684;&#x5EF6;&#x8FDF;&#x65F6;&#x95F4;(ms)&#xFF0C;
          &#x9ED8;&#x8BA4;&#x4E0D;&#x5EF6;&#x65F6;&#xFF1B;</p>
        <p>&#x662F;&#x4E00;&#x4E2A;&#x6027;&#x80FD;&#x8C03;&#x4F18;&#x5B57;&#x6BB5;,
          &#x662F;&#x5141;&#x8BB8;GIO&#x5728;&#x754C;&#x9762;&#x6539;&#x53D8;&#x540E;&#xFF0C;
          &#x5EF6;&#x65F6;&#x4E00;&#x6BB5;&#x65F6;&#x95F4;&#x518D;&#x6B21;&#x8FDB;&#x884C;View&#x7684;&#x53EF;&#x89C1;&#x6027;&#x68C0;&#x6D4B;&#x5224;&#x65AD;.
          &#x9ED8;&#x8BA4;&#x4E3A; 0 &#x662F;&#x7ECF;&#x8FC7; GIO &#x6027;&#x80FD;&#x6D4B;&#x8BD5;&#x7684;&#xFF0C;
          &#x76F8;&#x4FE1;&#x80FD;&#x591F;&#x6EE1;&#x8DB3;&#x5927;&#x90E8;&#x5206;&#x573A;&#x666F;&#x7684;&#x6027;&#x80FD;&#x8981;&#x6C42;&#x3002;
          &#x53EF;&#x4EE5;&#x5C06;&#x6B64;&#x503C;&#x8BBE;&#x4E3A; 200, 300, 400,
          500&#x3002;</p>
      </td>
    </tr>
  </tbody>
</table>**GlobalId 字段说明:**

GlobalId 字段的提供主要是为了适配 RecyclerView 这类的可复用 View 对浏览定义的影响。 GIO对View的可见性跟踪默认是对象级别的跟踪，所以默认情况下用户notifyDataSetChange时\(即使内容并没有改变\)， 由于所有View被重新Bind， 而且由于RecyclerView的复用机制， 并不能保证复用顺序有序, 可能触发多条浏览事件发出， 这显然是不符合预期的。 

例如：用户下拉刷新页面，新增了一条 item ，但是列表顺序变了和未刷新前不一致，此时可能会有多条浏览事件发出，不符合预期，所以提供 globalId 方案解决使得只发送列表中可见新出现的元素。

提供以下方法解决此问题: 

**1.  调用 Adapter 的 setHasStableIds\(true\), 并重写 getItemId 方法:** 

```java
Adapter adapter = new MAdapter();
adapter.setHasStableIds(true);

class MyAdapter extends RecyclerView.Adapter<xxx> {
    @Override
    public long getItemId(int position) {
        return position;
    }
}
```



**2. 如果不希望设置 hasStableIds， 可以在标记 markViewImpression 时使用 globalId 字段**

```java
...
@Override
public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
    holder.left.setText("position: " + position);
    JSONObject jsonObject = new JSONObject();
    try {
        jsonObject.put("position", position);
    } catch (JSONException e) {
        e.printStackTrace();
    }
    GrowingIO.getInstance().markViewImpression(
            new ImpressionMark(holder.left, "列表项:" + position)
                    .setGlobalId(String.valueOf(position))
                    .setVariable(jsonObject)
    );
}               
```

### 

### 停止半自动采集View的浏览事件

#### **stopMarkViewImpression\(View markedView\)**

已经标记的View对象，通知GIO停止标记跟踪View的可见性。

| 参数 | 说明 |
| :--- | :--- |
| view | **View:** 标记 view。 |



### **xml** **的快捷配置方式**

{% hint style="danger" %}
使用此方法标记View的浏览时， 请使用MobileDebugger验证， 如果没有发出， 请使用调用接口的形式。
{% endhint %}

同时提供一个在xml中配置GIO imp事件的简单配置, GIO会在视图改变时检测View的tag属性，**如果符合"gio-tag-事件名称" 标准**，此时会调用GrowingIO.getInstance\(\).markViewImpression\(new ImpressionMark\(thisView, “事件名称"\)方法。

```markup
<TextView
            android:text=“black_shirt"
            android:tag=“gio-tag-black_shirt”            
            android:gravity="center"            
            android:textquColor="@android:color/white"            
            android:background="@android:color/black"            
            android:layout_width="match_parent"            
            android:layout_height="300dp" />
```



上边代码片段标记的事件id为: “black\_shirt" ，需要注意的是 Android 中View的 tag可被系统， 第三方框架作为特殊的用途。



### 设置自动采集元素内容

{% hint style="info" %}
Android 无埋点 **SDK 2.8.4** 及以上支持。
{% endhint %}

#### setCollectContent

自动采集元素内容，事件变量为 `gio_v`。

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">collectV</td>
      <td style="text-align:left">
        <p>boolean: &#x662F;&#x5426;&#x81EA;&#x52A8;&#x91C7;&#x96C6;&#x5143;&#x7D20;&#x5185;&#x5BB9;&#xFF0C;&#x9ED8;&#x8BA4;&#x503C;&#x4E3A; <b>true</b>&#xFF1B;</p>
        <p>true &#x91C7;&#x96C6;&#x5143;&#x7D20;&#x5185;&#x5BB9;&#xFF1B;</p>
        <p>false &#x4E0D;&#x91C7;&#x96C6;&#x5143;&#x7D20;&#x5185;&#x5BB9;&#x3002;</p>
      </td>
    </tr>
  </tbody>
</table>**代码示例：**

```java
//自动采集元素内容并发送埋点事件，默认值为 true 采集元素内容。
GrowingIO.getInstance().markViewImpression(        
                new ImpressionMark(findViewById(R.id.tv_imp_without_v), "btn_buy")
                                .setCollectContent(true)
                                );
```



### 设置半自动浏览View的可见比例

{% hint style="info" %}
Android 无埋点 **SDK 2.8.5** 及以上支持。
{% endhint %}

#### setVisibleScale

设置 View 的可见比例，当可见的比例大于等于 visiableScale 则自动触发埋点事件。

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">visibleScale</td>
      <td style="text-align:left">
        <p><b>float:</b> &#x53C2;&#x6570;&#x8303;&#x56F4;&#x5728; 0-1&#xFF1B;</p>
        <p>&#x5F53;&#x4E3A; 0 &#x65F6;&#x4E3A;&#x53EF;&#x89C1;&#x4EFB;&#x610F;&#x50CF;&#x7D20;&#x5219;&#x89E6;&#x53D1;&#x57CB;&#x70B9;&#x4E8B;&#x4EF6;&#xFF1B;</p>
        <p>&#x5F53;&#x4E3A; 1 &#x65F6;&#x4E3A; View &#x5B8C;&#x5168;&#x53EF;&#x89C1;&#x5219;&#x89E6;&#x53D1;&#x57CB;&#x70B9;&#x4E8B;&#x4EF6;&#x3002;</p>
        <p><b>&#x9ED8;&#x8BA4;&#x503C;&#x4E3A;0&#xFF0C;&#x5143;&#x7D20;&#x4EFB;&#x610F;&#x50CF;&#x7D20;&#x53EF;&#x89C1;&#x53D1;&#x9001;&#x57CB;&#x70B9;&#x4E8B;&#x4EF6;</b>&#x3002;</p>
      </td>
    </tr>
  </tbody>
</table>**代码示例**

```java
//当元素大于等于一半可见时，自动采集元素内容并发送埋点事件。
GrowingIO.getInstance().markViewImpression(        
            new ImpressionMark(findViewById(R.id.tv_imp_without_v), "btn_buy")           
                        .setVisibleScale((float) 0.5)
            );
```

 

## 常用控件代码示例

### 1. View 代码示例

对于界面上普通的 View， 例如 Button、ImageView 等，demo如下：

```java
Button button = findViewById(R.id.add_to_cart);
GrowingIO.getInstance().markViewImpression(new ImpressionMark(button,"add_cart"));
```

### 2. Banner 代码示例

```java
class MyAdapter extends PagerAdapter {

    @Override
    public void onPageSelected(int position) {
        try {
            GrowingIO.getInstance().markViewImpression(
                    new ImpressionMark(mImageViews.get(position), "Banner")
                            .setVariable(new JSONObject()
                                    .put("content", imageDescription[position])
                                    .put("id", position)
                            )
            );
        } catch (JSONException e) {
            e.printStackTrace();
        }
    }
}
```

### 3. 列表下拉刷新或者局部刷新

列表上局部刷新，或者下拉列表刷新增加个别 item 的时候，我们预期应该发送改变了内容，或者是新增内容的元素的曝光事件。

{% hint style="info" %}
[这里参照上文中 GlobalId 字段说明的代码示例。](android-imp.md#globalid-zi-duan-shuo-ming)
{% endhint %}



## FAQ

### **1. Stop 半自动化 imp 什么时候调用**

业务场景中，不需要再关注某个已经标记采集半自动 imp 的 view 元素。

### **2. View 是否是完全可见的时候，SDK 才会发送事件？**

默认 View 任意可见像素则自动触发埋点事件。

### **3.当元素内容发生改变，会发送事件么？**

例如 TextView，当元素内容发生变化，不会再次发送事件。

