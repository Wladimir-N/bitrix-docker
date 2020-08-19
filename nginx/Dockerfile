FROM debian:bullseye
RUN apt update && \
    apt install --no-install-recommends --no-install-suggests -y nginx-light libnginx-mod-http-image-filter libnginx-mod-http-geoip2 apt-src libbrotli-dev git ca-certificates libgl-dev geoipupdate && \
    mkdir -p /usr/src/nginx && \
    cd /usr/src/nginx && \
    echo 'deb http://deb.debian.org/debian bullseye main non-free contrib' > /etc/apt/sources.list && \
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
ADD conf/GeoIP.conf /etc/GeoIP.conf
RUN rm -rf /usr/src/nginx && \
    cp /usr/src/nginx/nginx-*/debian/build-light/objs/ngx_http_brotli_*_module.so /usr/lib/nginx/modules && \
    echo 'load_module modules/ngx_http_brotli_filter_module.so;' > /usr/share/nginx/modules-available/mod-http-brotli.conf && \
    echo 'load_module modules/ngx_http_brotli_static_module.so;' >> /usr/share/nginx/modules-available/mod-http-brotli.conf && \
    ln -s /usr/share/nginx/modules-available/mod-http-brotli.conf /etc/nginx/modules-enabled && \
    /usr/bin/geoipupdate    
ADD conf/nginx.conf /etc/nginx/
ADD conf/default.conf /etc/nginx/conf.d/
ADD conf/upstream.conf /etc/nginx/conf.d/
ADD conf/return.conf /etc/nginx/
RUN usermod -u 1000 www-data
CMD ["nginx"]
EXPOSE 80 443
