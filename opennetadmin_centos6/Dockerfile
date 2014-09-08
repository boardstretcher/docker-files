# Dockerfile for opennetadmin
# https://github.com/boardstretcher/docker-files
FROM centos:centos6
MAINTAINER boardstretcher

# Variables
ENV SERVER_URL http://localhost:80

# Update container
RUN yum update -y

# Download and install software
ADD https://github.com/opennetadmin/ona/archive/ona-current.tar.gz /tmp/ona-current.tar.gz

# Licensed under "The MIT License" in 2014
