#http://phusion.github.io/baseimage-docker/#solution
#https://github.com/phusion/baseimage-docker

# Use phusion/baseimage as base image. To make your builds
# reproducible, make sure you lock down to a specific version, not
# to `latest`! See
# https://github.com/phusion/baseimage-docker/blob/master/Changelog.md
# for a list of version numbers.
# using latest version 0.9.16
FROM phusion/baseimage:0.9.16

MAINTAINER Sloppy Lopez <slopez@estudioweb130.com>

# Use baseimage-docker's init system.
CMD ["/sbin/my_init"]

# ...put your own build instructions here...
# Dependencies
# Add a repo where OpenJDK can be found.
RUN apt-get install -y software-properties-common
RUN add-apt-repository -y ppa:webupd8team/java
RUN apt-get update

# Auto-accept the Oracle JDK license
RUN echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | sudo /usr/bin/debconf-set-selections
RUN apt-get install -y oracle-java8-installer

# Set install version
ENV TC_PKG_NAME TeamCity-9.0.4.tar.gz
ENV DESTINATION TeamCity
ENV NODE_PLUGIN jonnyzzz.node.zip
ENV TC_SCRIPT teamcity

# Install ElasticSearch
RUN mkdir /opt/$DESTINATION; mkdir /opt/$DESTINATION/logs
COPY ./plugins/$NODE_PLUGIN /opt/$NODE_PLUGIN
COPY ./scripts/$TC_SCRIPT /etc/init.d/$TC_SCRIPT

RUN \
 cd /opt / && \
 wget http://download-cf.jetbrains.com/teamcity/$TC_PKG_NAME && \
 tar xvzf $TC_PKG_NAME && \
 rm -f $TC_PKG_NAME && \
 mv /opt/$NODE_PLUGIN ./$DESTINATION/plugins && \
 sudo useradd $TC_SCRIPT && \
 sudo chown -R $TC_SCRIPT:$TC_SCRIPT /opt/$DESTINATION && \
 sudo update-rc.d $TC_SCRIPT defaults && \
 sudo chmod +x /etc/init.d/$TC_SCRIPT

# Expose ports
EXPOSE 8111

# Define default command
CMD ["/opt/TeamCity/bin/teamcity-server.sh", "run"]

# ...put your own build instructions here...

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*