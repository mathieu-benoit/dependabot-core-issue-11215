FROM dhi.io/node:25-debian13-dev@sha256:12f5c65af8e922ce25077b85d9350b52796d3fa12fc935067c956796f1687b53 AS builder

WORKDIR /app

COPY package.json ./

RUN npm i --no-optional && npm cache clean --force

FROM dhi.io/node:25-debian13@sha256:ab8fca9f45cdead538eb0f0fbb58247d356243f9b7923216a5f6dd3c9e277287

ENV BLUEBIRD_WARNINGS=0 \
  NODE_ENV=production \
  NODE_NO_WARNINGS=1 \
  NPM_CONFIG_LOGLEVEL=warn \
  SUPPRESS_NO_CONFIG_WARNING=true

WORKDIR /app

COPY --from=builder --chown=node:node /app/node_modules ./node_modules

COPY --chown=node:node . /app

CMD ["node","/app/app.js"]

EXPOSE 3000