# Accessing MapR Database in Zeppelin Using the MapR Database Binary Connector

This section contains an example of an Apache Spark job that uses the MapR Database Binary Connector for Apache Spark to write and read a MapR Database Binary table. You can run this example using either the Livy or Spark interpreter. 

Before running the example, make sure you have configured either your [Livy](https://mapr.com/docs/61/Zeppelin/ConfigureLivyInterpreter.html#task_t1d_4yj_qbb__section_zwx_pdk_qbb) or [Spark](https://mapr.com/docs/61/Zeppelin/ConfigureSparkInterpreter.html#task_t1d_4yj_qbb__section_zwx_pdk_qbb) interpreter to run Spark jobs.

This example is derived from the example at [SparkSQL and DataFrames](https://mapr.com/docs/61/Spark/SparkSQLandDataFrames.html#concept_wl2_jk4_gz). That page provides a more detailed explanation of the code.


### Example

Set your interpreter to either `%livy.spark` or `%spark`, depending on whether you are using Livy or Spark.

Run the following Scala code examples using the MapR Database Binary Connector in your notebook:


<details> 
  <summary>Reading and writing data using DataFrame</summary>
    
```
import org.apache.spark.sql.{DataFrame, SQLContext}
import org.apache.spark.{SparkContext, SparkConf}
import org.apache.spark.sql.datasources.hbase.HBaseTableCatalog
case class HBaseRecordClass(
  col0: String,
  col1: Boolean,
  col2: Double,
  col3: Float,
  col4: Int,
  col5: Long,
  col6: Short,
  col7: String,
  col8: Byte)
object HBaseRecord {
  def apply(i:Int): HBaseRecordClass = {
    val s = "row" + "%03d".format(i)
    new HBaseRecordClass(s,
      i % 2 == 0,
      i.toDouble,
      i.toFloat,
      i,
      i.toLong,
      i.toShort,
      s"String$i extra",
      i.toByte)
  }
}
val tableName = "/user/mapruser1/test1" 
val cat = s"""{
          |"table":{"namespace":"default", "name":"$tableName"},
          |"rowkey":"key",
          |"columns":{
            |"col0":{"cf":"rowkey", "col":"key", "type":"string"},
            |"col1":{"cf":"cf1", "col":"col1", "type":"boolean"},
            |"col2":{"cf":"cf2", "col":"col2", "type":"double"},
            |"col3":{"cf":"cf3", "col":"col3", "type":"float"},
            |"col4":{"cf":"cf4", "col":"col4", "type":"int"},
            |"col5":{"cf":"cf5", "col":"col5", "type":"bigint"},
            |"col6":{"cf":"cf6", "col":"col6", "type":"smallint"},
            |"col7":{"cf":"cf7", "col":"col7", "type":"string"},
            |"col8":{"cf":"cf8", "col":"col8", "type":"tinyint"}
            |}
          |}""".stripMargin
val sqlContext = new SQLContext(sc)
import sqlContext.implicits._

def withCatalog(cat: String): DataFrame = {
  sqlContext
    .read
    .options(Map(HBaseTableCatalog.tableCatalog->cat))
    .format("org.apache.hadoop.hbase.spark")
    .load()
}
val data = (0 to 255).map { i =>
  HBaseRecord(i)
}
sc.parallelize(data).toDF.write.options(
  Map(HBaseTableCatalog.tableCatalog -> cat, HBaseTableCatalog.newTable -> "5")).format("org.apache.hadoop.hbase.spark").save()
val df = withCatalog(cat)
df.show
```
</details> 

The output looks like the following:

```
+----+--------------+-----+----+----+------+----+----+----+
|col4|          col7| col1|col3|col6|  col0|col8|col2|col5|
+----+--------------+-----+----+----+------+----+----+----+
|   0| String0 extra| true| 0.0|   0|row000|   0| 0.0|   0|
|   1| String1 extra|false| 1.0|   1|row001|   1| 1.0|   1|
|   2| String2 extra| true| 2.0|   2|row002|   2| 2.0|   2|
|   3| String3 extra|false| 3.0|   3|row003|   3| 3.0|   3|
|   4| String4 extra| true| 4.0|   4|row004|   4| 4.0|   4|
|   5| String5 extra|false| 5.0|   5|row005|   5| 5.0|   5|
|   6| String6 extra| true| 6.0|   6|row006|   6| 6.0|   6|
|   7| String7 extra|false| 7.0|   7|row007|   7| 7.0|   7|
|   8| String8 extra| true| 8.0|   8|row008|   8| 8.0|   8|
|   9| String9 extra|false| 9.0|   9|row009|   9| 9.0|   9|
|  10|String10 extra| true|10.0|  10|row010|  10|10.0|  10|
|  11|String11 extra|false|11.0|  11|row011|  11|11.0|  11|
|  12|String12 extra| true|12.0|  12|row012|  12|12.0|  12|
|  13|String13 extra|false|13.0|  13|row013|  13|13.0|  13|
|  14|String14 extra| true|14.0|  14|row014|  14|14.0|  14|
|  15|String15 extra|false|15.0|  15|row015|  15|15.0|  15|
|  16|String16 extra| true|16.0|  16|row016|  16|16.0|  16|
|  17|String17 extra|false|17.0|  17|row017|  17|17.0|  17|
|  18|String18 extra| true|18.0|  18|row018|  18|18.0|  18|
|  19|String19 extra|false|19.0|  19|row019|  19|19.0|  19|
+----+--------------+-----+----+----+------+----+----+----+
```


<details> 
  <summary>Reading and writing data using RDD</summary>
    
```import org.apache.hadoop.fs.{FileSystem, Path}
import org.apache.hadoop.hbase.client.{HBaseAdmin, Put, Get, Result}
import org.apache.hadoop.hbase.spark.HBaseContext
import org.apache.hadoop.hbase.spark.HBaseRDDFunctions._
import org.apache.hadoop.hbase.util.{Bytes => By}
import org.apache.hadoop.hbase.{CellUtil, HBaseConfiguration, HColumnDescriptor, HTableDescriptor, TableName}


val workingDir = "/user/" + sc.sparkUser + "/zeppelin/samples"
val tablePath = workingDir + "/sample_table_binary_rdd"
val columnFamily = "sample_cf"


// Clean directory in MapR-FS
val fs = FileSystem.get(sc.hadoopConfiguration)
val workingPath = new Path(workingDir)
if(!fs.exists(workingPath)) fs.mkdirs(workingPath)

// Initialize HBaseContext and HBaseAdmin
val hbaseConf = HBaseConfiguration.create()
val hbaseContext = new HBaseContext(sc, hbaseConf)
val hbaseAdmin = new HBaseAdmin(hbaseConf)

// Create empty table
if (hbaseAdmin.tableExists(tablePath)) {
    hbaseAdmin.disableTable(tablePath)
    hbaseAdmin.deleteTable(tablePath)
}
val tableDesc = new HTableDescriptor(tablePath)
tableDesc.addFamily(new HColumnDescriptor(By.toBytes(columnFamily)))
hbaseAdmin.createTable(tableDesc)


// Put data into table
val putRDD = sc.parallelize(Array(
  (By.toBytes("1"), (By.toBytes(columnFamily), By.toBytes("1"), By.toBytes("1"))),
  (By.toBytes("2"), (By.toBytes(columnFamily), By.toBytes("1"), By.toBytes("2"))),
  (By.toBytes("3"), (By.toBytes(columnFamily), By.toBytes("1"), By.toBytes("3"))),
  (By.toBytes("4"), (By.toBytes(columnFamily), By.toBytes("1"), By.toBytes("4"))),
  (By.toBytes("5"), (By.toBytes(columnFamily), By.toBytes("1"), By.toBytes("5")))
))

putRDD.hbaseBulkPut(hbaseContext, TableName.valueOf(tablePath),
  (putRecord) => {
    val put = new Put(putRecord._1)
    val (family, qualifier, value) = putRecord._2
    put.addColumn(family, qualifier, value)
    put
  })

// Get data from table
val getRDD = sc.parallelize(Array(
    By.toBytes("5"),
    By.toBytes("4"),
    By.toBytes("3"),
    By.toBytes("2"),
    By.toBytes("1")
))

val resRDD = getRDD.hbaseBulkGet[String](hbaseContext, TableName.valueOf(tablePath), 2,
    (record) => { new Get(record) },
    (result: Result) => {
      val it = result.listCells().iterator()
      val sb = new StringBuilder

      sb.append(By.toString(result.getRow) + ": ")
      while (it.hasNext) {
        val cell = it.next()
        val q = By.toString(CellUtil.cloneQualifier(cell))
        if (q.equals("counter")) {
          sb.append("(" + q + "," + By.toLong(CellUtil.cloneValue(cell)) + ")")
        } else {
          sb.append("(" + q + "," + By.toString(CellUtil.cloneValue(cell)) + ")")
        }
      }
      sb.toString()
    })

resRDD.collect().foreach(v => println(v))
```
</details>

The output looks like the following:

```
5: (1,5)
4: (1,4)
3: (1,3)
2: (1,2)
1: (1,1)
```


The Zeppelin on MapR Tutorial also includes a notebook with Scala code examples using the MapR Database Binary Connector. 

>See [MapR Database Binary Connector for Apache Spark](https://mapr.com/docs/61/Spark/SparkHBaseConnector.html#concept_gth_txm_gz) for additional information about this connector.