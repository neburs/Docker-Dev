FROM centos:7
MAINTAINER Ruben "ruben@vasallo.nom.es"

ENV TERM xterm-256color
ENV userdeb gitedit
ENV userid 1000
ENV groupdeb gitedit
ENV groupid 1000

#Install server
RUN 	yum -y install git perl cpan openssh-server

#Configure Server
RUN 	mkdir /git && \
	groupadd --gid $groupid $groupdeb && \
	useradd -m -b /git --uid $userid --gid $groupid --shell /git/gitolite/src/gitolite-shell -K PASS_MAX_DAYS=-1 $userdeb && \
	chmod +rx /etc/resolv.conf && \
	sed -i 's/#Protocol 2/Protocol 2/' /etc/ssh/sshd_config && \
	/usr/sbin/sshd-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
ENV HOME /git/$userdeb/

#Configure User & install gitolite
ADD adm.pub $HOME/adm.pub
RUN	chown -Rf $userdeb:$groupdeb $HOME && \
	chmod +rx  $HOME && \
	cd /git && \
	git clone git://github.com/sitaramc/gitolite && \
	mkdir -p $HOME/bin && \
	/git/gitolite/install -ln $HOME/bin
RUN	$HOME/bin/gitolite setup -pk $HOME/adm.pub || true
RUN	chown -Rf $userdeb:$groupdeb $HOME


CMD ["/usr/sbin/sshd", "-D"]
