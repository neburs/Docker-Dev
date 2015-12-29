FROM custom-centos-7:latest
MAINTAINER Ruben "ruben@vasallo.nom.es"

ENV TERM xterm-256color

#Configure TimeZone
RUN ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime

# Added nginx repo
RUN rpm --import https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
RUN echo "[mariadb]" > /etc/yum.repos.d/mariadb.repo && \
	echo "name=MariaDB" >> /etc/yum.repos.d/mariadb.repo && \
	echo "baseurl=http://yum.mariadb.org/10.0/centos7-amd64" >> /etc/yum.repos.d/mariadb.repo && \
	echo "gpgcheck=1" >> /etc/yum.repos.d/mariadb.repo && \	
	echo "gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB" >> /etc/yum.repos.d/mariadb.repo && \
	echo "enabled=1" >> /etc/yum.repos.d/mariadb.repo

# Install mariadb
RUN 	yum install -y \
	MariaDB-server-10.0.* \
	MariaDB-client-10.0.*

# Configure Store Data (uncomment if you like to save the mysql data in your host)
RUN sed -i "s#\[mariadb\]#\[mariadb\]\ndatadir=/datamysql\n#" /etc/my.cnf.d/server.cnf && \
	mkdir /datamysql && \
	chown mysql:mysql /datamysql

# Configure mariadb
RUN sed -i "s/\[mariadb\]/\[mariadb\]\ncollation-server = utf8_general_ci\ncharacter-set-server = utf8\n/" /etc/my.cnf.d/server.cnf && \
	/etc/init.d/mysql start; /usr/bin/mysqladmin -u root password 'test'

# For create new users connect to the database and execute this:
# CREATE USER 'user'@'localhost' IDENTIFIED BY 'password';
# GRANT ALL PRIVILEGES ON dbTest.* To 'user'@'localhost' IDENTIFIED BY 'password';

EXPOSE 3306

CMD ["mysqld_safe"]
