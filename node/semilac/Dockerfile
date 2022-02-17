FROM ubuntu

RUN adduser --disabled-password --uid 1000 owner
RUN mkdir /home/owner/app
RUN chown -R owner:owner /home/owner

RUN DEBIAN_FRONTEND=noninteractive TZ=Etc/UTC apt-get update -y && \
    DEBIAN_FRONTEND=noninteractive TZ=Etc/UTC apt-get install -y git curl zip unzip less wget && \
    DEBIAN_FRONTEND=noninteractive TZ=Etc/UTC apt-get install -y npm
RUN DEBIAN_FRONTEND=noninteractive TZ=Etc/UTC npm install -g bower gulp gulp-cli

