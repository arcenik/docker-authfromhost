################################################################################
FROM eboraas/debian:testing
MAINTAINER Francois Scala "github@arcenik.net"

################################################################################
RUN apt-get update 
RUN DEBIAN_FRONTEND=noninteractive apt-get install -yq sssd
ADD files/sss_fromproxy /etc/pam.d/


