# Sharing Zeppelin Notebook

Apache Zeppelin has a pluggable notebook storage mechanism controlled by zeppelin.notebook.storage configuration option with multiple implementations. There are few notebook storage systems available for a use out of the box:

* (default) use local file system and version it using local Git repository - [GitNotebookRepo](https://zeppelin.apache.org/docs/0.8.1/setup/storage/storage.html#notebook-storage-in-local-git-repository)
* all notes are saved in the notebook folder in your local File System - [VFSNotebookRepo](https://mapr.com/docs/61/Zeppelin/ZeppelinDockerRunParameters.html#concept_rhn_gb2_rbb__section_fbc_zv2_5bb)
* all notes are saved in the notebook folder in hadoop compatible file system - [FileSystemNotebookRepo](https://zeppelin.apache.org/docs/0.8.1/setup/storage/storage.html#notebook-storage-in-hadoop-compatible-file-system-repository)
* storage using Amazon S3 service - [S3NotebookRepo](https://zeppelin.apache.org/docs/0.8.1/setup/storage/storage.html#notebook-storage-in-s3)
* storage using Azure service - [AzureNotebookRepo](https://zeppelin.apache.org/docs/0.8.1/setup/storage/storage.html#notebook-storage-in-azure)
* storage using Google Cloud Storage - [GCSNotebookRepo](https://zeppelin.apache.org/docs/0.8.1/setup/storage/storage.html#notebook-storage-in-google-cloud-storage)
* storage using MongoDB - [MongoNotebookRepo](https://zeppelin.apache.org/docs/0.8.1/setup/storage/storage.html#notebook-storage-in-mongodb)
* storage using GitHub - [GitHubNotebookRepo](https://zeppelin.apache.org/docs/0.8.1/setup/storage/storage.html#notebook-storage-in-github)

By default, Zeppelin stores notebooks in the local file system in your container. An alternative is to store them in MapR Filesystem. This allows you to share the notebooks with other users.

Multiple storage systems can be used at the same time by providing a comma-separated list of the class-names in the configuration. By default, only first two of them will be automatically kept in sync by Zeppelin.

To store notebooks in MapR Filesystem, see [Notebook Storage](https://mapr.com/docs/61/Zeppelin/ZeppelinDockerRunParameters.html#concept_rhn_gb2_rbb__section_fbc_zv2_5bb).


[More information amout notebook storage options for Apache Zeppelin](https://zeppelin.apache.org/docs/0.8.1/setup/storage/storage.html#overview)