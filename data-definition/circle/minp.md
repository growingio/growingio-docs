# 小程序数据定义

推荐产品、市场、运营等非技术用户使用“交互式数据定义”进行数据定义；推荐产品、技术用户，使用“按路径和函数名称进行数据定义”来定义数据。

* [按路径和函数名称进行数据定义](minp.md#define-data-by-tagging-name)
  * [页面指标定义](minp.md#ye-mian-zhi-biao-ding-yi)
  * [行为指标定义](minp.md#hang-wei-zhi-biao-ding-yi)
* [交互式数据定义](minp.md#interactive-auto-data-define)
  * [交互式数据定义功能使用](minp.md#jiao-hu-shi-shu-ju-ding-yi-gong-neng-shi-yong)
  * [无埋点定义事件指标的管理](minp.md#wu-mai-dian-ding-yi-de-shi-jian-zhi-biao-de-guan-li)
  * [使用无埋点定义的指标](minp.md#shi-yong-quan-xuan-ding-yi-de-zhi-biao)
* [定义页面级变量](minp.md#define-page-dimensions)
  * [页面级变量使用的场景](minp.md#ye-mian-ji-bian-liang-shi-yong-de-chang-jing)

## 按路径和函数名称进行数据定义 <a id="define-data-by-tagging-name"></a>

按路径和函数名称进行数据定义，主要方便产品、技术人员，来根据页面和元素代码名称定义页面浏览和元素点击事件。

### 页面指标定义

进入无埋点数据定义功能页面后，显示的是所有自动采集到的页面。在这个页面，你可以看到以下信息：

1. 当前要定义事件的小程序的产品名
2. 当前要定义事件的小程序的 AppID
3. 当前要定义事件的小程序的过去 7 天数据表现
4. 当前要定义事件的小程序自动采集到的页面列表及其数据

![&#x5C0F;&#x7A0B;&#x5E8F;&#x9875;&#x9762;](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LFBrLMQ6Br7wdTsoufB%2F-LFBwCOk9pJuFNhJ7T5u%2F%E5%B0%8F%E7%A8%8B%E5%BA%8F%E9%A1%B5%E9%9D%A2%E5%9C%88%E9%80%89%E4%BB%8B%E7%BB%8D.png?alt=media&token=5b1c37ea-48a4-4ddc-a4cd-650c6df009dc)

从上图的样例可以看到，GrowingIO 小程序自动采集到 6 个页面，每个页面的具体访问趋势显示在列表内。如果要定义某个页面为指标用于后续分析，点击“**定义页面**“按钮，然后可以看到弹出框，如下图。

![&#x5C0F;&#x7A0B;&#x5E8F;&#x5B9A;&#x4E49;&#x9875;&#x9762;](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LFBrLMQ6Br7wdTsoufB%2F-LFCIlgMuUd8kMMlyBIc%2F%E5%9C%88%E9%80%89%E9%A1%B5%E9%9D%A2%E7%BB%86%E8%8A%82.png?alt=media&token=5b245215-0878-4209-bef8-77e2ba4a9864)

输入页面名称，点击保存，就定义好了一个页面指标。之后，就可以在分析工具里面使用这个指标了。

这里值得注意的是元素点击分布，显示的数据是在这个页面，行为被点击/输入的次数，可以理解成为简易的交互热图分布。

点击具体页面的容器区域，就会进入到该页面的行为指标定义页面，具体见下一节。

### 行为指标定义

点击某个页面后，可以进一步定义这个页面上的行为，显示的是该页面上所有自动采集到的行为元素。在这个页面，你可以看到以下信息：

1. 当前小程序页面的页面名称
2. 当前小程序页面的页面路径
3. 当前小程序页面的过去 7 天数据表现
4. 当前小程序页面自动采集到的行为列表及其数据

![&#x5C0F;&#x7A0B;&#x5E8F;&#x9875;&#x9762;&#x5185;&#x884C;&#x4E3A;&#x5C55;&#x793A;](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LFCqNxsL3IBFp-oAtuG%2F-LFD2JJj8RC0hIy797uZ%2F%E5%B0%8F%E7%A8%8B%E5%BA%8F%E8%A1%8C%E4%B8%BA%E5%9C%88%E9%80%89.png?alt=media&token=ff416bc8-3192-40df-aabb-9599d663834b)

 从上图的样例可以看到，GrowingIO 小程序在榜单页面自动采集到 2 个行为，每个行为的具体点击趋势显示在列表内。如果要定义某个行为为指标用于后续分析，点击“**定义行为**“按钮，然后可以看到弹出框，如下图。

![&#x5C0F;&#x7A0B;&#x5E8F;&#x5B9A;&#x4E49;&#x884C;&#x4E3A;](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LFCqNxsL3IBFp-oAtuG%2F-LFD59ZE5It-ZMFZ3yJs%2F%E5%9C%88%E9%80%89%E8%A1%8C%E4%B8%BA%E7%BB%86%E8%8A%82.png?alt=media&token=b8b3dc50-6138-4ff7-8b93-96ff8d4b05cf)

输入行为名称，点击保存，就定义好了一个行为指标。

**这里值得注意**的是元素内容分布和元素位置分布，显示的数据是在这个页面，行为被点击/输入时，对应的内容和位置的交互热图分布。在[无埋点采集事件](./)里介绍了，如果在视图里使用了 data-title 和 data-index 这种声明式编程，就能在行为数据里看到这些数据。

之后，就可以在分析工具里面使用这个事件了，同时可以在**数据管理-无埋点事件管理**中管理这个事件指标。

![](../../.gitbook/assets/image%20%2893%29.png)

## 交互式数据定义 <a id="interactive-tagging-define-data"></a>

  
交互式数据定义，主要帮助产品、运营、市场等非技术人员，来根据业务含义定义页面浏览和元素点击事件。

### 交互式数据定义功能使用

进入无埋点数据定义功能后，点击“交互式定义”入口按钮。​

![](../../.gitbook/assets/image%20%2838%29.png)

进入交互式定义页面：step1:请扫码将手机IP加入;step2:打开微信小程序。

就能看到微信头像出现在下方。（如果没有显示微信头像，是因为没有授权openid，请[查看SDK配置](../../sdk-integration/mina-sdk.md#sdk-wei-xin-yong-hu-shu-xing-she-zhi)）

![](../../.gitbook/assets/image%20%2832%29.png)

选择自己的头像（设备），点击“开始定义”。

在这个页面中，在微信小程序上实时操作，你的页面浏览或者元素点击事件的行为流会实时的显示在页面上。如果这个行为指标已被定义了，就会显示已定义标识和指标的名称。

![](../../.gitbook/assets/image%20%286%29.png)

鼠标移到元素所在行右侧，会出现“定义”的按钮。

![&#x5B9A;&#x4E49;&#x6309;&#x94AE;](../../.gitbook/assets/image%20%2853%29.png)

点击“定义元素”，会出现这个元素信息的弹窗。

![&#x5B9A;&#x4E49;&#x5143;&#x7D20;&#x5F39;&#x7A97;&#x793A;&#x610F;](../../.gitbook/assets/image%20%28141%29.png)

其中如果元素存在内容和位置的属性，就会展示“元素内容分布”和“元素内容分布”的信息，帮助你了解这个点击事件代表采集的具体数据。如示例中，点击了“电影列表”这个元素，GrowingIO采集到了这个列表元素，以及元素的内容属性和位置属性（为什么我没有看到元素的内容和属性数据？可能是元素代码信息标注不完全，详细请见[小程序SDK高级配置](../../sdk-integration/mina-sdk.md#sdk-gao-ji-she-zhi-shu-ju-cai-ji-pei-zhi)）。

行为名称，我们会自动抓取元素的某个属性，当然更建议修改为您便于理解和使用的名称（如果您集成了多个项目，建议您可以采用以下的命名定义元素： “小程序名称\_页面\_元素功能”）。

给行为事件定义名称，然后点击保存。这个事件就被定义为指标了。​

![&#x547D;&#x540D;&amp;&#x4FDD;&#x5B58;](../../.gitbook/assets/image%20%28104%29.png)

同理定义一个页面，点击“定义页面”，展示出定义页面的弹窗，“元素点击分布”tab上，展示这个页面上所有元素的点击分布，如果页面上没有可点击的元素，则显示为空。给页面定义一个名称（如果您集成了多个项目，建议您可以采用以下的命名建议：小程序名称\_页面“来命名页面），保存。

定义的内容会自动变成“已定义”的标识，并展示这个指标的名称。

![](../../.gitbook/assets/image%20%2883%29.png)

### 无埋点定义的事件指标的管理

定义完后的指标，会出现在“数据管理-无埋点事件管理“功能页面中。可以根据创建人、以及右上角的快速筛选等信息的查看。

![](../../.gitbook/assets/image%20%28120%29.png)

### 使用无埋点定义的指标

事件会自动出现在**”事件分析“、”漏斗分析”、“留存分析”** 的**“选择指标-自定义指标-我的指标分类”** 中，或者可以直接使用**搜索**进行查找。可以选择已定义的指标，进行进一步的数据分析。

![&#x4E8B;&#x4EF6;&#x5206;&#x6790;&#x9009;&#x62E9;&#x6307;&#x6807;&#x793A;&#x4F8B;](../../.gitbook/assets/image%20%28148%29.png)

## 定义页面级变量

在定义页面事件时，会出现一个 "页面级变量"tab。 这个功能是专门针对小程序开放的功能，主要满足快速定义页面属性，并根据页面属性进行行为分析的需求。

例如一个电影查询的APP，电影列表页的页面路径是 pages/list/list ，电影详情页的页面路径pages/item/item，其中电影列表页有多个类型，电影类型分为：动作，爱情，喜剧，其中分别展示Top200, Top100，Top10；电影详情页有多个类型，电影类型分为：动作，爱情，喜剧，其中展示每个具体的电影信息，具体如下

| **页面** | 具体页面信息 | **页面详细路径** |
| :--- | :--- | :--- |
| 电影列表页 | 动作电影Top100 | pages/list/list?type=action&title=top100 |
| 电影列表页 | 爱情电影Top200 | pages/list/list?type=love&title=top200 |
| 电影列表页 | 喜剧电影Top10 | pages/list/list?type=comedy&title=top10 |
| 电影详情页 | 玩具总动员（动画片） | pages/item/item?id=1001&type=animation |
| 电影详情页 | 霸王别姬（剧情片） | pages/item/item?id=1005&type=drama |

在定义“浏览电影列表页事件”的时候，可以定义页面路径 pages/list/list 的页面 ；进一步，想了解具体浏览的是什么类型和分类时，就可以通过“页面级变量” 的功能来实现定义。

点击“页面级变量”tab，GrowingIO会自动根据页面查询条件（页面路径？后的内容），抓取可定义的key（标识符），已被定义过的页面级变量，会被标记为已定义，且不能再次被定义；未被定义的标识，会显示“立刻定义”。

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LIjrnMeRJp_VAJty9Bq%2F-LIkKu23D79bXIAVYf-x%2Fimage.png?alt=media&token=86726fb0-bb63-467b-bfdf-9cbca3721757)

点击立即定义后，给标识符取一个变量名称，点击“确定”，即保存成了一个可用的页面级变量，并且在**全局生效**。

全局生效的含义是：type同时会对有type这个标识的电影详情页生效，即在定义电影详情页时，页面级变量type会自动标记为被定义。

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LIjrnMeRJp_VAJty9Bq%2F-LIkQu0XIrmbN5-5QfEI%2Fimage.png?alt=media&token=7bab3e2a-aaf1-42f1-94d8-5aee46c37d74)

### **页面级变量使用的场景**

分析场景示例：快速分析商品的转化率：

常用于商品详情页、搜索结果页等由同一个模板\(类\)填充的多个页面，以便区分这些页面间不同的业务含义。例如商品详情页可用页面级变量来标记：商品名称、物品ID、品类、价格等信息。如图示：![](https://docs.growingio.com/.gitbook/assets/ping-mu-kuai-zhao-20180104-xia-wu-2.05.17.png)​

在按上图所示，为所有商品详情页定义上页面级变量ID的标签后，在GrowingIO中，ID这个页面级变量均会成为“维度”，可在各事件分析、漏斗分析、留存分析中选用。例如在单图中，即可按物品ID来分解页面浏览量。

页面级变量同时可以拆分页面上的定义的点击事件：商品详情页上面常见会存在“加入购物车”、“确定购买”按钮，定义“加入购物车”&“确定购买”，这个行为指标同时可以使用这个点击元素所在页面的页面级变量做维度拆分。

事件分析选择指标&维度

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LIjrnMeRJp_VAJty9Bq%2F-LIkWGYi7nN443_56gai%2Fimage.png?alt=media&token=828f7faa-57f7-40bb-a3ea-843e48baef17)

图表呈现结果

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LIjrnMeRJp_VAJty9Bq%2F-LIkWDbANkvuUm54VSMt%2Fimage.png?alt=media&token=f608bf79-4274-4c84-b715-f324f3097110)

这样，就可以快速满足分析各种分类下的用户行为指标。

