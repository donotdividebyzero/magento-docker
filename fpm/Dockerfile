FROM php:7.1-fpm

RUN useradd magento_user --create-home --user-group --groups www-data
RUN mkdir /home/magento_user/.composer \
    && mkdir /home/magento_user/.ssh \
    && chown -R magento_user:magento_user /home/magento_user/.ssh/ \
    && chown -R magento_user:magento_user /home/magento_user/.composer/ \
    && chmod +x /usr/local/bin/*

RUN apt-get update && apt-get install -y -q --force-yes \
        bzip2 \
        cron \
        fontconfig \
        g++ \
        libmcrypt-dev \
        libfreetype6-dev \
        libicu-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libxml2-dev \
        libxslt1-dev \
        mysql-client \
        vim \
        zlib1g-dev \
        git \
        curl \
        sudo \
    && rm -rf /var/lib/apt/lists/* \
    && pecl install xdebug \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd iconv intl json mbstring mcrypt opcache pdo_mysql soap xsl zip \
    && docker-php-ext-enable xdebug \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer\
    && rm -rf /var/lib/apt/lists/*

RUN curl -Lsf 'https://storage.googleapis.com/golang/go1.8.3.linux-amd64.tar.gz' | tar -C '/usr/local' -xvzf -
ENV PATH /usr/local/go/bin:$PATH
RUN go get github.com/mailhog/mhsendmail
RUN cp /root/go/bin/mhsendmail /usr/bin/mhsendmail

RUN apt-get update && apt-get install -y -q --force-yes gpg

RUN cd && curl -sL https://deb.nodesource.com/setup_6.x -o nodesource_setup.sh \
    && chmod +x nodesource_setup.sh && ./nodesource_setup.sh && apt install -y -q nodejs

RUN echo "magento_user ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

ENV PATH $PATH:/var/www/html/bin
USER magento_user
WORKDIR /var/www/html