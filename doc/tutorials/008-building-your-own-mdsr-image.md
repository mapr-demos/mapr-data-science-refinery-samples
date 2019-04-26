# Building your own MapR Data Science Refinery Docker Image
MapR provides a preconfigured and prepackaged Docker image for the MapR Data Science Refinery. But, you can build your own custom Docker image.

1. Determine the OS of the Docker image you want to build:
Operating System	URL

```sh
CentOS	- `https://package.mapr.com/labs/data-science-refinery/v1.3.2/redhat/`
Ubuntu	- `https://package.mapr.com/labs/data-science-refinery/v1.3.2/ubuntu/`
```

2. Download the Dockerfile corresponding to your OS:

```
wget https://package.mapr.com/labs/data-science-refinery/v1.3.2/redhat/Dockerfile
```

3. Build the Docker image:
```
docker build -t my_custom_dsr .
```

>Note: The sample command specifies the current working directory (.) as the build path.

4. Confirm that the image appears in the following command's output:

```
docker image list
```
To run and configure the image, see [Zeppelin on MapR](https://mapr.com/docs/61/Zeppelin/Zeppelin.html#Zeppelin).