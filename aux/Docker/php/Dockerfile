# Instalación de PHP en Ubuntu 16.04
FROM ubuntu:16.04
 
LABEL MAINTAINER="Ignasi Gomis"
 
RUN apt-get update \
  && apt-get install -y locales \
  && locale-gen en_US.UTF-8
 
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US:en
ENV LC_ALL=en_US.UTF-8
 
RUN apt-get update \
  && apt-get install -y \
    curl \
    git \
    software-properties-common \
    unzip \
    zip \
  && add-apt-repository -y ppa:ondrej/php \
  && apt-get update \
  && apt-get install -y \
    php7.2-cli \
    php7.2-curl \
    php7.2-fpm \
    php7.2-gd \
    php7.2-imap \
    php7.2-mbstring \
    php7.2-mysql \
    php7.2-pgsql \
    php7.2-xml \
    php-memcached \
  && php -r "readfile('https://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \
  && mkdir /run/php \
  && apt-get remove -y --purge software-properties-common \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
 
COPY php-fpm.conf /etc/php/7.2/fpm/php-fpm.conf
COPY www.conf /etc/php/7.2/fpm/pool.d/www.conf
 
EXPOSE 9000
CMD ["php-fpm7.2"]
