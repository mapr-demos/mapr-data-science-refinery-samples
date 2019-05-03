# Zeppelin notebooks on MapR

Zeppelin notebooks are multipurpose notebooks that can handle all your analytics needs, from data ingestion, data discovery, data analytics, to data visualization and collaboration. 

After the Apache Zeppelin Docker image is running, access the Zeppelin notebook in your browser by specifying the following URL:

```
https://<docker-host-ip>:<port>
```

[]()

<details> 
  <summary>For our configuration</summary>
  
```
https://localhost:9996/
```

</details>

[]()

This URL loads the Zeppelin notebook’s home page. You must specify a secure URL.
If the Docker image is running on a remote node, such as a MapR edge node, replace `localhost` with the host name or IP address of the remote node. If you specified a different port number in your docker run command, replace `9996` with your port number.


<details> 

  <summary>Log in to Zeppelin using the user name and password you specified in your docker run command:</summary>
![Log in to Zeppelin](doc/tutorials/images/zeppelin-login.png)

</details>

[]()

<details> 

  <summary>Create your notebook:</summary>
![Create it in to Zeppelin](doc/tutorials/images/create-notebook.png)

</details>

[]()

<details> 
  <summary>The prepared Zeppelin notebook can import to your MapR DSR, click on Import note: button and select the JSON file or put the link to the notebook:</summary>
  
![Import Zeppelin notebook](doc/tutorials/images/zeppelin-import.png)

</details>