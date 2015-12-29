FROM php-fpm:latest
MAINTAINER Ruben "ruben@vasallo.nom.es"

ENV TERM xterm-256color
ENV userdeb nginx
ENV userid 1000
ENV groupdeb nginx
ENV groupid 1000
ENV LARA_ENV docker

# Install Node.js
RUN yum install -y \
        npm-1.3.* \
        nodejs-0.10.* \
	make tar bzip2

# Install Dependencies for packages
RUN yum install -y \
	libpng-devel-1.5.* 

# Configure NodeJs
RUN ln -s /usr/bin/node /usr/bin/nodejs

# Install Bower
RUN npm install -g bower@1.6.*

# Install Grunt
RUN npm install -g grunt-cli@0.1.*

# Install ApiDoc
RUN npm install -g apidoc@0.13.*

RUN chmod +r /etc/resolv.conf

RUN chown -Rf $userid:$groupid /home/$userdeb/

EXPOSE 8000 9000 9010 

CMD ["/opt/remi/php70/root/usr/sbin/php-fpm", "-F"]
