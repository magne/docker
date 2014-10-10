FROM azul/zulu-openjdk:8
MAINTAINER Magne Rasmussen <magne.rasmussen@gmail.com>

# Install Tomcat's native lib and wget
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y libtcnative-1 wget && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

ENV TEAMCITY_VERSION 8.1.4
ENV TEAMCITY_DATA_PATH /opt/teamcity-data

# Install TeamCity
#    curl -L 
ADD TeamCity-8.1.4.tar.gz /opt
RUN addgroup --system --quiet teamcity && \
    adduser --system --quiet --home /opt/teamcity --no-create-home --ingroup teamcity --disabled-password --shell /bin/bash --gecos 'TeamCity' teamcity && \
    echo 'wget -O - http://download.jetbrains.com/teamcity/TeamCity-$TEAMCITY_VERSION.tar.gz | tar xz -C /opt' && \
    mkdir -p $TEAMCITY_DATA_PATH/lib/jdbc $TEAMCITY_DATA_PATH/config && \
    wget -P $TEAMCITY_DATA_PATH/lib/jdbc http://jdbc.postgresql.org/download/postgresql-9.3-1101.jdbc41.jar && \
    chown -R teamcity:teamcity /opt/* && \
    sed -e "s/\.*<\/Host>.*$/<Valve className=\"org.apache.catalina.valves.RemoteIpValve\" protocolHeader=\"x-forwarded-proto\" \/><\/Host>/" \
        -i /opt/TeamCity/conf/server.xml

ADD run /usr/local/bin/run
RUN chmod +x /usr/local/bin/run

VOLUME ["/opt/TeamCity/logs"]

EXPOSE 8111

CMD ["/usr/local/bin/run"]
