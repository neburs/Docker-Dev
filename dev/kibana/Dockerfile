FROM custom-centos-7:latest
MAINTAINER Ruben "ruben@vasallo.nom.es"

ENV TERM xterm-256color

#Configure TimeZone
RUN ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime

# Install dependences for kibana
RUN yum -y install wget 

# Install kibana
RUN cd /root && \
	wget https://download.elastic.co/kibana/kibana/kibana-4.3.1-linux-x64.tar.gz && \
	tar -zxvf kibana-*.tar.gz && \
	mv kibana-*/ /kibana && \
	rm kibana-*.tar.gz
	
# confgigure kibana
RUN echo "elasticsearch_url: \"http://elastic-search-01.sandbox:9200\"" >> /kibana/config/kibana.yml

EXPOSE 5601

CMD ["/kibana/bin/kibana"]
