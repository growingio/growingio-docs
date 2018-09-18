# SDK 集成

### 1. 移动端 SDK 数据发送策略是怎样的？ {#data-send-policy}

为了在移动设备省电和数据发送的及时性之间取得一个比较好的平衡，移动端SDK会按照一定的策略定时定量发送数据。GrowingIO提供两个系统变量来控制发送数据的时间间隔和服务器请求的数量，这两个系统变量分别是：  


**FlushInterval：用来控制数据发送的时间间隔，默认值为30秒**

**BulkSize：用来控制数据发送的数据量大小，默认值为300个服务器请求**

默认状态下，移动端SDK会使用这两个系统变量来控制是否安排一次服务器请求的集中批量发送。也就是说，只要满足了一个条件就会立即发送一次。

例如，某客户在SDK里面设置了FlushInterval为20秒，设置了BulkSize为200。那么SDK只要满足了20秒或者服务器请求个数为200条就立刻安排一次数据的集中发送。

SDK 还提供了移动网络下的流量保护功能，对于用户来说，如果在移动网络下由GrowingIO SDK发送的服务器请求总数量到达了3M的数量，当天将停止在移动网络下继续发送服务器请求。

用户可以在离线环境下访问和使用移动应用，离线环境下产生的用户行为数据会在用户之后的第一次联网之后立刻发送，用户的离线数据最多可以保存7天。

### 2. 同时集成了web sdk和hybrid sdk会怎么处理？

我们提供了一个全局变量webViewRequestSend，在web sdk中会判断当用户设置webViewRequestSend为false并且hybrid sdk加载成功，则不进行web sdk数据的采集。

考虑到web sdk执行时有可能hybrid sdk还未注入成功，这种情况可能会造成两个sdk都采集数据，所以在web sdk采集的数据发送时，也会判断是否注入了hybrid sdk，如果注入了，则不发送web sdk采集的数据发送。

### 3. 同时集成了web sdk 1.x 和web sdk 2.x会怎么处理？

同时集成两个web sdk，都会只有第一个先执行的生效，虽然我们做了容错，但是并不支持用户同时集成两个sdk，没有必要。

### 4.元素没有imp怎么处理？

首先确认这个元素是不是通过display:none/其它 控制显隐的，对于display为none的元素，我们会只采集A和Button标签的浏览量，所以如果您想要一个display为none的元素&lt;或其子元素&gt;的浏览量，把元素改为A或button标签实现。

注意：对于IE8 及以下的 IE 浏览器版本，GrowingIO 无法统计元素浏览量 ，只会统计点击量。

### 5.对于display:none的元素，其子元素中的a/button只会采集一次浏览量，但是想每次曝光都采集一次浏览量怎么处理？

web sdk只有DOM结构改变的时候才会补发imp，而由display控制显隐并不会引起DOM结构的改变，所以要想每次曝光都采集一次imp，试试createElement和removeElement，但是频繁的话会影响性能哦。

### 6.对于一个元素不想采集数据，但是设置了growing-ignore之后，其子元素也都不采集数据了怎么处理？

web sdk的高级属性growing-ignore是具有继承性的，可以使用growing-title = ""，使不想采的那个元素v为空。



