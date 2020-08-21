FROM debian:bullseye
RUN apt update && \
    apt install --no-install-recommends --no-install-suggests -y mariadb-server mariadb-client
ADD my.cnf /etc/mysql/my.cnf
EXPOSE 3306
