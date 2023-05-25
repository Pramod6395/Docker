# Docker

What is containers?
- A way to package application with all the necessary dependencies and configuration.
- Portable artifact,easily shared and moved around.
- Make development and devployment more efficient.

Where do container live?
- Private and Public repository like DokcerHub.

## Set up & Installation:
Update the apt package index, and install the latest version of Docker Engine, containerd, and Docker Compose, or go to the next step to install a specific version:

```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin

```
if got Error like below:

```bash
Failed to start Docker Application Container Engine
docker.service: Failed with result 'exit-code'
or
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
```

### Solution:

```bash
systemctl start docker

```

 Verify that Docker Engine is installed correctly by running the hello-world image.:

```bash
sudo docker run hello-world

```


## Uninstalled Docker Completely From System (Linux):

### Step 1: To identify what installed package you have.
          
```bash
dpkg -l | grep -i docker

```

### Step 2: Remove All Pacakges.
          
```bash
sudo apt-get purge -y docker-engine docker docker.io docker-ce docker-ce-cli
sudo apt-get autoremove -y --purge docker-engine docker docker.io docker-ce  

```

The above commands will not remove images, containers, volumes, or user created configuration files on your host. If you wish to delete all images, containers, and volumes run the following commands:

```bash
sudo rm -rf /var/lib/docker /etc/docker
sudo rm /etc/apparmor.d/docker
sudo groupdel docker
sudo rm -rf /var/run/docker.sock

```
You have removed Docker from the system completely.

## Basic Docker Commands:

#### Pull the image from dockerhub:
```bash
docker pull [image name:tag]

```

#### Run the image:
```bash
docker run [image name]

```
If image is not available locally then this command will first pull image from dockerhub and then run its (pull+run).

#### Run the image with detached mode:
```bash
docker run -d [image name]

```

#### List of the images present locally:
```bash
docker images ls

```

#### List all containes:
```bash
docker ps -a

```
#### Start/Stop containes:
```bash
docker start/stop [container ID]

```
#### Bind Ports of host with containers:
```bash
docker run -p [host port]:[container port] --name [container name] [image name:tag]

```
#### Check containers logs:
```bash
docker logs [container ID]

```

#### Enter into container (get container terminal):
```bash
docker exec -it [container ID] /bin/bash

```

#### Delete image:
```bash
docker rmi [image-id/name]

```
#### Delete container:
```bash
docker rm [container-id/name]

```

#### Create own docker network:
```bash
docker network create [network name]

```
container in the same network can talk with container name (no need of port)
#### List docker network:
```bash
docker network network ls

```

## Dockerfile:
In above section we learn how to work with exiting docker images but what if we want to create our own docker image for project ?.
We use Dockerfile to create the image for our project.
Follow below steps:

#### Create new folder in your directory:
```bash
mkdir docker_demo

```

#### Create vertual enviroment:
```bash
virtualenv demo

```
#### Activate vertual enviroment:
```bash
source demo/bin/activate

```
#### Create file name hello.py and put below contain in the file:
```bash
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"


```

#### Create file of name Dockerfile and put below contain in it:

```bash
# syntax=docker/dockerfile:1
FROM ubuntu:22.04

# install app dependencies
RUN apt-get update && apt-get install -y python3 python3-pip
RUN pip install flask==2.1.*

# install app
COPY hello.py /

# final configuration
ENV FLASK_APP=hello
EXPOSE 8000
CMD flask run --host 0.0.0.0 --port 8000

```
#### create image:
```bash
sudo docker build -t [image name:tag] [path of Dockerfile]
ex: sudo docker build -t hello:latest .

```
#### Run the created image:
```bash
sudo docker run -p 8000:8000 hello:latest

```
From your computer, open a browser and navigate to http://localhost:8000 you will see app is working.

#### Use Volumes so you can change code file from outside directory:
```bash
sudo docker run -p 8000:8000 -v [host pwd]:[container dir] hello:latest

```
#### Save the image in tar file:
```bash
sudo docker save [image name ] > imagename.tar

OR

sudo docker save [image name] | gzip > imnagename.tar.gz

```

#### Load the saved image:
```bash
sudo docker load < imagename.tar

```
#### How to push docker image to DockerHub:

```bash
docker login -u pramopatil95
docker tag myjenkins-blueocean:2.332.3-1 pramopatil95/test:V1
docker push pramopatil95/test:V1
docker logout

```
