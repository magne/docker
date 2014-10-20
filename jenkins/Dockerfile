FROM azul/zulu-openjdk:8
MAINTAINER Magne Rasmussen <magne.rasmussen@gmail.com>

# Install Jenkins
RUN addgroup --system --quiet jenkins && \
    adduser --system --quiet --home /var/lib/jenkins --no-create-home --ingroup jenkins --disabled-password --shell /bin/bash --gecos 'Jenkins' jenkins && \
    mkdir -p /usr/share/jenkins /var/lib/jenkins /var/log/jenkins/ /var/cache/jenkins && \
    chown jenkins:jenkins /var/lib/jenkins /var/log/jenkins /var/cache/jenkins && \
    chmod u+rwx /var/lib/jenkins /var/log/jenkins && \
    chmod 750 /var/cache/jenkins

ADD http://mirrors.jenkins-ci.org/war/1.585/jenkins.war /usr/share/jenkins/jenkins.war

RUN chown -R jenkins:jenkins /usr/share/jenkins && \
    chmod -R u+rw /usr/share/jenkins/*

# Install git
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y git && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

ENV JENKINS_HOME /var/lib/jenkins

EXPOSE 8080

CMD su -l -c "export JENKINS_HOME=$JENKINS_HOME && java -Djava.awt.headless=true -Xmx256m -jar /usr/share/jenkins/jenkins.war --webroot=/var/cache/jenkins/war --httpPort=8080 --ajp13Port=-1" jenkins
