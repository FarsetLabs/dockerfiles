FROM java:7-jre
MAINTAINER Dan Porter <dpreid@gmail.com>

# MongoDB & JSVC
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10 \
    && echo "deb http://repo.mongodb.org/apt/debian wheezy/mongodb-org/3.0 main" > /etc/apt/sources.list.d/mongodb-org-3.0.list \
    && apt-get update && apt-get -y install mongodb-server jsvc

# UniFi 4.x
RUN curl -L -o unifi_sysvinit_all.deb https://www.ubnt.com/downloads/unifi/4.6.6/unifi_sysvinit_all.deb \
    && dpkg --install unifi_sysvinit_all.deb \
    && rm -f unifi_sysvinit_all.deb \
    && rm -rf /var/lib/unifi/*

EXPOSE 8080 8081 8443 8843 8880

VOLUME ["/var/lib/unifi"]

WORKDIR /var/lib/unifi

ADD run.sh /run.sh
RUN chmod 755 /run.sh

CMD ["/run.sh"]
