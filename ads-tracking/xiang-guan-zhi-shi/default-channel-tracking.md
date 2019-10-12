# 默认的渠道来源跟踪

* [1.访问来源](default-channel-tracking.md#1)
* [2.搜索词](default-channel-tracking.md#2)
* [3.自主投放追踪](default-channel-tracking.md#3)
* [4.公司域名与GIO短链建立映射关系](default-channel-tracking.md#4)
  * [4.1需求背景](default-channel-tracking.md#41)
  * [4.2解决方案](default-channel-tracking.md#42)
* [5.自主调用Api接口创建链接](default-channel-tracking.md#5)
  * [5.1输入](default-channel-tracking.md#51)
  * [5.2输出](default-channel-tracking.md#52)
  * [5.3接口请求示例](default-channel-tracking.md#53)

### 1. 访问来源 <a id="1"></a>

GrowingIO 会根据一次访问中的第一个页面的 referrer 判断访问来源，将referrer\_domain 作为访问来源；同时，GrowingIO 默认启用了「最终非直接点击」归因模型，回溯周期是 30 天（2016 年 1 月 30 日以前，回溯周期是 7 天）。  
以「今天的访问来源」举例：

* case1:某个用户今天最后一次访问的第一个页面referrer\_domain是google.com（外站），那么该次访问的来源会被判定为google.com；
* case2: 某个用户今天最后一次访问是直接访问本站\(referrer\_domain是本站或者没有referrer\)，由于启用了『最终非直接点击』归因模型，回溯期是30天；GrowingIO会追踪该用户在当天及过去30天中最后一次通过外部网站进入的访问，并将这次访问的referrer\_domain作为访问来源；如果回溯30天发现用户没有通过站外渠道进入网站，那么本次访问的来源判定为「直接访问」

如何判定本站和外站：  
GrowingIO通过项目 ID\(AI\) 来区分是否外站，如果当前 referrer 页面上有相同的项目 ID，则会被当成内部跳转，否则会外站。例如：用户直接输入 URL 或通过收藏夹等方式访问，没有 referrer ，会被记为直接访问。

最终非直接点击：  
该模型会忽略直接流量，将用户访问 100% 归功于用户在本次访问之前点击访问的最后一个站外渠道。该模型认为，如果直接访问来自被站外渠道吸引的用户，那么您可能希望过滤直接访问流量，仅关注站外渠道

下图中，回溯 30 天，「最终非直接点击」对应的是前天 Baidu 带来的访问；因此会将本次访问的来源归因为 Baidu

![](../../.gitbook/assets/bang-zhu-wen-dang-1.jpg)

下图中，回溯30天，找不到「最终非直接点击」；因此会将本次访问的来源归因为「直接访问」

![](../../.gitbook/assets/bang-zhu-wen-dang-2.jpg)

### 2. 搜索词 <a id="2"></a>

用户使用搜索引擎，并在搜索框中输入搜索词，如果用户通过搜索引擎的返回结果访问您的网站，我们会记录用户在搜索框中输入的「搜索词」。

GrowingIO 针对 Google \(含 DoubleClick \)、百度\( Baidu \)、搜狗\(Sogou\)、好搜\(Haosou\)、Bing 等 5 个搜索引擎的搜索词进行解析。注意，其中由于 Google 和百度对自然搜索词进行屏蔽加密，GrowingIO 会将此类流量归并到（xx 自然流量）类别下统计。

### 3. 自主投放追踪 <a id="3"></a>

我们提供utm参数和自定义参数的方式跟踪您网站的自主投放渠道  
具体请参照：[自主投放URL构建工具](https://assets.growingio.com/help/doc/%E8%AF%A5%E6%96%87%E6%A1%A3%E7%94%A8%E6%9D%A5%E7%94%9F%E6%88%90%E6%8A%95%E6%94%BEURL_V2.0.xlsm)

utm参数与GrowingIO提供的维度对应关系如下：

| 名称 | 维度 | 例子 | 含义 |
| :--- | :--- | :--- | :--- |
| 广告来源 | utm\_source | utm\_source＝baidu | 这个广告投放在百度上 |
| 广告媒介 | utm\_medium | utm\_medium＝cpc | 广告类型是点击付费 |
| 广告名称 | utm\_campaign | utm\_campaign＝tryitfree | 这次推广名称:tryitfree |
| 广告内容 | utm\_content | utm\_content=textlink | 广告内容是textlink |
| 广告关键字 | utm\_term | utm\_term＝免费试用 | 广告关键字:免费试用 |

GrowingIO直接支持百度统计的参数解析；如果您的自主投放追踪使用的是百度统计参数，您可以直接在GrowingIO单图功能中对用对应维度制图。

维度对应关系如下表所示：

| 名称（百度统计） | hm 参数名（百度统计） | 含 义 | GIO 参数名 | GIO 维度 |
| :--- | :--- | :--- | :--- | :--- |
| 媒介平台 （＊） | hmsr | 广告所投放的平台，如：新浪、搜狐等 | utm\_source | 广告来源 |
| 计划名称 | hmpl | 广告所属的推广计划，如：元旦促销 | utm\_medium | 广告媒介 |
| 单元名称 | hmcu | 广告所属的推广单元，如：七夕促销 | utm\_campaign | 广告名称 |
| 关键词 | hmkw | 该条广告对应的关键词 | utm\_term | 广告关键字 |
| 创意 | hmci | 广告内容的简要描述信息 | utm\_content | 广告内容 |

如果url中有中文字符，建议使用utf-8 encode中文字符，但请保留utm关键词。

### 4. 公司域名与GIO短链建立映射关系 <a id="4"></a>

#### 4.1 需求背景： <a id="41"></a>

场景一：针对诸如百度SEM等投放渠道，要求落地页链接为帐户注册主域名

场景二： 客户有品牌强化需求，希望用自己域名代替 GIO 短链域名

#### 4.2 解决方案：

贵司SRE运维人员或其他有权限解析域名的管理员新建一个子域名进行CNAME解析，替换GrowingIO后台监测域名，并且使用http协议。

#### 4.3 流程示例：

1、以百度后台为例，假设百度后台申请账号时，填写的主域名为 domain.com；

2、GrowingIO后台，生成的监测链接为 https://gio.ren/w/rABC；

3、域名管理员，新建子域名 tc.domain.com，使用 CNAME 解析到 gio.ren；

4、在投放使用时，将监测链接，gio.ren替换为子域名tc.domain.com：http://tc.domain.com/w/rABC。

（以上 tc.domain.com 仅为示例，具体看贵司的二级域名使用情况。）

#### 4.4 特殊说明：

{% hint style="info" %}
目前 GrowingIO 监测链接生成使用了 s.growingio.com 及 gio.ren ，如两域名都在投放使用，在映射时需对二者分别进行配置。
{% endhint %}

配置举例：

1、s.growingio.com 使用 s.domain.com 替换；

2、gio.ren 使用 tc.domain.com 替换。

### 5. 自主调用 API 接口创建链接 <a id="5"></a>

{% hint style="danger" %}
下线通知：网页推广监测链接创建 API 已合并至广告监测链接创建服务 API 下，此 API 接口计划于 19 年 12 月 1 日下线，请您尽快切换至新版 API 接口，文档位置：[推广网页创建 API ](../../api/ads-tracking-api.md#normallink-2) 。
{% endhint %}

POST **https://gta.growingio.com/api/v1/projects/project\_id/activities**

上述地址中的 project\_id 取值请参考[“GrowingIO接口认证”](../../api/authentication.md)章节的“术语”。

{% hint style="danger" %}
注意：将以下内容作为JSON Body，post到上述链接。**该接口已废弃，请尽快升级到**[**新版本接口**](../../api/ads-tracking-api.md#normallink-1)\*\*\*\*
{% endhint %}

#### 5.1 输入 <a id="51"></a>

| 字段名称\(\*为必填\) | 填写示例 | 中文含义 |
| :--- | :--- | :--- |
| \*advertiser | 普通推广为"none" | 推广平台 |
| \*href | www.growingio.com | 目标链接 |
| \*name | 12.16增长大会 | 名称\(落地页命名\)不可重复 |
| \*platform | 网页为"web" 移动应用为"mobile" | 平台 |
| \*utmCampaign | 增长大会活动首页 | 广告名称 |
| \*utmMedium | CPC | 广告媒介 |
| \*utmSource | 广点通 | 广告来源 |
| utmContent | 活动介绍和报名表单 | 广告内容 |
| utmTerm | 增长Growingio | 广告关键词 |
| bundleId | com.growing.growingapp | 苹果包名 |
| packageName | com.growingio.android.growingio.app | 安卓包名 |
| comment | 推广预算两万 | 备注 |

#### 5.2 输出 <a id="52"></a>

| 字段名称 | 示例 | 中文含义 |
| :--- | :--- | :--- |
| id | a9a7jrPB | -- |
| name | 12.16增长大会 | 名称\(落地页命名\) |
| href | www.growingio.com | 目标链接 |
| platform | 网页为"web" 移动应用为"mobile" | 平台 |
| utmCampaign | 增长大会活动首页 | 广告名称 |
| utmMedium | CPC | 广告媒介 |
| utmSource | 广点通 | 广告来源 |
| utmContent | 活动介绍和报名表单 | 广告内容 |
| utmTerm | 增长Growingio | 广告关键词 |
| comment | 推广预算两万 | 备注 |
| advertiser | 普通推广为"none" | 推广平台 |
| projectId | 5138bdb96 | 项目ID |
| creatorName | Jacky | 创建人 |
| createdAt | 1484397370856 | 创建时间 |
| shortUrl | **https://s.growingio.com/6XNmKl** | 用于投放的短链 |

#### 5.3 接口请求示例 <a id="53"></a>

```text
POST /api/v1/projects/nxog09md/activities HTTP/1.1
Host:  gta.growingio.com
Content-Type: application/json
X-Client-Id: zzzzzzzzzzzzn5cvuvzzzzzzzzz //来自于授权
Authorization: 1yL0Y3C8hm5zPwMDdgsnBBB0tZarChOnpBek057MKqhqkqvPdYyebOHtl5xANeF //来自于授权
Cache-Control: no-cache

{
        "advertiser": "none",
        "href": "http://m.youlanw.com/sh",
        "name": "12.16增长_Hello_World大会",
        "platform": "web",
        "utmCampaign": "增长大会活动首页",
        "utmMedium": "CPC_3",
        "utmSource": "广点通"
}
```

