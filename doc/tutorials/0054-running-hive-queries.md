# Running Hive Queries

This section contains samples of Apache Hive queries that you can run in your Apache Zeppelin notebook.

[Apache Hive](https://hive.apache.org/) data warehouse software facilitates reading, writing, and managing large datasets residing in distributed storage using SQL. Structure can be projected onto data already in storage. A command line tool and JDBC driver are provided to connect users to Hive.

**Note!** Before running Hive queries, make sure you have [configured the Hive JDBC interpreter](https://mapr.com/docs/61/Zeppelin/ConfigureJDBCInterpreter.html#concept_b5l_xdk_qbb__section_a5z_d2k_qbb). 

Also, see [MapR Data Science Refinery Support by MapR Core Version](https://mapr.com/docs/61/DataScienceRefinery/DSRSupportByCoreVersion.html) for limitations when connecting to a secure MapR 6.1 cluster.


The Zeppelin notebook for this section contains sample of Apache Hive queries that you can run in your MapR Data Science Refinery.

### Example

**Download Spending Dataset**

```
%sh

#remove existing copies of dataset
hadoop fs -rm  /tmp/expenses.csv

#fetch the dataset
wget https://data.gov.au/dataset/f84b9baf-c1c1-437c-8c1e-654b2829848c/resource/88399d53-d55c-466c-8f4a-6cb965d24d6d/download/healthexpenditurebyareaandsource.csv -O /tmp/expenses.csv

#remove header
sed -i '1d' /tmp/expenses.csv
#remove empty fields
sed -i "s/,,,,,//g" /tmp/expenses.csv
sed -i '/^\s*$/d' /tmp/expenses.csv

#put data
hadoop fs -put /tmp/expenses.csv /tmp
hadoop fs -ls -h /tmp/expenses.csv
rm /tmp/expenses.csv
```

**Dropping Hive table if exists**

```
%hive
drop table if exists `health_table`
```

**Create Hive table**

```
%hive

CREATE TABLE `health_table` (
`year` string ,
`state` string ,
`category` string ,
`funding_src1` string, 
`funding_src2` string,
`spending` int)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' STORED AS TextFile
```

**Load dataset into Hive table**

```
%hive

load data 
inpath '/tmp/expenses.csv'
into table health_table
```

**Grant permissions**

```
%hive

select count(*) from  health_table 
```

**Spending (In Billions) By State**

```
%hive

select state, sum(spending)/1000 SpendinginBillions
from health_table 
group by state
order by SpendinginBillions desc
```

**Spending (In Billions) By Year**

```
%hive

select year,sum(spending)/1000 SpendinginBillions 
from health_table 
group by year 
order by SpendinginBillions
```

**Spending (In Billions) By Category**

```
%hive

select category, sum(spending)/1000 SpendinginBillions from health_table 
group by category 
order by SpendinginBillions desc
```

The notebook for this section you can run just import it to the Zeppelin, click on  `Import note:` button and select the JSON file or put the link to the notebook:

<details> 
  <summary>Import Zeppelin notebook</summary>
  
![Import Zeppelin notebook](images/zeppelin-import.png)

</details>

[The Running Hive Queries in Zeppelin](notebook/running-hive-queries-in-zeppelin.json) notebook.


### About the dataset

Health expenditure in Australia data

```
  This dataset is public available for research. 
    Health expenditure in Australia
    Health expenditure occurs where money is spent on health goods and services. It occurs at different levels of government, 
    as well as by non-government entities such as private health insurers and individuals.
    In many cases, funds pass through a number of different entities before they are ultimately spent by providers 
    (such as hospitals, general practices and pharmacies) on health goods and services.
    The term ‘health expenditure’ in this context relates to all funds given to, or for, providers of health goods and services. 
    It includes the funds provided by the Australian Government to the state and territory governments, as well as the funds provided by the state and territory governments to providers.

    This data has been superseded, for more recent data on health expenditure, please the AIHW page on health expenditure.

This dataset was originally found on data.gov.au
    https://data.gov.au/data/dataset/f84b9baf-c1c1-437c-8c1e-654b2829848c
```

> The other example of using Hive you can find in the official [MapR Data Science Refinery documentation](https://mapr.com/docs/61/Zeppelin/ZeppelinHive.html)