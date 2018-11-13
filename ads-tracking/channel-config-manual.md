# 渠道配置指南

* [1.今日头条](channel-config-manual.md#31)
* [2. 广点通](channel-config-manual.md#32)
* [3. 爱奇艺](channel-config-manual.md#33)
* [4. 百度原生信息流](channel-config-manual.md#34)
* [5. Inmobi](channel-config-manual.md#35)
* [6. 微博超级粉丝通](channel-config-manual.md#36)

当前已对接精准匹配渠道有：

个推、今日头条、百度原生信息流、多盟、360点睛广告平台、uc（阿里汇川）、优酷、广点通、微信广告平台、爱奇艺、InMobi、小米、网易有道智选、巨掌积分墙、知乎广告平台、美图、陌陌、微博粉丝通、作业帮

当前已对接模糊匹配渠道：

时趣互动、趣米、腾讯视频、Adwords、百度搜索、腾讯新闻、Admob、百度DSP、畅思、艾德思齐、Vungle、搜狐视频、力美、chartboost、点入、百度联盟、新浪视频、搜狗搜索、Tapjoy、趣头条 Aiclk、神马搜索、Mobvista、Adconly、百思不得姐、勤诚互动、品友互动、小米互娱、斗鱼TV、快手、芋儿拍

（未出现在上述渠道名单的渠道可以在渠道管理功能中新增配置）

### 1. 今日头条 <a id="31"></a>

1. 进入GIO后台，为今日头条的投放生成一条监测链接。进入“渠道管理”模块，点击“今日头条”，“是否回传激活”字段设置为“是”。

![](https://docs.growingio.com/.gitbook/assets/9.png)

1. 进入头条后台，“工具箱”。新建“转化跟踪”。
2. 进行移动应用APP API转化新建。输入应用下载地址，比如APP的iTunes下载地址。
3. 配置完成后，请按照今日头条自动联调流程完成后续过程，联调成功后进行计划引用。

![](https://docs.growingio.com/.gitbook/assets/10%20%281%29.png)

### 2. 广点通 <a id="32"></a>

通过授权认证的方式配置您的腾讯社交广告平台投放监测

1.填写广点通后台投放账号（qq号）

2.填写投放账号id

地址：e.qq.com

3.进行授权认证

4.填写应用id（Android为应用宝应用id，iOS为苹果应用id）

5.保存并联调测试

a.进入工具箱页面，点击转化跟踪

b.点击“获取点击数据”按钮

c.选择移动平台，输入移动应用ID

d.将Ad Tracking后台生成的feedback URL填至此处

e.点击提交

f.输入联调设备进行联调

g.联调成功后，启用转化状态

![](../.gitbook/assets/image%20%28151%29.png)

![](../.gitbook/assets/image%20%2838%29.png)

### 3. 爱奇艺 <a id="33"></a>

1. 在GIO后台生成一条目标渠道为“爱奇艺”的监测链接。
2. 将链接放置到爱奇艺广告后台，创意部分的“第三方点击监测”上。

   ![](https://docs.growingio.com/.gitbook/assets/13.png)

### 4. 百度原生信息流 <a id="34"></a>

1.在GIO后台生成百度原生信息流监测链接后，进入百度推广后台：[https://www2.baidu.com](https://www2.baidu.com)

2.进入后台后选择“数据中心”

3.进入数据中心后选择“信息流推广”

4.在“工具中心”里面选择“转化追踪工具”，然后点击“新建转化”

5.选择“应用下载”

6.进入该页面后把GIO生成的百度信息流的链接填写到“监测地址”，里面将该页面的“akey值“”填写到GIO后

7.点击提交后，手机下载“百度推广APP”，在手机上用百度推广APP扫描图片上的二维码下载您的APP进行联调

8.下载完，打开后APP点击下一步

9.联调成功

![](../.gitbook/assets/image%20%2856%29.png)

![](../.gitbook/assets/image%20%2863%29.png)

### 5. Inmobi <a id="35"></a>

1. 进入Growingio广告监测后台，进入推广管理，创建监测并选取Inmobi渠道，生成监测链接。
2. 进入Inmobi后台，创建主张，选择GrowingIO渠道，创建成功后，将Inmobi生成的appID复制并填写到GrowingIO后台相应链接上。

   ![](https://docs.growingio.com/.gitbook/assets/inmobi1.png)

   ![](https://docs.growingio.com/.gitbook/assets/inmobi2.png)

### 6. 微博超级粉丝通 <a id="36"></a>

1.在GIO后台生成一条目标渠道为“超级粉丝通”的监测链接。

2.将监测链接填写到微博超级粉丝通后台

![](https://docs.growingio.com/.gitbook/assets/%E8%B6%85%E7%BA%A7%E7%B2%89%E4%B8%9D%E9%80%9A1.png)

3.将微博超级粉丝通后台生成的“company” 字段复制到GrowingIO后台

### 

