FROM azul/zulu-openjdk:8
MAINTAINER Magne Rasmussen <magne.rasmussen@gmail.com>

# Install unzip
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y unzip && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

ENV SONAR_VERSION 4.5.1

ADD sonarqube-4.5.1.zip /tmp/sonarqube-4.5.1.zip
# Install SonarQube
RUN addgroup --system --quiet sonar && \
    adduser --system --quiet --home /opt/sonar --no-create-home --ingroup sonar --disabled-password --shell /bin/bash --gecos 'Sonar' sonar && \
    echo 'wget -P /tmp http://dist.sonar.codehaus.org/sonarqube-$SONAR_VERSION.zip' && \
    unzip /tmp/sonarqube-$SONAR_VERSION.zip -d /opt && \
    rm /tmp/sonarqube-$SONAR_VERSION.zip && \
    mv /opt/sonarqube-$SONAR_VERSION /opt/sonar && \
    chown -R sonar:sonar /opt/sonar

# Update SonarQube configuration
RUN sed -e 's|#wrapper.java.additional.7=-server|wrapper.java.additional.7=-server|g' \
        -i /opt/sonar/conf/wrapper.conf

ADD run /usr/local/bin/run
RUN chmod +x /usr/local/bin/run

# Note: This container has no native volume, its expected to run with --volumes-from another

EXPOSE 9000

CMD ["/usr/local/bin/run"]
