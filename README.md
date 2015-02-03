# Docker Openstack Swift Multi-node 

This repository contains Dockerfile for creating openstack swift cluster. There is two Dockerfile one for proxy and another storage for creating images. The files folder contains related configuration files of proxy and storage node. First of all for running these Dockerfile need docker packages to your host OS. Install docker packages to your host.  Use Ubuntu 14.04 for installing docker.

## Swift Proxy Server Setup

Inside files folder there is ring files and configuration files related to swift proxy. Replace ring files as your swift cluster is configured and make changes in all configuration files as per your requirement. Change bind_ip inside proxy-server.conf file. After replacing and configure the proxy related file go to the directory which contains Docker file and run following command for creating proxy image.
```
docker build -t="swift-proxy" .
```
It will take some time for downloading and configuring image. After completing process for creating proxy container for swift object storage. Run below command for creating proxy container. Host networking mode is needed for this container to work, so the --net=host parameter must be used when starting up an instance of this container.
```
docker run -i -t --net=host --name="proxy" swift-proxy /bin/bash
```
Ensure proper ownership to /etc/swift folder.
```
chown -R swift:swift /etc/swift
```
Run the below command for start the all services of proxy node. Here we are using supervisor for controlling all services because docker container runs a single process when it is launched.
```
service mysql restart
service supervisor restart
``` 
Now create the database and keystone credential. For creating these things follow openstack keystone configuration guide.

## Swift Storage Server Setup

Replace all ring files inside files directory and configure all configuration files and run the following command for creating image.
```
docker build -t="swift-storage" .
```
In order for Docker to use whole file systems from its host OS, those file systems need to be mounted on the host OS, and they need to be passed to the container in the form of -v parameters. So create drives and write the file system over that drive and mount it.
For running storage container run following command
```
docker run -i -t --net=host --name="storage" -v /srv/node/sdb1:/srv/node/sdb1:rw swift-storage /bin/bash
```
Ensure proper ownership to all folders which related to storage container.
```
chown -R swift:swift /srv/node
chown -R swift:swift /etc/swift
```
Run the below command for start all services related to storage container.
```
service supervisor restart
swift-init all restart
```




  



	
