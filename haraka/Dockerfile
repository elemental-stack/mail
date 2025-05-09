ARG node_ver=18
FROM node:${node_ver}-alpine3.16

ARG haraka_ver=2.8.28
ARG build_rev=2

COPY start.sh /start.sh
RUN chmod +x /start.sh

RUN apk update \
 && apk upgrade \
 && apk add --no-cache \
        ca-certificates \
 && update-ca-certificates \
    \
 # Install tools for building Haraka
 && apk add --no-cache --virtual .build-deps \
        python3 g++ make \
 && apk add certbot

# Build and install Haraka

RUN npm install -g Haraka@${haraka_ver} \
 && haraka -i /haraka/

# See: https://github.com/haraka/Haraka/issues/2746#issuecomment-580387065

RUN sed -i -e 's,^max_unrecognized_commands,#max_unrecognized_commands,' \
        /haraka/config/plugins

ENV HARAKA_HOME=/haraka

WORKDIR /


EXPOSE 25 587

CMD chmod 755 start.sh && ./start.sh
