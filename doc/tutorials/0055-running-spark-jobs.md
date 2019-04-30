# Running Spark Jobs in Zeppelin

This section contains code samples for different types of Apache Spark jobs that you can run in your Apache Zeppelin notebook. You can run these examples using either the Livy or Spark interpreter. 

>Before running these examples, depending on whether you are using the [Livy](https://mapr.com/docs/61/Zeppelin/ConfigureLivyInterpreter.html#task_t1d_4yj_qbb) or [Spark](https://mapr.com/docs/61/Zeppelin/ConfigureSparkInterpreter.html#task_t1d_4yj_qbb) interpreter, make sure you have configured the interpreter.

The Zeppelin notebooks for this section shows how to run Apache Spark jobs using either the [Livy](https://mapr.com/docs/61/Zeppelin/ConfigureLivyInterpreter.html#task_t1d_4yj_qbb) or [Spark](https://mapr.com/docs/61/Zeppelin/ConfigureSparkInterpreter.html#task_t1d_4yj_qbb) interpreter.


### Example of using Spark SQL

>How to perform these actions using Pig in Zeppelin look to the section about [Pig](0052-pig-scripts.md).

**Load data into table**
```
%spark

import org.apache.commons.io.IOUtils
import java.net.URL
import java.nio.charset.Charset

// Zeppelin creates and injects sc (SparkContext) and sqlContext (HiveContext or SqlContext)
// So you don't need create them manually

// load bank data
val bankText = sc.parallelize(
    IOUtils.toString(
        new URL("http://s3.amazonaws.com/apache-zeppelin/tutorial/bank/bank.csv"),
        Charset.forName("utf8")).split("\n"))

case class Bank(age: Integer, job: String, marital: String, education: String, balance: Integer)

val bank = bankText.map(s => s.split(";")).filter(s => s(0) != "\"age\"").map(
    s => Bank(s(0).toInt, 
            s(1).replaceAll("\"", ""),
            s(2).replaceAll("\"", ""),
            s(3).replaceAll("\"", ""),
            s(5).replaceAll("\"", "").toInt
        )
).toDF()
bank.registerTempTable("bank")
```

**Get the number of each age where age is less than 30**

```
%spark.sql 

select age, count(1) value
from bank 
where age < 30 
group by age 
order by age
```

**The same as above, but use dynamic text form so that use can specify the variable maxAge in the textbox. Dynamic form is a very cool feature of Zeppelin, you can refer to this link) for details.**

```
%spark.sql 

select age, count(1) value 
from bank 
where age < ${maxAge=30} 
group by age 
order by age
```

**Get the number of each age for specific marital type, also use the dynamic form here. User can choose the marital type in the dropdown list.**

```
%spark.sql 

select age, count(1) value 
from bank 
where marital="${marital=single,single|divorced|married}" 
group by age 
order by age
```

The notebook for this section you can find in `Zeppelin Tutorial/Spark • Basic Features (Spark)` folder. 

Path in the filisystem`/opt/<user>/zeppelin/zeppelin-<version>/notebook/2A94M5J1Z`

<details> 
  <summary>MapR Data Science Refinery Tutorials</summary>

![MapR Data Science Refinery](images/welcome_zeppelin.png)

</details>


> If for some reason you can't find the default notebook for Spark SQL. The example located by path `notebooks/running-spark-sql-jobs-in-zeppelin.json`


### About the dataset

```
Citation Request:
  This dataset is public available for research. The details are described in [Moro et al., 2011]. 
  Please include this citation if you plan to use this database:

  [Moro et al., 2011] S. Moro, R. Laureano and P. Cortez. Using Data Mining for Bank Direct Marketing: An Application of the CRISP-DM Methodology. 
  In P. Novais et al. (Eds.), Proceedings of the European Simulation and Modelling Conference - ESM'2011, pp. 117-121, Guimarães, Portugal, October, 2011. EUROSIS.

  Available at: [pdf] http://hdl.handle.net/1822/14838
                [bib] http://www3.dsi.uminho.pt/pcortez/bib/2011-esm-1.txt
```

> The other examples of using Spark you can find in the official [MapR Data Science Refinery documentation](https://mapr.com/docs/61/Zeppelin/ZeppelinSpark.html)
