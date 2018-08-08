# 小程序圈选

* [页面指标定义](minp.md#ye-mian-zhi-biao-ding-yi)
* [圈选定义页面级变量](minp.md#define-page-dimensions)
* [行为指标定义](minp.md#hang-wei-zhi-biao-ding-yi)

## 页面指标定义

进入圈选页面后，显示的是所有自动采集到的页面。在这个页面，你可以看到以下信息：

1. 当前要圈选的小程序的产品名
2. 当前要圈选的小程序的 AppID
3. 当前要圈选的小程序的过去 7 天数据表现
4. 当前要圈选的小程序自动采集到的页面列表及其数据

![&#x5C0F;&#x7A0B;&#x5E8F;&#x9875;&#x9762;&#x5708;&#x9009;](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LFBrLMQ6Br7wdTsoufB%2F-LFBwCOk9pJuFNhJ7T5u%2F%E5%B0%8F%E7%A8%8B%E5%BA%8F%E9%A1%B5%E9%9D%A2%E5%9C%88%E9%80%89%E4%BB%8B%E7%BB%8D.png?alt=media&token=5b1c37ea-48a4-4ddc-a4cd-650c6df009dc)

从上图的样例可以看到，GrowingIO 小程序自动采集到 6 个页面，每个页面的具体访问趋势显示在列表内。如果要定义某个页面为指标用于后续分析，点击“**定义页面**“按钮，然后可以看到弹出框，如下图。

![&#x5C0F;&#x7A0B;&#x5E8F;&#x5B9A;&#x4E49;&#x9875;&#x9762;](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LFBrLMQ6Br7wdTsoufB%2F-LFCIlgMuUd8kMMlyBIc%2F%E5%9C%88%E9%80%89%E9%A1%B5%E9%9D%A2%E7%BB%86%E8%8A%82.png?alt=media&token=5b245215-0878-4209-bef8-77e2ba4a9864)

输入页面名称，点击保存，就定义好了一个页面指标。之后，就可以在分析工具里面使用这个指标了。

这里值得注意的是元素点击分布，显示的数据是在这个页面，行为被点击/输入的次数，可以理解成为简易的交互热图分布。

点击具体页面的容器区域，就会进入到该页面的行为指标定义页面，具体见下一节。

## 圈选定义页面级变量 {#define-page-dimensions}

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

### **页面级变量使用的场景** {#page-dimensions-analysis-scene}

分析场景示例：快速分析商品的转化率：

常用于商品详情页、搜索结果页等由同一个模板\(类\)填充的多个页面，以便区分这些页面间不同的业务含义。例如商品详情页可用页面级变量来标记：商品名称、物品ID、品类、价格等信息。如图示：![](https://docs.growingio.com/.gitbook/assets/ping-mu-kuai-zhao-20180104-xia-wu-2.05.17.png)​

在按上图所示，为所有商品详情页定义上页面级变量ID的标签后，在GrowingIO中，ID这个页面级变量均会成为“维度”，可在各事件分析、漏斗分析、留存分析中选用。例如在单图中，即可按物品ID来分解页面浏览量。

页面级变量同时可以拆分页面上的定义的点击事件：商品详情页上面常见会存在“加入购物车”、“确定购买”按钮，定义“加入购物车”&“确定购买”，这个行为指标同时可以使用这个点击元素所在页面的页面级变量做维度拆分。![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LIjrnMeRJp_VAJty9Bq%2F-LIkWGYi7nN443_56gai%2Fimage.png?alt=media&token=828f7faa-57f7-40bb-a3ea-843e48baef17)事件分析选择指标&维度![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LIjrnMeRJp_VAJty9Bq%2F-LIkWDbANkvuUm54VSMt%2Fimage.png?alt=media&token=f608bf79-4274-4c84-b715-f324f3097110)图表呈现结果

这样，就可以快速满足分析各种分类下的用户行为指标。

## 行为指标定义

进入页面的行为圈选页面后，显示的是该页面所有自动采集到的行为页面。在这个页面，你可以看到以下信息：

1. 当前小程序页面的页面名称
2. 当前小程序页面的页面路径
3. 当前小程序页面的过去 7 天数据表现
4. 当前小程序页面自动采集到的行为列表及其数据

![&#x5C0F;&#x7A0B;&#x5E8F;&#x9875;&#x9762;&#x5185;&#x884C;&#x4E3A;&#x5708;&#x9009;](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LFCqNxsL3IBFp-oAtuG%2F-LFD2JJj8RC0hIy797uZ%2F%E5%B0%8F%E7%A8%8B%E5%BA%8F%E8%A1%8C%E4%B8%BA%E5%9C%88%E9%80%89.png?alt=media&token=ff416bc8-3192-40df-aabb-9599d663834b)

 从上图的样例可以看到，GrowingIO 小程序在榜单页面自动采集到 2 个行为，每个行为的具体点击趋势显示在列表内。如果要定义某个行为为指标用于后续分析，点击“**定义行为**“按钮，然后可以看到弹出框，如下图。

![&#x5C0F;&#x7A0B;&#x5E8F;&#x5B9A;&#x4E49;&#x884C;&#x4E3A;](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LFCqNxsL3IBFp-oAtuG%2F-LFD59ZE5It-ZMFZ3yJs%2F%E5%9C%88%E9%80%89%E8%A1%8C%E4%B8%BA%E7%BB%86%E8%8A%82.png?alt=media&token=b8b3dc50-6138-4ff7-8b93-96ff8d4b05cf)

输入行为名称，点击保存，就定义好了一个行为指标。

**这里值得注意**的是元素内容分布和元素位置分布，显示的数据是在这个页面，行为被点击/输入时，对应的内容和位置的交互热图分布。在[无埋点采集事件](./)里介绍了，如果在视图里使用了 data-title 和 data-index 这种声明式编程，就能在行为数据里看到这些数据。

之后，就可以在分析工具里面使用这个指标了，同时可以在**数据管理-圈选指标**中管理这个指标。

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LGyovQeg-F1vyUV53N6%2F-LGyp73W8d2ZjpMXS8hJ%2Fimage.png?alt=media&token=ca5e17d1-fd60-42ba-ae96-0daa5c314f09)

## 

