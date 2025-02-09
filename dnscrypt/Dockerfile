FROM debian:bullseye-slim as downloader

ENV URL https://github.com/DNSCrypt/dnscrypt-proxy/releases/download/2.1.7/dnscrypt-proxy-linux_arm64-2.1.7.tar.gz

RUN apt-get update \
 && apt-get install -qqy \
    wget

RUN wget $URL -O /tmp/dnscrypt-proxy.tar.gz \
 ;  tar xzvf /tmp/dnscrypt-proxy.tar.gz -C /tmp/

FROM debian:bullseye-slim

RUN apt-get update \
 && apt-get upgrade -qqy \
 && apt-get install -qqy \
    ca-certificates \
 && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY --from=downloader /tmp/linux-arm64 /opt/dnscrypt-proxy

RUN mkdir /opt/dnscrypt-proxy/config

ENTRYPOINT [ "/opt/dnscrypt-proxy/dnscrypt-proxy", "-config",  "/opt/dnscrypt-proxy/config/dnscrypt-proxy.toml" ]
