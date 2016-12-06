# To build and run:
# docker build -t nagios:423_centos7 .
# docker run -t -i nagios:423_centos7

# Docker containers are supposedly made to run 1 service per container,.
# this is a dev container for testing nagios quickly - it runs all the
# services needed in one container. Do NOT USE THIS IN PRODUCTION.

# install nagios 4.2.3 on centos 7
FROM centos:centos7

# info
MAINTAINER szboardstretcher version: 0.1

# update container
RUN yum -y update
RUN yum -y install epel-release
RUN yum -y install gd gd-devel wget httpd php gcc make perl tar unzip sendmail supervisor

# users and groups
RUN adduser nagios
RUN groupadd nagcmd
RUN usermod -a -G nagcmd nagios
RUN usermod -a -G nagios apache

# get archives
ADD https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.2.3.tar.gz /root
ADD https://nagios-plugins.org/download/nagios-plugins-2.1.4.tar.gz /root

# install nagios
RUN cd /root/ && tar xf nagios-4.2.3.tar.gz
RUN cd /root/nagios-4.2.3 && ./configure --with-command-group=nagcmd
RUN cd /root/nagios-4.2.3 && make all && make install && make install-init
RUN cd /root/nagios-4.2.3 && make install-config && make install-commandmode && make install-webconf

# user/password = nagiosadmin/nagiosadmin
RUN echo "nagiosadmin:M.t9dyxR3OZ3E" > /usr/local/nagios/etc/htpasswd.users
RUN chown nagios:nagios /usr/local/nagios/etc/htpasswd.users

# install plugins
RUN cd /root && tar xf nagios-plugins-2.1.4.tar.gz
RUN cd /root/nagios-plugins-2.1.4 && ./configure --with-nagios-user=nagios --with-nagios-group=nagios
RUN cd /root/nagios-plugins-2.1.4 && make && make install

# create initial config
RUN /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg

# remove gcc
RUN yum -y remove gcc

# port 80
EXPOSE 25 80

# supervisor configuration
ADD supervisord.conf /etc/supervisord.conf

# start up nagios, sendmail, apache
CMD ["/usr/bin/supervisord"]
