################################################################################
FROM debian:buster
MAINTAINER Francois Scala "github@arcenik.net"

# Set apt-get to automatically retry if a package download fails
RUN echo 'Acquire::Retries "5";' > /etc/apt/apt.conf.d/99AcquireRetries
ADD http://httpredir.debian.org/debian/project/trace/ftp-master.debian.org /etc/trace_ftp-master.debian.org
ADD http://security.debian.org/project/trace/security-master.debian.org /etc/trace_security-master.debian.org

# Update and install sssd
RUN set -xe ;\
 apt-get -qq update ;\
 DEBIAN_FRONTEND=noninteractive apt-get -yqq install apt-utils ;\
 DEBIAN_FRONTEND=noninteractive apt-get -yqq dist-upgrade ;\
 DEBIAN_FRONTEND=noninteractive apt-get -yqq install sssd ;\
 DEBIAN_FRONTEND=noninteractive apt-get -yqq autoremove ;\
 apt-get -qq clean ;\
 rm -rf /var/lib/apt/lists/*

ADD files/sss_fromproxy /etc/pam.d/
