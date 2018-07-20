# 圈选指标管理

* [简介](quan-xuan-zhi-biao-guan-li.md#jian-jie)
* [简单指标](quan-xuan-zhi-biao-guan-li.md#jdzb)
* [合并简单指标](quan-xuan-zhi-biao-guan-li.md#he-bing)
* [复合指标](quan-xuan-zhi-biao-guan-li.md#fu-he)
* [常见问题](quan-xuan-zhi-biao-guan-li.md#faq)

### **简介**

圈选指标管理页面提供了对产品内所有通过圈选创建的指标的管理操作，核心功能包括：

1. 圈选指标的查看、编辑、权限分配
2. 创建及编辑复合指标、合并指标
3. 通过分类操作，对现存指标进行业务归类
4. 指标的状态监测

### **简单指标** {#jdzb}

当用户圈选了一个元素或页面后，就创建了一个**简单指标**。简单指标包含点击量和浏览量等常用统计量，可以在制作图表中进行选择。对简单指标可以在指标管理中进行如下操作：

1.分类整理：通过在【我的收藏】下使用【鼠标右键】创建不同的分类，并对分类赋予一定的业务含义，再把相应的指标移动到对应的分类下，可以完成指标的整理操作，同时在事件分析中，被分类的指标会按照构建的分类结构来使用。![](https://docs.growingio.com/.gitbook/assets/zhi-biao-guan-li-xin-jian-fen-lei.png)![](https://docs.growingio.com/.gitbook/assets/zhi-biao-guan-li-fen-lei-2.png)![](https://docs.growingio.com/.gitbook/assets/shi-jian-fen-xi-zhi-biao-xia-la.png)

2.查看和编辑：点击一个指标所在的行，可以在右侧弹出的指标详情中，查看指标的详细信息，同时可以修改指标的相关属性，比如：名称、页面、文本等。![](https://docs.growingio.com/.gitbook/assets/zhi-biao-guan-li-xiang-qing.png)

3.设置权限：针对想要分享供他人使用的指标，可以单选或多选对应的指标后，点击【分享】按钮进行权限的设置。![](https://docs.growingio.com/.gitbook/assets/zhi-biao-guan-li-quan-xian.png)

4.状态监测：针对核心关键指标，可以设置无数据状态监测，勾选单个或多个指标后，点击【状态监测】，设置相应的数据监测周期，已监测的指标会出现对应的flag标记，并可以通过标签快速刷选出异常指标。![](https://docs.growingio.com/.gitbook/assets/zhi-biao-guan-li-jian-ce.png)

5.指标转让：在实际场景中，很多时候会出现员工离职、账号删除等情况，此时原有指标的创建人已经无法操作该指标，因此管理员可以通过【指标转让】，重新设置指标的创建人。![](https://docs.growingio.com/.gitbook/assets/zhi-biao-guan-li-gong-neng-qu.png)

### 合并简单指标 {#he-bing}

您还可以使用“合并简单指标功能”将多个简单指标合并成一个。

1. 当转化流中有多个功能入口，您可以将多个入口合并起来，在漏斗功能中使用，分析整体转化情况。
2. 当产品中有一组相互关联的功能，希望分析这一组功能的整体点击率、留存率等指标，您可以将这一组功能合并起来，使用复合指标、留存等分析模块来进行分析。

当您把简单指标A、B、C合并后，合并指标的点击次数就是A、B、C点击次数的总和，点击用户量是A、B、C点击用户去重后的总量，也就是点击过A或B或C的用户量总和。

### **复合指标** {#fu-he}

如果需要进行更高级的自定义，来制作加权计算的统计量，可以创建**复合指标**。复合指标可以把各种简单指标加权组合，来更综合地衡量业务运营情况。

在指标管理中可以做到：

* 计算复合指标

  复合指标的创建入口位于页面的右上角。复合指标是由简单指标进行运算得到的，您可以将需要的简单指标拖拽到公式中，并且选择运算符号。

  指标相加：

![](https://docs.growingio.com/.gitbook/assets/zhi-biao-guan-li-xiang-jia.png)

指标相除：![](https://docs.growingio.com/.gitbook/assets/zhi-biao-guan-li-xiang-chu.png)

### 常见问题 {#faq}

**1，合并的简单指标和复合指标有什么区别呢？** 合并的简单指标表示您网站、App上的一组功能点，比如多个注册入口按钮等。 它的使用和圈选出的简单指标相同，在单图等分析模块选择该合并的简单指标，然后选择浏览/点击、次数/用户量进行分析。并且合并的简单指标可以在漏斗、留存分析中使用。 复合指标表示一个网站、App运行情况的数值指标，比如用户活跃度、页面浏览量等，是量化的衡量标准。复合指标不可以被用在漏斗、留存分析中。

