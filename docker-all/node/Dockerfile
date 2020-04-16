FROM ubuntu:18.04

ENV COIN_DATA /coin/data
ENV COIN_VERSION 6.0.2

RUN set -o errexit -o nounset \
    && apt update \
    && apt install wget net-tools curl -y \
    && rm -rf /var/cache/apt/ \
    && useradd -ms /bin/bash verge \
    && set -o errexit -o nounset \
    && cd /opt \
    && wget -O coin.tar.gz https://github.com/vergecurrency/verge/releases/download/v${COIN_VERSION}/verge-${COIN_VERSION}-x86_64-linux-gnu.tar.gz \
    && tar zxvf coin.tar.gz \
    && mkdir /coin /coin/data \
    && mv /opt/verge-${COIN_VERSION}/bin/verged /coin/  \
    && mkdir -p ${COIN_DATA} \
    && rm -rf /opt/* \
    && chown -R verge /coin

VOLUME ["/coin/data"]

COPY coin.conf /coin/coin.conf
USER verge

WORKDIR /coin

HEALTHCHECK --interval=1m --timeout=30s --start-period=10s --retries=3 CMD [ "[ "`curl -o /dev/null -s -w "%{http_code}\n" http://localhost:20102/`" == "405" ]; echo $? " ]

ENTRYPOINT [ "/coin/verged", "-datadir=/coin/data", "-conf=/coin/coin.conf" ]

EXPOSE 20102 21102 22332
