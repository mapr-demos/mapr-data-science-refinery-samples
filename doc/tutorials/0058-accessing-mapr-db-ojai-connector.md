# Accessing MapR Database in Zeppelin Using the MapR Database OJAI Connector

This section contains examples of Apache Spark jobs that use the MapR Database OJAI Connector for Apache Spark to read and write MapR Database JSON tables. The examples use the Spark Python interpreter. 

Before running the examples, make sure you have configured your [Spark](https://mapr.com/docs/61/Zeppelin/ConfigureSparkInterpreter.html#task_t1d_4yj_qbb__section_zwx_pdk_qbb) interpreter to run Spark jobs.

This example is derived from the example at [SparkSQL and DataFrames](https://mapr.com/docs/61/Spark/SparkSQLandDataFrames.html#concept_wl2_jk4_gz). That page provides a more detailed explanation of the code.


### Example

The following code sample creates a Spark DataFrame, inserts it into a MapR Database JSON table that is created as part of the insert, and then loads it into another DataFrame:

<details> 
  <summary>Inserting a Spark DataFrame into a MapR Database JSON Table</summary>

```
%spark.pyspark
df = sc.parallelize([ { "_id": "rsmith", "address": { "city": "San Francisco", "line": "100 Main Street", "zip": 94105 }, "dob": "1982-02-03", "first_name": "Robert", "interests": [ "electronics", "music", "sports" ], "last_name": "Smith" }, { "_id": "mdupont", "address": { "city": "San Jose", "line": "1223 Broadway", "zip": 95109 }, "dob": "1982-02-03", "first_name": "Maxime", "interests": [ "sports", "movies", "electronics" ], "last_name": "Dupont" }, { "_id": "jdoe", "address": None, "dob": "1970-06-23", "first_name": "John", "interests": None, "last_name": "Doe" }, { "_id": "dsimon", "address": None, "dob": "1980-10-13", "first_name": "David", "interests": None, "last_name": "Simon" }, { "_id": "alehmann", "address": None, "dob": "1980-10-13", "first_name": "Andrew", "interests": [ "html", "css", "js" ], "last_name": "Lehmann" } ]).toDF()

# Insert into MapR-DB table
spark.insertToMapRDB(df, "/user/mapruser1/table1", create_table=True)

# Load previously inserted data from MapR-DB table
df_loaded = spark.loadFromMapRDB("/user/mapruser1/table1").show()
```

</details> 


To bulk insert into a MapR Database JSON table that is created as part of the insert operation, you must order the records in the DataFrame as shown in the following example:

<details> 
  <summary>Inserting a Spark DataFrame into a MapR Database JSON Table Using Bulk Insert</summary>

```
%spark.pyspark
df = sc.parallelize([ { "_id": "rsmith", "address":{ "city": "San Francisco", "line": "100 Main Street", "zip": 94105 }, "dob": "1982-02-03", "first_name": "Robert", "interests": [ "electronics", "music", "sports" ], "last_name": "Smith" }, { "_id": "mdupont", "address":{ "city": "San Jose", "line": "1223 Broadway", "zip": 95109 }, "dob": "1982-02-03", "first_name": "Maxime", "interests": [ "sports", "movies", "electronics" ], "last_name": "Dupont" },{ "_id": "jdoe", "address": None, "dob": "1970-06-23", "first_name": "John", "interests": None, "last_name": "Doe" },{ "_id": "dsimon", "address": None, "dob": "1980-10-13", "first_name": "David", "interests": None, "last_name": "Simon" },{ "_id": "alehmann", "address": None, "dob": "1980-10-13", "first_name": "Andrew", "interests": [ "html", "css", "js" ], "last_name": "Lehmann" }]).toDF().orderBy("_id")

# Bulk insert into MapR-DB table
spark.insertToMapRDB(df, "/user/mapruser1/table2", create_table=True, bulk_insert=True)

# Load previously inserted data from MapR-DB table
df_loaded = spark.loadFromMapRDB("/user/mapruser1/table2").show()
```

</details> 


The following code sample uses projection and filtering when loading a Spark DataFrame from a MapR Database JSON table:

<details> 
  <summary>Selecting and Filtering Data when Loading a Spark DataFrame</summary>

```
%spark.pyspark

from pyspark.sql.functions import col, asc

df = sc.parallelize([{ "_id": "rsmith", "address": { "city": "San Francisco", "line": "100 Main Street", "zip": 94105 }, "dob": "1982-02-03", "first_name": "Robert", "interests": [ "electronics", "music", "sports" ], "last_name": "Smith" }, { "_id": "mdupont", "address": { "city": "San Jose", "line": "1223 Broadway", "zip": 95109 }, "dob": "1982-02-03", "first_name": "Maxime", "interests": [ "sports", "movies", "electronics" ], "last_name": "Dupont" }]).toDF()

spark.saveToMapRDB(df, "/user/mapruser1/table3", create_table=True)

# Load previously saved data from the MapR-DB table
df_loaded_select = spark.loadFromMapRDB("/user/mapruser1/table3")\
    .select("_id","first_name","address")\
    .filter(col("first_name") == "Maxime").show()
```

</details> 


The following code sample loads a Spark DataFrame from a MapR Database JSON table and joins the DataFrame with a second DataFrame:

<details> 
  <summary>Joining DataFrames when Loading a Spark DataFrame</summary>

```
%spark.pyspark
df = sc.parallelize([ { "_id": "rsmith", "address": { "city": "San Francisco", "line": "100 Main Street", "zip": 94105 }, "dob": "1982-02-03", "first_name": "Robert", "interests": [ "electronics", "music", "sports" ], "last_name": "Smith" }, { "_id": "mdupont", "address": { "city": "San Jose", "line": "1223 Broadway", "zip": 95109 }, "dob": "1982-02-03", "first_name": "Maxime", "interests": [ "sports", "movies", "electronics" ], "last_name": "Dupont" }, { "_id": "jdoe", "address": None, "dob": "1970-06-23", "first_name": "John", "interests": None, "last_name": "Doe" }, { "_id": "dsimon", "address": None, "dob": "1980-10-13", "first_name": "David", "interests": None, "last_name": "Simon" }, { "_id": "alehmann", "address": None, "dob": "1980-10-13", "first_name": "Andrew", "interests": [ "html", "css", "js" ], "last_name": "Lehmann" } ]).toDF()
dfProfessions = sc.parallelize([ { "_id": "rsmith", "profession": "Engineer" }, { "_id": "alehmann", "profession": "Doctor" }, { "_id": "alehmann", "profession": "Accountant" }, { "_id": "fake", "profession": "Software developer" } ]).toDF()

# Save to MapR-DB table
spark.saveToMapRDB(df, "/user/mapruser1/table4", create_table=True)

# Load previously saved data from MapR-DB Table and join with another DataFrame
df_loaded_select = spark.loadFromMapRDB("/user/mapruser1/table4")\
    .join(dfProfessions, "_id").show()
```

</details> 

The Zeppelin on MapR Tutorial includes notebooks with Python and Scala code examples using the MapR Database OJAI Connector.


See [MapR Database OJAI Connector for Spark](https://mapr.com/docs/61/Spark/NativeSparkConnectorJSON.html#concept_xpz_gxm_gz) for additional information about this connector.