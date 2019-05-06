# Running MapR Database Shell Commands in Zeppelin

This section contains a sample of MapR Database shell commands that you can run in your Apache Zeppelin notebook.

### Example

**Invoke the MapR Database shell:**

```
%maprdb.shell
```

**Create a MapR Database JSON table in your home directory on MapR file system:**

```
create /tmp/sample_table
```

**Insert some documents into the table:**

```
insert /tmp/sample_table --value '{"_id": "FYWN1w","name":"Dental by Design","city":"Ahwatukee","stars":4.0}'
insert /tmp/sample_table --value '{"_id": "He-G7v","name":"Stephen Szabo Salon","city":"McMurray","stars":3.0}'
insert /tmp/sample_table --value '{"_id": "KQPW8l","name":"Western Motor Vehicle","city":"Phoenix","stars":1.5}'
insert /tmp/sample_table --value '{"_id": "8DShNS","name":"Sports Authority","city":"Tempe","stars":3.0}'
insert /tmp/sample_table --value '{"_id": "PfOCPj","name":"Brick House Tavern + Tap","city":"Cuyahoga Falls","stars":3.5}'
insert /tmp/sample_table --value '{"_id": "o9eMRC","name":"Messina","city":"Stuttgart","stars":4.0}'
insert /tmp/sample_table --value '{"_id": "kCoE3j","name":"BDJ Realty","city":"Las Vegas","stars":4.0}'
insert /tmp/sample_table --value '{"_id": "OD2hnu","name":"Soccer Zone","city":"Las Vegas","stars":1.5}'
insert /tmp/sample_table --value '{"_id": "EsMcGi","name":"Any Given Sundae","city":"Wexford","stars":5.0}'
insert /tmp/sample_table --value '{"_id": "TGWhGN","name":"Detailing Gone Mobile","city":"Henderson","stars":5.0}'                   
```

**Retrieve the documents with at least a 4 star rating:**

```
find /tmp/sample_table  --where '{"$ge":{"stars":4}}'
```

The query returns the following:

```
{"_id": "EsMcGi","name":"Any Given Sundae","city":"Wexford","stars":5.0}
{"_id": "FYWN1w","name":"Dental by Design","city":"Ahwatukee","stars":4.0}
{"_id": "TGWhGN","name":"Detailing Gone Mobile","city":"Henderson","stars":5.0}
{"_id": "kCoE3j","name":"BDJ Realty","city":"Las Vegas","stars":4.0}
{"_id": "o9eMRC","name":"Messina","city":"Stuttgart","stars":4.0}
```


The prepared notebook for this section is ready to be imported to your MapR DSR. 

Click on `Import note:` button and select the JSON file `running-mapr-db-shell-commands.json` or put the link to it. 

<details> 
  <summary>Details</summary>
  
![Import Zeppelin notebook](images/zeppelin-import.png)

</details> 
