FROM ubuntu:14.04

ENV CONSUL_VERSION 0.6.4 
ENV CONSUL_SHA256 abdf0e1856292468e2c9971420d73b805e93888e006c76324ae39416edcf0627
ENV SERVICE_NAME consul
 
RUN apt-get update && apt-get install -y curl ca-certificates unzip

ADD https://releases.hashicorp.com/consul/${CONSUL_VERSION}/consul_${CONSUL_VERSION}_linux_amd64.zip /tmp/consul.zip

RUN echo "${CONSUL_SHA256}  /tmp/consul.zip" > /tmp/consul.sha256 \
  && sha256sum -c /tmp/consul.sha256 \
  && cd /bin \
  && unzip /tmp/consul.zip \
  && chmod +x /bin/consul \
  && rm /tmp/consul.zip

EXPOSE 8300 8301 8301/udp 8302 8302/udp 8400 8500 53 53/udp 8600 8600/udp

ADD ./config /config/

VOLUME ["/data"]

ENTRYPOINT ["/bin/consul", "agent", "-config-dir=/config", "-data-dir=/data"]


