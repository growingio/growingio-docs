# 小程序SDK集成前工作

## 确定集成SDK的项目

![&#x96C6;&#x6210;SDK&#x7684;&#x9636;&#x6BB5;&#x548C;&#x6D41;&#x7A0B;](../../.gitbook/assets/image%20%2815%29.png)

如果您已经注册GrowingIO,并且已经有创建或者集成SDK的项目了，您可以进行如下选择：

1. 使用已有项目的ID，添加新的应用，集成SDK。
2. 创建一个新的项目，然后集成SDK。

`*注：如果已有项目且集成了其他应用，新建的项目，数据不能和原有项目中的数据合并。例如：拥有小程序A，iOS端产品，如果各个应用集成在同一个项目ID中，且都上传登录用户ID，登录用户计算时会去重，并且在使用高级分析功能时，可以同时选择跨平台的行为和登录用户。但是如果小程序和APP不在同一个项目ID下，登录用户计算分别在两个项目中，不会去重计算；高级分析功能中，也不能跨平台选择登录用户ID和行为。`

如果你还未注册 GrowingIO，请[点击链接访问注册页面](https://accounts.growingio.com/signup?utm_source=docs&utm_content=minp)，完成注册后你会看到使用引导，点击“**添加追踪代码**“即可开始。

## 1.在已有项目中添加小程序应用

在你的 GrowingIO 项目页面点击右上角项目切换控件，在下拉框点击“**项目管理”**，在弹出的列表中选择“应用管理“。在项目概览页面，点击“**新建应用**“来创建一个新应用。

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LH3J3bJm8y879FZ1E7G%2F-LH3Jrrg6GYh7ccjZGaG%2Fimage.png?alt=media&token=a8744aff-4d74-451b-9adf-ec773e5cbb5c)

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LH3J3bJm8y879FZ1E7G%2F-LH3KSslW7JKLy4bb98K%2Fimage.png?alt=media&token=04f949af-bbb1-49f5-98e2-c212022a9c1d)

然后会到达SDK集成页面，填写小程序的应用名称，和小程序的AppID ,点击“下一步”，即可以到达小程序SDK接入的页面。

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LH3J3bJm8y879FZ1E7G%2F-LH3JTZYNHN07NUun-S6%2Fimage.png?alt=media&token=ea6a9dc2-5bc9-408d-b50f-f9c734f5cf0d)

###  {#chuang-jian-xin-de-growingio-xiang-mu}

## 2.创建新的GrowingIO项目

如果你还未注册 GrowingIO，请[点击链接访问注册页面](https://accounts.growingio.com/signup?utm_source=docs&utm_content=minp)，完成注册后你会看到使用引导，点击“**添加追踪代码**“即可开始。

如果你已经注册 GrowingIO，使用小程序分析功能需要用一个全新的项目的话，在你的 GrowingIO 项目页面点击右上角项目切换控件，在下拉框点击“**项目管理”**，在弹出的列表中选择“**项目概览**“。在项目概览页面，点击“**新建项目**“来创建一个新项目。在创建好的新项目里，你会看到使用引导，点击“**添加追踪代码**“即可开始。![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LGyRLnN1UW6BL8O3mEr%2F-LGySEePLI1xZvcmo0KX%2Fimage.png?alt=media&token=c9960fdc-81b9-42a1-8604-069d0ad6bdaf)![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD4kKkCTHNxUGbu1QWO%2F-LGyRLnN1UW6BL8O3mEr%2F-LGySTNxnseL1EsH7kN8%2Fimage.png?alt=media&token=91d05ea5-95d4-4104-b228-0f1837d5201b)

在代码集成页面，选择“**小程序**“平台，输入**应用名称**和**小程序 AppID**，点击下一步。

