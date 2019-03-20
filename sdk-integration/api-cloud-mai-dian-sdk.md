---
description: GrowingIO 埋点 SDK 仅自动采集设备信息和您埋点内容。
---

# API Cloud埋点 SDK

## **1.配置Config.xml文件** 

**使用此模块前先配置config.xml文件,方法如下**

• 名称：GrowingIO

• 必要参数: accountId, urlScheme

• 可选参数: trackerHost, reportHost, dataHost, gtaHost, wsHost, zone

```markup
<feature name="GrowingIO">
<param name="android_accountId" value="xxxxx"/>
<param name="ios_accountId" value="xxxx"/>
<param name="ios_urlScheme" value="xx ios项目的urlScheme  xx"/>
<param name="android_urlScheme" value="xx android项目的urlScheme  xx"/>
<param name="trackerHost" value="xxxxx"/>
<param name="reportHost" value="xxxxx"/>
<param name="dataHost" value="xxxxx"/>
<param name="gtaHost" value="xxxxx"/>
<param name="wsHost" value="xxxxx"/>
<param name="zone" value="xxxxx"/>
<param name="channel" value="xxxx"/>
<param name="debug" value="true or false"/>
</feature>
<preference name="urlScheme" value=" xx ios项目的urlScheme  x " />
<preference name="urlScheme" value=" xx android项目的urlScheme  x " />
```

    **注意preference的urlScheme需要配置两个， 一个为Android项目的， 另一个为IOS项目的， 如果只有一个平台填写自己相应平台的即可, 同理feature中的android\_urlScheme与ios\_urlScheme**

**注意preference中ios项目的urlScheme在前,android项目的urlScheme在后,需要保证顺序**

## **2.下载模块zip包**

  iOS模块包：[下载](https://github.com/growingio/APICloud-growingio/blob/master/iOS/iOS/GrowingIO_iOS.zip)

  Android模块包：[下载](https://github.com/growingio/APICloud-growingio/blob/master/android/GrowingIO.zip)

## **3.添加模块**

  开发控制台-&gt; 选择应用-&gt; 模块-&gt; 自定义模块-&gt; 点击上传-&gt; 编写自定义模块信息\(注意:模块名称要和zip包名称一致\)-&gt; 点击添加模块"+" -&gt; 在已添加模块中确认是否成功添加。

{% hint style="warning" %}
注意：在自动定义模块中上传了压缩包，保存成功后。一定要点击添加模块后面的“+”，否则不是真正添加成功。添加成功后，去已添加模块中能看到刚刚添加的模块。
{% endhint %}

![](../.gitbook/assets/image%20%28200%29.png)

## **4.Android的额外操作**

     Android云编译Loader为AppLoader， 使用自定义模块式需要编译Android自定义loader, 否则会出现模块未绑定错误, 另外需要注意的是在使用自定义loader时 请勾选 **使用升级环境编译**选项

具体步骤如下:

（1）模块-自定义loader: 请勾选 **使用升级环境编译**

![](../.gitbook/assets/image%20%28241%29.png)

（2）云编译时， 请勾选 **使用升级环境编译**

![](../.gitbook/assets/image%20%28250%29.png)

## **5.插件支持的方法**

### （1）init\(\)

       **此接口为Android初始化， 在require后调用，iOS不需要，iOS已自动初始化**建议在require GrowingIO时调用此接口

```javascript
 vargio =null;
     apiready=function(){
         gio =api.require('GrowingIO');
         gio.init();
     }
```

### （2）track\(event, callback\)

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;&#x540D;</th>
      <th style="text-align:left">&#x7C7B;&#x578B;</th>
      <th style="text-align:left">&#x662F;&#x5426;&#x5FC5;&#x586B;</th>
      <th style="text-align:left">&#x53C2;&#x6570;&#x63CF;&#x8FF0;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">event</td>
      <td style="text-align:left">object</td>
      <td style="text-align:left">&#x662F;</td>
      <td style="text-align:left">
        <p>key:eventId(string&#x7C7B;&#x578B;,&#x5FC5;&#x8981;key) value:(string&#x7C7B;&#x578B;)</p>
        <p>key:eventLevelVariable(string&#x7C7B;&#x578B;,&#x975E;&#x5FC5;&#x8981;key)
          value:(object&#x7C7B;&#x578B;)</p>
        <p>key:number(string&#x7C7B;&#x578B;, &#x975E;&#x5FC5;&#x8981;key) value(number&#x7C7B;&#x578B;)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">callback</td>
      <td style="text-align:left">&#x51FD;&#x6570;</td>
      <td style="text-align:left">&#x5426;</td>
      <td style="text-align:left">
        <p>allback {function (ret)}&#xFF1A;&#x6267;&#x884C;&#x5B8C;&#x8BFB;&#x53D6;&#x64CD;&#x4F5C;&#x540E;&#x7684;&#x56DE;&#x8C03;&#x51FD;&#x6570;&#x3002;</p>
        <p>ret &#x4E3A; callback &#x51FD;&#x6570;&#x7684;&#x53C2;&#x6570;&#xFF0C;&#x6709;&#x4E24;&#x4E2A;&#x5C5E;&#x6027;:</p>
        <p>status:&#x7ED3;&#x679C;2&#x79CD; true, false &#x90FD;&#x4E3A;&#x5E03;&#x5C14;&#x7C7B;&#x578B;</p>
        <p>msg:&#x7ED3;&#x679C;string&#x7C7B;&#x578B;</p>
      </td>
    </tr>
  </tbody>
</table>调用示例：

```javascript
var gio = api.require('GrowingIO');  //引用模块
gio.track({
            eventId: 'GIOKey'
        },function(ret, err){
            //回调函数事件处理
        });
```

### （3）setEvar\(conversionVariables, callback\)

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;&#x540D;</th>
      <th style="text-align:left">&#x7C7B;&#x578B;</th>
      <th style="text-align:left">&#x662F;&#x5426;&#x5FC5;&#x586B;</th>
      <th style="text-align:left">&#x53C2;&#x6570;&#x63CF;&#x8FF0;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">conversionVariables</td>
      <td style="text-align:left">object</td>
      <td style="text-align:left">&#x662F;</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">callback</td>
      <td style="text-align:left">&#x51FD;&#x6570;</td>
      <td style="text-align:left">&#x5426;</td>
      <td style="text-align:left">
        <p>callback {function (ret)}&#xFF1A;&#x6267;&#x884C;&#x5B8C;&#x8BFB;&#x53D6;&#x64CD;&#x4F5C;&#x540E;&#x7684;&#x56DE;&#x8C03;&#x51FD;&#x6570;&#x3002;</p>
        <p>ret &#x4E3A;callback &#x51FD;&#x6570;&#x7684;&#x53C2;&#x6570;&#xFF0C;&#x6709;&#x4E24;&#x4E2A;&#x5C5E;&#x6027;:</p>
        <p>status:&#x7ED3;&#x679C;2&#x79CD;true, false &#x90FD;&#x4E3A;&#x5E03;&#x5C14;&#x7C7B;&#x578B;</p>
        <p>msg:&#x7ED3;&#x679C;string&#x7C7B;&#x578B;</p>
      </td>
    </tr>
  </tbody>
</table>调用示例：

```javascript
var gio = api.require('GrowingIO');  //引用模块
gio.setEvar({
           "ekey":"evalue","Date":"2018-07-02"
      },function(ret, err){
           //回调函数事件处理
        });
```

### （4）setPeopleVariable\(peopleVariables, callback\)

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;&#x540D;</th>
      <th style="text-align:left">&#x7C7B;&#x578B;</th>
      <th style="text-align:left">&#x662F;&#x5426;&#x5FC5;&#x586B;</th>
      <th style="text-align:left">&#x53C2;&#x6570;&#x63CF;&#x8FF0;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">peopleVariables</td>
      <td style="text-align:left">object</td>
      <td style="text-align:left">&#x662F;</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">callback</td>
      <td style="text-align:left">&#x51FD;&#x6570;</td>
      <td style="text-align:left">&#x5426;</td>
      <td style="text-align:left">
        <p>callback {function (ret)}&#xFF1A;&#x6267;&#x884C;&#x5B8C;&#x8BFB;&#x53D6;&#x64CD;&#x4F5C;&#x540E;&#x7684;&#x56DE;&#x8C03;&#x51FD;&#x6570;&#x3002;</p>
        <p>ret &#x4E3A;callback &#x51FD;&#x6570;&#x7684;&#x53C2;&#x6570;&#xFF0C;&#x6709;&#x4E24;&#x4E2A;&#x5C5E;&#x6027;:</p>
        <p>status:&#x7ED3;&#x679C;2&#x79CD;true, false &#x90FD;&#x4E3A;&#x5E03;&#x5C14;&#x7C7B;&#x578B;</p>
        <p>msg:&#x7ED3;&#x679C;string&#x7C7B;&#x578B;</p>
      </td>
    </tr>
  </tbody>
</table>调用示例：

```javascript
var gio = api.require('GrowingIO');  //引用模块
gio.setPeopleVariable({
           "ekey":"evalue","Date":"2018-07-02"
      },function(ret, err){
            //回调函数事件处理
        });
```

### （5）setUserId\(userIdObject, callback\)

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;&#x540D;</th>
      <th style="text-align:left">&#x7C7B;&#x578B;</th>
      <th style="text-align:left">&#x662F;&#x5426;&#x5FC5;&#x586B;</th>
      <th style="text-align:left">&#x53C2;&#x6570;&#x63CF;&#x8FF0;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">userIdObject</td>
      <td style="text-align:left">object</td>
      <td style="text-align:left">&#x662F;</td>
      <td style="text-align:left">key:userId(string&#x7C7B;&#x578B;,&#x5FC5;&#x8981;key) value:(string&#x6216;&#x8005;number&#x7C7B;&#x578B;)</td>
    </tr>
    <tr>
      <td style="text-align:left">callback</td>
      <td style="text-align:left">&#x51FD;&#x6570;</td>
      <td style="text-align:left">&#x5426;</td>
      <td style="text-align:left">
        <p>callback {function (ret)}&#xFF1A;&#x6267;&#x884C;&#x5B8C;&#x8BFB;&#x53D6;&#x64CD;&#x4F5C;&#x540E;&#x7684;&#x56DE;&#x8C03;&#x51FD;&#x6570;&#x3002;</p>
        <p>ret &#x4E3A; callback &#x51FD;&#x6570;&#x7684;&#x53C2;&#x6570;&#xFF0C;&#x6709;&#x4E24;&#x4E2A;&#x5C5E;&#x6027;:</p>
        <p>status:&#x7ED3;&#x679C;2&#x79CD;true, false &#x90FD;&#x4E3A;&#x5E03;&#x5C14;&#x7C7B;&#x578B;</p>
        <p>msg:&#x7ED3;&#x679C;string&#x7C7B;&#x578B;</p>
      </td>
    </tr>
  </tbody>
</table>调用示例：

```javascript
var gio = api.require('GrowingIO');  //引用模块
  gio.setUserId({
             "userId":"GIO"
        },function(ret, err){
             //回调函数事件处理
        });
```

### （6）clearUserId\(callback\)

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;&#x540D;</th>
      <th style="text-align:left">&#x7C7B;&#x578B;</th>
      <th style="text-align:left">&#x662F;&#x5426;&#x5FC5;&#x586B;</th>
      <th style="text-align:left">&#x53C2;&#x6570;&#x63CF;&#x8FF0;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">callback</td>
      <td style="text-align:left">&#x51FD;&#x6570;</td>
      <td style="text-align:left">&#x5426;</td>
      <td style="text-align:left">
        <p>callback {function (ret)}&#xFF1A;&#x6267;&#x884C;&#x5B8C;&#x8BFB;&#x53D6;&#x64CD;&#x4F5C;&#x540E;&#x7684;&#x56DE;&#x8C03;&#x51FD;&#x6570;&#x3002;</p>
        <p>ret &#x4E3A;callback &#x51FD;&#x6570;&#x7684;&#x53C2;&#x6570;&#xFF0C;&#x6709;&#x4E24;&#x4E2A;&#x5C5E;&#x6027;:</p>
        <p>status:&#x7ED3;&#x679C;2&#x79CD;true, false &#x90FD;&#x4E3A;&#x5E03;&#x5C14;&#x7C7B;&#x578B;</p>
        <p>msg:&#x7ED3;&#x679C;string&#x7C7B;&#x578B;</p>
      </td>
    </tr>
  </tbody>
</table>调用示例：

```javascript
var gio = api.require('GrowingIO');  //引用模块
gio.clearUserId(function(ret, err){
             //回调函数事件处理
        });
```

## **6.常见问题**

### 1，提示无法检测到urlScheme?

 答：\(1\)查看config.xml是否配置正确

         \(2\)需要同步代码到云端,云编译生效

### 2，模拟器无法test? 

答： 只能真机测试

### 3 ，如何查看发送的数据? 

答： 您可以使用GrowingIO官网提供的[mobileDebugger](growingio-debugger/#growingio-mobile-debugger)工具来查看

### 4 ，此模块是否包含IDFA? 

答： 包含IDFA, GrowingIO 使用IDFA 来做来源管理激活设备的精确匹配，让你更好的衡量广告效果。

### 5 官网web提示未检测到sdk? 

答: 请使用正式版包来操作几次

## App Store 提交应用注意事项

GrowingIO会启用IDFA，所以在向App Store 提交应用时，需要：

• 对于问题 **Does this app use the Advertising Identifier \(IDFA\)**，选择 **YES**。

• 对于选项**Attribute this app installation to a previously served advertisement**，打勾。

对于选项**Attribute an action taken within this app to a previously served advertisement**，打勾。

