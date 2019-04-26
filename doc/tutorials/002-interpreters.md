# Zeppelin's interpreters, how to configure and use
Apache Zeppelin interpreters enable you to access specific languages and data processing backends.

Out-of-box, the interpreters in Apache Zeppelin on MapR are preconfigured to run against different backend engines. You may need to perform manual steps to configure the Livy, Spark, and JDBC interpreters. No additional steps are needed to configure and run the Pig and Shell interpreters.

Follow the link to get to know how to configure [Livy, Spark, and JDBC interpreters](https://mapr.com/docs/61/Zeppelin/ConfigureInterpreters.html)

#### Supported Zeppelin Interpreters
Apache Zeppelin on MapR supports the following interpreters:

Shell
With the Shell interpreter, you can invoke system shell commands. If you have a MapR Filesystem mount point, you can access MapR Filesystem using shell commands like ls and cat by using the [FUSE-Based POSIX Client](https://mapr.com/docs/61/Zeppelin/ZeppelinDockerRunParameters.html#concept_rhn_gb2_rbb__section_i4r_5c2_rbb). See [Running Shell Commands in Zeppelin]().

Pig
The Apache Pig interpreter enables you to run Apache Pig scripts and queries. See [Running Pig Scripts in Zeppelin]().

JDBC - Drill and Hive
Apache Zeppelin on MapR provides preconfigured Apache Drill and Apache Hive JDBC interpreters. See [Running Drill Queries in Zeppelin]() and [Running Hive Queries in Zeppelin]().

Livy
The Apache Livy interpreter is a RESTful interface for interacting with Apache Spark. With this interpreter, you can run interactive Scala, Python, and R shells, and submit Spark jobs.

Spark
The Apache Spark interpreter is available starting in MapR Data Science Refinery It provides an alternative to the Livy interpreter.

MapR Database Shell
The MapR Database Shell interpreter allows you to run commands available in [MapR Database Shell (JSON Tables)](https://mapr.com/docs/61/ReferenceGuide/mapr_dbshell.html#mapr_dbshell) in the Zeppelin UI. Using dbshell commands, you can access MapR Database JSON tables without having to write Spark code. See [Running MapR Database Shell Commands in Zeppelin]().

#### Unsupported Zeppelin Interpreters
Apache Zeppelin on MapR does not support the HBase interpreter. To access MapR Database binary tables, use the [MapR Database Binary Connector for Apache Spark](https://mapr.com/docs/61/Spark/SparkHBaseConnector.html#concept_gth_txm_gz) with either the Livy or Spark interpreter.


#### Writing a New Interpreter

You are able to write your own interpreter. Creating a new interpreter is quite simple. Just extend `org.apache.zeppelin.interpreter` abstract class and implement some methods. 

The whole [instruction](https://zeppelin.apache.org/docs/0.8.1/development/writing_zeppelin_interpreter.html) about how to write a new interpreter.