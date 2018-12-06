# 渠道配置指南

* [1.今日头条](channel-config-manual.md#31)
* [2. 广点通](channel-config-manual.md#32)
* [3. 爱奇艺](channel-config-manual.md#33)
* [4. 百度原生信息流](channel-config-manual.md#34)
* [5. Inmobi](channel-config-manual.md#35)
* [6. 微博超级粉丝通](channel-config-manual.md#36)
* [7. 腾讯社交广告](https://docs.growingio.com/ads-tracking/channel-config-manual#7-teng-xun-she-jiao-guang-gao-marketing-api)

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

![](../.gitbook/assets/image%20%28184%29.png)

![](../.gitbook/assets/image%20%2846%29.png)

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

![](../.gitbook/assets/image%20%2868%29.png)

![](../.gitbook/assets/image%20%2876%29.png)

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

### 7. 腾讯社交广告 Marketing api

腾讯社交广告（广点通），归因与展现方式为：

1\)在GrowingIO后台，创建“腾讯社交广告”监测链接，并绑定广点通账号，并进行相应的授权；

2\)GroiwngIO实时将激活数据发送给广点通，广点通返回激活信息，点击时间，广告名称等信息；

3\)GroiwngIO根据广点通返回的归因结果，按照广告主在热云后台的“推广参数”进行二次归因，并展示在GroiwngIO后台。  


1、在GIO后台创建Normal—Link链接，目标渠道选择“腾讯社交广告”，然后点击保存

![](../.gitbook/assets/image%20%28150%29.png)

2、点击保存后进入绑定与授权，

1）投放账号：填写对应的腾讯社交广告后台的登录账号（QQ号）。

2）投放账号ID：填写对应的腾讯社交广后台的账户ID，然后点击授权。

3）应用ID：填写您选取应用的对应ID，示例：IOS填写Appstore地址上面的数字。  


![](../.gitbook/assets/image%20%2897%29.png)

其中，“投放账号”和“投放账号ID”，与广点通后台相对应。

获取方式如下：

![](../.gitbook/assets/image%20%28172%29.png)

应用标识，请与广点通后台填写的“应用宝ID”或“苹果商店ID”保持一致。

* 图示为苹果商店ID获取方式。

![](../.gitbook/assets/image%20%2825%29.png)

3、点击授权之后再这里进行同意授权，同事返回GIO后台获取监测链接  


![](../.gitbook/assets/image%20%28141%29.png)

4、进入腾讯社交广告后台开始联调，在工具箱里面选择“转化跟踪”。

![](../.gitbook/assets/image%20%2893%29.png)

5、点击“获取点击数据”，按照图示在移动应用上填写应用ID，在feedback URL上填写GIO生成的监测链接。

![](../.gitbook/assets/image%20%2820%29.png)

6、开始联调。

![](../.gitbook/assets/image%20%2819%29.png)

7、填写对应的设备号。

![](../.gitbook/assets/image%20%2891%29.png)

8、联调成功，去Appstore或者应用宝下载您的APP。

![](../.gitbook/assets/image%20%2894%29.png)

