FROM php:7.3-apache-stretch

# Install apt packages
#
# Required for php extensions
# * gd: libpng-dev, libjpeg62-turbo-dev
# * imagick: libmagickwand-dev
# * imap: libc-client-dev, libkrb5-dev
# * intl: libicu-dev
# * soap: libxml2-dev
# * zip: libzip-dev
#
# Used in the build process
# * git
# * nodejs
# * sudo
# * unzip
# * zip
#
# Other Utilities
# * bash-completion
# * iproute2 (required to get host ip from container)
# * msmtp-mta (for routing mail to maildev)
# * rsync
# * nano
# * vim
# * less

RUN apt-get update \
  && apt-get install -y --no-install-recommends \
  apt-transport-https

RUN curl -sL https://deb.nodesource.com/setup_12.x | bash - \
  && apt-get update \
  && apt-get install -y --no-install-recommends \
  bash-completion \
  default-mysql-client \
  git \
  iproute2 \
  libc-client-dev \
  libicu-dev \
  libjpeg62-turbo-dev \
  libkrb5-dev \
  libmagickwand-dev \
  libpng-dev \
  libxml2-dev \
  libzip-dev \
  msmtp-mta \
  nano \
  nodejs \
  rsync \
  sudo \
  unzip \
  vim \
  zip \
  && rm -r /var/lib/apt/lists/*

# Install php extensions (curl, json, mbstring, openssl, posix, phar
# are installed already and don't need to be specified here)
RUN docker-php-ext-install bcmath \
  && docker-php-ext-configure gd --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/ \
  && docker-php-ext-install gd \
  && docker-php-ext-install gettext \
  && docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
  && docker-php-ext-install imap \
  && docker-php-ext-install intl \
  && docker-php-ext-install mysqli \
  && docker-php-ext-install opcache \
  && docker-php-ext-install pdo_mysql \
  && docker-php-ext-install soap \
  && docker-php-ext-install zip

# Install and enable imagick PECL extensions
RUN pecl install imagick \
  && docker-php-ext-enable imagick

# Install xdebug PECL extension
RUN pecl install xdebug

RUN a2enmod rewrite

RUN a2enmod headers 

ARG BUILDKIT_UID=1000

ARG BUILDKIT_GID=$BUILDKIT_UID

RUN addgroup --gid=$BUILDKIT_GID buildkit

RUN useradd --home-dir /buildkit --create-home --uid $BUILDKIT_UID --gid $BUILDKIT_GID buildkit

COPY sudo /etc/sudoers.d/buildkit

USER buildkit

WORKDIR /buildkit

ENV PATH="/buildkit/bin:${PATH}"

RUN mkdir .composer

RUN mkdir .drush

RUN mkdir .npm

USER root

COPY msmtprc /etc/msmtprc

COPY apache.conf /etc/apache2/conf-enabled/civicrm.conf

RUN curl -LsS https://download.civicrm.org/civix/civix.phar -o /usr/local/bin/civix \
  && chmod +x /usr/local/bin/civix 

RUN php -r "readfile('https://github.com/drush-ops/drush/releases/download/8.1.17/drush.phar');" > drush \
  && chmod +x drush \
  && mv drush /usr/local/bin

RUN curl -LsS https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.5/wkhtmltox_0.12.5-1.stretch_amd64.deb -o /tmp/wkhtmltopdf.deb \
  && apt-get update \
  && apt install -y /tmp/wkhtmltopdf.deb
  
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
  && php composer-setup.php --install-dir=/usr/local/bin --filename=composer  

RUN a2enmod vhost_alias
  
COPY ./docker-civicrm-entrypoint /usr/local/bin

RUN chmod u+x /usr/local/bin/docker-civicrm-entrypoint

ENTRYPOINT [ "docker-civicrm-entrypoint" ]

CMD ["apache2-foreground"]
