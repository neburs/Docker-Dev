FROM custom-centos-7:latest
MAINTAINER Ruben "ruben@vasallo.nom.es"

ENV TERM xterm-256color

#Configure TimeZone
RUN ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime

# Add epel and remi repos
RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-5.noarch.rpm; rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm

# Install Varnish
RUN rpm --nosignature -i https://repo.varnish-cache.org/redhat/varnish-4.1.el7.rpm && \
	yum install -y varnish-4.1.*

EXPOSE 80

CMD ["varnishd", "-F"]

# @see: https://fedoramagazine.org/varnish-site-faster-stable/
