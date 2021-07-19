FROM debian:bullseye
RUN echo 'deb http://deb.debian.org/debian bullseye main non-free contrib' > /etc/apt/sources.list && \
    echo 'deb-src http://deb.debian.org/debian bullseye main non-free contrib' >> /etc/apt/sources.list && \
    apt update && \
    apt install --no-install-recommends --no-install-suggests -y nginx-core libnginx-mod-http-image-filter libnginx-mod-http-geoip2 apt-src libbrotli-dev git ca-certificates libgl-dev geoipupdate && \
    mkdir -p /usr/src/nginx && \
    cd /usr/src/nginx && \
    apt-src update && \
    apt-src install nginx && \
    git clone https://github.com/google/ngx_brotli.git && \
    cd ngx_brotli && \
    git submodule update --init --recursive && \
    sed -i 's/--without-http_userid_module/--without-http_userid_module --add-dynamic-module=\/usr\/src\/nginx\/ngx_brotli/' /usr/src/nginx/nginx-*/debian/rules && \
    cd /usr/src/nginx && \
    apt-src build nginx
ADD conf/GeoIP.conf /etc/GeoIP.conf
RUN cp /usr/src/nginx/nginx-*/debian/build-light/objs/ngx_http_brotli_*_module.so /usr/lib/nginx/modules && \
    rm -rf /usr/src/nginx && \
    echo 'load_module modules/ngx_http_brotli_filter_module.so;' > /usr/share/nginx/modules-available/mod-http-brotli.conf && \
    echo 'load_module modules/ngx_http_brotli_static_module.so;' >> /usr/share/nginx/modules-available/mod-http-brotli.conf && \
    ln -s /usr/share/nginx/modules-available/mod-http-brotli.conf /etc/nginx/modules-enabled && \
    /usr/bin/geoipupdate && \
    echo 'fastcgi_param  GEOIP_COUNTRY      $geoip2_data_country_name;' >> /etc/nginx/fastcgi_params && \
    echo 'fastcgi_param  GEOIP_COUNTRY_CODE $geoip2_data_country_code;' >> /etc/nginx/fastcgi_params && \
    echo 'fastcgi_param  GEOIP_CITYNAME     $geoip2_data_city_name;' >> /etc/nginx/fastcgi_params && \
    echo 'fastcgi_param  GEOIP_REGION_GEONAMEID $geoip2_data_region_geonameid;' >> /etc/nginx/fastcgi_params && \
    echo 'fastcgi_param  GEOIP_REGION_ISO   $geoip2_data_region_iso;' >> /etc/nginx/fastcgi_params && \
    echo 'fastcgi_param  GEOIP_REGION       $geoip2_data_region_name;' >> /etc/nginx/fastcgi_params && \
    rm /etc/nginx/sites-enabled/default
ADD conf/nginx.conf /etc/nginx/
ADD conf/default.conf /etc/nginx/conf.d/
ADD conf/upstream.conf /etc/nginx/conf.d/
RUN usermod -u 1000 www-data
CMD ["nginx"]
EXPOSE 80 443
