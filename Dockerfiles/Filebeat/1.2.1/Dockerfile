FROM ubuntu:14.04

ENV FILEBEAT_VERSION 1.2.2
ENV FILEBEAT_SHA1 824c0b3dce16e3efd7b72b5799e97cc865951ade
ENV SERVICE_NAME filebeat

RUN apt-get update && \
        apt-get install -y curl wget apt-transport-https

RUN curl -L -O https://download.elastic.co/beats/filebeat/filebeat_${FILEBEAT_VERSION}_amd64.deb
RUN dpkg -i filebeat_${FILEBEAT_VERSION}_amd64.deb

EXPOSE 6000

VOLUME ["/logs"]
VOLUME ["/dockerlogs"]

ENTRYPOINT ["/usr/bin/filebeat", "-e", "-v", "-c", "/etc/filebeat/filebeat.yml"]
