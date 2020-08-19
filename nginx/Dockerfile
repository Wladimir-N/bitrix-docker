FROM debian:bullseye
RUN apt update && \
    apt install --no-install-recommends --no-install-suggests -y nginx-light libnginx-mod-http-image-filter libnginx-mod-http-geoip2 apt-src libbrotli-dev git ca-certificates libgl-dev && \
    mkdir -p /usr/src/nginx && \
    cd /usr/src/nginx && \
    echo 'deb-src http://deb.debian.org/debian bullseye main non-free contrib' >> /etc/apt/sources.list && \
    apt-src update && \
    apt update && \
    apt-src install nginx && \
    git clone https://github.com/google/ngx_brotli.git && \
    cd ngx_brotli && \
    git submodule update --init --recursive && \
    sed -i 's/--without-http_userid_module/--without-http_userid_module --add-dynamic-module=\/usr\/src\/nginx\/ngx_brotli/' /usr/src/nginx/nginx-*/debian/rules && \
    cd /usr/src/nginx && \
    apt-src build nginx
RUN cp /usr/src/nginx/nginx-*/debian/build-light/objs/ngx_http_brotli_*_module.so /usr/lib/nginx/modules
ADD conf/nginx.conf /etc/nginx/
ADD conf/default.conf /etc/nginx/conf.d/
ADD conf/upstream.conf /etc/nginx/conf.d/
RUN usermod -u 1000 www-data
CMD ["nginx"]
EXPOSE 80 443
