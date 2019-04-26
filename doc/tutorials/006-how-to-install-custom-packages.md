# How to install custom packages

You can install custom Python packages either by manually installing packages on each node in your MapR cluster or by using Conda. Using Conda allows you to perform the install from your Zeppelin host node without having to directly access your MapR cluster. 

You can run only version of Python in your Zeppelin notebook.

> Important: MapR supports the Python libraries included in the Zeppelin container, but does not support the libraries in custom Python packages. You should use Python versions that match the versions installed on your MapR cluster nodes. Choosing a Zeppelin Docker image OS that matches the OS running in your MapR cluster minimizes library version differences.

[Manually Installing Custom Packages for PySpark](https://mapr.com/docs/61/Zeppelin/ManualInstallPySpark.html)
Use the Python package manager pip (or pip3 for PySpark3) to manually install custom packages on each node in your MapR cluster. You need administrative access on your cluster nodes to install the packages.

[Installing Custom Packages for PySpark Using Conda](https://mapr.com/docs/61/Zeppelin/InstallPySparkCondaV1.1.html)
To install custom packages for Python 2 (or Python 3) using Conda, you must create a custom Conda environment and pass the path of the custom environment in your docker run command.


#### How to install custom packages