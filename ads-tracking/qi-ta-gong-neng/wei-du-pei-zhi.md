# 维度配置

当您需要根据业务情况，希望投放链接可以统计到更多维度数据时，可以使用链接自定义维度配置。

举例，如果您想跟踪不同区域的效果数据，可以在此新建自定义为维度参数“area”，显示名称“区域”。配置后，可以在任一GrowingIO的移动监测链接中使用。形如：https://gio.ren/r3jEmQe 这样的链接，配置“area”自定义维度后，可直接在链接后缀此参数，得到 https://gio.ren/r3jEmQe?area=huabei 的新链接。

进行投放后，增加的“区域”维度可以在事件分析/用户分群/漏斗等模块引用查看“区域”维度中不同值的表现。

其中自定义的维度链接参数需以数字+英文字母表达。显示名称则为在GrowingIO后台中维度的显示名称，可使用中文表达。

### 1 配置链接维度参数

![](https://docs.growingio.com/.gitbook/assets/-LGNxeGABUADKiTWTaEM-LWk46FvcjNEct7W32BH-LWk7SOghdU0RUKmSt4aWeChatfb5d7d6f862565236153d6fe65d1357c.png)

### 2 维度参数拼接 

在链接创建环节中创建一条短链，形如：https://gio.ren/r3jEmQe 这样的链接，可以直接使用 Query String 的方式拼接成：https://gio.ren/r3jEmQe?city=beijing 。则投放北京区域可以直接使用此链接。

### 3 数据查看

在事件分析/分群/漏斗分析等模块中，引用维度“城市”查看不同推广城市带来的效果：

![](https://docs.growingio.com/.gitbook/assets/-LGNxeGABUADKiTWTaEM-LWk46FvcjNEct7W32BH-LWk86wwMZx6TT8wDZH9WeChat071b06ea77621edc0a375dbb24acab68.png)

{% hint style="warning" %}
维度值建议使用英文或数字。如果使用中文，请使用 URL Encode 编码。

实例：https://gio.ren/r3jEmQe?keyword=%e5%a2%9e%e9%95%bf
{% endhint %}



