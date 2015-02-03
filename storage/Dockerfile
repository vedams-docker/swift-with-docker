FROM ubuntu:14.04
CMD /bin/bash
MAINTAINER Vedams <vedamsdocker@gmail.com>

# Install repository  for latest version of swift installation

RUN sudo apt-get update -y
RUN sudo apt-get install ntp -y
RUN sudo apt-get install vim -y
RUN sudo  apt-get install ubuntu-cloud-keyring
RUN sudo echo "deb http://ubuntu-cloud.archive.canonical.com/ubuntu" "trusty-updates/juno main" > /etc/apt/sources.list.d/cloudarchive-juno.list
RUN sudo apt-get update && apt-get dist-upgrade -y

# Install supervisor for handling sub-process of swift

RUN sudo apt-get install supervisor -y
RUN mkdir -p /var/log/supervisor

#Installing swift supporting file system. 

RUN sudo apt-get install xfsprogs rsync -y
RUN mkdir -p /etc/swift

# Installing storage packages
RUN sudo apt-get install swift swift-account swift-container swift-object -y

# Add swift storage file
ADD files/supervisord.conf /etc/supervisor/conf.d/supervisord.conf
ADD files/rsyncd.conf /etc/rsyncd.conf 
ADD files/account-server.conf /etc/swift/account-server.conf
ADD files/object-server.conf /etc/swift/object-server.conf
ADD files/container-server.conf /etc/swift/container-server.conf
ADD files/account.ring.gz /etc/swift/account.ring.gz
ADD files/container.ring.gz /etc/swift/container.ring.gz
ADD files/object.ring.gz /etc/swift/object.ring.gz
ADD files/swift.conf /etc/swift/swift.conf

RUN perl -pi -e 's/RSYNC_ENABLE=false/RSYNC_ENABLE=true/' /etc/default/rsync

RUN mkdir -p mkdir -p /srv/node/sdb1
RUN mkdir -p /var/cache/swift
RUN chown -R swift:swift /srv/node
RUN chown -R swift:swift /var/cache/swift





