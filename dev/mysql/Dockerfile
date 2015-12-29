FROM custom-centos-7:latest
MAINTAINER Ruben "ruben@vasallo.nom.es"

ENV TERM xterm-256color

#Configure TimeZone
RUN ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime

# Added epel mysql repo
RUN rpm -Uvh http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm; yum update -y

# Install MysqlServer
RUN yum install -y \
	mysql-community-server-5.6.* \
	mysql-community-client-5.6.*

# Configure mysql server (uncomment if you like to save the mysql data in your host)
RUN sed -i 's#datadir=/var/lib/mysql#datadir=/datamysql#' /etc/my.cnf

# Configure user root
RUN mysql_install_db --user=mysql && \
	(/usr/bin/mysqld_safe &); sleep 5; /usr/bin/mysqladmin -u root password 'root' \
     	sleep 5; echo "CREATE USER 'root'@'%' IDENTIFIED BY 'root'; GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION" | mysql -uroot -proot

EXPOSE 3306

CMD [ "mysqld_safe"]
