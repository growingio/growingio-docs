# 原始数据导出格式及处理建议

## 导出格式

在原始数据导出接口中导出的所有数据皆为csv格式:

* 分隔符： ,
* 包围符："
* 转义符：\

## 处理建议与示例

### Spark处理

建议下载数据后，将下载的压缩文件放于hdfs的以日期建立目录结构，同一小时或者同一天的数据放在同一目录下，然后通过spark streaming的fileStream接口监控根目录，读取变动的文件内容。

```text
streamingContext.fileStream[KeyClass, ValueClass, InputFormatClass](dataDirectory)
```

在依赖中添加：

```text
groupId: com.databricksartifactId: spark-csv_2.10version: 1.4.0
```

具体数据操作参考spark-csv\([https://github.com/databricks/spark-csv](https://github.com/databricks/spark-csv)\)

### Scala/Java程序处理示例

推荐使用 `org.apache.commons.csv` 来处理下载到本地的数据文件，如下面的例子：

```text
import java.io.{BufferedReader, File, FileInputStream, InputStreamReader}
import org.apache.commons.csv.{CSVFormat, CSVParser, QuoteMode}
import scala.collection.JavaConverters._

object Test extends App {
  val file = new File("xxx")
  val br = new BufferedReader(new InputStreamReader(new FileInputStream(file)))
  val csvFileFormat = CSVFormat.DEFAULT.withEscape('\\').withQuote('"')
  val csvParser = new CSVParser(br, csvFileFormat)
  val records = csvParser.getRecords

  for (record <- records.asScala) {
    val sb = new StringBuilder()
    val length = record.size()
    (0 until length).foreach(i => {
      sb.append(record.get(i))
      sb.append(",")
    })
    println(sb.toString)
  }
}
```

### Python处理建议及示例

```text
import csv

with open(FILE_NAME, "rb") as f:
    reader = csv.reader(f, quotechar='"', escapechar='\\')
    for line in reader:
        print(line)
```

## 导入到数据仓库示例

### 导入到Hive

1. 使用Hive新建外部表，如下图所示。
2. 使用hadoop fs -put /xx.csv /tmp/test\_export/ 将csv放到外部表目录下。

```text
CREATE EXTERNAL TABLE TEST_EXPORT
(
sessionId STRING,
time BIGINT,
sendTime BIGINT,
pageTime BIGINT,
domain STRING,
page STRING,
queryParameters STRING,
eventName STRING,
eventNumber DOUBLE,
eventVariable map<string, string>,
loginUserId STRING
)
ROW FORMAT SERDE EATE EXTE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
STORED AS TEXTFILE
location '/tmp/test_export'
tblproperties ("skip.header.line.count"="1", "quote.delim"="\"", "escape.delim"="\\")
```

### 利用Spark导入到其他数据仓库

1. 获取DataFrame，代码如下。
2. 使用databricks提供的函数Load到Hive数据库\(这里省去spark和hive的配置\)，如： df.select\("sessionId", "time", "sendTime", "pageTime", "domain", "page", "queryParameters", "eventName", "eventNumber", "eventVariable", "loginUserId"\).write.mode\("append"\).insertInto\("TEXT\_EXPORT"\)

```text
val df = spark.read
	.option("header","true")
	.option("escape", "\\")
	.option("quote", "\"")
	.csv("filePath")
```

## **md5进行文件完整性校验**

用户如果对文件完整性有担心，可以对[原始数据导出 API](https://docs.growingio.com/docs/api/raw-data-api/)第三步下载时response的headers中x-amz-meta-md5-hash的value值（文件的md5）进行校验。若校验未通过，可重启第三步，轮询获取。

eg:    Headers信息如下

![](../../.gitbook/assets/image%20%28144%29.png)

         md5校验结果

![](../../.gitbook/assets/image%20%2842%29.png)

