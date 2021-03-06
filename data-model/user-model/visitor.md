# 访问用户

## 访问用户定义

访问用户是GrowingIO对访问您的应用（包括网页、App、微信小程序等，下同）的用户的一种识别机制。

每一个访问您的应用的用户都会在对应的设备中生成并记录一个唯一的ID，我们称之为访问用户ID。

对于不同平台类型的应用，GrowingIO提供了多种识别方案，从而尽可能的实现对用户的唯一标识。

同一个项目中相同的访问用户ID会识别为一个用户。

## 访问用户ID生成机制

### WEB JS SDK（Web页面与H5页面）

GrowingIO会使用随机UUID的方法生成访问用户ID，并将之记录在浏览器的Cookie中

### iOS SDK

GrowingIO会按照下面的顺序获取访问用户ID：

1. 用户自定义访问用户ID
2. IDFA
3. IDFV
4. 随机GUID

并将之存储至keychain中。

### Android SDK

GrowingIO会按照如下顺序获取访问用户ID：

1. Android ID
2. IMEI
3. 随机UUID

进行 MD5 加密后将之存储至设备本地文件系统。

### 小程序 SDK

GrowingIO默认使用UUID（随机UUID的方法生成访问用户ID，并将之记录在浏览器的Cookie中）来作为访问用户ID，您也可以通过 API 设置使用openid作为访问用户ID。

访问用户ID将保存到微信浏览器的cookie中。

## 常见问题

1. 访问用户在哪些情况下会发生变化？

   JS：用户使用浏览器的隐身模式、用户手动清空Cookie、同一个用户使用多个浏览器

   iOS：App的发布证书变动、用户还原了手机

   Android：用户删除了应用重新安装或者是清除了数据

   微信小程序：用户长按小程序入口的小程序图标，删除了小程序；更换了手机设备；手动更新了微信的缓存等。



