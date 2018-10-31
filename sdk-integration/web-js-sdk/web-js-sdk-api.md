# Web JS SDK API

* [最新版 Web JS SDK（2.x）API](web-js-sdk-api.md#11-api-jian-jie)
  * [1.1 API 简介](web-js-sdk-api.md#11-api-jian-jie)
  * [1.2 初始化 \(init\)​​](web-js-sdk-api.md#12-chu-shi-hua-init)
  * [1.3 设置登录用户id \(setUserId\)​​](web-js-sdk-api.md#13-she-zhi-deng-lu-yong-hu-idsetuserid)
  * [1.4 清除登录用户id \(clearUserId\)​​](web-js-sdk-api.md#14-qing-chu-deng-lu-yong-hu-idclearuserid)
  * [1.5 设置登录用户级变量（people.set）](web-js-sdk-api.md#15-she-zhi-yong-hu-ji-bian-liang-peopleset)​
  * [1.6 设置访问用户级变量（visitor.set）](web-js-sdk-api.md#16-she-zhi-fang-wen-yong-hu-ji-bian-liang-visitorset)
  * [1.7 设置页面级变量（page.set）​](web-js-sdk-api.md#16-she-zhi-ye-mian-ji-bian-liang-pageset)
  * [1.8 设置转化变量（evar.set）​](web-js-sdk-api.md#17-she-zhi-zhuan-hua-bian-liang-evarset)
  * [1.9 设置自定义事件和事件级变量（track）​](web-js-sdk-api.md#18-she-zhi-zi-ding-yi-shi-jian-he-shi-jian-ji-bian-liang-track)
  * [1.10 手动发送页面浏览事件 \(sendPage\)​​](web-js-sdk-api.md#19-shou-dong-fa-song-ye-mian-lan-shi-jian-sendpage-sendpage)
  * [1.11 GDPR 数据采集开关](web-js-sdk-api.md#110-gdpr-shu-ju-cai-ji-kai-guan)
* [旧版 Web JS SDK \(1.x\) API](web-js-sdk-api.md#2-jiu-ban-web-js-sdk-1-x-api)

### 1.1 API 简介

```javascript
// 初始化参数
gio('init', projectId, options); 

// 修改系统变量API
gio('config', options);

// 发送事件API
gio('track', eventId);
gio('track', eventId, number);
gio('track', eventId, eventLevelVariables);
gio('track', eventId, number, eventLevelVariables);

// 发送页面级变量API
gio('page.set', key, value);
gio('page.set', pageLevelVariables);

// 发送转化变量API
gio('evar.set', key, value);
gio('evar.set', conversionVariables);

// 发送登录用户变量API
gio('people.set', key, value);
gio('people.set', customerVariables);

// 发送访问用户变量API
gio('visitor.set', key, value);
gio('visitor.set', visitorVariables);

// 设置登录用户ID
gio('setUserId', userId); 

// 清除登录用户ID
gio('clearUserId');
```

### 1.2 初始化 \(init\)​

初始化参数，用来设置项目ID和一些常用的配置项：

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| projectId | String | 是 | 项目ID |
| options | JSON Object | 否 | 系统变量配置 |

```javascript
//init API原型
gio('init', projectId, options);
```

```javascript
//init API调用示例
//配置imp类型的数据关闭发送
gio('init', '1234567890', {'imp':false});
```

### 1.3 设置登录用户 ID（setUserId）

当用户登录之后调用 setUserId API ，设置登录用户 ID 。

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| userId | String | 是 | 用户的登录用户ID |

```javascript
//setUserId API原型
gio('setUserId', userId);
```

```javascript
//setuserId API调用示例
gio('setUserId', '1234567890');
```

### 1.4 清除登录用户 ID（clearUserId）

当用户登出之后调用 clearUserId ，清除已经设置的登录用户 ID 。

```javascript
//clearUserId API原型和调用示例
gio('clearUserId');
```

### 1.5 设置登录用户级变量（people.set）

发送登录用户信息用于登录用户信息相关分析，在添加代码之前必须在打点管理界面上声明登录用户变量。

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| key | String | 否 | 登录用户变量的标识符 |
| value | String | 否 | 登录用户变量的值 |
| customerVariables | JSON Object | 否 | 包含登录用户变量的JSON对象 |

```javascript
// people.set API原型
gio('people.set', key, value);
gio('people.set', customerVariables);
```

```javascript
// people.set API调用示例一
gio('people.set', 'gender', 'male');
```

```javascript
//people.set API调用示例二
gio('people.set', {'gender':'male', 'age':'25'});
```

### 1.6  设置访问用户级变量（visitor.set）

发送访问用户信息用于访问用户信息相关分析，在添加代码之前必须在打点管理界面上声明访问用户变量。

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| key | String | 否 | 访问用户变量的标识符 |
| value | String | 否 | 访问用户变量的值 |
| customerVariables | JSON Object | 否 | 包含访问用户变量的JSON对象 |

```javascript
// visitor.set API原型
gio('visitor.set', key, value);
gio('visitor.set', visitorVariables);
```

```javascript
// visitor.set API调用示例一
gio('visitor.set', 'gender', 'male');
```

```javascript
//people.set API调用示例二
gio('visitor.set', {'gender':'male', 'age':'25'});
```

### 1.7 设置页面级变量（page.set）

发送页面级别的维度信息，在添加代码之前必须在打点管理界面上声明页面级变量。

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| key | String | 否 | 页面级变量的标识符 |
| value | String | 否 | 页面级变量的值 |
| pageLevelVariables | JSON Object | 否 | 包含页面级变量的JSON对象，暨页面级别的信息 |

```javascript
// page.set API原型
gio('page.set', key, value);
gio('page.set', pageLevelVariables);
```

```javascript
// page.set API调用示例一
gio('page.set', {'pageName': 'Home Page', 'author': 'Zhang San'});
```

```javascript
// page.set API调用示例二
gio('page.set', 'author', 'Zhang San');
```

### 1.8 设置转化变量（evar.set）

发送一个转化信息用于高级归因分析，在添加代码之前必须在打点管理界面上声明转化变量。

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| key | String | 否 | 转化变量的标识符 |
| Value | String | 否 | 转化变量的值 |
| conversionVariables | JSON Object | 否 | 包含转化变量的JSON对象 |

```javascript
// evar.set API原型
gio('evar.set', key, value);
gio('evar.set', conversionVariables);
```

```javascript
// evar.set API调用示例一
gio('evar.set', 'campaignId'，'1234567890');
```

```javascript
// evar.set API调用示例二
gio('evar.set', {'campaignId': '1234567890', 'campaignOwner':'lisi'});
```

### 1.9 设置自定义事件和事件级变量（track）

在添加所需要发送的事件代码之前，需要在打点管理用户界面配置事件以及事件级变量。

| 参数名称 | 参数类型 | 是否必须 | 说明 |
| :--- | :--- | :--- | :--- |
| eventId | String | 是 | 事件标识符 |
| number | Number | 否 | 事件的数值，没有number参数时，事件默认加1；当出现number参数时，事件自增number的数值。 |
| eventLevelVariables | JSON Object | 否 | 包含事件级变量的JSON对象，暨事件发生时所伴随的维度信息。 |

```javascript
// track API原型
gio('track', eventId, eventLevelVariables);
gio('track', eventId, number, eventLevelVariables);
```

```javascript
// track API调用示例一
gio('track', 'registerSuccess');
```

```javascript
// track API调用示例二
gio('track', 'registerSuccess', {'gender':'male', 'age':21});
```

```javascript
// track API调用示例三
gio('track', 'loanAmount', 800000, {'loanType':'houseMortgage','province':'Zhejiang'});
```

### 1.10 手动发送页面浏览事件 sendPage \(sendPage\)​

在默认情况下，由于用户浏览网站的交互行为导致当前页面的 URL 产生变化时，GrowingIO 的 Web JS SDK 会发送一个 page 类型的请求。在一些特殊的情况下，例如用户在访问单页应用（Single Page Application）类型的网站时，用户的操作会导致业务上面理解的页面产生了变化，但是当前的 URL 可能并没有改变。

这时，可以调用GrowingIO提供的 sendPage 接口手动发送页面浏览事件。这个接口的调用将会发送出一条‘page’类型的数据，GIO 服务器在收到 page 类型的数据之后，页面浏览量这个预定义指标会加 1。

```javascript
//sendPage API原型和调用示例
gio('sendPage'); // 放在send之后
```

### 1.11 GDPR 数据采集开关

GrowingIO 全面支持 [欧盟《一般数据保护条例》 \(GDPR\)](../privacy.md#4)

* 关闭或开启全局数据采集

```javascript
// 开启gdpr，停止数据采集 
gio('config',{"dataCollect":"false"}); 
// 关闭gdpr，开始数据采集 
gio('config',{"dataCollect":"true"}); // 放在init和send之间
```

* 获取访问用户 ID

```javascript
gio('getVisitUserId'); // 放在send之后
```

##  2.旧版 [Web JS SDK（1.x）](./#2-1) API

GrowingIO 的数据分析工具本身提供了例如 “访问来源”，“关键字”，“城市”,“操作系统"，”浏览器“等等这些维度。这些维度都可以和用户创建的指标进行多维的分析。但是因为每个公司的产品都有各自的用户维度，比如客户所服务的公司，用户正在使用的产品版本等等，为了能够让数据分析变得更加的灵活，我们在 JS SDK 中提供了用户自定义维度的 API 接口:

```javascript
_vds.push(['setCS1', 'CS1的key', 'CS1的value']);
_vds.push(['setCS2', 'CS2的key', 'CS2的value']);
_vds.push(['setCS3', 'CS3的key', 'CS3的value']);
...
_vds.push(['setCS10', 'CS10的key', 'CS10的value']);
```

在 JS SDK 中，我们总计支持上传 10 个自定义维度 CS1 - CS10，**所有CS属性都必须是用户的属性，不能是订单 ID，商品ID 等和用户没有确定的关联关系的属性。**

**CS字段设置条件和限制**

1. CS 字段不能是和用户没有直接关系的属性，比如不能是订单 ID，商品 ID 等。
2. CS1 字段：在 GrowingIO 系统中用于识别注册用户的身份，因此 CS1 的 value 必须填写用户的唯一身份标示 ID。
3. CS2 字段：在 GrowingIO 系统中用于识别 SaaS 客户的租户，因此所有的 SaaS 用户必须填写租户的唯一身份标示 ID，非 SaaS 用户不做限定。
4. 对于未登录用户，不要设置任何CS字段。
5. 如果没有用到所有的CS字段，剩下的可以不设置。
6. 同一个CS字段，必须保持在各个平台意义相同。
7. CS11~CS20 不支持在 SDK 中上传，必须通过服务器上传，具体请参考 [用户变量上传 API](https://docs.growingio.com/docs/api/user-property-upload#3-jiu-ban-ben-shang-chuan-jie-kou)。

如下例子中，总计上传 5个用户属性，分别是：

**CS1:** user\_id:100324  
**CS2:** company\_id:943123  
**CS3:** user\_name:张溪梦  
**CS4:** company\_name:GrowingIO  
**CS5:** sales\_name:销售员小王

```javascript
    <script type='text/javascript'>
        var _vds = _vds || [];
        window._vds = _vds;
        (function(){
          _vds.push(['setAccountId', '您的项目ID']);

          _vds.push(['setCS1', 'user_id', '100324']);
          _vds.push(['setCS2', 'company_id', '943123']);
          _vds.push(['setCS3', 'user_name', '张溪梦']);
          _vds.push(['setCS4', 'company_name', 'GrowingIO']);
          _vds.push(['setCS5', 'sales_name', '销售员小王']);

          (function() {
            var vds = document.createElement('script');
            vds.type='text/javascript';
            vds.async = true;
            vds.src = ('https:' == document.location.protocol ? 'https://' : 'http://') + 'assets.growingio.com/vds.js';
            var s = document.getElementsByTagName('script')[0];
            s.parentNode.insertBefore(vds, s);
          })();
        })();
    </script>
```

**需要注意，CS11~CS20 不支持在 SDK 中上传，必须通过服务器上传，具体请参考** [**用户变量上传 API**](https://docs.growingio.com/docs/api/user-property-upload#3-jiu-ban-ben-shang-chuan-jie-kou)\*\*\*\*

**在上传成功两小时后，您需要在「项目管理-项目配置-CS 配置中」进行字段配置和激活，配置成功后便可开始使用 CS 字段进行分析。**

