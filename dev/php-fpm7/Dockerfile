FROM custom-centos-7:latest
MAINTAINER Ruben "ruben@vasallo.nom.es"

ENV TERM xterm-256color
ENV userdeb nginx
ENV userid 1000
ENV groupdeb nginx
ENV groupid 1000

#Configure TimeZone
RUN ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime

# Added epel and remi repos
RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-5.noarch.rpm; rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm

# Install configure root pw
# Remember to start the log system, execute "/etc/init.d/rsyslog start" using the root user
RUN echo "root:test" | chpasswd

# Install php-fpm 
RUN yum --enablerepo=remi install -y \
	php70-php-cli-7.0.* \
	php70-php-common-7.0.* \
	php70-php-fpm-7.0.* \
	php70-php-gd-7.0.* \
	php70-php-intl-7.0.* \
	php70-php-mbstring-7.0.* \
	php70-php-mcrypt-7.0.* \
	php70-php-mysqlnd-7.0.* \
#php56-php-pecl-oauth-1.2.* \
	php70-php-opcache-7.0.* \
	php70-php-pdo-7.0.* \
	php70-php-pear-1.10.* \
	php70-php-pecl-apcu-5.1.* \
	php70-php-pecl-geoip-1.1.* \
#php56-php-pecl-igbinary-1.2.* \
#php56-php-pecl-jsonc-1.3.* \
	php70-php-pecl-msgpack-2.0.* \
#php56-php-pecl-sqlite-2.0.* \
	php70-php-pecl-zip-1.13.* \
	php70-php-process-7.0.* \
	php70-php-xml-7.0.* \
	php70-php-pecl-xdebug-2.4.* \
#php56-php-pecl-memcache-3.0.8-5.el7.remi \
	php70-php-pecl-memcached-2.2.* \
#php56-php-pecl-mongo-1.5.8-1.el7.remi \
	php70-php-pecl-amqp-1.7.* \
#php56-php-pecl-redis-2.2.5-5.el7.remi \
	php70-php-bcmath-7.0.* \
	php70-php-imap-7.0.* \
	php70-php-pgsql-7.0.* \
#php56-php-curl-5.6.* \
	php70-php-imap-7.0.* \
	gettext-0.18.*

# Configure links to use php and pear
RUN ln -s /opt/remi/php70/root/usr/bin/php /usr/bin/php && \
	ln -s /opt/remi/php70/root/usr/bin/pear /usr/bin/pear && \
	ln -s /opt/remi/php70/root/usr/bin/php-cgi /usr/bin/php-cgi && \
	ln -s /opt/remi/php70/root/usr/bin/pecl /usr/bin/pecl
#You will must find the phpcsprecomit in /opt/remi/php70/root/usr/bin/scripts/phpcs-svn-pre-commit

# Add user for develop
RUN groupadd --gid $groupid $groupdeb; useradd -m -b /home --uid $userid --gid $groupid --shell /bin/bash $userdeb
RUN chmod +rx /etc/resolv.conf
ENV HOME /home/$userdeb/

# Install & configure Git
RUN yum install -y git
ADD .gitconfig /home/$userdeb/.gitconfig
RUN chown $userid:$groupid /home/$userdeb/.gitconfig

# Configure php
RUN sed -i 's#;date.timezone =.*#date.timezone = "Europe/Madrid"#' /etc/opt/remi/php70/php.ini && \
	sed -i 's#upload_max_filesize = 2M#upload_max_filesize = 35M#' /etc/opt/remi/php70/php.ini && \
	sed -i 's#memory_limit = 128M#memory_limit = 165M#' /etc/opt/remi/php70/php.ini && \
	sed -i 's#post_max_size = 8M#post_max_size = 35M#' /etc/opt/remi/php70/php.ini && \
	sed -i -e "s/daemonize\s*=\s*yes/daemonize = no/g" /etc/opt/remi/php70/php-fpm.conf && \
	sed -i "/^listen = /clisten = 0.0.0.0:9000" /etc/opt/remi/php70/php-fpm.d/www.conf && \
	sed -i "/^listen.allowed_clients/c;listen.allowed_clients =" /etc/opt/remi/php70/php-fpm.d/www.conf && \
	sed -i "/^;catch_workers_output/ccatch_workers_output = yes" /etc/opt/remi/php70/php-fpm.d/www.conf && \
	sed -i "s#user = apache#user = $userdeb #" /etc/opt/remi/php70/php-fpm.d/www.conf && \
	sed -i "s#group = apache#group = $groupdeb #" /etc/opt/remi/php70/php-fpm.d/www.conf
ADD 410/ /data/www/410/
RUN echo "xdebug.profiler_output_dir=/var/tmp/profiler" >> /etc/opt/remi/php70/php.d/15-xdebug.ini && \
	echo "xdebug.profiler_enable_trigger=0" >> /etc/opt/remi/php70/php.d/15-xdebug.ini && \
	echo "xdebug.profiler_enable=0" >> /etc/opt/remi/php70/php.d/15-xdebug.ini && \
	echo "xdebug.max_nesting_level=500" >> /etc/opt/remi/php70/php.d/15-xdebug.ini && \
	echo "xdebug.show_local_vars=1" >> /etc/opt/remi/php70/php.d/15-xdebug.ini && \
	echo "xdebug.remote_enable=1" >> /etc/opt/remi/php70/php.d/15-xdebug.ini && \
	echo "xdebug.remote_port=9015" >> /etc/opt/remi/php70/php.d/15-xdebug.ini && \
	echo "xdebug.remote_handler=dbgp" >> /etc/opt/remi/php70/php.d/15-xdebug.ini && \
	echo "xdebug.idekey=\"PHPSTORM\"" >> /etc/opt/remi/php70/php.d/15-xdebug.ini && \
	echo "xdebug.remote_log=\"/var/tmp/xdebug.log\"" >> /etc/opt/remi/php70/php.d/15-xdebug.ini && \
	echo "xdebug.remote_autostart=1" >> /etc/opt/remi/php70/php.d/15-xdebug.ini && \
	echo "xdebug.remote_connect_back=1" >> /etc/opt/remi/php70/php.d/15-xdebug.ini && \
	chown -Rf $userid:$groupid /data && \
	chown -Rf $userid:$groupid /etc/opt/remi/ && \
        chown -Rf $userid:$groupid /var/opt/remi/php70/ && \
	chmod -Rf u+w /etc/opt/remi/ && \
	chmod -Rf u+w /var/opt/remi/php70/

# Install Composer
RUN php -r "readfile('https://getcomposer.org/installer');" | php -- --install-dir=/usr/local/bin --filename=composer
ENV COMPOSER_HOME /home/$userdeb/.composer

# Install Devel tools
RUN yum install -y vim tig && \
	pear install PHP_CodeSniffer-2.4.0 && \
	ln -s /opt/remi/php70/root/usr/bin/phpcs /usr/bin/phpcs
RUN git clone -b 2.3.5 https://github.com/magicmonty/bash-git-prompt.git /home/$userdeb/.bash-git-prompt && \
	echo "GIT_PROMPT_ONLY_IN_REPO=1" >> /home/$userdeb/.bashrc && \
	echo "source ~/.bash-git-prompt/gitprompt.sh" >> /home/$userdeb/.bashrc && \
	chown -Rf $userid:$groupid /home/$userdeb/.bash-git-prompt
RUN yum install -y python && \
	cd /home/$userdeb && \
	git clone https://github.com/karan/joe.git joe && \
	cd joe && \
	python setup.py install && \
	rm -rf /home/$userdeb/joe
	
#You will must find the phpcsprecomit in /opt/remi/php70/root/usr/bin/scripts/phpcs-svn-pre-commit

#Install custom shell
RUN 	yum install -y wget && \
	cd /etc/yum.repos.d/ && \
	wget http://download.opensuse.org/repositories/shells:/fish:/release:/2/CentOS_7/shells:fish:release:2.repo && \
	yum install -y fish && \
	cd /home/$userdeb && \
	ln -s /home/$userdeb/.bash-git-prompt/ /home/$userdeb/.gitprompt 
RUN 	cd /home/$userdeb && wget -q https://raw.githubusercontent.com/oh-my-fish/oh-my-fish/master/bin/install && \
	chmod +x install
RUN 	cd /home/$userdeb && ./install || true
RUN 	rm /home/$userdeb/install
RUN	fish -c 'omf install' || true
RUN 	echo "set --global --export LANG en_US.UTF-8" >> /home/$userdeb/.config/fish/config.fish && \
	echo ". $fish_path/plugins/emoji-clock/emoji-clock.fish" >> /home/$userdeb/.config/fish/config.fish && \
	echo ". ~/.bash-git-prompt/gitprompt.fish" >> /home/$userdeb/.config/fish/config.fish && \
	echo "set fish_path $HOME/.oh-my-fish" >> /home/$userdeb/.config/fish/config.fish && \
	chown -Rf $userid:$groupid /home/$userdeb/.config/


# Add user for develop
#ADD sshkey/devglobal /home/$userdeb/.ssh/devglobal
RUN 	mkdir -p /home/$userdeb/.ssh && chown -Rf $userid:$groupid /home/$userdeb/.ssh && \
	echo "IdentityFile ~/.ssh/devglobal" >> /etc/ssh/ssh_config && \
	chmod +r /etc/resolv.conf

EXPOSE 9000 

CMD ["/opt/remi/php70/root/usr/sbin/php-fpm", "-F"]
