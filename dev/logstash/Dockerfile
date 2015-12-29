FROM custom-centos-7:latest
MAINTAINER Ruben "ruben@vasallo.nom.es"

ENV TERM xterm-256color

#Configure TimeZone
RUN ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime

#Install Java8
RUN     cd /opt && \
        yum install -y wget && \
        wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u40-b25/jre-8u40-linux-x64.tar.gz" && \
        tar xvf jre-8*.tar.gz && \
        alternatives --install /usr/bin/java java /opt/jre1.8*/bin/java 1 && \
        rm -f /opt/jre-8*.tar.gz

# Added logstash repo
RUN rpm --import http://packages.elasticsearch.org/GPG-KEY-elasticsearch && \
	echo "[logstash-2.1]" > /etc/yum.repos.d/logstash.repo && \
	echo "name=logstash repository for 2.x packages" >> /etc/yum.repos.d/logstash.repo && \
	echo "baseurl=http://packages.elasticsearch.org/logstash/2.1/centos" >> /etc/yum.repos.d/logstash.repo && \
	echo "gpgcheck=1" >> /etc/yum.repos.d/logstash.repo && \
	echo "gpgkey=http://packages.elasticsearch.org/GPG-KEY-elasticsearch" >> /etc/yum.repos.d/logstash.repo && \
	echo "enabled=1" >> /etc/yum.repos.d/logstash.repo 

# Install Logstash
RUN yum -y install logstash-2.1.*

# Configure JAVA_HOME
RUN ln -s /opt/jre* /opt/java
# For manual set type => export JAVA_HOME=/opt/java
ENV JAVA_HOME /opt/java
RUN mkdir -p /etc/pki/tls/certs; mkdir -p /etc/pki/tls/private
ADD logstash-forwarder.crt /etc/pki/tls/certs/logstash-forwarder.crt
ADD logstash-forwarder.key /etc/pki/tls/private/logstash-forwarder.key
ADD configuration/* /etc/logstash/conf.d/
RUN chmod -Rf +x /etc/logstash/conf.d/

EXPOSE 5001 

CMD ["/opt/logstash/bin/logstash", "-f", "/etc/logstash/conf.d/*.conf"]
