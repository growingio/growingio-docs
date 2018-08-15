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

