FROM nginx:1.15

ENV UNIX_USER=1000

# create 'owner' user
RUN useradd -u ${UNIX_USER} owner
RUN chsh -s /bin/bash owner

RUN sed -i -e 's/user  nginx;/user owner;/g' /etc/nginx/nginx.conf

RUN apt-get update
RUN apt-get install -y curl
RUN mkdir -p /var/www/html/
RUN chown -R owner:owner /var/www/
