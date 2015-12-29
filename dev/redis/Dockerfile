FROM custom-centos-7:latest
MAINTAINER Ruben "ruben@vasallo.nom.es"

ENV TERM xterm-256color

#Configure TimeZone
RUN ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime

# Added epel and remi repos
RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-5.noarch.rpm; rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm

# Install Redis Server
RUN yum --enablerepo=remi install -y redis-3.0.*

# Configure Redis Server
RUN sed -i "/bind 127.0.0.1/s/^/#/g" /etc/redis.conf

# Configure Redis Cli
RUN ln -s /usr/bin/redis-cli /usr/bin/redis

EXPOSE 6379

CMD [ "/usr/bin/redis-server", "/etc/redis.conf"]
