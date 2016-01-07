FROM custom-centos-7:latest
MAINTAINER Ruben "ruben@vasallo.nom.es"

ENV TERM xterm-256color

#Configure TimeZone
RUN ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime

# Added mongo repo
RUN echo "[mongodb]" > /etc/yum.repos.d/mongodb.repo && \
	echo "name=MongoDB Repository" >> /etc/yum.repos.d/mongodb.repo && \
	echo "baseurl=https://repo.mongodb.org/yum/redhat/\$releasever/mongodb-org/3.2/x86_64/" >> /etc/yum.repos.d/mongodb.repo && \
	echo "gpgcheck=0" >> /etc/yum.repos.d/mongodb.repo && \
	echo "enabled=1" >> /etc/yum.repos.d/mongodb.repo

# Install MongoDB
RUN yum install -y \
	mongodb-org-3.2.* \
	mongodb-org-server-3.2.* \
	mongodb-org-shell-3.2.* \
	mongodb-org-mongos-3.2.* \
	mongodb-org-tools-3.2.*

# Configure MongoDb
RUN sed -i "/bindIp: 127.0.0.1/s/^/#/g" /etc/mongod.conf && \
	mkdir -p /data/db

EXPOSE 27017

CMD [ "mongod" ]
