FROM centos:centos7

MAINTAINER szboardstretcher version: 0.1

RUN yum update -y

RUN yum -y install http://files.omdistro.org/releases/1.30/omd-1.30.rhel7.x86_64.rpm

RUN echo yes | omd setup
RUN cp /usr/lib64/python2.7/hashlib.py /opt/omd/versions/1.30/lib/python/

# Set up a default site with no TMPFS
RUN omd create default
RUN omd config default set TMPFS off
RUN omd config default set APACHE_TCP_ADDR 0.0.0.0

ADD runner.sh /runner.sh

EXPOSE 80 5000
ENTRYPOINT ["/runner.sh"]
