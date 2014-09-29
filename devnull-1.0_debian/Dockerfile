# installation of DaaS in container
FROM debian 

MAINTAINER boardstretcher version: 1.0

# update container
RUN apt-get -y update
RUN apt-get -y upgrade

# nginx
RUN apt-get install -y nginx
ADD nginx.conf /etc/nginx/nginx.conf

# discard service
RUN apt-get install -y openbsd-inetd
ADD inetd.conf /etc/inetd.conf

# ports
EXPOSE 9
EXPOSE 80

# start up nginx
CMD /usr/sbin/inetd && nginx
