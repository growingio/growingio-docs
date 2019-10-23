# 爬虫规则

* [处理预定义爬虫数据的规则](bot-rule.md#1)
* [爬虫规则对数据分析工具的影响](bot-rule.md#2)

**爬虫规则只对Web平台发送过来的服务器请求生效。**

在网站集成了GrowingIO Web JS SDK 之后，用户访问网站的行为数据会以服务器请求的方式发送给 GrowingIO 的服务器，每一条服务器请求都会经过爬虫规则的判断，判断该服务器请求是否是预定义爬虫产生的。

### 1.处理预定义爬虫数据的规则 <a id="1"></a>

在下面表格中的爬虫为目前 GrowingIO 的预定义爬虫

![](../.gitbook/assets/image%20%28247%29.png)

### 2.对数据分析的影响 <a id="2"></a>

![](https://docs.growingio.com/.gitbook/assets/botruleimpactondatavisualizationtools.png)

GrowingIO 爬虫规则会过滤单图，漏斗流程，分群，细查，实时等数据工具中的数据。 

**但是出于性能等方面的考虑，目前爬虫规则并没有过回溯功能中的数据。**

