FROM alpine:latest

ENV CONSUL_VERSION=0.6.3
ENV S6_OVERLAY_VERSION=v1.17.2.0

ADD https://github.com/just-containers/s6-overlay/releases/download/${S6_OVERLAY_VERSION}/s6-overlay-amd64.tar.gz /tmp/
RUN gunzip -c /tmp/s6-overlay-amd64.tar.gz | tar -xf - -C /

ADD https://github.com/janeczku/go-dnsmasq/releases/download/1.0.0/go-dnsmasq_linux-amd64 /usr/local/bin/go-dnsmasq
RUN chmod +x /usr/local/bin/go-dnsmasq

RUN apk add --update bash openssl unzip && \
    rm -rf /var/cache/apk/*

RUN cd /tmp && \
    wget https://releases.hashicorp.com/consul/${CONSUL_VERSION}/consul_${CONSUL_VERSION}_linux_amd64.zip && \
    cd /usr/local/bin && \
    unzip /tmp/consul_${CONSUL_VERSION}_linux_amd64.zip && \
    mkdir -p /etc/consul && \
    mkdir -p /var/consul && \
    addgroup consul && \
    adduser -D -G consul consul && \
    chown consul:consul /var/consul && \
    rm -fr /tmp/*

ADD rootfs /

EXPOSE 8300 8301 8301/udp 8302 8302/udp 8400 53 53/udp

ENTRYPOINT ["/init"]

