---
description: 适用于 SDK 2.x 以上，12 月 11 日上线。
---

# 查询条件直接设置成页面级变量

打开你的网址，看一看你的链接，是不是经常有这样的部分：

https://www.growingio.com/projects/1/homepage/overview?platform=ios&channel=banner

“?”后面的部分我们通常叫做查询条件（Query），在这个 URL 中标记了这个页面是在哪个平台（platform）打开的，以及通过哪个渠道（channel）访问的等重要信息，不同的公司有不同的应用，这种应用非常常见，那么，我们如何把这些重要的信息利用起来呢？

现在不需要埋点和开发，可以直接将这些查询条件设置成页面级变量，即维度；不仅如此，相比与埋点信息的延时发送，GrowingIO 将页面与查询条件中的信息同时发送、采集和计算，没时差，再也不会有那么多的空值了。

### 1.将查询条件直接设置成页面级变量

作为 SDK 版本为 2.x 以上的用户，现在就可以在「事件和变量」和「web 圈选」中直接设置查询条件为页面级变量（即维度）了：

![](../../.gitbook/assets/shi-jian-he-bian-liang.png)

![web &#x5708;&#x9009;](../../.gitbook/assets/image%20%2824%29.png)

{% hint style="info" %}
遵循两个原则：

1.全局生效。一旦设置，全站采集，对域名下的所有加载了 SDK 的页面生效。

2.埋点优先。埋点页面的数据不受任何影响，只在没有埋点或延时导致埋点数据没有发出来时，会取与「标识符」相同的查询条件对应的值。
{% endhint %}

### **2.案例详解**

如果你之前进行了埋点，那么这个功能上线后会发生什么变化呢？

电商客户 S 为了分析商品的售卖情况，在详情页进行了埋点，将每个商品的编号、商品名称等设置成了页面级变量，已经实施后上线了，其中商品编号标识符为 id，是从内容中取值的。接下来，用户用这个页面级变量来拆分商品详情页，进行分析。

假设商品详情页的 URL 是这样的 www.s.com/pro?id=342817&city=bj ，我们可以看到查询条件中的 id 就是这个商品的商品编号， **当这个功能上线后，由于「商品编号标识符： id」与「查询条件中的：id 」是一样的，我们会从打点设置的规则和 URL 中的查询条件中同时取 id 的值，**有如下几种情况：

> 表格第四列「埋点取的值」 - 客户打点时的规则是从「内容」中取值；
>
> 表格第五列「URL 取的值」 - 这个功能上线后我们从 URL 中取到对应的 key 的值。

{% hint style="success" %}
对于下面表格内容的总结

1.只要埋点取到了值，就取埋点的值。（表格中的第 1 个和第 2 个情况）

2.如果一个页面上，没有埋点的值，不管是因为这个页面没有埋点，还是因为发送时间导致没有发送出来，会取查询条件中取到的值。（表格中的第 3 个和第 4 个情况）
{% endhint %}

<table>
  <thead>
    <tr>
      <th style="text-align:left">#</th>
      <th style="text-align:left"></th>
      <th style="text-align:left">&#x53D8;&#x5316;</th>
      <th style="text-align:left">&#x57CB;&#x70B9;&#x53D6;&#x7684;&#x503C;</th>
      <th style="text-align:left">URL &#x53D6;&#x7684;&#x503C;</th>
      <th style="text-align:left">&#x4E4B;&#x524D;</th>
      <th style="text-align:left">&#x4E4B;&#x540E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">&#x7B2C; 1 &#x4E2A;&#x60C5;&#x51B5;</td>
      <td style="text-align:left">
        <p><b>&#x5546;&#x54C1;&#x8BE6;&#x60C5;&#x9875;1</b>
        </p>
        <p>&#x5546;&#x54C1;&#x9875;&#x5185;&#x5BB9;&#x4E2D;&#x7684;&#x503C; 342817</p>
        <p>www.s.com/pro?id=342817&amp;city=bj</p>
      </td>
      <td style="text-align:left">&#x65E0;&#x53D8;&#x5316;</td>
      <td style="text-align:left"><b>342817</b>
      </td>
      <td style="text-align:left">342817</td>
      <td style="text-align:left">342817</td>
      <td style="text-align:left"><b>342817</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&#x7B2C; 2 &#x4E2A;&#x60C5;&#x51B5;</td>
      <td style="text-align:left">
        <p><b>&#x5546;&#x54C1;&#x8BE6;&#x60C5;&#x9875;1 </b>
        </p>
        <p>&#x5546;&#x54C1;&#x9875;&#x5185;&#x5BB9;&#x4E2D;&#x7684;&#x503C; ac487</p>
        <p>www.s.com/pro?id=342816&amp;city=bj</p>
      </td>
      <td style="text-align:left">&#x65E0;&#x53D8;&#x5316;</td>
      <td style="text-align:left"><b>ac487</b>
      </td>
      <td style="text-align:left">342816</td>
      <td style="text-align:left">ac487</td>
      <td style="text-align:left"><b>ac487</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&#x7B2C; 3 &#x4E2A;&#x60C5;&#x51B5;</td>
      <td style="text-align:left">
        <p><b>&#x5546;&#x54C1;&#x8BE6;&#x60C5;&#x9875;2 </b>
        </p>
        <p>&#x5546;&#x54C1;&#x9875;&#x5185;&#x5BB9;&#x4E2D;&#x7684;&#x503C; 342815
          &#xFF0C;&#x4F46;&#x662F;&#x56E0;&#x4E3A;&#x65F6;&#x95F4;&#x7684;&#x7F18;&#x6545;&#xFF0C;&#x6CA1;&#x6709;&#x53D1;&#x51FA;&#x6765;&#x8FD9;&#x4E2A;&#x503C;</p>
        <p>www.s.com/pro?id=342815&amp;city=bj</p>
      </td>
      <td style="text-align:left">&#x8865;&#x4E86;&#x503C;</td>
      <td style="text-align:left">NA</td>
      <td style="text-align:left"><b>342815</b>
      </td>
      <td style="text-align:left">NA</td>
      <td style="text-align:left"><b>342815</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&#x7B2C; 4 &#x4E2A;&#x60C5;&#x51B5;</td>
      <td style="text-align:left">
        <p><b>&#x5546;&#x54C1;&#x8BA2;&#x5355;&#x9875;1 </b>
        </p>
        <p>&#x8BA2;&#x5355;&#x9875;&#x6CA1;&#x6709;&#x6253;&#x70B9;&#xFF0C;&#x6240;&#x4EE5;&#x8FD9;&#x91CC;&#x6CA1;&#x6709;&#x6253;&#x70B9;&#x7684;&#x503C;</p>
        <p>www.s.com/orderform?id=53462&amp;city=bj</p>
      </td>
      <td style="text-align:left">&#x8865;&#x4E86;&#x503C;</td>
      <td style="text-align:left">NA</td>
      <td style="text-align:left">NA</td>
      <td style="text-align:left">
        <p><b>53462</b>
        </p>
        <p>&#x26A0;&#xFE0F;&#xFF1A;&#x672C;&#x6765;&#x7528;&#x6237;&#x6CA1;&#x6709;&#x6253;&#x70B9;&#xFF0C;&#x62FF;&#x4E0D;&#x5230;&#x6570;&#x636E;&#xFF0C;&#x73B0;&#x5728;&#x6709;&#x4E86;&#x8BA2;&#x5355;&#x7F16;&#x53F7;&#x4E1A;&#x52A1;&#x610F;&#x4E49;&#x7684;&#x6570;&#x636E;&#x3002;&#x8FD9;&#x4E2A;&#x4E1A;&#x52A1;&#x610F;&#x4E49;&#x4E0E;&#x524D;&#x9762;&#x7684;&#x5546;&#x54C1;&#x7F16;&#x53F7;&#x4E0D;&#x540C;&#x3002;</p>
      </td>
      <td style="text-align:left"><b>53462</b>
      </td>
    </tr>
  </tbody>
</table>### 3.其他特殊情况

* 取到的查询条件是b=时，则传 N/A，与现在一致；
* 查询条件里有 b=1&b=2 ，取 b 的值为 2 ；
* URL 中存在 ?a=1\#?b=2 ，第一个?是查询条件，即查询条件为a=1，第一个\#是hash，即hash（路径中的一部分）为?b=2；
* 解析移动端的数据；



\*\*\*\*

