FROM custom-centos-7:latest
MAINTAINER Ruben "ruben@vasallo.nom.es"

ENV TERM xterm-256color
ENV userdeb elasticuser
ENV userid 1000
ENV groupdeb elasticgroup
ENV groupid 1000

#Configure TimeZone
RUN ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime

#Install Java8
RUN 	cd /opt && \
	yum install -y wget && \
	wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u40-b25/jre-8u40-linux-x64.tar.gz" && \
	tar xvf jre-8*.tar.gz && \
	alternatives --install /usr/bin/java java /opt/jre1.8*/bin/java 1 && \
	rm -f /opt/jre-8*.tar.gz


# Added elastic search repo
RUN rpm --import https://packages.elastic.co/GPG-KEY-elasticsearch && \
	echo "[elasticsearch-2.x]" > /etc/yum.repos.d/elasticsearch.repo && \
	echo "name=Elasticsearch repository for 2.x packages" >> /etc/yum.repos.d/elasticsearch.repo && \
	echo "baseurl=http://packages.elastic.co/elasticsearch/2.x/centos" >> /etc/yum.repos.d/elasticsearch.repo && \
	echo "gpgcheck=1" >> /etc/yum.repos.d/elasticsearch.repo && \
	echo "gpgkey=http://packages.elastic.co/GPG-KEY-elasticsearch" >> /etc/yum.repos.d/elasticsearch.repo && \
	echo "enabled=1" >> /etc/yum.repos.d/elasticsearch.repo 
	

# Install ElasticSearch 
RUN yum install -y \
	elasticsearch-2.1.* \
	which 

# Add user for run elasticsearch
RUN groupadd --gid $groupid $groupdeb; useradd -m -b /home --uid $userid --gid $groupid --shell /bin/bash $userdeb
RUN chmod +rx /etc/resolv.conf
ENV HOME /home/$userdeb/

# Configure ElasticSearch
RUN  	mkdir -p /usr/share/elasticsearch/data \
		/usr/share/elasticsearch/logs \
		/usr/share/elasticsearch/config \
		/usr/share/elasticsearch/config/scripts && \
	chown -R $userdeb:$groupdeb /usr/share/elasticsearch/data \
                /usr/share/elasticsearch/logs \
                /usr/share/elasticsearch/config \
                /usr/share/elasticsearch/config/scripts

ADD logging.yml /usr/share/elasticsearch/config/logging.yml
ADD elasticsearch.yml /usr/share/elasticsearch/config/elasticsearch.yml
RUN chown -R $userdeb:$groupdeb /usr/share/elasticsearch/config/


# Install encoding
RUN /usr/share/elasticsearch/bin/plugin install analysis-icu

# Install elasticsearch-kopf plugin
RUN /usr/share/elasticsearch/bin/plugin install lmenezes/elasticsearch-kopf
# Note: for use this plugin type http://localhost:9200/_plugin/kopf in your favourite browser

# Install elasticsearch-head
RUN /usr/share/elasticsearch/bin/plugin install mobz/elasticsearch-head
# Note: for use this plugin type http://localhost:9200/_plugin/head in your favourite browser

USER $userdeb

EXPOSE 9200-9300

CMD ["/usr/share/elasticsearch/bin/elasticsearch"]
