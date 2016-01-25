#DOCKER PROJECT

Prerequisites
---------------
You must have configured your computer for works git with github

1. Install docker core
---------------

* For Fedora
>
> $ sudo yum -y remove docker
> $ sudo yum -y install docker-io
>
> $ sudo systemctl start docker
>
> $ sudo groupadd docker
> $ sudo chown root:docker /var/run/docker.sock
> $ sudo usermod -a -G docker $USERNAME
>
> Note: For start at boot
> $ sudo systemctl enable docker
>
> Note2: Remember add interface docker0 to a trusted area in your firwall or disable the firewall
>
> $ firewall-cmd --zone=trusted --change-interface=docker0
>
> @see: https://fedoraproject.org/wiki/FirewallD

* For Ubuntu
>
> $ sudo apt-get update
> $ sudo apt-get install docker.io
>
> Note: Add the docker group if it doesn't already exist.
>
> $ sudo groupadd docker
>
> Note: Add the connected user "${USER}" to the docker group.
> Change the user name to match your preferred user.
> You may have to logout and log back in again for
> this to take effect.
>
> $ sudo gpasswd -a ${USER} docker
>
> Note: Restart the Docker daemon.
>  If you are in Ubuntu 14.04, use docker.io instead of docker
>
> $ sudo service docker restart
>
>
> Note: For prevent problems with file permissions please switch storage driver to devicemapper
>
> $ echo 'DOCKER_OPTS="$DOCKER_OPTS --storage-driver=devicemapper"' >> /etc/default/docker

* For Mac

See: [https://docs.docker.com/installation/mac/](https://docs.docker.com/installation/mac/)



NOTE 1: Be sure that your private key for bitbucked is in ~/.ssh/devglobal

NOTE 2: Be sure that your port 53 is free in your host pc (Use "lsof -Pnl +M -i4 | grep 53" to view if the port is free)

NOTE 3: If you've installed dnsmasq in your host, disable it (systemctl disable dnsmasq). See more in http://website-humblec.rhcloud.com/how-to-disable-dnsmasq-offered-dns-service/


2. Run for First time
---------------

>
> $ ./build --from-remote

If remote fail

>
> $ ./build --from-local

3. Start containers
---------------

>
> $ ./start


4. Stop containers
---------------

>
> $ ./stop

FOR USE
---------------

Edit your own /etc/hosts and add this hosts:
>
> ::1 api.sandbox
> ::1 web.sandbox

For join to a container

>
> ./dssh [the name of the container]

IF DECIDE USE REMOTE OPTION
---------------

Enable insecure remote registry on the docker daemon

* For Ubuntu
>
> $ echo 'DOCKER_OPTS=" --insecure-registry 192.168.1.82:5000"' >> /etc/default/docker
>

* For Fedora
>
> $ echo "OPTIONS='--selinux-enabled --insecure-registry 192.168.1.82:5000'" >> /etc/sysconfig/docker
>
 

