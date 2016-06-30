FROM sequenceiq/hadoop-docker:2.5.2
MAINTAINER Jose Luis Salas

RUN curl -LO http://www-eu.apache.org/dist/pig/pig-0.16.0/pig-0.16.0.tar.gz
RUN tar xzf pig-0.16.0.tar.gz

ENV PATH $PATH:/pig-0.16.0/bin:/usr/local/hadoop/bin
