FROM php:7.1-fpm

#base
RUN sed  -i 's/deb\.debian\.org/mirrors.163.com/g' /etc/apt/sources.list && \
    sed  -i 's/security\.debian\.org\/security/mirrors.163.com\/debian-security/g' /etc/apt/sources.list && \
    apt-get update -yqq && apt-get install git wget zip unzip tzdata supervisor -yqq  && \
    apt-get install -y locales && \
    apt-get install -y libmcrypt-dev libpq-dev libcurl4-gnutls-dev libicu-dev libvpx-dev \
    libjpeg-dev libpng-dev libxpm-dev zlib1g-dev libfreetype6-dev libxml2-dev libexpat1-dev \
    libbz2-dev libgmp3-dev libldap2-dev unixodbc-dev libsqlite3-dev libaspell-dev libpcre3-dev libtidy-dev \
    && rm -rf /var/lib/apt/lists/* 
    
# set timezone
ENV TZ  Asia/Shanghai
RUN echo $TZ > /etc/timezone && \
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && \
    dpkg-reconfigure -f noninteractive tzdata

# set locales
RUN locale-gen en_US.UTF-8 && DEBIAN_FRONTEND=noninteractive dpkg-reconfigure locales && locale-gen en_US.UTF-8  
ENV LANG=en_US.UTF-8 LANGUAGE=en_US:zh  LC_ALL=en_US.UTF-8
RUN localedef -i en_US -f UTF-8 en_US.UTF-8


RUN docker-php-ext-install pdo_mysql mbstring mcrypt curl json gd xml zip opcache pcntl bcmath

RUN usermod -u $UID www-data

# php composer
RUN php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" && \
    php composer-setup.php && php -r "unlink('composer-setup.php');" && \
    mv composer.phar /usr/local/bin/composer && chmod +x /usr/local/bin/composer && \
    composer config -g repo.packagist composer https://packagist.laravel-china.org

RUN composer global require "phpunit/phpunit=*" && \
    composer global require "squizlabs/php_codesniffer=*"

ENV PATH="$PATH:/root/.composer/vendor/bin"
    
# /usr/bin/supervisord -c /etc/supervisor/supervisord.conf --nodaemon
# docker build -t ksc/php:7.1-fpm . --no-cache
# docker tag ksc/php:7.1-fpm registry.cn-hangzhou.aliyuncs.com/58tu/php:7.1-fpm
# docker push registry.cn-hangzhou.aliyuncs.com/58tu/php:7.1-fpm