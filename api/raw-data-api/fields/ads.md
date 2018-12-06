# 广告相关数据

## ads\_track\_activation——广告激活事件

| 原始数据导出2.0字段名称 | 原始数据导出1.0字段名称 | 字段格式 | 字段说明 | 示例值 | 备注 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| visitUserId | userId | string\(64\) | 访问用户ID（visit user id） | fc55728b-41ab-42ff-8b1f-714e44c65fd6 | 匿名的访问用户ID，由GrowingIO自动生成。 |
| idfa | idfa | string\(40\) | IDFA（id for advertiser） | F7A29C20-FD0D-4B68-BF4D-39EFC5C9A9C2 | iOS平台用于广告监测的ID：IDFA（id for advertiser） |
| imei | imei | string\(40\) | IMEI | 100500636E9AA9 | Android平台用于广告监测的ID |
| uuid | uuid | string\(40\) | UUID | 00000000-37fc-f5ob-1e8d-484b190312e1 | 用于广告监测的ID的UUID格式 |
| androidId | androidid | string\(40\) | Android ID | 2cab90e2a3b489ed | SSAID，又称为Android ID |
| ip | ip | string\(15\) | IP地址（ip address） | 127.0.0.1 | ​ |
| userAgent | useragent | string\(512\) | User Agent，例如浏览器信息或者移动设备信息 | Mozilla/5.0 \(iPhone; CPU iPhone OS 11\_2\_6 like Mac OS X\) AppleWebKit/604.5.6 \(KHTML, like Gecko\) Mobile/15D100 | ​ |
| platform | platform | string\(20\) | 平台（platform） | iOS | ​ |
| operatingSystemVersion | osversion | string\(50\) | 操作系统版本（operating system version） | iOS 11.2.6 | ​ |
| sendTime | sendtime | bigint | 发送时间（send time） | 1521315962327 | 请求在SDK发送的时间戳 |
| linkId | link\_id | string\(20\) | 链接ID（link id） | Yo1KJXRl | 监测链接ID |
| campaignId | campaign\_id | string\(20\) | 活动ID（campaign id） | GPndl79Y | 活动ID |
| channelId | channel\_id | string\(20\) | 渠道ID（channel id） | inmobi | 渠道ID |
| linkName | link\_name | string\(60\) | 链接名称 | 测试链接 | 2018/5/8 开始生效 |
| campaignName | campaign\_name | string\(60\) | 活动名称 | 双十一推广 | 2018/5/8 开始生效 |
| channelName | channel\_name | string\(60\) | 渠道名称 | 今日头条 | 2018/5/8 开始生效 |
| adsVariable | N/A | Map&lt;String, String&gt; | 链接维度参数 | {''city" -&gt; "beijing"} | 2018/8/7 开始生效 |

## ads\_track\_click——广告点击事件

| 原始数据导出2.0字段名称 | 原始数据导出1.0字段名称 | 字段格式 | 字段说明 | 示例值 | 备注 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| idfa | idfa | string\(40\) | IDFA（id for advertiser） | F9AFF106-3C41-4B78-B20B-D3BB166DA620 | iOS平台用于广告监测的ID：IDFA（id for advertiser） |
| imei | imei | string\(40\) | IMEI | 100500636E9AA9 | Android平台用于广告监测的ID |
| uuid | uuid | string\(40\) | UUID | 00000000-37fc-f5ob-1e8d-484b190312e1 | 用于广告监测的ID的UUID格式 |
| androidId | androidId | string\(40\) | Android ID | 2cab90e2a3b489ed | SSAID，又称为Android ID |
| ip | ip | string\(15\) | IP地址（ip address） | 127.0.0.1 | IP地址（ip address） |
| userAgent | useragent | string\(512\) | User Agent，例如浏览器信息或者移动设备信息 | Mozilla/5.0 \(Linux; Android 4.0.4; A31 Build/A31\) AppleWebKit/537.36 \(KHTML, like Gecko\) Version/4.0 Chrome/30.0.0.0 Mobile Safari/537.36 | ​ |
| platform | platform | string\(20\) | 平台（platform） | iOS | 访问所属平台，可能值为 iOS / Android / Web 等 |
| operatingSystemVersion | osversion | string\(50\) | 操作系统版本（operating system version） | iOS 11.2.6 | ​ |
| eventTime | eventtime | bigint | 点击请求时间（click request time） | 1521315962320 | 请求在SDK发送的时间戳 |
| linkId | link\_id | string | 链接ID（link id） | Yo1KJXRl | 监测链接ID |
| campaignId | campaign\_id | string\(20\) | 活动ID（campaign id） | GPndl79Y | 活动ID |
| channelId | channel\_id | string\(20\) | 渠道ID（channel id） | inmobi | 渠道ID |
| linkName | link\_name | string\(60\) | 链接名称 | 测试链接 | 2018/5/8 开始生效 |
| campaignName | campaign\_name | string\(60\) | 活动名称 | 双十一推广 | 2018/5/8 开始生效 |
| channelName | channel\_name | string\(60\) | 渠道名称 | 今日头条 | 2018/5/8 开始生效 |
| adsVariable | N/A | Map&lt;String, String&gt; | 链接维度参数 | {''city" -&gt; "beijing"} | 2018/8/7 开始生效 |

