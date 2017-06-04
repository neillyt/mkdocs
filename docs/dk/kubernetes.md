### Kubernetes

Minions = physical or virtual nodes
Pods = A cluster of one or more containers. each pod has it's own IP but pods can share ports
Labels = key/value pairs attached to any object in the system.
Selectors = Queries that are made against labels.
Controllers = Used in the management of the cluster. Controllers manage a set of Pods. Controllers use a YAML configuration file to maintain the desired state of the cluster. If a pod goes down the controller brings it up
Services = Allows pods to work together

Service definitions are written in YAML.

### Kubernetes Common Commands
man kubectl-get
kubectl get nodes = show all nodes connected to master
kubectl describe nodes
kubectl get nodes -o jsonpath='{.items[\*].status.addresses[?(@.type=="ExternalIP")].address}'
kubectl get pods

### Docker Common Commands  

-i = interactive  
-t = terminal  
-d = daemonize (run in background)  
-p = port   local:remote (80:80)  
  - this allows us to map a random local port to an application specific port on the container. For example, if you want multiple containers running Apache or Nginx, you can assign arbitrary ports on the localhost to port 80 on the container. If we have 4 containers, we can have:
    - port 55000:80 on container1
    - port 55001:80 on container2  
    - port 55002:80 on container3  
    - and so on.
- u = user ID to connect to container as


docker pull ubuntu
docker pull ubuntu:xenial
docker run -i -t ubuntu /bin/bash
docker run -itd ubuntu /bin/bash
docker inspect ubuntu
docker images
docker attach
docker ps = show all docker processes
docker logs <docker image> = show logs for container


### Getting Started
Install and Enable NTP  

`$ yum -y install chronyd`  
`$ systemctl enable chronyd`  


### Set up the Docker Repo  
Create the docker repo in /etc/yum.repos.d/

`$ vim /etc/yum.repos.d/docker.repo`  

```
[virt7-docker-common-release]
name=virt7-docker-common-release
baseurl=http://cbs.centos.org/repos/virt7-docker-common-release/x86_64/os/
gpgcheck=0
```

Download a docker Ruby container:  
`$ docker pull training/sinatra`

Launch that container interactively with a bash shell:  
`$ docker run -it training/sinatra /bin/bash`

Exit the container and look at all docker containers on the system:  
`$ docker ps -a`

Removing a docker image:  
`$ docker rmi <image id>`

#### The Dockerfile
A Dockerfile is the main configuration file for creating a custom Docker image. Example:  

`$ vim dockerfile`

```
# Dockerfile based on the latest CentOS 7 image - non privileged
FROM centos:latest
MAINTAINER tizitomm@gmail.com
RUN useradd -ms thn16
USER thn16
```

Build a Docker image using the Dockerfile we created:  
`$ docker build -t centos7/nonroot:v1 dockerfile`

Connect to your Docker container as the container's root user:  
`$ docker exec -u 0 -it <container_name> /bin/bash`


# Kubernetes

https://linuxacademy.com/cp/socialize/index/type/community_post/id/15747



### Configuring a Pod
Our configuration is a .YAML or .json file that will define the desired state of the Pod. This is how Kubernetes manages the Pods - when a Pod is no longer in the desired state Kubernetes will automatically make adjustments to the cluster to bring it to the desired state.  

Various configuration settings:
- apiVersion: the Kubernetes API version our file is using.
- kind: The type of configuration this is (Pod)
- metadata: Data about the Pod

Create a configuration file for nginx:  
`$ vim nginx.yaml`  

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.7.9
    ports:
    - containerPort: 80  
```

### Launch a Pod  
Using our "nginx.yaml" Pod configuration, use *kubectl* to launch a pod:  
`$ kubectl create -f ./nginx.yaml`
