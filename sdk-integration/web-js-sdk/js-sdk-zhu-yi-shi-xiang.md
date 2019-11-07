# Web JS SDK 常见问题

### 1. SDK 支持哪些前端框架？

JS SDK 支持所有主流前端框架，包括但不限于React.js, Vue.js, Angular.js等。

### 2. 为什么我的网站要**允许 iframe 加载？**

在 GrowingIO 平台上使用可视化圈选事件功能需要使用 iframe 技术来加载你的页面。如果你的网站禁止了 iframe 加载，就无法正常使用圈选功能定义事件，所以需要设置http响应头允许 iframe 加载。

如果你的网站使用https协议，需向响应头添加配置

```javascript
Content-Security-Policy: frame-ancestors 'self' https://www.growingio.comX-Frame-Options: Allow-From https://www.growingio.com
```

如果你的网站使用http协议，需向响应头添加配置

```javascript
Content-Security-Policy: frame-ancestors 'self' http://www.growingio.comX-Frame-Options: Allow-From http://www.growingio.com
```

由于 Chrome 浏览器已经不再支持 X-Frame-Options 配置项，如果你只需在 Chrome 浏览器中进行圈选，建议通过浏览器检查后，只给 Chrome 请求的响应头添加配置

```javascript
Content-Security-Policy: frame-ancestors 'self' http://www.growingio.com https://www.growingio.com
```

如果出于某些原因你不能改变这一设置，建议下载 [GrowingIO Chrome web圈选插件](../../data-definition/circle/web.md#13-cha-jian-quan-xuan) 进行圈选。

### 3. 为什么我的网站不能复写 **window 对象？**

可视化圈选事件功能使用 HTML5 的 postMessage 技术完成跨域通信。如果window.top、window.parent、window.name被复写，将使通信中断，无法圈选。

如果出于某些原因你不能改变这一设置，建议下载 [GrowingIO Chrome web圈选插件](../../data-definition/circle/web.md#13-cha-jian-quan-xuan) 进行圈选。

### 4. **页面内部嵌入的 iframe 元素如何加载 SDK？**

iframe 元素可以将一个页面嵌入到另一个页面里，iframe 元素会创建包含另外一个文档的内联框架（即行内框架）。简单理解 iframe 可以将多个相互独立的页面展示在一页上。所以我们需要在 iframe 内部再次加载 SDK 代码收集 iframe 内部的元素浏览、点击数据。 同普通网页加载 SDK 方式相同，将 SDK 复制到 iframe 标签内部即可完成 SDK 安装。

### 5. 同时集成了web sdk和hybrid sdk会怎么处理？

我们提供了一个全局变量 webViewRequestSend，在 web sdk 中会判断当用户设置 webViewRequestSend 为 false 并且 hybrid sdk 加载成功，则不进行 web sdk 数据的采集。

### 6. 同时集成了web sdk 1.x 和web sdk 2.x会怎么处理？

同时集成两个web sdk，都会只有第一个先执行的生效，虽然我们做了容错，但是并不支持用户同时集成两个sdk，没有必要。

### 7.元素没有元素浏览量怎么处理？

首先确认这个元素是不是通过display:none/其它 控制显隐的，对于 display 为 none 的元素，我们会只采集 A 和 Button 标签的浏览量，所以如果你想要一个 display 为 none 的元素&lt;或其子元素&gt;的浏览量，把元素改为 A 或 button 标签实现。并且还需要确认你的网站是否[关闭了元素浏览量的采集](./#22-imp-xi-tong-bian-liang)。

注意：对于IE8 及以下的 IE 浏览器版本，GrowingIO 无法统计元素浏览量 ，只会统计点击量。

### 8.对于 display:none 的元素，其子元素中的a/button只会采集一次浏览量，但是想每次曝光都采集一次浏览量怎么处理？

web sdk 只有 DOM 结构改变的时候才会发送元素浏览量，而由 display 控制显隐并不会引起 DOM 结构的改变，所以要想每次曝光都采集一次 imp，试试 createElement 和 removeElement，但是频繁的话会影响性能哦。

### 9.设置了growing-ignore之后，其子元素也都不采集数据了怎么处理？

web sdk 的高级属性 growing-ignore 是具有继承性的，可以使用growing-title = ""，使不想采的那个元素v为空。



