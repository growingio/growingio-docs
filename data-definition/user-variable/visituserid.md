# 访问用户变量

* [访问用户简介](visituserid.md#fang-wen-yong-hu-jian-jie)
* [配置和上传](https://growingio.gitbook.io/docs/~/edit/drafts/-LPyrvcsY8gb75CQCoOG/v/master/data-definition/user-variable/visituserid#pei-zhi-he-shang-chuan)
  * [第一步：在 “事件和变量”中完成配置](https://growingio.gitbook.io/docs/~/edit/drafts/-LPyrvcsY8gb75CQCoOG/v/master/data-definition/user-variable/visituserid#di-yi-bu-zai-shi-jian-he-bian-liang-zhong-wan-cheng-pei-zhi)
  * [第二步：代码部署](https://growingio.gitbook.io/docs/~/edit/drafts/-LPyrvcsY8gb75CQCoOG/v/master/data-definition/user-variable/visituserid#di-er-bu-dai-ma-bu-shu)
  * [第三步：数据校验](https://growingio.gitbook.io/docs/~/edit/drafts/-LPyrvcsY8gb75CQCoOG/v/master/data-definition/user-variable/visituserid#di-san-bu-shu-ju-xiao-yan)
* [用户变量的持久性范围](https://growingio.gitbook.io/docs/~/edit/drafts/-LPyrvcsY8gb75CQCoOG/v/master/data-definition/user-variable/visituserid#yong-hu-bian-liang-de-chi-jiu-xing-fan-wei)

用户变量用来保存跟用户本身相关的信息，例如用户的姓名，性别，会员卡等级等等。不建议在用户变量中保存跟交互行为相关的信息，例如当前正在浏览的商品，当前正在查看的某一个 SaaS 项目。这些随着用户的交互行为会发生变化的信息 GrowingIO 建议使用页面变量和转化变量来保存。

不同于登录用户变量，访问用户变量支持对在非登录状态下的访问用户进行标记，便于后续做用户信息的相关分析。在添加所需要设置的访问用户变量的代码之前，需要在 GrowingIO 产品的`自定义事件和变量`管理页面配置访问用户级变量。

当前 GrowingIO 访问用户变量支持 Web、APP、微信小程序等平台；

## 访问用户简介

访问用户是 GrowingIO 对访问您的应用（包括网页、App、微信小程序等，下同）的用户的一种识别机制。每一个访问您的应用的用户都会在对应的设备中生成并记录一个唯一的ID，我们称之为访问用户ID。

对于不同平台类型的应用，GrowingIO 提供了多种识别方案，从而尽可能的实现对用户的唯一标识。访问用户在不同平台的标识请参考[访问用户模型文档](../../data-model/user-model/visitor.md)。

## 配置和上传 <a id="pei-zhi-he-shang-chuan"></a>

### **第一步：在 “事件和变量”中完成配置** <a id="di-yi-bu-zai-shi-jian-he-bian-liang-zhong-wan-cheng-pei-zhi"></a>

梳理业务需求，确认需要上传的用户变量,请勿直接开始代码的部署,需要先到 GrowingIO 后台找到 【数据管理】-【事件和变量】- 【访问用户变量】功能，在其中完成对应的配置。

#### 自定义变量规范 <a id="zi-ding-yi-bian-liang-gui-fan"></a>

在具体的将自定义变量添加到网站或者移动应用上之前，GrowingIO 要求先在打点管理的界面上进行变量的声明操作，然后再开展具体的添加自定义变量的操作。在自定义变量的声明界面上需要输入下列对所有变量来说通用的信息：

#### **标识符** <a id="biao-shi-fu"></a>

* 标识符是代表某一个具体变量的一种具有**唯一性**要求的符号，标识符用来唯一的代表一个打点事件或者变量，即整个打点系统内部标识符在所有的变量和打点指标之间是唯一的。在新创建任何一种类型的变量的时候，这个新的变量的标识符不能跟任何一个已有变量和打点事件的标识符重复。在一个变量的标识符确定之后，在代码中就可以使用这个标识符来唯一代表该变量，请仔细检查确保声明的标识符和代码中使用标识符一致。如果出现了不一致的情况，GrowingIO 将视为没有收到声明了的变量的数据，而那个在代码中发送的跟声明中标识符不一样的变量的数据会被 GrowingIO 认为没有声明而丢弃。GrowingIO 虽然支持标识符的修改，但是不建议频繁修改，因为每一次标识符的修改都需要在声明界面和代码上同时作出修改，以保证标识符的一致性。
* 标识符的设置细节规则如下
  * 自定义事件和所有自定义变量类型的标识符整体范围内不允许重名
  * 标识符仅允许大小写英文、数字、下划线、以及英文冒号，并且不能以数字和冒号开
  * 标识符的长度限制在 50 个英文字符之内

#### **名称** <a id="ming-cheng"></a>

* 名称是变量具体在使用的时候显示在报表上的一种变量的称呼。客户给变量一个名称，这个名称会出现在报表界面上用以指代当前所设置的变量对应的维度。名称允许客户经常改动，需要注意的是，名称修改以后，对应的报表界面上的维度名称也会跟着修改。
* 名称的设置细节：名称允许重名，可以使用大小写英文、中文以及各种符号，长度限制在 30 个字符之内。

#### **描述** <a id="miao-shu"></a>

* 可以使用较长的一段文字来描述某一个变量设置的目的，意义。
* 描述的设置细节规则：描述允许重复，可以使用大小写英文、中文以及各种符号，长度限制在 150 个字符之内。

### **第二步：代码部署** <a id="di-er-bu-dai-ma-bu-shu"></a>

完成了配置后，即可在代码中完成以上设计的 “自定义事件和变量” 的部署。具体的说，就是调用 GrowingIO 提供的API接口，上传数据。

* ​[JS 接口文档](https://growingio.gitbook.io/docs/~/drafts/-LPyrvcsY8gb75CQCoOG/primary/sdk-integration/web-js-sdk#3-web-js-sdk-2-1-api)​
* [​Android 接口文档​](https://docs.growingio.com/docs/sdk-integration/android-sdk/#setvisitor)
* [​iOS 接口文档​](https://docs.growingio.com/docs/sdk-integration/ios-sdk/#setvisitor)
* [​小程序接口文档​](https://docs.growingio.com/docs/sdk-integration/mina-sdk#fang-wen-yong-hu-bian-liang)

API中给出了访问用户变量的上传方式

### 第三步：数据校验 <a id="di-san-bu-shu-ju-xiao-yan"></a>

在完成了【数据管理】-【事件与变量】-【访问用户变量】的配置，以及代码实施后，我们当然需要对数据是否成功上传进行校验。校验工作分为两步完成。

#### **数据校验第一步：本地开发环境校验** <a id="shu-ju-xiao-yan-di-yi-bu-ben-di-kai-fa-huan-jing-xiao-yan"></a>

GrowingIO 提供了 SDK debug 模式以及 debug 工具，来帮助您完成数据的校验。具体请参考 [Debugger 最佳实践](https://growingio.gitbook.io/docs/~/drafts/-LPyrvcsY8gb75CQCoOG/primary/sdk-integration/growingio-debugger/best-practice#ppl-yong-hu-bian-liang-shi-jian)。

#### **数据校验第二步：GrowingIO 后台图表验证** <a id="shu-ju-xiao-yan-di-er-bu-growingio-hou-tai-tu-biao-yan-zheng"></a>

在 GrowingIO 分析后台，找到 “分析” - “新建事件分析”，选择一个事件后，在 "维度" 中查看设置的用户变量是否有数据。当然，您需要首先确已经完成了用户变量的数据上传。

至此，您已经完成了 “自定义变量” 的上传，如您在配置或添加代码中有任何疑问，请联系您的客户成功经理咨询，或在工单系统中反馈问题。谢谢。

## **用户变量的持久性范围** <a id="yong-hu-bian-liang-de-chi-jiu-xing-fan-wei"></a>

用户变量默认的持久性范围是永远。当某个用户配置上某一个用户变量的某一个值时，如果后面没有再修改，那么这个用户在今后会一直保持这个值。也就是说，用户在之后不管多久的时间段内发生的各种各样的事件都可以归到这个用户变量的值上，除非之后显式的更改了这个用户变量的值。

