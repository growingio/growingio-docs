# 腾讯社交广告

###  腾讯社交广告—Marketing API <a id="7-teng-xun-she-jiao-guang-gao-marketing-api"></a>

腾讯社交广告（广点通），归因与展现方式为：‌

1\)在GrowingIO后台，创建“腾讯社交广告”监测链接，并绑定广点通账号，并进行相应的授权；‌

2\)GroiwngIO实时将激活数据发送给广点通，广点通返回激活信息，点击时间，广告名称等信息；‌

3\)GrowingIO会根据用户的广告点击数据与激活数据做归因匹配，并将归因的结果展示在GrowingIO后台数据中。‌

1、在GIO后台创建 App 推广链接，目标渠道选择“腾讯社交广告”，然后点击保存‌

![](../../.gitbook/assets/image%20%28161%29.png)

2、点击保存后进入绑定与授权，‌

1）投放账号：填写对应的腾讯社交广告后台的登录账号（QQ号）。‌

2）投放账号ID：填写对应的腾讯社交广后台的账户ID，然后点击授权。‌

3）应用ID：填写您选取应用的对应ID，示例：iOS填写Appstore地址上面的数字。‌

![](../../.gitbook/assets/image%20%2877%29.png)

其中，“投放账号”和“投放账号ID”，与广点通后台相对应。‌

获取方式如下：‌

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LRQkXUuEuD5xER41iwA%2F-LRQsE1caJLiZwqN5Z1H%2Fimage.png?alt=media&token=5fbebb02-f520-435d-97e1-60db25a507bc)

应用标识，请与广点通后台填写的“应用宝ID”或“苹果商店ID”保持一致。‌

* 图示为苹果商店ID获取方式。

‌

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LRQkXUuEuD5xER41iwA%2F-LRQkdwds05hJHuw19rZ%2Fimage.png?alt=media&token=c93ef93f-5921-4310-a1a1-aba72cb33365)

3、点击授权之后再这里进行同意授权，授权成功后，在GIO后台获取监测链接‌

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LRQkXUuEuD5xER41iwA%2F-LRQkcMxpIjuQnlj6LnP%2Fimage.png?alt=media&token=c39a8174-0521-4587-832f-deef1ffa365e)

4、进入腾讯社交广告后台开始联调，在工具箱里面选择“转化跟踪”。‌

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LRQkXUuEuD5xER41iwA%2F-LRQkaShjg8W52eL9L-b%2Fimage.png?alt=media&token=ae247be7-1e91-4026-9b05-b72f958d7891)

5、点击“获取点击数据”，按照图示在移动应用上填写应用ID，在feedback URL上填写GIO生成的监测链接。‌

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LRQjWfPi27_KI29rnuM%2F-LRQjb-aikyoOQAZ1HaP%2Fimage.png?alt=media&token=a42ffc37-f15a-47bf-84e3-fb68c0dbe934)

6、开始联调。‌

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LRQjWfPi27_KI29rnuM%2F-LRQj_IlBBNx-6cRsNSC%2Fimage.png?alt=media&token=4002ca9f-18ec-48f2-ab9d-4302ffa74d17)

7、填写对应的设备号。‌

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LRQid00mZKBhPNo-Go3%2F-LRQj827zov8vmhfTJHe%2Fimage.png?alt=media&token=bd02bbdc-8f4f-4d98-ab35-d6d161ce4388)

8、联调成功，去Appstore或者应用宝下载您的APP。

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LGNxeGABUADKiTWTaEM%2F-LRQid00mZKBhPNo-Go3%2F-LRQixJgnlvkJ8zfqNdA%2Fimage.png?alt=media&token=0a829cfd-9360-40bd-a506-a28844a03d98)

