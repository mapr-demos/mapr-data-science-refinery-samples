# Running Shell Commands in Zeppelin

This section shows you how to access files in your local file system and MapR Filesystem by using shell commands in your Apache Zeppelin notebook.

To use POSIX shell commands to access MapR Filesystem, you must have a MapR Filesystem mount point in your container. The [FUSE-Based POSIX Client](https://mapr.com/docs/61/Zeppelin/ZeppelinDockerRunParameters.html#concept_rhn_gb2_rbb__section_i4r_5c2_rbb) provides this functionality.

In the following example, your MapR Filesystem mount point is in `/mapr` and your cluster name is `<name>.cluster.com`. You can find the name of your cluster in the file `/opt/mapr/conf/mapr-clusters.conf` on your MapR cluster.

1. **Create a file of test data in `/tmp:`**

<details> 
  <summary>Shell commands</summary>

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

</details>

[]()

2. **Copy the file to your home directory in MapR Filesystem (/user/mapr) and display the contents of the file:**

<details> 
  <summary>POSIX</summary>

```
%sh
cp /tmp/test.data /mapr/<name>.cluster.com/user/mapr
cat /mapr/<name>.cluster.com/user/mapr/test.data
```

</details>

[]()

<details> 
  <summary>Hadoop</summary>

```
%sh
hadoop fs -put /tmp/test.data /user/mapr
hadoop fs -cat /user/mapr/test.data
```

</details>

[]()

The prepared notebook for this section is ready to be imported to your MapR DSR. 

Click on `Import note:` button and select the JSON file `running-shell-commands-in-zeppelin.json` or put the link to it. 

<details> 
  <summary>Details</summary>
  
![Import Zeppelin notebook](images/zeppelin-import.png)

</details> 