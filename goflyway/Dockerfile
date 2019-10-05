FROM alpine:latest

RUN apk --no-cache add ca-certificates wget && \
    wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub && \
    wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.28-r0/glibc-2.28-r0.apk && \
    apk add glibc-2.28-r0.apk && \
    wget -N --no-check-certificate "https://github.com/coyove/goflyway/releases/download/2.0.0rc1/goflyway_linux_amd64.tar.gz" && \
    tar -xzf "goflyway_linux_amd64.tar.gz" && rm -rf "goflyway_linux_amd64.tar.gz" && \
    chmod +x goflyway

ADD entrypoint.sh /entrypoint.sh

RUN chmod +x entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
