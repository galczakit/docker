FROM alpine

RUN adduser --disabled-password --uid 1000 owner
RUN mkdir /home/owner/app
RUN chown -R owner:owner /home/owner

RUN apk add git curl zip unzip less wget
RUN apk add --update npm
RUN npm install -g bower gulp gulp-cli
RUN echo http://dl-2.alpinelinux.org/alpine/edge/community/ >> /etc/apk/repositories
RUN apk --no-cache add shadow
RUN apk add coreutils
RUN apk add bash
