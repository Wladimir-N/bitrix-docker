FROM debian:bullseye
RUN apt update && \
    apt install --no-install-recommends --no-install-suggests -y nginx-light libnginx-mod-http-image-filter apt-src && \
    mkdir -p /usr/src/nginx && \
    cd /usr/src/nginx
    
ADD conf/nginx.conf /etc/nginx/
ADD conf/default.conf /etc/nginx/conf.d/
ADD conf/upstream.conf /etc/nginx/conf.d/

RUN usermod -u 1000 www-data

CMD ["nginx"]

EXPOSE 80 443
