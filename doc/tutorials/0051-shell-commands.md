# Running Shell Commands in Zeppelin

This section shows you how to access files in your local file system and MapR Filesystem by using shell commands in your Apache Zeppelin notebook.

To use POSIX shell commands to access MapR Filesystem, you must have a MapR Filesystem mount point in your container. The [FUSE-Based POSIX Client](https://mapr.com/docs/61/Zeppelin/ZeppelinDockerRunParameters.html#concept_rhn_gb2_rbb__section_i4r_5c2_rbb) provides this functionality.

In the following example, your MapR Filesystem mount point is in `/mapr` and your cluster name is `node3.cluster.com`. You can find the name of your cluster in the file `/opt/mapr/conf/mapr-clusters.conf` on your MapR cluster.

**Create a file of test data in `/tmp:`**

```
%sh

cat > /tmp/test.data << EOF
John,Smith
Brian,May
Rodger,Taylor
John,Deacon
Max,Plank
Freddie,Mercury
Albert,Einstein
Fedor,Dostoevsky
Lev,Tolstoy
Niccolo,Paganini
EOF
```

**Copy the file to your home directory in MapR Filesystem (/user/mapr) and display the contents of the file:**

POSIX
```
%sh
cp /tmp/test.data /mapr/my.cluster.com/user/mapruser1
cat /mapr/my.cluster.com/user/mapruser1/test.data
```

Hadoop
```
%sh
hadoop fs -put /tmp/test.data /user/mapruser1
hadoop fs -cat /user/mapruser1/test.data
```

The Zeppelin notebook for this section shows how to access files in your local file system and MapR Filesystem by using shell commands.

To run the notebook [Running Shell Commands in Zeppelin](notebook/running-shell-commands-in-zeppelin.json) just import it to the Zeppelin, click on  `Import note:` button and select the JSON file or put the link to the notebook:


<details> 
  <summary>Import Zeppelin notebook</summary>
  ![Import Zeppelin notebook](images/zeppelin-import.png)

</details> 
