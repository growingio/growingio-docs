---
description: 埋点事件管理功能帮助您对产品内所有的埋点事件进行统一的维护与管理
---

# 埋点事件管理

* [​1. 简介](data-management-variables.md#1-jian-jie)
* ​2[. 埋点事件管理功能使用​](data-management-variables.md#2-mai-dian-shi-jian-guan-li-gong-neng-shi-yong)
  * [​2.1 埋点事件配置](data-management-variables.md#21-mai-dian-shi-jian-pei-zhi)
  * [2.2 ​埋点事件管理](data-management-variables.md#22-mai-dian-shi-jian-guan-li)

## **1. 简介**

除了通过圈选创建无埋点事件外，GrowingIO 亦支持通过代码埋点的方式上报埋点事件，如注册成功、购买成功等。在进行代码埋点前，需要您先进行成埋点事件 + 变量的设计，并在GrowingIO 系统中进行配置（参考：[埋点事件实施](https://growingio.gitbook.io/docs/data-definition/custom-event)​）。但您完成系统配置与代码部署后，即在 GrowingIO 完成了埋点事件的创建。

完成埋点事件的创建后，便可以在埋点事件管理界面中对埋点事件进行管理。您可以通过点击导航栏【数据中心】-【数据管理】按钮，进入数据管理界面，点击左侧【埋点事件】菜单，进入合并事件管理界面。

埋点事件管理提供的核心功能包括：

1. 创建埋点事件；
2. 埋点事件的查看、编辑、权限分配；
3. 收藏常用的无埋点事件；
4. 为无埋点事件添加业务标签，对事件进行业务归类；
5. 无埋点事件的状态监测；
6. 导出无埋点事件列表；

## **2. 埋点事件管理功能使用**

埋点事件管理主要功能可以分为埋点事件配置与埋点事件管理两部分，使用说用如下：

### **2.1 埋点事件配置**

点击埋点事件管理界面左上角【创建埋点事件】按钮，开始进行埋点事件的配置。  
对于每一个埋点事件，建议您在配置之前，先按下表列出配置细项，其中

* **名称：**供 GrowingIO 后台使用的事件名称
* **描述：**可选，仅作备注使用
* **标识符：**此事件在代码中的标识，可以为英文、下划线和数字。不允许使用数据开头，且大小写敏感；
* **事件级变量：**此埋点事件关联的事件级变量；

![](https://lh3.googleusercontent.com/he6NWLKLOqRiUfMWjWuhVsaSg2OVV8GhYIKC4B_wn8ZKFIZSnml4Nsru7Lr5BwqToYmGwXNUVri3Fp9jKYn2a02Hgtj1r1l_wGN-AhdtNnEV3A0c6S9VPX100VNgW01LcGXEaRH2)

接着将配置细项填入埋点事件配置界面，点击右下角确定按钮，即完成埋点事件的配置。在完成了配置，以及正确的代码实施后。我们即可开始在GrowingIO 使用埋点事件。

埋点事件详细的部署说明请参考[埋点事件实施](https://growingio.gitbook.io/docs/data-definition/custom-event)​。

### **2.2 埋点事件管理**

埋点事件一经保存后，便可以在埋点事件管理界面中对埋点事件进行管理。埋点事件列表呈现了您项目中所有的埋点事件以及各埋点事件的类型、创建人、创建时间、更新时间、状态等属性。您可以在埋点事件列表界面根据您的需求搜索或是筛选您需要的埋点事件。

点击任一埋点事件，您可以在右侧弹出的事件详情中，查看埋点事件的定义条件与统计趋势，以及修改事件的名称、标识符、描述，或是关联的事件级变量等。另外，您也可以在事件详情当中为事件添加业务标签。

![](https://lh4.googleusercontent.com/VPjtvVhqmdoWcGSNVJzQvH8nWCROsmzJYrY9xJ6flzEcHQCeob25xhR2OODh2aE8WHUL5TTRhkxjHWET8Irlik9Y6YTLzNZYBbDNnjw3eAzVwD5SaC-Xflhq-oUPv-tiowkCPuD9)

