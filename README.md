# Running a UDPipe 2 server on Windows
If you have tried to run a local UDPipe 2 server on Windows, you may have encountered many problems. Here is a method using Docker that worked for me.

## Step 1
Install models from [LINDAT repository](https://lindat.mff.cuni.cz/repository/xmlui/?locale-attribute=en). In this example, [UDPipe 2.15 model](https://lindat.mff.cuni.cz/repository/xmlui/handle/11234/1-5797) is used.

## Step 2
Unzip it and find the folder with the model for the language you need. It must be a directory ***.model**.

## Step 3
Install [Docker](https://www.docker.com) and run the Docker Engine (you can open Docker Desktop, and it will be started automatically).

## Step 4
Create a directory with:
* a chosen model
* [udpipe2.py](https://github.com/ufal/udpipe/blob/e7e95586c92a6e07fbe71418611f83132ee342ca/udpipe2.py)
* [udpipe2_dataset.py](https://github.com/ufal/udpipe/blob/e7e95586c92a6e07fbe71418611f83132ee342ca/udpipe2_dataset.py)
* [udpipe2_eval.py](https://github.com/ufal/udpipe/blob/e7e95586c92a6e07fbe71418611f83132ee342ca/udpipe2_eval.py)
* [udpipe2_server.py](https://github.com/ufal/udpipe/blob/e7e95586c92a6e07fbe71418611f83132ee342ca/udpipe2_server.py)
* [wembedding_service](https://github.com/ufal/wembedding_service/tree/88b2aacff27ddfa3493abb5f9d5662b794b51d44)
* [Dockerfile](https://github.com/dmytryk28/udpipe-local-windows/blob/main/Dockerfile) (you can change the model or port if needed)

## Step 5
Build a Docker image:
```
docker build -t udpipe-server .
```
Run a Docker container:
```
docker run -p <HOST_PORT>:<CONTAINER_PORT> udpipe-server 
```
* HOST_PORT: the port on your host machine
* CONTAINER_PORT: the port within the container which is specified in Dockerfile

To stop the server you need to stop the container (use Docker Desktop) and delete it. When you start the server again, you don't need to build a new image, just run a new container.

## Step 6
Once all the modules are installed and the server is up and running, you can test it by opening the link in your browser:
```
http://localhost:<HOST_PORT>/process?tokenizer&tagger&parser&data=<TEXT>
```
* HOST_PORT: the port on your host machine
* TEXT: any text in the language for which you selected the UDPipe model

The first request may take a few minutes as additional dependencies are installed into the container. Subsequent requests will work faster.

## Step 7
You can work with your UDPipe server via [REST API](https://lindat.mff.cuni.cz/services/udpipe/api-reference.php) using localhost address and the chosen port.
