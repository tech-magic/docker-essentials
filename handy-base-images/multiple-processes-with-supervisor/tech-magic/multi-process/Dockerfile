#------------------------------START BASE IMAGE -----------------------------
# We use base docker image centos version 7
FROM centos:7

MAINTAINER Dhanushka Gunawardane "dhanushka.gunawardhane@ebuilder.com"
MAINTAINER Wimal Perera "wimal.perera@ebuilder.com"

# Make the system upto date
RUN yum update -y

ENV REFRESHED_AT 2015-05-18

# Install tar, epel and supervisor
RUN yum install tar -y
RUN yum install epel-release -y
RUN yum install supervisor -y
RUN yum clean all

# Setup /root as work dir and copy downloaded jdk 1.8 setup and logstash 1.4.2.tar.gz into docker image
WORKDIR /root
COPY software/jdk-8u45-linux-x64.rpm /root/jdk-8u45-linux-x64.rpm
COPY software/logstash-1.4.2.tar.gz /opt/logstash/logstash-1.4.2.tar.gz

# We add 2 users
# User dropwizard (in dropwizard usergroup) will run dropwizard microservice
# User logstash (in logstash usergroup) will run logstash shipper
RUN adduser --system --user-group dropwizard
RUN adduser --system --user-group logstash

# Both dropwizard and logstash will have a common user group ebmservices
RUN groupadd -r ebmservices 

RUN usermod -a -G ebmservices dropwizard
RUN usermod -a -G ebmservices logstash

RUN mkdir /opt/dropwizard
RUN mkdir /var/log/dropwizard

RUN  chgrp -R ebmservices /var/log/supervisor
RUN  chgrp -R ebmservices /var/log/dropwizard
RUN  chmod -R 2770 /var/log/dropwizard

RUN  chown -R dropwizard:dropwizard /opt/dropwizard
RUN  chown -R logstash:logstash /opt/logstash

#------------------------------------------------------------------------------
#installing supervisor 
#------------------------------------------------------------------------------

RUN touch /etc/supervisord.conf

RUN echo "[supervisord]" >> /etc/supervisord.conf && \
    echo "nodaemon=true" >> /etc/supervisord.conf && \
    echo "" >> /etc/supervisord.conf

RUN echo "[program:logstash]" >> /etc/supervisord.conf && \
    echo "command=/opt/logstash/logstash/bin/logstash agent -f /opt/logstash/logstash_forwarder.conf" >> /etc/supervisord.conf && \
    echo "user=logstash" >> /etc/supervisord.conf && \
    echo "stderr_logfile = /var/log/supervisor/logstash_stderr.log" >> /etc/supervisord.conf && \
    echo "stderr_logfile_maxbytes = 50MB" >> /etc/supervisord.conf && \
    echo "stderr_logfile_backups = 10" >> /etc/supervisord.conf && \
    echo "stdout_logfile = /var/log/supervisor/logstash_stdout.log" >> /etc/supervisord.conf && \
    echo "stdout_logfile_maxbytes = 50MB" >> /etc/supervisord.conf && \
    echo "stdout_logfile_backups = 10" >> /etc/supervisord.conf && \
    echo "" >> /etc/supervisord.conf

#-------------------------------------------------------------------------------
#installing java
#-------------------------------------------------------------------------------

RUN rpm -Uvh jdk-8u45-linux-x64.rpm
RUN rm jdk-8u45-linux-x64.rpm
RUN alternatives --install /usr/bin/javac javac /usr/java/latest/bin/javac 200000

ENV PATH /usr/java/jdk1.8.0_45/jre/bin/java:$PATH
ENV JAVA_HOME /usr/java/jdk1.8.0_45

#--------------------------------------------------------------------------------
# install logstash [forwarder]
# -------------------------------------------------------------------------------

WORKDIR /opt/logstash
RUN /usr/bin/tar xzf logstash-1.4.2.tar.gz
RUN rm logstash-1.4.2.tar.gz
RUN mv logstash-1.4.2 logstash
ENV LOGSTASH_VERSION 1.4.2

WORKDIR /root

#-------------------------------END BASE IMAGE ----------------------------------


