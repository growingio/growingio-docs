# SDK 集成

### 1. 移动端 SDK 数据发送策略是怎样的？ <a id="data-send-policy"></a>

为了在移动设备省电和数据发送的及时性之间取得一个比较好的平衡，移动端 SDK 会按照一定的策略定时定量发送数据。GrowingIO 提供两个系统变量来控制发送数据的时间间隔和服务器请求的数量，这两个系统变量分别是：

**FlushInterval：用来控制数据发送的时间间隔，默认值为 30 秒**

**BulkSize：用来控制数据发送的数据量大小，默认值为 300 个服务器请求**

默认状态下，移动端 SDK 会使用这两个系统变量来控制是否安排一次服务器请求的集中批量发送。也就是说，只要满足了一个条件就会立即发送一次。

例如，某客户在 SDK 里面设置了 FlushInterval 为 20 秒，设置了 BulkSize 为 200。那么 SDK 只要满足了 20 秒或者服务器请求个数为 200 条就立刻安排一次数据的集中发送。

SDK 还提供了移动网络下的流量保护功能，对于用户来说，如果在移动网络下由 GrowingIO SDK 发送的服务器请求总数量到达了 3M 的数量，当天将停止在移动网络下继续发送服务器请求。

用户可以在离线环境下访问和使用移动应用，离线环境下产生的用户行为数据会在用户之后的第一次联网之后立刻发送，用户的离线数据最多可以保存 7 天。

### 2. 同时集成了 web SDK 和 hybrid SDK 会怎么处理？

我们提供了一个全局变量 webViewRequestSend，在 web SDK 中会判断当用户设置 webViewRequestSend为 false 并且 hybrid SDK 加载成功，则不进行 web SDK 数据的采集。

考虑到 web SDK 执行时有可能 hybrid SDK还未注入成功，这种情况可能会造成两个 SDK 都采集数据，所以在 web SDK 采集的数据发送时，也会判断是否注入了 hybrid SDK，如果注入了，则不发送 web SDK采集的数据发送。

### 3. 同时集成了 web SDK 1.x 和web SDK 2.x 会怎么处理？

同时集成两个 web SDK，都会只有第一个先执行的生效，虽然我们做了容错，但是并不支持用户同时集成两个 SDK，没有必要。

### 4.元素没有 imp 怎么处理？

首先确认这个元素是不是通过 display:none/ 其它 控制显隐的，对于 display 为 none 的元素，我们会只采集 A 和 utton 标签的浏览量，所以如果您想要一个 display 为 none 的元素 &lt;或其子元素&gt; 的浏览量，把元素改为 A 或 button 标签实现。

注意：对于 IE8 及以下的 IE 浏览器版本，GrowingIO 无法统计元素浏览量 ，只会统计点击量。

### 5.对于 display:none 的元素，其子元素中的 a/button 只会采集一次浏览量，但是想每次曝光都采集一次浏览量怎么处理？

web SDK 只有 DOM 结构改变的时候才会补发 imp，而由 display 控制显隐并不会引起 DOM 结构的改变，所以要想每次曝光都采集一次 imp，试试 createElement和removeElement，但是频繁的话会影响性能哦。

### 6.对于一个元素不想采集数据，但是设置了growing-ignore之后，其子元素也都不采集数据了怎么处理？

web SDK 的高级属性 growing-ignore 是具有继承性的，可以使用 growing-title = ""，使不想采的那个元素 v为空。



