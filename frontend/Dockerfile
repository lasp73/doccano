FROM node:13.2-alpine

WORKDIR /app

COPY . /app/

# Runing git config --global url."https://".insteadOf git:// in order
# to prevent problems to connect to github (using 'git ls-remote')
# E.g.: "/usr/bin/git ls-remote -h -t git://github.com/adobe-webplatform/eve.git"

RUN apk add -U --no-cache git python3 make g++ \
  && git config --global url."https://".insteadOf git:// \
  && npm install -g npm@latest \
  && npm install \
  && npm run build \
  && apk del --no-cache git make g++
