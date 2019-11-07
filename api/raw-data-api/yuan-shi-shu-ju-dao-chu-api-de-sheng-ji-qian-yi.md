# 原始数据导出 API 的升级迁移

## 原始数据导出 2.0 和原始数据导出 1.0 主要区别

#### 接口请求 URL <a id="&#x63A5;&#x53E3;&#x8BF7;&#x6C42;-url"></a>

原始数据导出 1.0 接口格式：

`https://www.growingio.com/insights/:ai/:date.json`

原始数据导出 2.0 接口格式：

`https://www.growingio.com/v2/insights/{export_type}/{data_type}/{ai}/{export_date}.json`

#### 接口响应数据格式 <a id="&#x63A5;&#x53E3;&#x54CD;&#x5E94;&#x6570;&#x636E;&#x683C;&#x5F0F;"></a>

原始数据导出 1.0 接口返回数据格式：

```text
{
  "status":"FINISHED",
  "downlinks":[
    "link1",
    "link2"
  ]
}
```

原始数据导出 2.0 接口返回数据格式：

```text
{
  "status”:"FINISHED",
  "downloadLinks":[
    "link1",
    "link2"
  ],
  "exportType":"",
  "dataType":"",
  "exportDate":"",
  "exportVersion":"",
  "requestTime":"",
  "errorMsg":""
}
```

#### 数据文件的获取方式和组织 <a id="&#x6570;&#x636E;&#x6587;&#x4EF6;&#x7684;&#x83B7;&#x53D6;&#x65B9;&#x5F0F;&#x548C;&#x7EC4;&#x7EC7;"></a>

原始数据导出 1.0

1. 按天和小时的维度区分获取数据，一次获取所有类型的数据
2. 天数据一次返回所有7种类型当天的7个数据文件
3. 小时数据一次返回所有7种类型当天特定小时的7个数据文件

原始数据导出 2.0

1. 按导出类型（天或小时）和数据类型（v2共9种）区分获取数据，一次获取一种类型特定时间的数据
2. 天数据返回特定日期所有小时的数据文件
3. 小时数据根据确定的数据类型和时间获取确定类型确定小时的1个数据文件

## 原始数据导出提供的数据类型

原始数据导出 1.0 提供的数据类型：

visit、page、action、action\_tag、custom\_attr、ads\_track\_click、ads\_track\_activation一共7种类型的数据

原始数据导出 2.0 提供的数据类型：

visit、page、action、action\_tag、custom\_event（原custom\_attr）、ads\_track\_click、ads\_track\_activation、pvar（新增）、evar（新增）一共9种类型的数据

#### 数据格式变化 <a id="&#x6570;&#x636E;&#x683C;&#x5F0F;&#x53D8;&#x5316;"></a>

visit：新增字段，部分字段名称变化

page：新增字段，部分字段名称变化

action：部分字段名称变化

action\_tag：字段名称变化

ads\_track\_activation：部分字段名称变化

custom\_event：原custom\_attr，新增字段，部分字段名称变化

pvar：新增类型

evar：新增类型

## 如何升级原始数据导出功能

如果您目前使用的API版本是1.0，想要升级为2.0，请联系客户成功经理。如果您已经从1.0升级到了2.0，请注意如下事项：

* 从API2.0接口开通那一刻起，API1.0接口依旧可以继续使用，但是**`一个月`**之后就会失效，请您在过渡期内尽快升级您的使用代码。
* API2.0接口从开通的那一刻起就开始生效，比如您于`2018-08-01 10:30:00`升级到了API2.0接口，那么API2.0接口就可以下载`2018-08-01 11:00:00`之后的数据了，但是之前的数据还需要通过API1.0去获取。



