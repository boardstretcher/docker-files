# installation of DaaS in container
FROM debian 

MAINTAINER boardstretcher version: 1.0

# update container
RUN apt-get -y update
RUN apt-get -y upgrade
RUN apt-get install -y nginx

# configuration
ADD nginx.conf /etc/nginx/nginx.conf

# port 80
EXPOSE 80

# start up nginx
CMD ["nginx"]
