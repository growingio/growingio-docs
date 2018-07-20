# 小程序圈选

* [页面指标定义](mina-circle.md#ye-mian-zhi-biao-ding-yi)
* [行为指标定义](mina-circle.md#hang-wei-zhi-biao-ding-yi)

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

