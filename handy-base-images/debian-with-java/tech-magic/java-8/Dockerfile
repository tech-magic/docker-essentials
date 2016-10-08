# Let us call this base docker image as "tech-magic/java-8"

FROM debian:8.0
# Official debian (jessie) base, see URL below for more info
# https://github.com/docker-library/official-images/tree/master/library

MAINTAINER Wimal Perera <wimal.perera@ebuilder.com>

# Control the java versions to be grabbed from oracle
ENV JAVA_VERSION 8u40
ENV JAVA_MINOR_VERSION b26

# Update debian
RUN apt-get update && apt-get upgrade -q -y

# Install wget and curl
RUN apt-get install -y wget curl

# Get and install oracle correctly (based on version above)
RUN cd /opt && \
    wget --dot-style=mega --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" \
         http://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION}-${JAVA_MINOR_VERSION}/jdk-${JAVA_VERSION}-linux-x64.tar.gz && \
    cd /opt && \
    tar xfvz jdk-${JAVA_VERSION}-linux-x64.tar.gz && \
    rm jdk-${JAVA_VERSION}-linux-x64.tar.gz && \
    cd /opt/jdk* && \
    export JAVA_HOME=`pwd` && \
    update-alternatives --install /usr/bin/java java ${JAVA_HOME}/bin/java 200 \
                        --slave /usr/bin/jar jar ${JAVA_HOME}/bin/jar \
                        --slave /usr/bin/jarsigner jarsigner ${JAVA_HOME}/bin/jarsigner \
                        --slave /usr/bin/javac javac ${JAVA_HOME}/bin/javac \
                        --slave /usr/bin/javadoc javadoc ${JAVA_HOME}/bin/javadoc \
                        --slave /usr/bin/javah javah ${JAVA_HOME}/bin/javah \
                        --slave /usr/bin/javap javap ${JAVA_HOME}/bin/javap \
                        --slave /usr/bin/javaws javaws ${JAVA_HOME}/bin/javaws

# make the "en_US.UTF-8" locale so utf-8 will enabled by default
RUN apt-get update && apt-get install -y locales && rm -rf /var/lib/apt/lists/* \
	&& localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8
ENV LANG en_US.utf8




