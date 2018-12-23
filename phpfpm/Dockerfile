FROM  php:7.2-fpm

# Update system
RUN apt-get update && apt-get install -y --no-install-recommends \
    apt-utils \
    inetutils-ping \
    inetutils-traceroute \
    g++ \
    curl \
    openssl \
    libicu-dev \
    vim \
    joe \
    memcached \
    libmemcached-dev \
    zlib1g-dev \
    libxml2-dev \
    locate \
    sysstat \
    psmisc \
    dialog \
    syslog-ng \
    logrotate \
    less \
    strace \
    git \
    ssh \
    sudo \
    unzip \
    libgd2-xpm-dev \
#    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
#    && docker-php-ext-install -j$(nproc) gd \
    && php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer

#Install mysql client
RUN apt-get install -y mysql-client

#Install PHP extensions
RUN docker-php-ext-install pdo pdo_mysql
RUN docker-php-ext-install soap mbstring
RUN docker-php-ext-install bcmath
RUN docker-php-ext-install gd
RUN pecl install xdebug && docker-php-ext-enable xdebug
RUN printf "xdebug.remote_port=9999\nxdebug.remote_enable=on\nxdebug.remote_connect_back=1\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

# Configuration: System
RUN sed -i -e "s/ -nobackup/-nobackup/g" /etc/joe/joerc
RUN sed -i -e "s/#compress/compress/g" /etc/logrotate.conf

# Configuration: Time Zone
RUN printf '[PHP]\ndate.timezone = "America/Los_Angeles"\n' > /usr/local/etc/php/conf.d/tzone.ini

# Configuration: Error Logging
RUN sed -i -e "s/;log_level = notice/log_level = error/g" /usr/local/etc/php-fpm.d/www.conf
RUN sed -i -e "s/;syslog.facility/syslog.facility/g" /usr/local/etc/php-fpm.d/www.conf
RUN sed -i -e "s/;syslog.ident/syslog.ident/g" /usr/local/etc/php-fpm.d/www.conf

# Configuration: Error Logging: Log and Display Errors
RUN sed -i -e "s/;php_admin_flag\\[log_errors] = on/php_admin_flag[log_errors] = on/g" /usr/local/etc/php-fpm.d/www.conf
RUN sed -i -e "s/;php_flag\\[display_errors] = off/php_flag[display_errors] = on/g" /usr/local/etc/php-fpm.d/www.conf

# Configuraiton: Error Logging: Establish logging endpoints
RUN sed -i -e "s/;php_admin_value\\[error_log] = \/var\/log\/fpm-php.www.log/php_admin_value[error_log] = \/var\/log\/php-fpm\/error.log/g" /usr/local/etc/php-fpm.d/www.conf
#RUN sed -i -e "s/;error_log = log\/php-fpm.log/error_log = \/var\/log\/php-fpm\/error-error_log.log/g" /usr/local/etc/php-fpm.d/www.conf
RUN sed -i -e "s/;error_log = log\/php-fpm.log/error_log = \/proc\/1\/fd\/2/g" /usr/local/etc/php-fpm.d/www.conf

# Configuration: Error Logging: Directory
RUN mkdir /var/log/php-fpm
RUN chown www-data /var/log/php-fpm
RUN chgrp www-data /var/log/php-fpm

# Configuration: Catch Worker Output
RUN sed -i -e "s/;catch_workers_output = yes/catch_workers_output = yes/g" /usr/local/etc/php-fpm.d/www.conf

# Configuration: Memory Limit
RUN mkdir -p /usr/local/etc/php/conf.d
ADD usr/local/etc/php/conf.d/memory_limit.ini /usr/local/etc/php/conf.d

# Change www-data to run on uid 1000, so we don't get permission errors
RUN usermod -u 1000 www-data
RUN groupmod -g 1000 www-data

# Logrotate setup
ADD etc/logrotate.d/php-fpm /etc/logrotate.d

# Log filtering
#ADD usr/local/bin/catchOutput /usr/local/bin
#ADD usr/local/bin/sanitizeAndLog /usr/local/bin
#ADD etc/syslog-ng/conf.d/php-fpm-log.conf /etc/syslog-ng/conf.d
#ADD usr/local/bin/docker-php-extras /usr/local/bin
#RUN sed -i -e 's/#SYSLOGNG_OPTS/SYSLOGNG_OPTS/g' /etc/default/syslog-ng
#RUN sed -i -E 's/^(\s*)system\(\);/\1unix-stream("\/dev\/log");/' /etc/syslog-ng/syslog-ng.conf
#RUN sed -i -e 's/set -e/set -e\n\/usr\/local\/bin\/docker-php-extras/g' /usr/local/bin/docker-php-entrypoint
