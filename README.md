# MapR Data Science Refinery Tutorial

The repository contains examples explain the key features of using MapR Data Science Refinery.

## Introduction

[The MapR Data Science Refinery (DSR)](https://mapr.com/products/data-science-refinery/) is an easy-to-deploy and scalable data science toolkit with native access to all platform assets and superior out-of-the-box security. 

The MapR Data Science Refinery offers:
- **Access to All Platform Assets** - [The MapR FUSE-based POSIX Client](https://mapr.com/docs/61/AdministratorGuide/MapRfusePOSIXClient.html) allows app servers, web servers, and other client nodes and apps to read and write data directly and securely to a MapR cluster, like a Linux filesystem. In addition, connectors are provided for interacting with both [MapR-DB](https://mapr.com/docs/61/MapROverview/maprDB-overview.html) and [MapR-ES](https://mapr.com/docs/61/MapROverview/c_mapr_streams.html) via [Apache Spark connectors](https://mapr.com/docs/61/Spark/SparkConnectorsMapRDB.html?hl=apache%2Cspark%2Cconnectors).
- **Superior Security** - The MapR Platform is secure by default, and Apache Zeppelin on MapR leverages and integrates with this security layer using the built-in capabilities provided by the [MapR Persistent Application Container (PACC)](https://mapr.com/products/persistent-application-client-container/).
- **Extensibility** - Apache Zeppelin is paired with the [Helium](https://zeppelin.apache.org/docs/0.8.0/development/helium/overview.html) framework to offer pluggable visualization capabilities.
- **Simplified Deployment** - A preconfigured [Docker container](https://hub.docker.com/r/maprtech/data-science-refinery) provides the ability to leverage MapR as a persistent data store.


## Contents 

1. [Configuration options](doc/tutorials/configuration.md)
1. [Zeppelin interpreters on MapR](doc/tutorials/interpreters.md)
1. [Zeppelin notebooks on MapR](doc/tutorials/notebooks-accessing-creating.md)
1. [Visualization in MapR DSR](doc/tutorials/visualization.md)
1. [Examples of using MapR DSR with different backend engines](doc/tutorials/use-cases.md)
	- [Running Shell Commands](doc/tutorials/shell-commands.md)   
	- [Running Pig Scripts](doc/tutorials/pig-scripts.md)
	- [Running Drill Queries](doc/tutorials/drill-queries.md)
	- [Running Hive Queries](doc/tutorials/running-hive-queries.md)
	- [Running Spark Jobs](doc/tutorials/running-spark-jobs.md)
    - [Running MapR DB Shell Commands](doc/tutorials/running-mapr-db-shell-commands.md)
	- [Accessing MapR DB in Zeppelin Using the MapR Database Binary Connector](doc/tutorials/accessing-mapr-db-binary-connector.md)
	- [Accessing MapR DB in Zeppelin Using the MapR Database OJAI Connector](doc/tutorials/accessing-mapr-db-ojai-connector.md)
	- [Accessing MapR Event Store For Apache Kafka in Zeppelin Using the Livy Interpreter](doc/tutorials/event-store-kafka-using-livy.md)    
	- [Accessing MapR Event Store For Apache Kafka in Zeppelin Using the Spark Interpreter](doc/tutorials/event-store-kafka-using-spark.md)
1. [Installing custom packages (Tensorflow)](doc/tutorials/how-to-install-custom-packages.md)
1. [Sharing Zeppelin Notebook on MapR DSR](doc/tutorials/sharing-zeppelin-notebook.md)
1. [Building your own MapR DSR Docker Image](doc/tutorials/building-your-own-mdsr-image.md)
1. [Troubleshooting Zeppelin on MapR DSR](doc/tutorials/troubleshooting-zeppelin.md)
1. [Official documentation](https://mapr.com/docs/61/DataScienceRefinery/DataScienceRefineryOverview.html)


### Prerequisites
* MapR Converged Data Platform 6.1 or [MapR Container for Developers](https://mapr.com/docs/home/MapRContainerDevelopers/MapRContainerDevelopersOverview.html)
* JDK 8
* All notebooks for the tutorial are located in [/notebooks](/notebooks)


### MapR Data Science Refinery Architecture
![MapR Data Science Refinery Architecture](doc/tutorials/images/mapr-data-science-refinery.png)