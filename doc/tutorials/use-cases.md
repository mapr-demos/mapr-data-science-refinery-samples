# Examples of using MapR DSR with different backend engines

This section contains examples of how to use Apache Zeppelin interpreters to access the different backend engines. This includes running Apache Pig scripts, Apache Drill queries, Apache Hive queries, and Apache Spark jobs, as well as accessing MapR Database and MapR Event Store For Apache Kafka.

Immediately after installation, you can see several prepared examples for demonstration. Located in two folders:

- MapR Tutorial
- Zeppelin Tutorial 

<details> 
  <summary>MapR Data Science Refinery Tutorials UI</summary>

![MapR Data Science Refinery](images/welcome_zeppelin.png)

</details>

[]()

To see other features of using MapR Data Science Refinery follow the links below. Also you can run the prepared notebooks in your MapR DSR just [import](doc/tutorials/notebooks-accessing-creating.md) them to it.

1. [Running Shell Commands in Zeppelin](doc/tutorials/shell-commands.md) 
This section shows you how to access files in your local file system and MapR Filesystem by using shell commands in your Apache Zeppelin notebook.

1. [Running Pig Scripts in Zeppelin](doc/tutorials/pig-scripts.md) 
This section contains a sample of an Apache Pig script that you can run in your Apache Zeppelin notebook.

1. [Running Drill Queries in Zeppelin](doc/tutorials/drill-queries.md) 
This section contains samples of Apache Drill queries that you can run in your Apache Zeppelin notebook.

1. [Running Hive Queries in Zeppelin](doc/tutorials/running-hive-queries.md) 
This section contains samples of Apache Hive queries that you can run in your Apache Zeppelin notebook.

1. [Running Spark Jobs in Zeppelin](doc/tutorials/running-spark-jobs.md) 
This section contains code samples for different types of Apache Spark jobs that you can run in your Apache Zeppelin notebook. You can run these examples using either the Livy or Spark interpreter.

1. [Running MapR Database Shell Commands in Zeppelin](doc/tutorials/running-mapr-db-shell-commands.md) 
This section contains a sample of MapR Database shell commands that you can run in your Apache Zeppelin notebook.

1. [Accessing MapR Database in Zeppelin Using the MapR Database Binary Connector](doc/tutorials/accessing-mapr-db-binary-connector.md) 
This section contains an example of an Apache Spark job that uses the MapR Database Binary Connector for Apache Spark to write and read a MapR Database Binary table. You can run this example using either the Livy or Spark interpreter. 

1. [Accessing MapR Database in Zeppelin Using the MapR Database OJAI Connector](doc/tutorials/accessing-mapr-db-ojai-connector.md) 
This section contains examples of Apache Spark jobs that use the MapR Database OJAI Connector for Apache Spark to read and write MapR Database JSON tables. The examples use the Spark Python interpreter. 

1. [Accessing MapR Event Store For Apache Kafka in Zeppelin Using the Livy Interpreter](doc/tutorials/event-store-kafka-using-livy.md) 
This section contains a MapR Event Store For Apache Kafka streaming example that you can run in your Apache Zeppelin notebook using the Livy interpreter.

1. [Accessing MapR Event Store For Apache Kafka in Zeppelin Using the Spark Interpreter](doc/tutorials/event-store-kafka-using-spark.md) 
This section contains a MapR Event Store For Apache Kafka streaming example that you can run in your Apache Zeppelin notebook using the Spark interpreter. 

>All notebooks for the tutorial are located in [/notebooks](/notebooks)