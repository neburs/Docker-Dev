FROM custom-centos-7:latest
MAINTAINER Ruben "ruben@vasallo.nom.es"

ENV TERM xterm-256color

#Configure TimeZone
RUN ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime

# Install sendmail 
RUN yum install -y \
	sendmail-8.14.* \
	sendmail-cf-8.14.* \
	m4-1.4.*

# configure
RUN	sed -i -e "s/DAEMON_OPTIONS(\`Port=smtp,Addr=127.0.0.1, Name=MTA')dnl/DAEMON_OPTIONS(\`Port=smtp, Name=MTA')dnl/g" /etc/mail/sendmail.mc && \
	m4 /etc/mail/sendmail.mc > /etc/mail/sendmail.cf && \
	echo "Connect:172                           RELAY" >> /etc/mail/access && \
	echo "Connect:10                            RELAY" >> /etc/mail/access && \
	makemap hash /etc/mail/access.db < /etc/mail/access

EXPOSE 25 

# NOTE: Run without dns configuration

CMD ["sendmail", "-bD", "-d0.4"]
