FROM custom-centos-7:latest
MAINTAINER Ruben "ruben@vasallo.nom.es"

ENV TERM xterm-256color

#Configure TimeZone
RUN ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime

# Added nginx repo
RUN echo "[nginx]" > /etc/yum.repos.d/nginx.repo && \
	echo "name=nginx repo" >> /etc/yum.repos.d/nginx.repo && \
	echo "baseurl=http://nginx.org/packages/centos/\$releasever/\$basearch/" >> /etc/yum.repos.d/nginx.repo && \
	echo "gpgcheck=0" >> /etc/yum.repos.d/nginx.repo && \
	echo "enabled=1" >> /etc/yum.repos.d/nginx.repo 

# Install nginx
RUN yum install -y nginx-1.8.*
ADD nginx.conf /etc/nginx/nginx.conf

# Install Devel tools
RUN yum install -y vim

EXPOSE 80 443 
CMD ["nginx"]
