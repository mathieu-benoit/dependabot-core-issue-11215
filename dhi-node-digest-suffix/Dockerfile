FROM dhi.io/node:25-debian13-dev@sha256:d9ad8f3a2acb57bd4a55de494dddf0c866e5851f2d561f74c13b1cddfefd2270 AS builder

WORKDIR /app

COPY package.json ./

RUN npm i --no-optional && npm cache clean --force

FROM dhi.io/node:25-debian13@sha256:7539746ae5fe68175b2f31d6221705cdaa90daa2b34d569292af483cea3bfc2d

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