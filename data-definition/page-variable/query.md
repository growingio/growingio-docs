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

![&#x4E8B;&#x4EF6;&#x548C;&#x53D8;&#x91CF;](https://lh6.googleusercontent.com/A0rAnnpKQUnR0_mRW77uotOr7eQxB731Q61cvL3pIQe-PrZ_0qaez69oAWZ6Da5eKzVICJtvrS2vX934zQ9UhlHgkV0-Nenh_Oo1_9uMbjBJzj3Je8zRnUr_TJZOWIO4cfDbwDm1)

![web &#x5708;&#x9009;](../../.gitbook/assets/image%20%2812%29.png)

{% hint style="info" %}
遵循两个原则：

1.全局生效。一旦设置，全站采集，对域名下的所有加载了 SDK 的页面生效。

2.埋点优先。埋点页面的数据不受任何影响，只在没有埋点或延时导致埋点数据没有发出来时，会取与「标识符」相同的查询条件对应的值。
{% endhint %}

### **2.案例详解**

如果你之前进行了埋点，那么这个功能上线后会发生什么变化呢？

电商客户 S 为了分析商品的售卖情况，在详情页进行了埋点，将每个商品的编号、商品名称等设置成了页面级变量，已经实施后上线了，其中商品编号标识符为 id，是从内容中取值的。接下来，用户用这个页面级变量来拆分商品详情页，进行分析。

商品详情页的 URL 是这样的 [www.s.com/pro?id=342817&city=bj](http://www.s.com/pro?id=342817&city=bj) ，我们可以看到查询条件中的 id 就是这个商品的商品编号， **当这个功能上线后，由于「商品编号标识符： id」与「查询条件中的：id 」是一样的，我们会从打点设置的规则和 URL 中的查询条件中同时取 id 的值，**有如下几种情况：

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
      <th style="text-align:left">变化</th>
      <th style="text-align:left">埋点取的值</th>
      <th style="text-align:left">URL 取的值</th>
      <th style="text-align:left">之前</th>
      <th style="text-align:left">之后</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">第 1 个情况</td>
      <td style="text-align:left">
        <p>商品详情页1</p>
        <ul>
          <li>商品页内容中的值 342817</li>
          <li><a href="http://www.s.com/pro?id=342817&amp;city=bj">ww.s.com/pro?id=342817&city=bj</a>
          </li>
        </ul>
      </td>
      <td style="text-align:left">无变化</td>
      <td style="text-align:left"><b>342817</b>
      </td>
      <td style="text-align:left">342817</td>
      <td style="text-align:left">342817</td>
      <td style="text-align:left"><b>342817</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">第 2 个情况</td>
      <td style="text-align:left">
        <p>商品详情页1</p>
        <ul>
          <li>商品页内容中的值 ac487</li>
          <li><a href="http://www.s.com/pro?id=342817&amp;city=bj">ww.s.com/pro?id=342816&city=bj</a>
          </li>
        </ul>
      </td>
      <td style="text-align:left">无变化</td>
      <td style="text-align:left"><b>ac487</b>
      </td>
      <td style="text-align:left">342816</td>
      <td style="text-align:left">ac487</td>
      <td style="text-align:left"><b>ac487</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">第 3 个情况</td>
      <td style="text-align:left">
        <p>商品详情页2</p>
        <ul>
          <li>商品页内容中的值 342815 ，但是因为时间的缘故，没有发出来这个值</li>
          <li><a href="http://www.s.com/pro?id=342817&amp;city=bj">ww.s.com/pro?id=342815&city=bj</a>
          </li>
        </ul>
      </td>
      <td style="text-align:left">补了值</td>
      <td style="text-align:left">NA</td>
      <td style="text-align:left"><b>342815</b>
      </td>
      <td style="text-align:left">NA</td>
      <td style="text-align:left"><b>342815</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">第 4 个情况</td>
      <td style="text-align:left">
        <p>商品订单页1</p>
        <ul>
          <li>订单页没有打点，所以这里没有打点的值</li>
          <li><a href="http://www.s.com/pro?id=342817&amp;city=bj">ww.s.com/orderform</a>
            <a
            href="http://www.s.com/pro?id=342817&amp;city=bj">?id=53462&city=bj</a>
          </li>
        </ul>
      </td>
      <td style="text-align:left">补了值</td>
      <td style="text-align:left">NA</td>
      <td style="text-align:left">NA</td>
      <td style="text-align:left">
        <p><b>53462</b>
        </p>
        <p>⚠️：本来用户没有打点，拿不到数据，现在有了订单编号业务意义的数据。这个业务意义与前面的商品编号不同。</p>
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

