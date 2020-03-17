FROM php:7.3.1-apache

RUN apt-get update && apt-get upgrade -y && apt-get autoremove -y

RUN apt-get install -y \
    wget \
    python \
    vim \
    libzip-dev \
    zip \
    git
    
# Install GD
RUN apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd

# Install Imagemagick
RUN apt-get install -y libmagickwand-dev --no-install-recommends \
    && pecl install imagick \
    && docker-php-ext-enable imagick \
    && rm -r /var/lib/apt/lists/*

# Install Intl
RUN apt-get install -y libicu-dev \
    && docker-php-ext-install intl

ENV XDEBUG_ENABLE 0
RUN pecl config-set preferred_state beta \
    && pecl install -o -f xdebug \
    && rm -rf /tmp/pear \
    && pecl config-set preferred_state stable

# Install Mysql
RUN docker-php-ext-install mysqli pdo_mysql

# Install mbstring
RUN docker-php-ext-install mbstring

# Install soap
RUN apt-get install -y libxml2-dev \
    && docker-php-ext-install soap

# Install opcache
RUN docker-php-ext-install opcache

# Install PHP zip extension
RUN docker-php-ext-install zip

# Install XML
RUN docker-php-ext-install xml

# Install MySQL CLI Client
RUN docker-php-ext-install mysqli

# Install PHP exif extension
RUN docker-php-ext-install exif

# Install Node.js
#RUN \
#    cd /tmp && \
#    wget http://nodejs.org/dist/v10.15.0/node-v10.15.0.tar.gz && \
#    tar xvzf node-v10.15.0.tar.gz && \
#    rm -f node-v10.15.0.tar.gz && \
#    cd node-v* && \
#    ./configure && \
#    CXX="g++ -Wno-unused-local-typedefs" make && \
#    CXX="g++ -Wno-unused-local-typedefs" make install && \
#    cd /tmp && \
#    rm -rf /tmp/node-v* && \
#    npm install -g npm && \
#    echo -e '\n# Node.js\nexport PATH="node_modules/.bin:$PATH"' >> /root/.bashrc

# Install PHP Unit
RUN \
    wget https://phar.phpunit.de/phpunit.phar && \
    chmod +x phpunit.phar && \
    mv phpunit.phar /usr/local/bin/phpunit && \
    phpunit --version

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer

# Define PHP_TIMEZONE env variable
ENV PHP_TIMEZONE Asia/Manila

# Configure Apache Document Root
ENV APACHE_DOC_ROOT /var/www/html

# Allow everyone to connect via SSH
RUN echo 'SSHD: ALL' >> /etc/hosts.allow

# Enable Apache mod_rewrite
RUN a2enmod rewrite
RUN a2enmod ssl

RUN useradd -ms /bin/bash joctabio -u1000
