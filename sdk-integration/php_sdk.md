# PHP 埋点 SDK（Beta）

## 集成PHP SDK

1. 下载 PHP 文件并解压：[https://assets.growingio.com/php/sdk/GrowingIO.php](https://assets.growingio.com/php/sdk/GrowingIO.php)
2. 将sdk加入到您的产品代码中

参考如下Demo程序：

```php
<?php
/**
 * GrowingIO PHP SDK Demo
 * User: tianyi
 * Date: 2018-12-28
 * Time: 13:34
 */

include_once "GrowingIO.php";

// 请在您调试前，将accountID修改为您的项目AccountID
// 所有自定义事件需要提前在GrowingIO产品中进行定义
// 所有自定义事件的属性也需要提前在GrowingIO产品中进行定义

$accountID = "";

$gio = GrowingIO::getInstance($accountID, array());

//$gio->track($登录用户ID，$自定义事件关键字， array(自定义变量关键字=>自定义变量值));
$gio->track("17729", "BuyProduct", array("product_type" => "水果", "amount" => "5"));

?>
```

## 配置PHP SDK

目前PHP SDK支持Debug模式，可以在标准输出中输出实际的自定义事件内容，如：

```php
$gio = GrowingIO::getInstance($accountID, array("debug"=>true));
```

## 程序调试

GrowingIO建议您按照如下步骤进行埋点数据的开发联调

1. 在GrowingIO的网站中创建自定义事件以及对应的自定义事件变量
2. 在您的PHP项目中集成GrowingIO PHP SDK的依赖（首次集成需要）
3. 参考上面的文档使用debug模式初始化GrowingIO PHP SDK
4. 在您的项目中找到合适的埋点位置，参考上面的例子填入自定义事件需要的字段
5. 执行对应修改部分的单元测试，或者编写一段测试程序运行修改部分的代码，确保触发埋点事件
6. 在输出的日志中查找是否包含期望事件内容
7. 在以上步骤全部成功后，去除debug模式

