# Docker containers are supposedly made to run 1 service per container,.
# this is a dev container for testing nagios quickly - it runs all the
# services needed in one container. Do NOT USE THIS IN PRODUCTION.

# install nagios 4.0.8 on centos 7
FROM centos:centos7

# info
MAINTAINER szboardstretcher version: 0.1

# update container
RUN yum -y update
RUN yum -y install epel-release
RUN yum -y install gd gd-devel wget httpd php gcc make perl tar sendmail supervisor

# users and groups
RUN adduser nagios
RUN groupadd nagcmd
RUN usermod -a -G nagcmd nagios
RUN usermod -a -G nagios apache

# get archives

ADD http://downloads.sourceforge.net/project/nagios/nagios-4.x/nagios-4.0.8/nagios-4.0.8.tar.gz nagios-4.0.8.tar.gz
ADD http://www.nagios-plugins.org/download/nagios-plugins-2.0.3.tar.gz nagios-plugins-2.0.3.tar.gz

# install nagios
RUN tar xf nagios-4.0.8.tar.gz
RUN cd nagios-4.0.8 && ./configure --with-command-group=nagcmd
RUN cd nagios-4.0.8 && make all && make install && make install-init
RUN cd nagios-4.0.8 && make install-config && make install-commandmode && make install-webconf

# user/password = nagiosadmin/nagiosadmin
RUN echo "nagiosadmin:M.t9dyxR3OZ3E" > /usr/local/nagios/etc/htpasswd.users
RUN chown nagios:nagios /usr/local/nagios/etc/htpasswd.users

# install plugins
RUN tar xf nagios-plugins-2.0.3.tar.gz
RUN cd nagios-plugins-2.0.3 && ./configure --with-nagios-user=nagios --with-nagios-group=nagios
RUN cd nagios-plugins-2.0.3 && make && make install

# create initial config
RUN /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg

# some bug fixes
RUN touch /var/www/html/index.html
RUN chown nagios.nagcmd /usr/local/nagios/var/rw
RUN chmod g+rwx /usr/local/nagios/var/rw
RUN chmod g+s /usr/local/nagios/var/rw

# init bug fix
# RUN sed -i '/$NagiosBin -d $NagiosCfgFile/a (sleep 10; chmod 666 \/usr\/local\/nagios\/var\/rw\/nagios\.cmd) &' /etc/init.d/nagios

# remove gcc
RUN yum -y remove gcc

# port 80
EXPOSE 25 80

# supervisor configuration
ADD supervisord.conf /etc/supervisord.conf

# start up nagios, sendmail, apache
CMD ["/usr/bin/supervisord"]
