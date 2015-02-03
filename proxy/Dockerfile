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
ADD files/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Install database for manage keystone information

RUN sudo apt-get install mariadb-server python-mysqldb -y
RUN service mysql restart

# Install keystone for authentication of user and tenant

RUN sudo apt-get install keystone python-keystoneclient -y
ADD files/keystone.conf /etc/keystone/keystone.conf
RUN rm -f /var/lib/keystone/keystone.db

# Install swift packages  
RUN mkdir -p /etc/swift
RUN apt-get install swift swift-proxy python-swiftclient python-keystoneclient python-keystonemiddleware memcached -y
ADD files/proxy-server.conf /etc/swift/proxy-server.conf
ADD files/swift.conf /etc/swift/swift.conf

# Copy ring files to the swift directory

ADD files/account.ring.gz /etc/swift/account.ring.gz
ADD files/container.ring.gz /etc/swift/container.ring.gz
ADD files/object.ring.gz /etc/swift/object.ring.gz


# Permission for swift

RUN chown -R swift:swift /etc/swift
RUN service supervisor restart





