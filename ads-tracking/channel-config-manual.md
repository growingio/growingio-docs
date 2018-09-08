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

### 1. 今日头条 {#31}

1. 进入GIO后台，为今日头条的投放生成一条监测链接。进入“渠道管理”模块，点击“今日头条”，“是否回传激活”字段设置为“是”。

![](https://docs.growingio.com/.gitbook/assets/9.png)

1. 进入头条后台，“工具箱”。新建“转化跟踪”。
2. 进行移动应用APP API转化新建。输入应用下载地址，比如APP的iTunes下载地址。
3. 配置完成后，请按照今日头条自动联调流程完成后续过程，联调成功后进行计划引用。

![](https://docs.growingio.com/.gitbook/assets/10%20%281%29.png)

### 2. 广点通 {#32}

1. 进入GIO后台，为广点通的投放生成一条监测链接。进入“渠道配置”模块，点击“广点通”，“是否回传激活”字段设置为“是”。
2. 进入广点通后台，“工具箱”。创建移动应用转化
3. 填写具体内容，转化类别选择“激活”，转化方案选择“API方案一”。
4. FeedbackURL填写GIO后台分配的链接。
5. 完成创建转化后，记录您的账户ID，转化里的encrypt\_key以及sign\_key。将信息填写到GIO后台相应链接上。
6. 启用联调测试，按照流程进行联调。首先，点击“现在联调”。

![](https://docs.growingio.com/.gitbook/assets/11.png)

1. 然后填写设备号并提交：

![](https://docs.growingio.com/.gitbook/assets/12.png)

特别说明：  
对于安卓手机，键盘依次输入："\*\#06\#"会出现本机IMEI号。对于iOS设备，以关键词“IDFA”搜索AppStore后，下载相关应用，从应用里获取当前设备IDFA。

1. 完成以上步骤后，需要将转化规则的状态改为：启用。

### 3. 爱奇艺 {#33}

1. 在GIO后台生成一条目标渠道为“爱奇艺”的监测链接。
2. 将链接放置到爱奇艺广告后台，创意部分的“第三方点击监测”上。

   ![](https://docs.growingio.com/.gitbook/assets/13.png)

### 4. 百度原生信息流 {#34}

1.在GIO后台生成百度原生信息流监测链接后，进入百度推广后台：[https://www2.baidu.com](https://www2.baidu.com)

 2.进入后台后选择“数据中心”

![](https://lh3.googleusercontent.com/dsTdgXzFovIzhb04vFZ9rSeaelw9jg0r2YyY-MtsqyGgHeDsdwlibxRI93GxQwqZ83Wm8TNqsiVoo_JPSszLzv9DJV1Gz_385JsNxPfeyOTjJSslQmdchcVzr7nH7ilBDWUp7xAu)

3.进入数据中心后选择“信息流推广”

![](https://lh4.googleusercontent.com/5L1SLuNv_2_bPMvl-Y4Lg7GTci6ANhxI6B718creLjn1kiA04_iLTsrZvDyvn-C2fuGmU8fG12VTd5wz5eTPr_A18XZPC1i9wsogeHL5m9bkDHDwq99Y5WvdrtbHmNIHCpMzBCnw)

4.在“工具中心”里面选择“转化追踪工具”，然后点击“新建转化”

![](https://lh6.googleusercontent.com/wKAwoZHxzJhE8NfpHQUj56Q8dRoj37veZ2--0ocpyVyguHeqV5xuPpZ_lSrkMWxOKW4u6tE0LwjNLo1IH6uiCuowfv43SkvD8aDo3hDLXbfWYZ5ilL8BCIfy1lc6xleUq-QqQsTp)

5.选择“应用下载”

![](https://lh3.googleusercontent.com/_a3YZ2dJmlO0XF0mcNOQJbbuKHtnGR4O4A15hA4Ax9E20XlDx33vOmcT1-cdJJMdKkfh2cIWv2TTvcyUO1gJDOxBoZDgpnpDd7KXlrhvn9HYH_meWpEvEGzG3RFssO8jn4gDf9H-)

6.进入该页面后把GIO生成的百度信息流的链接填写到“监测地址”，里面将该页面的“akey值“”填写到GIO后台

![](https://lh4.googleusercontent.com/BPw7FK9ZncOKikNRx0LgGTIr8s47LajFyhJIz-1JFla0si7gkCnbQh5cI7IoMp77iFzk8lMZBezemMxHdFbuOYMja9XmOcr9Da7_HpdZweJTVNL_PWykTIxMH7bfmcOhxspGoNX_)

  
****7.点击提交后，手机下载“百度推广APP”，在手机上用百度推广APP扫描图片上的二维码下载您的APP进行联调  
****

![](https://lh4.googleusercontent.com/VeFB9NEP6JONFsYkKywu1jBS6KvezE6kicHNbypEg207y5vzm52hAmNtnKi0v2uwC55e_vYEwh9qH4p3bk77KGSCBiP9jmy9p9C6WIiQwTKct7AYc5zC5yKNsYE0QoXfo5iOU4iM)



8.下载完，打开后APP点击下一步

![](https://lh3.googleusercontent.com/HmHwrY9L7aEx7C2IpQpztIL_Kkx22x-lcmptJHSaCGq5EFetQm6tq6cx1n4LouzNxgUM_RkHiViEfFQxZbVwP1iVdT8SuuWwZzBRHYF-fOsdqAwKe4oIIxBMcbSJOkcg5HmHRO8q)

9.联调成功

![](https://lh6.googleusercontent.com/79d5flDHKw0Lswcc56fC2pyDfX2UNbc-k1PfRAHgr9NFVLQ3wazVkeWpebBAwCCVaRHUqJvnHj75KbtAmSXFuc35D2ao1q_CAHYvItmjhADu14lWouTfORjGQWvEIKZkaaeystCo)

### 5. Inmobi {#35}

1. 进入Growingio广告监测后台，进入推广管理，创建监测并选取Inmobi渠道，生成监测链接。
2. 进入Inmobi后台，创建主张，选择GrowingIO渠道，创建成功后，将Inmobi生成的appID复制并填写到GrowingIO后台相应链接上。

   ![](https://docs.growingio.com/.gitbook/assets/inmobi1.png)

   ![](https://docs.growingio.com/.gitbook/assets/inmobi2.png)

### 6. 微博超级粉丝通 {#36}

1.在GIO后台生成一条目标渠道为“超级粉丝通”的监测链接。

2.将监测链接填写到微博超级粉丝通后台

![](https://docs.growingio.com/.gitbook/assets/%E8%B6%85%E7%BA%A7%E7%B2%89%E4%B8%9D%E9%80%9A1.png)

3.将微博超级粉丝通后台生成的“company” 字段复制到GrowingIO后台

### 

