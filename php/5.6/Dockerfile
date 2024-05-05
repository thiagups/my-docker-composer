FROM ubuntu:focal

# Definir variável de ambiente DEBIAN_FRONTEND como noninteractive
ENV DEBIAN_FRONTEND=noninteractive

ENV ACCEPT_EULA=Y

RUN apt-get update && apt-get install -y software-properties-common \
    && add-apt-repository ppa:ondrej/php && apt-get update
    
# Instalar o Git
RUN apt-get update && apt-get install -y git

# Atualizar pacotes e instalar o Apache2
RUN apt-get update && apt-get install -y apache2

# Habilitar os módulos rewrite e headers
RUN a2enmod rewrite headers

# Instala o PHP 5.6 e extensões necessárias
RUN apt-get update -y && apt-get install -y php5.6 \
    php5.6-cli \
    php5.6-mysql \
    php5.6-mbstring \
    php5.6-xml \
    php5.6-zip \
    php5.6-gd \
    php5.6-curl \
    php5.6-intl \
    php5.6-dev

#PHP, extenções e outros pacote necessarios (#SOAP)
RUN apt-get update -y && apt-get install -y --no-install-recommends \
    libxml2-dev \
    libpng-dev \
    libjpeg-dev \
    libmcrypt4 \
    libmcrypt-dev \
    libcurl3-dev \
    libxslt-dev \
    libicu-dev \
    fontconfig \
    g++ \
    wget \
    curl \
    php-dev \
    nano \
    xvfb \
    libfontconfig1 \
    libjpeg8 \
    libxext6 \
    libxrender1 \
    xfonts-75dpi \
    git \
    xfonts-base \
    && rm -rf /var/lib/apt/lists/*

RUN a2enmod php5.6

RUN apt-get remove --purge -y php8*

RUN apt-get update -y && apt-get install -y libx11-dev \
    libjpeg62 \
    libxtst6

# Faz o download do wkhtmltopdf e o instala
# RUN wget -q -O wkhtmltox.deb "https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.stretch_amd64.deb" \
# RUN wget "https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.focal_amd64.deb" \
#COPY config/wkhtmltox_0.12.6-1.focal_amd64.deb /
#RUN dpkg -i /wkhtmltox_0.12.6-1.focal_amd64.deb \
#    && apt-get install -f -y \
#    && rm /wkhtmltox_0.12.6-1.focal_amd64.deb

RUN apt-get update -y \
    && apt-get install -y libical-dev \
    && apt-get install -y php-pear \
    && echo "extension=yaml.so" >> /etc/php/5.6/cli/php.ini

RUN apt-get install -y php-xml php5.6-xml
RUN sed -i "$ s|\-n||g" /usr/bin/pecl

# Download e extração do código-fonte da extensão DOM
#RUN mkdir /tmp/dom \
#    && cd /tmp/dom \
#    && wget https://github.com/php/php-src/archive/refs/tags/php-5.6.40.tar.gz \
#    && tar -xzf php-5.6.40.tar.gz --strip-components=1 \
#    && rm php-5.6.40.tar.gz \
#    && echo "extension=dom.so" >> /etc/php/5.6/apache2/php.ini \
#    && echo "extension=dom.so" >> /etc/php/5.6/cli/php.ini

# Instala a extensão Calendar
RUN apt-get update -y \
    && apt-get install -y libgmp-dev re2c libmhash-dev file

RUN apt-get update && apt-get install -y libc-client-dev libkrb5-dev \
    && rm -r /var/lib/apt/lists/*

# Instala as dependências necessárias
RUN apt-get update && apt-get install -y \
    libzip-dev \
    libfreetype6-dev \
    libonig-dev \
    libpq-dev

#IMAGEMAGICK
RUN apt-get update && apt-get install -y \
    libmagickwand-dev --no-install-recommends \
    && pecl install imagick

# Instalação da extensão gettext
RUN apt-get install -y gettext

# Instala o Composer
RUN apt-get install -y curl && \
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN rm -rf /etc/php/8*

RUN apt-get install -y php5.6-xdebug

RUN touch /tmp/xdebug.log \
    && chmod 777 -f /tmp/xdebug.log

# Configurar o Xdebug
RUN echo "zend_extension=/usr/lib/php/20131226/xdebug.so" > /etc/php/5.6/mods-available/xdebug.ini \
    && echo "xdebug.remote_enable=1" >> /etc/php/5.6/mods-available/xdebug.ini \
    && echo "xdebug.remote_autostart=1" >> /etc/php/5.6/mods-available/xdebug.ini \
    && echo "xdebug.auto_trace=1" >> /etc/php/5.6/mods-available/xdebug.ini \
    && echo "xdebug.remote_handler=dbgp" >> /etc/php/5.6/mods-available/xdebug.ini \
    && echo "xdebug.remote_port=9003" >> /etc/php/5.6/mods-available/xdebug.ini \
    && echo "xdebug.remote_connect_back=0" >> /etc/php/5.6/mods-available/xdebug.ini \
    && echo "xdebug.idekey=VSCODE" >> /etc/php/5.6/mods-available/xdebug.ini \ 
    && echo "xdebug.mode=debug,profile" >> /etc/php/5.6/mods-available/xdebug.ini \
    && echo "xdebug.start_with_request=yes" >> /etc/php/5.6/mods-available/xdebug.ini \
    && echo "xdebug.output_dir=/tmp/grinds" >> /etc/php/5.6/mods-available/xdebug.ini \
    && echo "xdebug.remote_log=/tmp/xdebug.log" >> /etc/php/5.6/mods-available/xdebug.ini \
    && echo "xdebug.default_enable=1" >> /etc/php/5.6/mods-available/xdebug.ini \
    && echo "xdebug.profiler_enable=0" >> /etc/php/5.6/mods-available/xdebug.ini \
    && echo "xdebug.auto_trace=0" >> /etc/php/5.6/mods-available/xdebug.ini \
    && echo "xdebug.coverage_enable=0" >> /etc/php/5.6/mods-available/xdebug.ini \
    && echo "xdebug.remote_host=host.docker.internal" >> /etc/php/5.6/mods-available/xdebug.ini
    # Exemplos
    # && echo "xdebug.remote_host=host.docker.internal" >> /etc/php/5.6/mods-available/xdebug.ini
    # && echo "xdebug.remote_host=192.168.2.140" >> $PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini

# xDebug
# RUN echo 'zend_extension=/usr/local/lib/php/extensions/no-debug-non-zts-20131226/xdebug.so' >> /etc/php/5.6/mods-available/xdebug.ini \

# Ativa o módulo Xdebug
# RUN ln -s /etc/php/5.6/mods-available/xdebug.ini /etc/php/5.6/cli/conf.d/20-xdebug.ini && \
#     ln -s /etc/php/5.6/mods-available/xdebug.ini /etc/php/5.6/apache2/conf.d/20-xdebug.ini

# RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf
RUN echo "ServerName database.mysql" >> /etc/apache2/apache2.conf

RUN sed -i 's/AllowOverride None/AllowOverride All/g' /etc/apache2/apache2.conf
RUN sed -i "s/short_open_tag = Off/short_open_tag = On/" /etc/php/5.6/apache2/php.ini

RUN echo "extension=gmp.so" > /etc/php/5.6/apache2/conf.d/gmp.ini
RUN echo "extension=mcrypt.so" > /etc/php/5.6/apache2/conf.d/mcrypt.ini

# Reinicia o servidor Apache
# RUN service apache2 restart

WORKDIR /var/www/html

EXPOSE 80

CMD ["apache2ctl", "-D", "FOREGROUND"]
# CMD ["tail","-f","/dev/null"]