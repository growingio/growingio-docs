# 验证打点事件

* [概述](best-practice.md#gai-shu)
* [cstm \(事件以及关联的事件级变量\) 事件](best-practice.md#cstm-shi-jian-yi-ji-guan-lian-de-shi-jian-ji-bian-liang-shi-jian)
  * [场景一：无关联事件级变量的计数器类型场景](best-practice.md#chang-jing-yi-wu-guan-lian-shi-jian-ji-bian-liang-de-ji-shu-qi-lei-xing-chang-jing)
  * [场景二：有关联事件级变量的计数器类型场景](best-practice.md#chang-jing-er-you-guan-lian-shi-jian-ji-bian-liang-de-ji-shu-qi-lei-xing-chang-jing)
  * [场景三：无关联事件级变量的数值类型场景](best-practice.md#chang-jing-san-wu-guan-lian-shi-jian-ji-bian-liang-de-shu-zhi-lei-xing-chang-jing)
  * [场景四：有关联事件级变量的数值类型场景](best-practice.md#chang-jing-si-you-guan-lian-shi-jian-ji-bian-liang-de-shu-zhi-lei-xing-chang-jing)
* [pvar\(页面级变量\) 事件](best-practice.md#pvar-ye-mian-ji-bian-liang-shi-jian)
  * [场景](best-practice.md#chang-jing)
  * [页面级变量配置方式示例](best-practice.md#ye-mian-ji-bian-liang-pei-zhi-fang-shi-shi-li)
  * [对应的代码](best-practice.md#dui-ying-de-dai-ma-2)
  * [数据验证方法](best-practice.md#shu-ju-yan-zheng-fang-fa-2)
* [evar \(转化变量\) 事件](best-practice.md#evar-zhuan-hua-bian-liang-shi-jian)
  * [场景](best-practice.md#chang-jing-1)
  * [转化变量配置方式示例](best-practice.md#zhuan-hua-bian-liang-pei-zhi-fang-shi-shi-li)
  * [对应的代码](best-practice.md#dui-ying-de-dai-ma-2)
  * [数据验证方法](best-practice.md#shu-ju-yan-zheng-fang-fa-2)
* [ppl \(用户变量\) 事件](best-practice.md#ppl-yong-hu-bian-liang-shi-jian)
  * [场景一：用户变量之登录用户ID](best-practice.md#chang-jing-yi-yong-hu-bian-liang-zhi-deng-lu-yong-hu-id)
  * [场景二：其他用户变量](best-practice.md#chang-jing-er-qi-ta-yong-hu-bian-liang)

## 概述

GrowingIO Debugger 工具能够帮助工程师、技术实施顾问清楚地看到发送出去的服务器请求是什么？发送的时机是什么？数据是不是按照实施方案中所描述的那样进行发送。有了 GrowingIO Debugger 的帮助，实施工程师可以通过高质量的实施，给数据需求方带来高质量高准确性的数据。

本文主要讲述如何使用 GrowingIO Debugger 工具验证打点数据。

打点数据分为 cstm、pvar、evar、ppl 四种数据类型，本文将通过场景化及流程图的形式详细介绍每种打点数据的验证方式。

Debugger 安装/打开方式请见：[Web Debugger](./#growingio-web-debugger)（用于验证网站、H5、M站的数据） 、[Mobile Debugger](./#growingio-mobile-debugger)（用于验证 App 的数据）

## **cstm \(**事件以及关联的事件级变量**\) 事件**

验证“事件以及关联的事件级变量” 数据

### **场景一：无关联事件级变量的计数器类型场景**

需求：以“登录成功”这个事件为例，打点记录登录成功的次数

#### **事件配置方式示例**

| **标识符** | **名称** | **事件级变量** | **类型** | **描述** |
| :--- | :--- | :--- | :--- | :--- |
| loginSuccess | 登录成功 | 无 | 计数器 | 登录成功次数 |

#### **对应的代码**

| 平台 | 方法原型 | 代码示例 |
| :--- | :--- | :--- |
| JS SDK | gio\('track', eventId\) ; | gio\('track', 'loginSuccess'\); |
| Android SDK | GrowingIO.getInstance\(\).track\(`String` eventId\); | GrowingIO.getInstance\(\).track\("loginSuccess"\); |
| iOS SDK | + \(void\)track:\(NSString \*\)eventId; | \[Growing track: @"loginSuccess"\]; |

#### **数据验证方法**

在对应的应用（网站、Android 或者 iOS App）中触发登录成功事件，通过 Debugger 工具验证数据准确性

按照如下流程图验证

![](../../.gitbook/assets/cstm-shi-jian.png)

在本例中，如下图的数据请求说明打点代码生效

![](../../.gitbook/assets/image%20%28388%29.png)

### **场景二：有关联事件级变量的计数器类型场景**

需求：以“登录成功”这个事件为例，打点记录登录成功的次数，同时需要区分不同登录方式对应的登录成功次数

那么登录方式就作为“登录成功”这个事件的属性，也就是事件级变量

#### **事件配置方式示例**

| **标识符** | **名称** | **事件级变量** | **类型** | **描述** |
| :--- | :--- | :--- | :--- | :--- |
| loginSuccess | 登录成功 | 登录方式 | 计数器 | 登录成功次数 |

#### **事件级变量配置方式示例**

| 标识符 | 名称 | 类型 | 描述 |
| :--- | :--- | :--- | :--- |
| loginWay\_var | 登录方式 | 字符串 | 登录方式，取值包括QQ、微信、手机等 |

#### **对应的代码**

此示例中的自定义事件为“登录成功（loginSuccess）”，关联一个事件级变量为“登录方式（loginWay\_var）”

| 平台 | 方法原型 | 代码示例 |
| :--- | :--- | :--- |
| JS SDK | gio\('track', eventId, eventLevelVariables\); | gio\('track', 'loginSuccess', {loginWay\_var':'QQ'}\) |
| Android SDK | GrowingIO.getInstance\(\).track\(`String` eventId, `JSONObject`  eventLevelVariables\); | GrowingIO.getInstance\(\).track\("loginSuccess", new JSONObject\(\).put\("loginWay\_var","QQ"\)\); |
| iOS SDK | + \(void\)track:\(NSString \*\)eventId withVariable:  \(NSDictionary&lt;NSString \*, NSObject \*&gt; \*\)variable; | \[Growing track:@"loginSuccess" withVariable:  @{@"loginWay\_var":@"QQ"}\]; |

#### **数据验证方法**

在对应的应用（网站、Android 或者 iOS App）中触发登录成功事件，通过 Debugger 工具验证数据准确性

按照如下流程图验证

![](../../.gitbook/assets/cstm-shi-jian-2.png)

在本例中，如下图的数据请求说明打点代码生效

![](../../.gitbook/assets/image%20%28303%29.png)

## **pvar\(**页面级变量**\) 事件**

验证 “页面级变量” 数据

### **场景**

在商品详情页，设置“商品名称”、“商品品类”作为页面级变量

### **页面级变量配置方式示例**

| 标识符 | 名称 | 描述 |
| :--- | :--- | :--- |
| skuName\_pvar | 商品名称 | 商品名称 |
| skuCategory\_pvar | 商品品类 | 商品品类，例如裙子、鞋靴等 |

### **对应的代码**

此示例中的页面级变量为“商品名称（skuName\_pvar）”、“商品品类（skuCategory\_pvar）”，在商品详情页面上设置了这两个页面级变量

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x5E73;&#x53F0;</th>
      <th style="text-align:left">&#x65B9;&#x6CD5;&#x539F;&#x578B;</th>
      <th style="text-align:left">&#x4EE3;&#x7801;&#x793A;&#x4F8B;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">JS SDK</td>
      <td style="text-align:left">gio(&apos;page.set&apos;, key, value);&#x6216;gio(&apos;page.set&apos;,
        pageLevelVariables);</td>
      <td style="text-align:left">gio(&apos;page.set&apos;, {&apos;skuName_pvar&apos;: &apos;&#x5973;&#x58EB;&#x4E2D;&#x8DDF;&#x51C9;&#x978B;&apos;,
        &apos;skuCategory_pvar&apos;: &apos;&#x978B;&#x9774;&apos;});</td>
    </tr>
    <tr>
      <td style="text-align:left">Android SDK</td>
      <td style="text-align:left">
        <p>GrowingIO.getInstance().setPageVariable(<code>Activity</code> activity, <code>String</code>key, <code>String</code> value);</p>
        <p>&#x6216;</p>
        <p>GrowingIO.getInstance().setPageVariable(<code>Activity</code> activity, <code>JSONObject </code>pageLevelVariables);</p>
      </td>
      <td style="text-align:left">
        <p>JSONObject jsonObject = new JSONObject(); jsonObject.put(&quot;skuName_pvar&quot;,
          &quot;&#x5973;&#x58EB;&#x4E2D;&#x8DDF;&#x51C9;&#x978B;&quot;); jsonObject.put(&quot;skuCategory_pvar&quot;,
          &quot;&#x978B;&#x9774;&quot;);</p>
        <p>GrowingIO.getInstance().setPageVariable(GoodsDetailActivity, jsonObject);</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">iOS SDK</td>
      <td style="text-align:left">
        <p>+ (void)setPageVariableWithKey:(NSString *)key
          <br />andStringValue:(NSString *)stringValue
          <br />toViewController:(UIViewController*)viewController;</p>
        <p>&#x6216;</p>
        <p>+ (void)setPageVariable:(NSDictionary&lt;NSString *,
          <br />NSObject *&gt; *)variable toViewController:
          <br />(UIViewController *)viewController;</p>
      </td>
      <td style="text-align:left">[Growing setPageVariable:@{@&quot;skuName_pvar&quot;:@&quot;&#x5973;&#x58EB;&#x4E2D;&#x8DDF;&#x51C9;&#x978B;&quot;,
        @&quot;skuCategory_pvar&quot;:@&quot;&#x978B;&#x9774;&quot;} toViewController:GoodsDetailViewController];</td>
    </tr>
  </tbody>
</table>### **数据验证方法**

在对应的应用（网站、Android 或者 iOS App）中打开设置了页面级变量的商品详情页，通过 Debugger 工具验证数据准确性

按照如下流程图验证

![](../../.gitbook/assets/pvar.png)

在本例中，如下图的数据请求说明打点代码生效

![](../../.gitbook/assets/image%20%28205%29.png)

## **evar \(**转化变量**\) 事件**

验证 “转化变量” 数据

### **场景**

在购物场景中，用户可以通过首页Banner、商品列表页等方式进入某个商品详情页，进而产生下单行为，想要知道不同的前序行为对于下单的贡献，就可以将商品详情页的入口来源定义为转化变量，用于分解下单行为（埋点事件）

### **转化变量配置方式示例**

| 标识符 | 名称 | 描述 | 归因 | 失效 |
| :--- | :--- | :--- | :--- | :--- |
| enterSource\_evar | 商品详情页的入口来源 | 取值包括首页Banner、商品列表页等 | 根据需求选择（不涉及数据验证） | 根据需求选择（不涉及数据验证） |

### **对应的代码**

此示例中的转化变量为“商品详情页的入口来源（enterSource\_evar）”，当进入详情页时设置了这个转化变量

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x5E73;&#x53F0;</th>
      <th style="text-align:left">&#x65B9;&#x6CD5;&#x539F;&#x578B;</th>
      <th style="text-align:left">&#x4EE3;&#x7801;&#x793A;&#x4F8B;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">JS SDK</td>
      <td style="text-align:left">gio(&apos;evar.set&apos;, key, value);&#x6216;gio(&apos;evar.set&apos;,
        conversionVariables);</td>
      <td style="text-align:left">gio(&apos;evar.set&apos;, &apos;enterSource_evar&apos;, &apos;&#x9996;&#x9875;Banner&apos;);&#x6216;gio(&apos;evar.set&apos;,
        {&apos;enterSource_evar&apos;: &apos;&#x9996;&#x9875;Banner&apos;});</td>
    </tr>
    <tr>
      <td style="text-align:left">Android SDK</td>
      <td style="text-align:left">
        <p>GrowingIO.getInstance().setEvar(String key, String value);</p>
        <p>&#x6216;</p>
        <p>GrowingIO.getInstance().setEvar(JSONObject conversionVariables);</p>
      </td>
      <td style="text-align:left">
        <p>GrowingIO.getInstance().setEvar(&quot;enterSource_evar&quot;, &quot;&#x9996;&#x9875;Banner&quot;);</p>
        <p>&#x6216;</p>
        <p>JSONObject jsonObject = new JSONObject();</p>
        <p>jsonObject.put(&quot;enterSource_evar&quot;, &quot;&#x9996;&#x9875;Banner&quot;);</p>
        <p>GrowingIO.getInstance().setEvar(jsonObject);</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">iOS SDK</td>
      <td style="text-align:left">
        <p>+ (void)setEvarWithKey:(NSString *)key
          <br />andStringValue:(NSString *)stringValue;</p>
        <p>&#x6216;</p>
        <p>+ (void)setEvar:(NSDictionary&lt;NSString *, NSObject *&gt; *)variable;</p>
      </td>
      <td style="text-align:left">
        <p>[Growing setEvarWithKey:@&quot;enterSource_evar&quot;
          <br />andStringValue:@&quot;&#x9996;&#x9875;Banner&quot;];</p>
        <p>&#x6216;</p>
        <p>[Growing setEvar:@{@&quot;enterSource_evar&quot;:@&quot;&#x9996;&#x9875;Banner&quot;}];</p>
      </td>
    </tr>
  </tbody>
</table>### **数据验证方法**

在对应的应用（网站、Android 或者 iOS App）中触发设置了转化变量的时机，通过 Debugger 工具验证数据准确性

按照如下流程图验证

![](../../.gitbook/assets/evar.png)

在本例中，如下图的数据请求说明打点代码生效

![](../../.gitbook/assets/image%20%2814%29.png)

## **ppl \(**用户变量**\) 事件**

验证 “用户变量” 数据

### **场景一：用户变量之登录用户ID**

验证上传的登录用户ID数据（因登录用户ID是主键，因此若未上传登录用户ID，其他任何上传的用户变量都是无效的。）

#### **登录用户ID配置方式**

![](../../.gitbook/assets/7.-yong-hu-bian-liang-he-id.png)

#### **对应的代码**

此示例中的用户变量为登录用户ID，在用户登录时设置

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x5E73;&#x53F0;</th>
      <th style="text-align:left">&#x65B9;&#x6CD5;&#x539F;&#x578B;</th>
      <th style="text-align:left">&#x4EE3;&#x7801;&#x793A;&#x4F8B;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">JS SDK</td>
      <td style="text-align:left">
        <p>// &#x7528;&#x6237;&#x767B;&#x5F55;&#x65F6;&#xFF0C;&#x8BBE;&#x7F6E;&#x767B;&#x5F55;&#x7528;&#x6237;ID</p>
        <p>gio(&apos;setUserId&apos;, userId);</p>
        <p>// &#x7528;&#x6237;&#x9000;&#x51FA;&#x767B;&#x5F55;&#x65F6;&#xFF0C;&#x6E05;&#x9664;&#x767B;&#x5F55;&#x7528;&#x6237;ID</p>
        <p>gio(&apos;clearUserId&apos;);</p>
      </td>
      <td style="text-align:left">
        <p>// &#x7528;&#x6237;&#x767B;&#x5F55;&#x65F6;&#xFF0C;&#x8BBE;&#x7F6E;&#x767B;&#x5F55;&#x7528;&#x6237;ID</p>
        <p>gio(&apos;setUserId&apos;, &apos;123456&apos;);</p>
        <p>// &#x7528;&#x6237;&#x9000;&#x51FA;&#x767B;&#x5F55;&#x65F6;&#xFF0C;&#x6E05;&#x9664;&#x767B;&#x5F55;&#x7528;&#x6237;ID</p>
        <p>gio(&apos;clearUserId&apos;);</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Android SDK</td>
      <td style="text-align:left">
        <p>//&#x7528;&#x6237;&#x767B;&#x5F55;&#x65F6;&#xFF0C;&#x8BBE;&#x7F6E;&#x767B;&#x5F55;&#x7528;&#x6237;ID</p>
        <p>GrowingIO.getInstance().setUserId(String userId);</p>
        <p>//&#x7528;&#x6237;&#x9000;&#x51FA;&#x767B;&#x5F55;&#x65F6;&#xFF0C;&#x6E05;&#x9664;&#x767B;&#x5F55;&#x7528;&#x6237;ID</p>
        <p>GrowingIO.getInstance().clearUserId();</p>
      </td>
      <td style="text-align:left">
        <p>//&#x7528;&#x6237;&#x767B;&#x5F55;&#x65F6;&#xFF0C;&#x8BBE;&#x7F6E;&#x767B;&#x5F55;&#x7528;&#x6237;ID</p>
        <p>GrowingIO.getInstance().setUserId(&quot;123456&quot;);</p>
        <p>//&#x7528;&#x6237;&#x9000;&#x51FA;&#x767B;&#x5F55;&#x65F6;&#xFF0C;&#x6E05;&#x9664;&#x767B;&#x5F55;&#x7528;&#x6237;ID</p>
        <p>GrowingIO.getInstance().clearUserId();</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">iOS SDK</td>
      <td style="text-align:left">
        <p>//&#x7528;&#x6237;&#x767B;&#x5F55;&#x65F6;&#xFF0C;&#x8BBE;&#x7F6E;&#x767B;&#x5F55;&#x7528;&#x6237;ID</p>
        <p>+ (void)setUserId:(NSString *)userId;</p>
        <p>//&#x7528;&#x6237;&#x9000;&#x51FA;&#x767B;&#x5F55;&#x65F6;&#xFF0C;&#x6E05;&#x9664;&#x767B;&#x5F55;&#x7528;&#x6237;ID</p>
        <p>+ (void)clearUserId;</p>
      </td>
      <td style="text-align:left">
        <p>//&#x7528;&#x6237;&#x767B;&#x5F55;&#x65F6;&#xFF0C;&#x8BBE;&#x7F6E;&#x767B;&#x5F55;&#x7528;&#x6237;ID</p>
        <p>[Growing setUserId:@&quot;123456&quot;];</p>
        <p>//&#x7528;&#x6237;&#x9000;&#x51FA;&#x767B;&#x5F55;&#x65F6;&#xFF0C;&#x6E05;&#x9664;&#x767B;&#x5F55;&#x7528;&#x6237;ID</p>
        <p>[Growing clearUserId];</p>
      </td>
    </tr>
  </tbody>
</table>#### **数据验证方法**

在对应的应用（网站、Android 或者 iOS App）中的进行登录、退出登录、切换账号登录的操作，通过 Debugger 工具验证数据准确性

按照如下流程图验证

![](../../.gitbook/assets/ppl.png)

在本例中，如下图的数据请求说明打点代码生效

![](../../.gitbook/assets/image%20%2850%29.png)

![](../../.gitbook/assets/image%20%28298%29.png)

### **场景二：其他用户变量**

验证上传的除登录用户ID之外的其他用户变量数据（因登录用户ID是主键，因此若未上传登录用户ID，其他任何上传的用户变量都是无效的，所以请确保已经上传登录用户ID）

本例中除登录用户ID之外，还上传了用户性别、用户年龄这两个用户变量

#### **其他用户变量配置方式示例**

| 标识符 | 名称 | 描述 | 归因 |
| :--- | :--- | :--- | :--- |
| gender\_ppl | 用户性别 | 用户性别 | 根据需求选择（不涉及数据验证） |
| age\_ppl | 用户年龄 | 用户年龄 | 根据需求选择（不涉及数据验证） |

#### **对应的代码**

此示例中的用户变量为“用户性别（gender\_ppl）”、“用户年龄（age\_ppl）”，在用户登录或者变量值发生变化时进行设置

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x5E73;&#x53F0;</th>
      <th style="text-align:left">&#x65B9;&#x6CD5;&#x539F;&#x578B;</th>
      <th style="text-align:left">&#x4EE3;&#x7801;&#x793A;&#x4F8B;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">JS SDK</td>
      <td style="text-align:left">gio(&apos;people.set&apos;, key, value);&#x6216;gio(&apos;people.set&apos;,
        customerVariables);</td>
      <td style="text-align:left">gio(&apos;people.set&apos;, {&apos;gender_ppl&apos;: &apos;&#x7537;&apos;,
        &apos;age_ppl&apos;: 25});</td>
    </tr>
    <tr>
      <td style="text-align:left">Android SDK</td>
      <td style="text-align:left">
        <p>GrowingIO.getInstance().setPeopleVariable(String key, String value);</p>
        <p>&#x6216;</p>
        <p>GrowingIO.getInstance().setPeopleVariable(JSONObject peopleVariables);</p>
      </td>
      <td style="text-align:left">
        <p>JSONObject jsonObject = new JSONObject();</p>
        <p>jsonObject.put(&quot;gender_ppl&quot;, &quot;&#x7537;&quot;);</p>
        <p>jsonObject.put(&quot;age_ppl&quot;, 25);</p>
        <p>GrowingIO.getInstance().setPeopleVariable(jsonObject);</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">iOS SDK</td>
      <td style="text-align:left">
        <p>+ (void)setPeopleVariableWithKey:(NSString *)key
          <br />andStringValue:(NSString *)stringValue;</p>
        <p>&#x6216;</p>
        <p>+ (void)setPeopleVariable:(NSDictionary&lt;NSString *, NSObject *&gt;
          *)variable;</p>
      </td>
      <td style="text-align:left">[Growing setPeopleVariable:@{@&quot;gender_ppl&quot;:@&quot;&#x7537;&quot;,
        @&quot;age_ppl&quot;:@25}];</td>
    </tr>
  </tbody>
</table>#### **数据验证方法**

在对应的应用（网站、Android 或者 iOS App）中触发对应的用户变量，通过 Debugger 工具验证数据准确性

按照如下流程图验证

![](../../.gitbook/assets/ppl2.png)

在本例中，如下图的数据请求说明打点代码生效

![](../../.gitbook/assets/image%20%28193%29.png)

