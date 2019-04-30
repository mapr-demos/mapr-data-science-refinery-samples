# MapR Data Science Refinery configuration options

> To run the Apache Zeppelin container, you must access the Zeppelin Docker image from MapR’s public repository, 
run the Docker image, and access the deployed container from your web browser. From your browser, you can create Zeppelin notebooks.

In our tutorial we will be running MapR Data Science Refinery from Docker image with Zeppelin PACC. 
To pull and run the Docker image, you must first install [Docker](https://docs.docker.com/engine/installation/) on the host where you want to run the container.

Running a container with `docker run` command. Follow the link to understand all [Zeppelin Docker Parameters](https://mapr.com/docs/home/Zeppelin/ZeppelinDockerRunParameters.html)
##### Some common parameters needed to be configured 

```
docker run -it \                                                                # docker run command
-v /sys/fs/cgroup:/sys/fs/cgroup:ro \                                           # default Docker options
-e MAPR_CLDB_HOSTS=<docker-host-ip> \                                           # default PACC options
-e MAPR_CLUSTER=<cluster-name> \                                                # .
-e MAPR_CONTAINER_USER=<user-name> \                                            # MapR security opyions
-e MAPR_CONTAINER_UID=<uid> \                                                   # .
-e MAPR_CONTAINER_GROUP=<group-name> \                                          # .
-e MAPR_CONTAINER_GID=<gid> \                                                   # .
-e MAPR_CONTAINER_PASSWORD=<password> \                                         # .    
-e MAPR_TZ=<time-zone> \                                                        # .
-e MAPR_TICKETFILE_LOCATION=<ticket-file-container-location> \                  # .
-v <ticket-file-host-location>:<ticket-file-container-location>:ro              # .
--privileged --cap-add SYS_ADMIN --cap-add SYS_RESOURCE \                       # MapR Posix client options
--device /dev/fuse \                                                            # .
-e MAPR_MOUNT_PATH=/mapr/ \                                                     # .
-e ZEPPELIN_SSL_PORT=9996 -p 9996:9996 \                                        # Zeppelin port
--network=bridge \                                                              #
--add-host=<host-name>:<host-ip> --add-host=<host-name>:<host-ip> \             # . add cluster nodes to /etc/hosts of container
-p 10000-10010:10000-10010 \                                                    # options to make livy interpreter works with --network=bridge
-e HOST_IP=<docker-host-ip> \                                                   # .
-e MAPR_HS_HOST=<historyserver-ip> \                                            # options to make Pig interpreter works
-v /tmp/hive-site.xml:/opt/mapr/spark/spark<version>/conf/hive-site.xml:ro \    # options to make Spark works with Hive
maprtech/data-science-refinery                                                  # Docker image name
```



<details> 
  <summary>`docker run` command used for our tutorial</summary>
  
  ```
  docker run -it \
-v /sys/fs/cgroup:/sys/fs/cgroup:ro \
-e MAPR_CLDB_HOSTS=192.168.33.13 \
-e MAPR_CLUSTER=node3.cluster.com \
-e MAPR_CONTAINER_USER=mapr \
-e MAPR_CONTAINER_UID=5000 \
-e MAPR_CONTAINER_GROUP=mapr \
-e MAPR_CONTAINER_GID=5000 \
-e MAPR_CONTAINER_PASSWORD=mapr \
-e MAPR_TZ=US/Pacific \
--privileged --cap-add SYS_ADMIN --cap-add SYS_RESOURCE --device /dev/fuse \
-e MAPR_MOUNT_PATH=/mapr/ \
-e ZEPPELIN_SSL_PORT=9996 -p 9996:9996 \
--network=bridge \
--add-host="node3:192.168.33.13" --add-host="node3.cluster.com:192.168.33.13" \
-p 10000-10010:10000-10010 \
-e HOST_IP=192.168.33.1 \
-p 10011-10021:10011-10021 \
-e LIVY_RSC_PORT_RANGE="10011~10021" \
-p 13011-13021:13011-13021 \
-e SPARK_PORT_RANGE="13011~13021" \
-e MAPR_HS_HOST=192.168.33.13 \
-v /tmp/hive-site.xml:/opt/mapr/spark/spark-2.3.2/conf/hive-site.xml:ro \
maprtech/data-science-refinery
  ```
</details> 


> After installing Docker, if you configure a memory limit, ensure that it is at least 3.5 GB of memory. Otherwise, you may not be able to start the Zeppelin container or may encounter log in problems.
Ensure that container can resolve hostnames of cluster nodes.


#### Some useful commands for work with docker images and containers:

1. To run existing container:

```
docker exec -it -u mapruser1 389b9b3 bash -l 
```

2. To stop and remove container: 

```
docker container list
docker container stop <id>
docker container list -a (shows stopped containers)
docker container rm
```

3. To remove all stopped containers:

```
docker container list -q -a | xargs docker container rm
```

4. To copy file inside container:

```
docker cp /tmp/hive-site.xml 389b9b:/tmp/
```
