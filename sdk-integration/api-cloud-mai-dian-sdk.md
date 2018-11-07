---
description: GrowingIO 埋点 SDK 仅自动采集设备信息和您埋点内容数据
---

# API Cloud埋点SDK

## **1.配置Config.xml文件** 

**使用此模块前先配置config.xml文件,方法如下**

• 名称：GrowingIO

• 必要参数: accountId, urlScheme

• 可选参数: trackerHost, reportHost, dataHost, gtaHost, wsHost, zone

```text
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

  iOS模块包：[下载](https://github.com/growingio/APICloud-growingio/blob/develop/iOS/iOS/GrowingIO_iOS.zip)

  Android模块包：[下载](https://github.com/growingio/APICloud-growingio/blob/develop/android/GrowingIO.zip)

## **3.添加模块**

  开发控制台-&gt; 选择应用-&gt; 模块-&gt; 自定义模块-&gt; 点击上传-&gt; 编写自定义模块信息\(注意:模块名称要和zip包名称一致\)-&gt; 点击添加模块"+" -&gt; 在已添加模块中确认是否成功添加。

![](../.gitbook/assets/image%20%28109%29.png)

## **4.Android的额外操作**

     Android云编译Loader为AppLoader， 使用自定义模块式需要编译Android自定义loader, 否则会出现模块未绑定错误, 另外需要注意的是在使用自定义loader时 请勾选 **使用升级环境编译**选项

具体步骤如下:

（1）模块-自定义loader: 请勾选 **使用升级环境编译**

![](../.gitbook/assets/image%20%28135%29.png)

（2）云编译时， 请勾选 **使用升级环境编译**

![](../.gitbook/assets/image%20%28141%29.png)

## **5.插件支持的方法**

### （1）init\(\)

       **此接口为Android初始化， 在require后调用，iOS不需要，iOS已自动初始化**建议在require GrowingIO时调用此接口

```text
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
      <th style="text-align:left">参数名</th>
      <th style="text-align:left">类型</th>
      <th style="text-align:left">是否必填</th>
      <th style="text-align:left">参数描述</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">event</td>
      <td style="text-align:left">object</td>
      <td style="text-align:left">是</td>
      <td style="text-align:left">
        <p>key:eventId(string类型,必要key) value:(string类型)</p>
        <p>key:eventLevelVariable(string类型,非必要key) value:(object类型)</p>
        <p>key:number(string类型, 非必要key) value(number类型)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">callback</td>
      <td style="text-align:left">函数</td>
      <td style="text-align:left">否</td>
      <td style="text-align:left">
        <p>allback {function (ret)}：执行完读取操作后的回调函数。</p>
        <p>ret 为 callback 函数的参数，有两个属性:</p>
        <p>status:结果2种 true, false 都为布尔类型</p>
        <p>msg:结果string类型</p>
      </td>
    </tr>
  </tbody>
</table>### （3）setEvar\(conversionVariables, callback\)

<table>
  <thead>
    <tr>
      <th style="text-align:left">参数名</th>
      <th style="text-align:left">类型</th>
      <th style="text-align:left">是否必填</th>
      <th style="text-align:left">参数描述</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">conversionVariables</td>
      <td style="text-align:left">object</td>
      <td style="text-align:left">是</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">callback</td>
      <td style="text-align:left">函数</td>
      <td style="text-align:left">否</td>
      <td style="text-align:left">
        <p>callback {function (ret)}：执行完读取操作后的回调函数。</p>
        <p>ret 为callback 函数的参数，有两个属性:</p>
        <p>status:结果2种true, false 都为布尔类型</p>
        <p>msg:结果string类型</p>
      </td>
    </tr>
  </tbody>
</table>### （4）setPeopleVariable\(peopleVariables, callback\)

<table>
  <thead>
    <tr>
      <th style="text-align:left">参数名</th>
      <th style="text-align:left">类型</th>
      <th style="text-align:left">是否必填</th>
      <th style="text-align:left">参数描述</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">peopleVariables</td>
      <td style="text-align:left">object</td>
      <td style="text-align:left">是</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">callback</td>
      <td style="text-align:left">函数</td>
      <td style="text-align:left">否</td>
      <td style="text-align:left">
        <p>callback {function (ret)}：执行完读取操作后的回调函数。</p>
        <p>ret 为callback 函数的参数，有两个属性:</p>
        <p>status:结果2种true, false 都为布尔类型</p>
        <p>msg:结果string类型</p>
      </td>
    </tr>
  </tbody>
</table>### （5）setUserId\(userIdObject, callback\)

<table>
  <thead>
    <tr>
      <th style="text-align:left">参数名</th>
      <th style="text-align:left">类型</th>
      <th style="text-align:left">是否必填</th>
      <th style="text-align:left">参数描述</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">userIdObject</td>
      <td style="text-align:left">object</td>
      <td style="text-align:left">是</td>
      <td style="text-align:left">key:userId(string类型,必要key) value:(string或者number类型)</td>
    </tr>
    <tr>
      <td style="text-align:left">callback</td>
      <td style="text-align:left">函数</td>
      <td style="text-align:left">否</td>
      <td style="text-align:left">
        <p>callback {function (ret)}：执行完读取操作后的回调函数。</p>
        <p>ret 为 callback 函数的参数，有两个属性:</p>
        <p>status:结果2种true, false 都为布尔类型</p>
        <p>msg:结果string类型</p>
      </td>
    </tr>
  </tbody>
</table>### （6）clearUserId\(callback\)

<table>
  <thead>
    <tr>
      <th style="text-align:left">参数名</th>
      <th style="text-align:left">类型</th>
      <th style="text-align:left">是否必填</th>
      <th style="text-align:left">参数描述</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">callback</td>
      <td style="text-align:left">函数</td>
      <td style="text-align:left">否</td>
      <td style="text-align:left">
        <p>callback {function (ret)}：执行完读取操作后的回调函数。</p>
        <p>ret 为callback 函数的参数，有两个属性:</p>
        <p>status:结果2种true, false 都为布尔类型</p>
        <p>msg:结果string类型</p>
      </td>
    </tr>
  </tbody>
</table>## **6.常见问题**

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

