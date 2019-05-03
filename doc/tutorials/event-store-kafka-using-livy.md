# Accessing MapR Event Store For Apache Kafka in Zeppelin Using the Livy Interpreter

This section contains a MapR Event Store For Apache Kafka streaming example that you can run in your Apache Zeppelin notebook using the Livy interpreter.

> **Note:** See [MapR Data Science Refinery Support by MapR Core Version](https://mapr.com/docs/61/DataScienceRefinery/DSRSupportByCoreVersion.html) for limitations in version support when accessing MapR Event Store.

[Configure the Livy interpreter](https://mapr.com/docs/61/Zeppelin/ConfigureLivyInterpreter.html#task_t1d_4yj_qbb). Make sure to follow the steps described in the Spark Jobs section to allow Spark jobs to run in parallel.

The example references a stream named `test_stream` created in the path `/streaming_test/test_stream`. The stream contains a topic called `test_topic`. You can use the following commands to create this stream and topic, but you cannot run them in Zeppelin; they must be run in a MapR cluster.

```
hadoop fs -mkdir /streaming_test
hadoop fs -chown <user>:<group> /streaming_test
sudo su - <user>
maprcli stream create -path /streaming_test/test_stream
maprcli stream topic create -path /streaming_test/test_stream -topic test_topic
```

**When the stream and topic are available, perform the following actions in your notebook:**

<details> 
  <summary>Create a streaming consumer in your notebook using the %livy.spark interpreter:</summary>

```
import org.apache.kafka.clients.consumer.ConsumerConfig

import org.apache.spark.SparkConf
import org.apache.spark.streaming.{Seconds, StreamingContext}
import org.apache.spark.streaming.kafka09.{ConsumerStrategies, KafkaUtils, LocationStrategies}    

val ssc = new StreamingContext(sc, Seconds(1))

val topicsSet = Set("/streaming_test/test_stream:test_topic")
val kafkaParams = Map[String, String](
  ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG -> "localhost:9092",
  ConsumerConfig.GROUP_ID_CONFIG -> "none",
  ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG ->
    "org.apache.kafka.common.serialization.StringDeserializer",
  ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG ->
    "org.apache.kafka.common.serialization.StringDeserializer",
  ConsumerConfig.AUTO_OFFSET_RESET_CONFIG -> "earliest",
  ConsumerConfig.ENABLE_AUTO_COMMIT_CONFIG -> "false" 
)

val consumerStrategy =
      ConsumerStrategies.Subscribe[String, String](topicsSet, kafkaParams)
val messages = KafkaUtils.createDirectStream[String, String](
      ssc,
      LocationStrategies.PreferConsistent,
      consumerStrategy)

val lines = messages.map(_.value())
val words = lines.flatMap(_.split(" "))
val wordCounts = words.map(x => (x, 1L)).reduceByKey(_ + _)
wordCounts.print()

ssc.start()
ssc.awaitTerminationOrTimeout(3 * 60 * 1000)
```

</details>

[]()


<details> 
  <summary>Create a streaming producer in another notebook, also with the %livy.spark interpreter:</summary>

```
import java.util.Properties
import org.apache.kafka.clients.producer.{KafkaProducer, ProducerRecord}

val props = new Properties()
props.put("bootstrap.servers", "localhost:9092")
props.put("acks", "all")
props.put("retries", "0")
props.put("batch.size", "16384")
props.put("linger.ms", "1")
props.put("buffer.memory", "33554432")
props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer")
props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer")

val producer = new KafkaProducer[String, String](props)

for (i <- 1 to 1000) {
    val message = new ProducerRecord[String, String]("/streaming_test/test_stream:test_topic", i.toString(), i.toString())
    producer.send(message)
}
```

</details>

[]()


<details> 
  <summary>Start running the consumer notebook. Wait until the Livy session in YARN for this consumer is initialized and running. You can determine this by locating the session in the YARN resource manager UI:</summary>
  
![the YARN resource manager UI](images/yurn-ui-livy.png)

</details> 

[]()

The URL for the YARN resource manager is one of the following:

Secure cluster:

```
https://<resource-manager-host>:8090
```

Non-secure cluster:

```
http://<resource-manager-host>:8088
```

**Run the producer notebook several times.***
The consumer notebook has a three-minute timeout that was set by the following line:

```
ssc.awaitTerminationOrTimeout(3 * 60 * 1000)
```

You will see output in the consumer notebook after the timeout has expired.


The prepared notebooks for this section is ready to be imported to your MapR DSR. 

Click on `Import note:` button and select the JSON file `consumer-event-store-for-kafka-use-livy.json` and `producer-event-store-for-kafka-use-livy.json` or put the links to them. 

<details> 
  <summary>Details</summary>
  
![Import Zeppelin notebook](images/zeppelin-import.png)

</details> 