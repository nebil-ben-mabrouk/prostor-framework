FROM prostor.java:8

MAINTAINER Nebil Ben Mabrouk "nebil.benmabrouk@orange.com"

RUN apt-get update && \
  apt-get install -y unzip wget supervisor docker.io openssh-server && \
  apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
  mkdir /var/run/sshd && \
  sed -i 's/PermitRootLogin without-password/PermitRootLogin yes/' /etc/ssh/sshd_config && \
  wget -q -O - http://mirrors.sonic.net/apache/storm/apache-storm-1.0.2/apache-storm-1.0.2.tar.gz | tar -xzf - -C /opt

ENV STORM_HOME /opt/apache-storm-1.0.2
RUN groupadd storm; useradd --gid storm --home-dir /home/storm --create-home --shell \
  /bin/bash storm; chown -R storm:storm $STORM_HOME; mkdir /var/log/storm ; \
  chown -R storm:storm /var/log/storm

RUN ln -s $STORM_HOME/bin/storm /usr/bin/storm

ADD storm.yaml $STORM_HOME/conf/storm.yaml
ADD cluster.xml $STORM_HOME/logback/cluster.xml
ADD config-supervisord.sh /usr/bin/config-supervisord.sh
ADD start-supervisor.sh /usr/bin/start-supervisor.sh

RUN chmod +x /usr/bin/config-supervisord.sh
RUN chmod +x /usr/bin/start-supervisor.sh

RUN echo [supervisord] | tee -a /etc/supervisor/supervisord.conf ; echo nodaemon=true | tee -a /etc/supervisor/supervisord.conf

