# install sensu-client on centos 7
FROM centos:centos7

# info
MAINTAINER szboardstretcher version: 66.6

# requirements
RUN yum install -y epel-release
RUN yum makecache fast
RUN yum install -y wget deltarpm systemd-devel supervisor

# support go environment vars
ENV ENVTPL_VERSION=0.2.3
RUN \
	curl -Ls https://github.com/arschles/envtpl/releases/download/${ENVTPL_VERSION}/envtpl_linux_amd64 > /usr/local/bin/envtpl && \
	chmod +x /usr/local/bin/envtpl

# templates and bin
COPY conf/supervisord.conf /etc
COPY yum.repos.d/sensu.repo /etc/yum.repos.d
COPY templates /etc/sensu/templates

# install sensu
RUN \
	yum install -y sensu 

# install plugins
RUN \
	/opt/sensu/embedded/bin/gem install --no-rdoc --no-ri sensu-plugins-cpu-checks && \
	/opt/sensu/embedded/bin/gem install --no-rdoc --no-ri sensu-plugins-memory-checks && \
	/opt/sensu/embedded/bin/gem install --no-rdoc --no-ri sensu-plugins-disk-checks	&& \
	/opt/sensu/embedded/bin/gem install --no-rdoc --no-ri sensu-plugins-load-checks	

# environment variables
ENV \
    #Client Config
	CLIENT_NAME=sensu-CLIENTNUM \
	CLIENT_SUBSCRIPTIONS=all,default \
	CLIENT_BIND=127.0.0.1 \
	CLIENT_ADDRESS=127.0.0.1 \
	CLIENT_DEREGISTER=true \
	CLIENT_PID=/var/run/sensu/sensu-client.pid \
	LOG_LEVEL=warn \
	LOG_FILE=/var/log/sensu/sensu-client.log \

    #Transport
	TRANSPORT_NAME=redis \

	REDIS_HOST=104.236.75.69 \
	REDIS_PORT=6379 \

    #Common Config
	CONFIG_DIR=/etc/sensu/conf.d \
	CHECK_DIR=/etc/sensu/check.d \
	EXTENSION_DIR=/etc/sensu/extensions \
	PLUGINS_DIR=/etc/sensu/plugins \
	HANDLERS_DIR=/etc/sensu/handlers

RUN mkdir -p $CONFIG_DIR $CHECK_DIR $EXTENSION_DIR $PLUGINS_DIR $HANDLERS_DIR

RUN \
	cd /etc/sensu/templates/ && \
	for FILE in *.tmpl; do envtpl -in $FILE > /etc/sensu/conf.d/${FILE%.*}; done

CMD ["/usr/bin/supervisord"]

# vim
# set ts=4
