FROM node:8.16.2-alpine

WORKDIR /usr/src/app

ENV VERSION a8bc5da2c1da282c96b6b85d7deaaf770b522563

RUN apk add --no-cache git \
    && git clone https://github.com/vergecurrency/bitcore.git ./ \
    && rm -rf ./packages/bitcor* \ 
    && rm -rf ./packages/crypto-wal* \
    && git checkout ${VERSION} \
    && sed -i 's/CHAIN=BTC/CHAIN=XVG/' ./packages/insight-previous/package.json \
    && sed -i 's/API_PREFIX=https:.*\/api//' ./packages/insight-previous/package.json \
    && sed -i 's/serve www/serve www -l tcp:\/\/0.0.0.0:5000/' ./packages/insight-previous/package.json \
    && cd /usr/src/app/packages/insight-previous/ \
    && npm install \
    && npm run build:prod 

WORKDIR /usr/src/app/packages/insight-previous/

EXPOSE 5000

HEALTHCHECK --interval=1m --timeout=30s --start-period=10s --retries=3 CMD [ "[ "`curl -o /dev/null -s -w "%{http_code}\n" http://localhost:5000/`" == "200" ]; echo $? " ]

ENTRYPOINT [ "npm", "run" ,"serve" ]
