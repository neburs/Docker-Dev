FROM custom-centos-7:latest
MAINTAINER Ruben "ruben@vasallo.nom.es"

ENV TERM xterm-256color

#Configure TimeZone
RUN ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime

# Install dependencies
RUN yum install -y \
	make \
	gcc \
	gcc-c++ \
	wget \
	zlib-devel \
	bzip2-devel \
	openssl-devel \
	ncurses-devel \
	sqlite-devel \
	readline-devel \
	tk-devel \
	gdbm-devel \
	db4-devel \
	libpcap-devel \
	xz-devel

#Install python
RUN mkdir /python && \
	cd /python && \
	wget https://www.python.org/ftp/python/2.7.11/Python-2.7.11.tgz && \
	tar xvzf Python-2.7.11.tgz && \
	cd Python-2.7.11 && \
	./configure --with-cxx-main=/usr/bin/g++ --prefix=/usr/local --enable-unicode=ucs4 --enable-shared LDFLAGS="-Wl,-rpath /usr/local/lib" && \
	make && \
	make altinstall 
	#ln -s -f /python/Python-2.7.11/python /usr/bin/python && \
	#sed -i 's&#!/usr/bin/python&#!/usr/bin/python2&' /usr/bin/yum 
#ENV PATH /python/Python-2.7.11/:$PATH

# Install pip python package manager
RUN mkdir /python/pip && \
	cd /python/pip && \
	wget https://bootstrap.pypa.io/get-pip.py && \
	/python/Python-2.7.11/python get-pip.py

# Install flask dev
RUN pip install flask

# Install Devel tools
RUN yum install -y \
	vim \
	git

EXPOSE 5000
ENTRYPOINT ["/python/Python-2.7.11/python"]

##For build
#docker build -t=flask-python --force-rm=true .

##For run type:
#docker run -d -p 5000:5000 --name=py-01 -h py-01 -v /projects/data:/data flask-python /data/index.py
