################################################################################
FROM debian:bookworm-slim
LABEL org.opencontainers.image.authors="github@arcenik.net"
LABEL org.opencontainers.image.source="https://github.com/arcenik/docker-authfromhost"

# Set apt-get to automatically retry if a package download fails
RUN echo 'Acquire::Retries "5";' > /etc/apt/apt.conf.d/99AcquireRetries
ADD http://httpredir.debian.org/debian/project/trace/ftp-master.debian.org /etc/trace_ftp-master.debian.org
ADD http://security.debian.org/project/trace/security-master.debian.org /etc/trace_security-master.debian.org

# Update and install sssd
RUN set -xe ;\
 apt-get -qq update ;\
 DEBIAN_FRONTEND=noninteractive apt-get -yqq -o=Dpkg::Use-Pty=0 install apt-utils ;\
 DEBIAN_FRONTEND=noninteractive apt-get -yqq -o=Dpkg::Use-Pty=0 dist-upgrade ;\
 DEBIAN_FRONTEND=noninteractive apt-get -yqq -o=Dpkg::Use-Pty=0 install sssd ;\
 DEBIAN_FRONTEND=noninteractive apt-get -yqq -o=Dpkg::Use-Pty=0 autoremove ;\
 apt-get -qq clean ;\
 rm -rf /var/lib/apt/lists/*

ADD files/sss_fromproxy /etc/pam.d/
