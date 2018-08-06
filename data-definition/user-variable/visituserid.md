# 访问用户变量（小程序）

* [访问用户简介](visituserid.md#fang-wen-yong-hu-jian-jie)
* [访问用户变量定义](visituserid.md#fang-wen-yong-hu-bian-liang-ding-yi)

## 访问用户简介

访问用户是GrowingIO对访问您的应用（包括网页、App、微信小程序等，下同）的用户的一种识别机制。每一个访问您的应用的用户都会在对应的设备中生成并记录一个唯一的ID，我们称之为访问用户ID。

对于不同平台类型的应用，GrowingIO提供了多种识别方案，从而尽可能的实现对用户的唯一标识。访问用户在不同平台的标识请参考[访问用户模型文档](../../data-model/user-model/visitor.md)。

## 访问用户变量定义

给访问用户\(未注册你的服务的账号的用户\)附上额外的信息，便于后续做用户信息相关分析。**需要注意：当前访问用户变量只在小程序中支持**。

[小程序 SDK](../../sdk-integration/mina-sdk.md#fang-wen-yong-hu-bian-liang) 中给出了接口定义，请参考。



