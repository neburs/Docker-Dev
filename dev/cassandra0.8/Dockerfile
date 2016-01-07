FROM custom-centos-7:latest
MAINTAINER Ruben "ruben@vasallo.nom.es"

ENV TERM xterm-256color
ENV casandraversion 0.8.10
ENV CASSANDRA_CONF /cassandra/conf
ENV CASSANDRA_HOME /cassandra


#Configure TimeZone
RUN ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime

# install dependencies for cassandra
#Install Java8
RUN     cd /opt && \
        yum install -y wget && \
        wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u40-b25/jre-8u40-linux-x64.tar.gz" && \
        tar xvf jre-8*.tar.gz && \
        alternatives --install /usr/bin/java java /opt/jre1.8*/bin/java 1 && \
        rm -f /opt/jre-8*.tar.gz


# download cassandra
RUN 	cd / && \
	wget http://archive.apache.org/dist/cassandra/$casandraversion/apache-cassandra-$casandraversion-bin.tar.gz && \
	tar -xvf apache-cassandra-$casandraversion-bin.tar.gz && \
	mv apache-cassandra-$casandraversion cassandra && \
	rm apache-cassandra-$casandraversion-bin.tar.gz

#configure cassandra
RUN mkdir -p /var/log/cassandra && \
	mkdir -p /var/lib/cassandra && \
	sed -i 's#JVM_OPTS="$JVM_OPTS -Xss128k"#JVM_OPTS="$JVM_OPTS -Xss256k"#' /cassandra/conf/cassandra-env.sh && \
	ln -s /cassandra/bin/cassandra-cli /usr/bin/cassandra-cli && \
	ln -s /cassandra/bin /usr/local/share/cassandra

EXPOSE 9160

CMD [ "/cassandra/bin/cassandra", "-f" ]

# For connect
# docker exec -ti casandra-container cassandra-cli --host localhost
