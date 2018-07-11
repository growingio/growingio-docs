# GrowingIO Debugger

* [GrowingIO Web Debugger](growingio-debugger.md#growingio-web-debugger)
  * [Web Debugger 安装](growingio-debugger.md#web-debugger-an-zhuang)
  * [使用 Web Debugger 测试数据](growingio-debugger.md#shi-yong-web-debugger-ce-shi-shu-ju)
* [GrowingIO Mobile Debugger](growingio-debugger.md#growingio-mobile-debugger)
  * [启动Mobile Debugger ](growingio-debugger.md#qi-dong-mobile-debugger)
  * [使用 Moible Debugger 测试数据](growingio-debugger.md#shi-yong-mobile-debugger-ce-shi-shu-ju)

GrowingIO Debugger是GrowingIO推出的调试 SDK所发送数据的工具。在GrowingIO Debugger的帮助下，实施工程师可以看到在什么样的页面上，在什么时机向GrowingIO发送了什么样的服务器请求。

在数据驱动增长的实践中，数据准确性一直以来是一个必须重视的课题。数据分析行业中有一句谚语：Garbage In, Garbage Out，简称GIGO。这句谚语告诉我们：如果我们在实施阶段的方案有纰漏导致收集的数据就是错误的，那么基于错误的数据得出的结论就一定是有问题的，所以我们必须重视GrowingIO产品的实施工作，来确保实施方案的严谨正确，从而保证收集到的数据是尽最大可能准确的。

而在GrowingIO产品的实施过程中，GrowingIO Debugger能够帮助工程师、技术实施顾问清楚的看到发送出去的服务器请求是什么？发送的时机是什么？数据是不是按照实施方案中所描述的那样进行发送。有了GrowingIO Web Debugger的帮助，实施工程师可以通过高质量的实施，给分析师带来高质量高准确性的数据，这将大大有利于在企业内部养成数据驱动决策的文化和习惯。使用数据，使用高质量的数据帮助企业获得真正的增长。

### GrowingIO Web Debugger

####  Web Debugger 安装

GrowingIO Web Debugger 是基于Chrome浏览器的一个插件，用户需要按照标准Chrome浏览器插件的步骤和方式来按照。具体的步骤如下：

1、下载 [growingio\_web\_debugger.zip](http://assets.growingio.com/growingio_web_debugger.zip) 到本地磁盘，并完成解压缩到一个目录下。

![](https://docs.growingio.com/.gitbook/assets/webdebuggerinstall2.png)

2、打开Chrome浏览器，点击右侧的自定义及控制Google Chrome，在弹出的窗口中点击更多工具-&gt;扩展程序选项

3、进入到扩展程序管理界面后，勾选开发者模式选项框，然后点击加载已解压的扩展程序按钮，并在弹出的对话框中找到GrowingIO Web Debugger解压后的地址，点击确定。GrowingIO Web Debugger将会作为Chrome的扩展程序安装完毕。如下图

![](https://docs.growingio.com/.gitbook/assets/webdebuggerinstall3.png)

4、注意观察浏览器右上角的扩展程序图标如果出现如下图所示的图标，则表示GrowingIO Web Debugger安装成功!

![](https://docs.growingio.com/.gitbook/assets/image%20%283%29.png)

#### 使用 Web Debugger 测试数据

在添加了GrowingIO Web SDK的页面上，在空白处点击右键-&gt;检查打开Chrome浏览器的Dev Tool，可以看到出现了一个新的Tab为：GIO Web Debugger。实施技术顾问就可以在这个Tab页面上进行添加代码后的质量验证工作了。如下图

![](https://docs.growingio.com/.gitbook/assets/webdebuggerinstall5.png)

在此概览中可能出现的数据日志含义分别有：

page：页面浏览数据

vst：用户访问数据

clck：点击行为数据

chng：输入框行为

sbmt：表单提交行为

cstm：“自定义事件+事件级变量” 数据

pvar：“页面级变量” 数据

evar：“转化变量” 数据

ppl：“用户变量” 数据

###  GrowingIO Mobile Debugger

#### 启动Mobile Debugger

**第一步、进入Mobile Debugger启动页**

在右上侧的项目管理中选择Mobile Debugger，进入Mobile Debugger启动页，如下图

![Mobile Debugger&#x5165;&#x53E3;](https://docs.growingio.com/.gitbook/assets/image%20%289%29.png)

![Mobile Debugger&#x542F;&#x52A8;&#x9875;](https://docs.growingio.com/.gitbook/assets/image%20%286%29.png)

**第二步、扫码唤起APP**

1. 选择项目中需要进行测试的应用，并保证手机中已经安装该APP，且该APP已经集成GrowingIO 2.2.0及以上的SDK。
2. 使用手机浏览器扫描入口的二维码唤起Debug的APP，需要注意微信中扫码无法唤起APP。

#### 使用 Mobile Debugger 测试数据

在唤起Debug的APP后，该APP采集的行为数据以及当前页面截图就会出现在网页上，测试同学可以根据数据看数据的采集以及发送情况，对数据进行测试。

![Debugger&#x5DE5;&#x4F5C;&#x53F0;](https://docs.growingio.com/.gitbook/assets/image%20%285%29.png)

在此概览中可能出现的数据日志含义分别有：

page：页面浏览数据

vst：用户访问数据

clck：点击行为数据

chng：输入框行为数据

cstm：“事件以及关联的事件级变量” 数据

pvar：“页面级变量” 数据

evar：“转化变量” 数据

ppl：“用户变量” 数据

imp（元素浏览数据）数据量级过大，影响Mobile Debugger性能，Mobile Debugger不展示这部分数据。

### GrowingIO 小程序 Debugger

数据校验Debugger功能，可以支持用户根据自己的交互体验，实时看到GrowingIO采集的数据，完成数据收集的校验。

在完成SDK集成，点击“去检测”后，会跳转进入小程序Debugger的页面。进入Debugger页面后：

1. 保证操作集成小程序SDK的手机和登录GrowingIO的电脑处在同一网路环境下。
2. 打开微信，进入集成了小程序SDK的小程序中。
3. 等待5秒左右，可以看到用户显示在页面中。

![](../.gitbook/assets/image%20%286%29.png)

在小程序Debugger页面，会展示目前接入Debugger实时传输数据的小程序用户，以及部分用户微信信息、设备、操作系统的信息。但是部分信息的展示，需要在SDK中设置微信用户属性设置。详情请见 SDK 微信用户属性设置。

![](../.gitbook/assets/image%20%284%29.png)

  
选择自己的用户头像，点击“下一步”，进入用户行为记录页面。

和小程序产生交互（访问、页面浏览、点击等），即在此页面可以看到数据行为的记录。

![](../.gitbook/assets/2018-07-10-23.04.19.gif)

