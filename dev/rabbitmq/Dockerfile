FROM custom-centos-7:latest
MAINTAINER Ruben "ruben@vasallo.nom.es"

ENV TERM xterm-256color

#Configure TimeZone
RUN ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime

# Added epel and remi repos
RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-5.noarch.rpm; rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm

# Install rabbitmq
RUN     yum install -y erlang-R16B-03.* && \
        rpm --import https://www.rabbitmq.com/rabbitmq-signing-key-public.asc && \
        rpm -ivh http://www.rabbitmq.com/releases/rabbitmq-server/v3.6.0/rabbitmq-server-3.6.0-1.noarch.rpm

# Install Plugins
# visit http://localhost:15672/ for view rabbitmq_management plugin
# NOTE: user: guest pw: guest
RUN timeout --foreground 300 rabbitmq-plugins enable --offline rabbitmq_management

# Configure env
RUN echo "[{rabbit, [{loopback_users, []}]}]." > /etc/rabbitmq/rabbitmq.config

EXPOSE 5672 15672

CMD ["/usr/lib/rabbitmq/bin/rabbitmq-server", "-F"]
