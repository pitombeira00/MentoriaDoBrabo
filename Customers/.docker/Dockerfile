FROM php:8.1.4

#PACOTE DE EXTENSOES PHP
ADD https://raw.githubusercontent.com/mlocati/docker-php-extension-installer/master/install-php-extensions /usr/local/bin/

#PERMISSÕES DE ESCRITA E LEITURA DA EXTENSÃO
RUN chmod uga+x /usr/local/bin/install-php-extensions && sync

#UPDATE E UPGRADE DOS PACOTES DA IMAGEM
RUN DEBIAN_FRONTEND=noninteractive apt-get update && apt-get upgrade -y -q \
    && DEBIAN_FRONTEND=noninteractive apt-get install -qq -y \
      build-essential \
      mariadb-client \
      curl \
      git \
      vim \
      zip unzip \
      redis-server \
      libhiredis-dev \
      autoconf \
      nginx-extras \
    && install-php-extensions \
        ctype \
        iconv \
        xml \
        tokenizer \
        mysqli \
        pdo_mysql \
        bcmath \
        uuid

RUN echo "memory_limit=-1" > $PHP_INI_DIR/conf.d/memory-limit.ini
COPY php/php.ini /usr/local/etc/php/


RUN pecl channel-update pecl.php.net \
	&& pecl install #XDEBUG_VERSION#

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && ln -s $(composer config --global home) /root/composer
ENV PATH=$PATH:/root/composer/vendor/bin COMPOSER_ALLOW_SUPERUSER=1

WORKDIR /app