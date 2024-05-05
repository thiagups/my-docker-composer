# Use a imagem base do Ubuntu 20.04 (Focal Fossa)
FROM ubuntu:focal

# Defina a variável de ambiente DEBIAN_FRONTEND como noninteractive
ENV DEBIAN_FRONTEND=noninteractive

# Atualize o sistema e instale software-properties-common
RUN apt-get update && apt-get install -y software-properties-common

# Adicione o repositório PPA do PHP 7.3
RUN add-apt-repository ppa:ondrej/php && apt-get update

# Instale o Git
RUN apt-get install -y git

# Atualize os pacotes e instale o Apache2
RUN apt-get update && apt-get install -y apache2

# Habilite os módulos rewrite e headers
RUN a2enmod rewrite headers

# Instale o PHP 7.3 e extensões necessárias
RUN apt-get install -y php7.3 \
    php7.3-cli \
    php7.3-mysql \
    php7.3-mbstring \
    php7.3-xml \
    php7.3-zip \
    php7.3-gd \
    php7.3-curl \
    php7.3-intl

# Instale extensões adicionais
RUN apt-get install -y libxml2-dev \
    libpng-dev \
    libjpeg-dev \
    libmcrypt-dev \
    libcurl3-dev \
    libxslt-dev \
    libicu-dev \
    fontconfig \
    g++ \
    wget \
    curl \
    nano \
    xvfb \
    libfontconfig1 \
    libjpeg8 \
    libxext6 \
    libxrender1 \
    xfonts-75dpi \
    xfonts-base

# Instale extensões e dependências adicionais
RUN apt-get install -y libx11-dev \
    libjpeg62 \
    libxtst6

# Instale o Composer
RUN apt-get install -y curl && \
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Remova as versões anteriores do PHP
RUN apt-get remove --purge -y php8*

# Configurar o Xdebug para PHP 7.3
RUN apt-get install -y php-xdebug

# Crie o arquivo de configuração para o Xdebug
RUN echo "zend_extension=xdebug.so" > /etc/php/7.3/apache2/conf.d/20-xdebug.ini && \
    echo "xdebug.remote_enable=1" >> /etc/php/7.3/apache2/conf.d/20-xdebug.ini && \
    echo "xdebug.remote_autostart=1" >> /etc/php/7.3/apache2/conf.d/20-xdebug.ini && \
    echo "xdebug.remote_handler=dbgp" >> /etc/php/7.3/apache2/conf.d/20-xdebug.ini && \
    echo "xdebug.remote_port=9003" >> /etc/php/7.3/apache2/conf.d/20-xdebug.ini && \
    echo "xdebug.remote_connect_back=0" >> /etc/php/7.3/apache2/conf.d/20-xdebug.ini && \
    echo "xdebug.idekey=VSCODE" >> /etc/php/7.3/apache2/conf.d/20-xdebug.ini && \
    echo "xdebug.mode=debug,profile" >> /etc/php/7.3/apache2/conf.d/20-xdebug.ini && \
    echo "xdebug.start_with_request=yes" >> /etc/php/7.3/apache2/conf.d/20-xdebug.ini && \
    echo "xdebug.output_dir=/tmp/grinds" >> /etc/php/7.3/apache2/conf.d/20-xdebug.ini && \
    echo "xdebug.remote_log=/tmp/xdebug.log" >> /etc/php/7.3/apache2/conf.d/20-xdebug.ini && \
    echo "xdebug.default_enable=1" >> /etc/php/7.3/apache2/conf.d/20-xdebug.ini && \
    echo "xdebug.profiler_enable=0" >> /etc/php/7.3/apache2/conf.d/20-xdebug.ini && \
    echo "xdebug.auto_trace=0" >> /etc/php/7.3/apache2/conf.d/20-xdebug.ini && \
    echo "xdebug.coverage_enable=0" >> /etc/php/7.3/apache2/conf.d/20-xdebug.ini && \
    echo "xdebug.remote_host=host.docker.internal" >> /etc/php/7.3/apache2/conf.d/20-xdebug.ini

# Configurar o Apache
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf
RUN sed -i 's/AllowOverride None/AllowOverride All/g' /etc/apache2/apache2.conf

# Configurar o PHP
RUN sed -i "s/short_open_tag = Off/short_open_tag = On/" /etc/php/7.3/apache2/php.ini
RUN echo "extension=gmp.so" > /etc/php/7.3/apache2/conf.d/gmp.ini
RUN echo "extension=mcrypt.so" > /etc/php/7.3/apache2/conf.d/mcrypt.ini

# Reiniciar o servidor Apache
CMD ["apache2ctl", "-D", "FOREGROUND"]
