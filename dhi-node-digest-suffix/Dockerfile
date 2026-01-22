FROM dhi.io/node:25-debian13-dev@sha256:30eea31957c2c2037eb41c40e43f1b26f6b8537adfe0364808aa90988718f37b AS builder

WORKDIR /app

COPY package.json ./

RUN npm i --no-optional && npm cache clean --force

FROM dhi.io/node:25-debian13@sha256:eb1f1cd9b0cdd8f87c69a72d06999a7ecab3b9eabf25f977aae25e7596eb2a51

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