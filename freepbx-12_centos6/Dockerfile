# install asterisk 13 on centos 6.6
FROM centos:centos6

# info
MAINTAINER szboardstretcher version:1.0

# update container
RUN yum install -y epel-release
RUN yum update -y
RUN yum groupinstall -y core
RUN yum groupinstall -y base
RUN yum install -y gcc gcc-c++ lynx bison mysql-devel mysql-server php php-xml php-mysql php-pear \
	php-mbstring tftp-server httpd make ncurses-devel libtermcap-devel sendmail sendmail-cf   \
	caching-nameserver sox newt-devel libxml2-devel libtiff-devel audiofile-devel gtk2-devel  \
	subversion kernel-devel git subversion kernel-devel php-process crontabs cronie 	  \
	cronie-anacron doxygen sqlite-devel vim wget speex-devel gsm-devel libuuid-devel

RUN pear channel-update pear.php.net
RUN pear install db

# asterisk source requirements
WORKDIR /root/

ADD http://srtp.sourceforge.net/srtp-1.4.2.tgz /root/srtp-1.4.2.tgz
ADD http://www.digip.org/jansson/releases/jansson-2.5.tar.gz /root/jansson-2.5.tar.gz
ADD http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-13-current.tar.gz /root/asterisk-13-current.tar.gz
RUN git clone https://github.com/asterisk/pjproject /root/pjproject

RUN tar xf srtp-1.4.2.tgz
RUN tar xf jansson-2.5.tar.gz
RUN tar xf asterisk-13-current.tar.gz

WORKDIR /root/srtp/
RUN autoconf && ./configure && sed -i "s/CFLAGS.= -Wall/CFLAGS = -fPIC -Wall/g" Makefile &&\
	make &&  make install && cp /usr/local/lib/libsrtp.a /lib

WORKDIR /root/pjproject/
RUN ./configure --prefix=/usr --enable-shared --disable-sound --disable-resample \
	--disable-video --disable-opencore-amr --with-external-speex --with-external-srtp        \
	--with-external-gsm --libdir=/usr/lib64 && \
	make dep && make && make install

WORKDIR /root/jansson-2.5
RUN ./configure --prefix=/usr/ && make && make install && ldconfig

WORKDIR /root/asterisk-13.0.0
RUN ./configure && make && make install


ENV VER_FREEPBX 12.0

WORKDIR /usr/src
RUN git clone http://git.freepbx.org/scm/freepbx/framework.git freepbx

WORKDIR /usr/src/freepbx
RUN git checkout release/${VER_FREEPBX}
RUN adduser asterisk -M -c "Asterisk User"
RUN touch /var/run/asterisk/asterisk.pid
RUN chown -R asterisk. /var/run/asterisk
RUN chown -R asterisk. /etc/asterisk
RUN chown -R asterisk. /var/{lib,log,spool}/asterisk
RUN chown -R asterisk. /usr/lib/asterisk
RUN chown -R asterisk. /var/www/
RUN sed -i "s/\(^upload_max_filesize = \).*/\\120M/" /etc/php.ini
RUN cp /etc/httpd/conf/httpd.conf /etc/httpd/conf/httpd.conf_orig
RUN sed -i "s/^\(User\|Group\).*/\\1 asterisk/" /etc/httpd/conf/httpd.conf
RUN sed -i "s/AllowOverride None/AllowOverride All/g" /etc/httpd/conf/httpd.conf
RUN ldconfig
RUN rm -rf /var/www/html

ENV ASTERISK_DB_PW amp109

WORKDIR /usr/src/freepbx
RUN service mysqld start
RUN mysqladmin -u root create asterisk
RUN mysqladmin -u root create asteriskcdrdb
RUN mysql -u root asterisk < SQL/newinstall.sql
RUN mysql -u root asteriskcdrdb < SQL/cdr_mysql_table.sql
RUN mysql -u root -e "GRANT ALL PRIVILEGES ON asterisk.* TO asteriskuser@localhost IDENTIFIED BY \'${ASTERISK_DB_PW}\';"
RUN mysql -u root -e "GRANT ALL PRIVILEGES ON asteriskcdrdb.* TO asteriskuser@localhost IDENTIFIED BY \'${ASTERISK_DB_PW}\';"
RUN mysql -u root -e "flush privileges;"
 with cd('/usr/src/freepbx:
RUN ./start_asterisk start
RUN ./install_amp --dbname asterisk --username asteriskuser --password ${ASTERISK_DB_PW}
RUN amportal a ma download manager
RUN amportal a ma install manager
RUN amportal a ma download userman
RUN amportal a ma install userman
RUN amportal a ma download sipstation
RUN amportal a ma install sipstation
RUN amportal a ma installall
RUN amportal chown
RUN amportal a ma refreshsignatures
RUN amportal a reload
RUN mysql -u root -D asterisk -e "UPDATE modules SET signature = ( SELECT signature FROM ( SELECT signature FROM modules WHERE id = 2 ) t ) WHERE id = 1;"
RUN ln -s /var/lib/asterisk/moh /var/lib/asterisk/mohmp3
RUN service httpd restart
RUN amportal restart
