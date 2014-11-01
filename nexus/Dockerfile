FROM azul/zulu-openjdk:8
MAINTAINER Magne Rasmussen <magne.rasmussen@gmail.com>

ENV NEXUS_HOME /opt/nexus
ENV NEXUS_VERSION 3.0.0-m1

# Install Nexus
ADD nexus-base-template-3.0.0-SNAPSHOT.tar.gz /opt
RUN addgroup --system --quiet nexus && \
    adduser --system --quiet --home /opt/nexus --no-create-home --ingroup nexus --disabled-password --shell /bin/bash --gecos 'Nexus' nexus && \
    echo 'wget -O - http://www.sonatype.org/downloads/nexus-$NEXUS_VERSION-bundle.tar.gz | tar xz -C /opt' && \
    mv /opt/nexus-* /opt/nexus && \
    chown -R nexus:nexus /opt/nexus

ADD run /usr/local/bin/run
RUN chmod +x /usr/local/bin/run

# Note: This container has no native volume, its expected to run with --volumes-from another

EXPOSE 8081

CMD ["/usr/local/bin/run"]
