FROM ubuntu

ARG UNIX_USER_ARG=1000
ENV UNIX_USER=${UNIX_USER_ARG}

RUN adduser --disabled-password --uid ${UNIX_USER} owner
RUN mkdir /home/owner/app
RUN chown -R owner:owner /home/owner

RUN DEBIAN_FRONTEND=noninteractive TZ=Etc/UTC apt-get update -y && \
    DEBIAN_FRONTEND=noninteractive TZ=Etc/UTC apt-get install -y git curl zip unzip less wget && \
    DEBIAN_FRONTEND=noninteractive TZ=Etc/UTC apt-get install -y npm
RUN DEBIAN_FRONTEND=noninteractive TZ=Etc/UTC npm install -g bower gulp gulp-cli

