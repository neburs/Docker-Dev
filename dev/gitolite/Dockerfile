FROM centos:8
MAINTAINER Ruben "ruben@vasallo.nom.es"

ENV TERM xterm-256color
ENV userdeb gitedit
ENV userid 1000
ENV groupdeb gitedit
ENV groupid 1000
ENV gitolite_version v3.6.11

#Configure TimeZone
RUN ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime

# Configure Locale
RUN yum install -y glibc-locale-source && \
    localedef -v -c -i en_US -f UTF-8 en_US.utf8 || true

#Install server
RUN  yum -y install \
      git \
      perl \
      cpan \
      openssh-server \
      openssh-clients

#Configure Server
RUN mkdir /git && \
	groupadd --gid $groupid $groupdeb && \
	useradd -m -b /git --uid $userid --gid $groupid -K PASS_MAX_DAYS=-1 $userdeb
ENV HOME /git/$userdeb/

#Configure User & install gitolite
RUN	chown -Rf $userdeb:$groupdeb $HOME && \
	chmod +rx  $HOME && \
	cd /git && \
	git clone -b $gitolite_version git://github.com/sitaramc/gitolite && \
	mkdir -p $HOME/bin && \
	/git/gitolite/install -ln $HOME/bin && \
  mkdir -p /git/gitedit/.gitolite/logs/ && \
  chown -Rf $userdeb:$groupdeb /git
RUN ssh-keygen -A && \
    rm -f /run/nologin

CMD ["/usr/sbin/sshd", "-D"]


##### to configure
## first generate a key with this command
# ssh-keygen -N "" -f ~/.ssh/adm
## create next dir inside the container
# mkdir /git/gitedit/.ssh/
## copi the key inside the container
# dcker cp ~/.ssh/adm.pub container_name:/git/gitedit/.ssh/adm.pub
## execute inside the container the next command
# su gitedit -c "/git/gitolite/src/gitolite setup -pk /git/gitedit/.ssh/adm.pub"

##### to build the container
# docker build -t=custom-gitolite:3.6.11 --force-rm .
##### to run the container
# docker run -d -v ~/dir_gitolite_conf:/git/gitedit/ -p 23:22 custom-gitolite:3.6.11

# git clone git@localhost:gitolite-admin ~/gitolite-admin
