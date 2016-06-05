# https://github.com/monitoringartist/zabbix-server-stress-test

## Dockerized compilation (build from remote URL or local PATH):
# docker build --rm=true -t local/zabbix-module-compilation https://github.com/monitoringartist/zabbix-server-stress-test.git#master:dockerfiles/centos/
# docker build --rm=true -t local/zabbix-module-compilation .
# docker run --rm -v /tmp:/tmp local/zabbix-module-compilation cp /root/zabbix/src/modules/zabbix_module_stress/zabbix_module_stress.so /tmp/zabbix_module_stress.so
# docker rmi -f local/zabbix-module-compilation
## use/copy file /tmp/zabbix_module_stress.so

# Define required CentOS version by using FROM tag. Avalaible: centos7/centos6/...
FROM centos:centos7

MAINTAINER "Jan Garaj" <info@monitoringartist.com>

# Define required Zabbix version (tag/<version>) or branch (branches/<version>), e.g. tags/2.4.5, or branches/2.4  
ENV ZABBIX_VERSION=branches/3.0

WORKDIR /root

RUN \
   yum -y install git subversion automake autoconf gcc make && \
   git clone https://github.com/monitoringartist/zabbix-server-stress-test && \
   mkdir ~/zabbix/ && \
   svn co svn://svn.zabbix.com/${ZABBIX_VERSION} ~/zabbix/ && \
   cd ~/zabbix/ && \
   ./bootstrap.sh && \
   ./configure --enable-agent && \
   cp -R ~/zabbix-server-stress-test/src/modules/zabbix_module_stress/ ~/zabbix/src/modules/ && \
   cd ~/zabbix/src/modules/zabbix_module_stress && \
   make
