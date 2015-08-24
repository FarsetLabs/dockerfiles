FROM farsetlabs/oracle-java8
MAINTAINER Dan Porter <dpreid@gmail.com>

# Define commonly used JAVA_HOME variable
ENV JAVA_HOME /usr/lib/jvm/java-8-oracle

# MongoDB & JSVC
RUN apt-get update \
    && apt-get -y install mongodb-server jsvc \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# UniFi 4.x
RUN wget -O unifi_sysvinit_all.deb https://www.ubnt.com/downloads/unifi/4.6.6/unifi_sysvinit_all.deb \
    && dpkg --install unifi_sysvinit_all.deb \
    && rm -f unifi_sysvinit_all.deb \
    && rm -rf /var/lib/unifi/*

EXPOSE 8080 8081 8443 8843 8880

VOLUME ["/var/lib/unifi"]

WORKDIR /var/lib/unifi

ADD run.sh /run.sh
RUN chmod 755 /run.sh

CMD ["/run.sh"]
