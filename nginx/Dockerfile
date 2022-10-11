FROM nginx:1.15

ARG UNIX_USER_ARG=1000
ENV UNIX_USER=${UNIX_USER_ARG}

# create 'owner' user
RUN adduser --disabled-password --uid ${UNIX_USER} owner
RUN chsh -s /bin/bash owner

RUN sed -i -e 's/user  nginx;/user owner;/g' /etc/nginx/nginx.conf

RUN apt-get update
RUN apt-get install -y curl
RUN mkdir -p /var/www/html/
RUN chown -R owner:owner /var/www/

RUN apt-get install -y openssh-server
RUN service ssh start
