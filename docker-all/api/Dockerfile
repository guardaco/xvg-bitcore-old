FROM node:8.16.2-alpine

WORKDIR /usr/src/app

ENV BITCORE_CONFIG_PATH=/usr/src/app/bitcore.config.json

ENV VERSION a8bc5da2c1da282c96b6b85d7deaaf770b522563

RUN apk add --no-cache git python alpine-sdk \
    && git clone https://github.com/vergecurrency/bitcore.git ./ \
    && git checkout ${VERSION} \
    && rm -rf ./packages/bitcore-wal*  \ 
    && rm -rf ./packages/insight* \
    && rm -rf ./packages/crypto-wal* \
    && npm install \
    && npm i -g lerna \
    && lerna bootstrap \
    && apk del alpine-sdk

EXPOSE 3000

HEALTHCHECK --interval=1m --timeout=30s --start-period=10s --retries=3 CMD [ "[ "`curl -o /dev/null -s -w "%{http_code}\n" http://localhost:3000/api/XVG/mainnet/block/tip`" == "200" ]; echo $? " ]

ENTRYPOINT [ "npm", "run", "node" ]
