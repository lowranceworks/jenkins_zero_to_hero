FROM remote-host

COPY ./conf/nginx.repo /etc/yum.repos.d/nginx.repo

RUN                                                                          \
  yum -y install nginx-1.12.2 openssl --enablerepo=nginx                  && \
  yum -y install https://centos7.iuscommunity.org/ius-release.rpm         && \
  yum -y install                                                             \
    php72u-fpm                                                               \
    php72u-cli                                                               \
    php72u-mysqlnd                                                           \
    php72u-soap                                                              \
    php72u-xml                                                               \
    php72u-zip                                                               \
    php72u-json                                                              \
    php72u-mcrypt                                                            \
    php72u-mbstring                                                          \
    php72u-zip                                                               \
    php72u-gd                                                                \
     --enablerepo=ius && yum clean all

EXPOSE 80 443

VOLUME /var/www/html /var/log/nginx /var/log/php-fpm /var/lib/php-fpm

COPY ./conf/nginx.conf /etc/nginx/conf.d/default.conf

COPY ./bin/start.sh /start.sh

RUN mkdir -p /run/php-fpm  && \
    chmod +x /start.sh

CMD /start.sh