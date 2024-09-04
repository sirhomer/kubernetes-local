FROM debian:9
FROM debian:9

# Agregar los repositorios antiguos de Debian 9 y evitar la validación de las fechas
RUN echo 'deb http://archive.debian.org/debian/ stretch main contrib non-free' > /etc/apt/sources.list && \
    echo 'deb http://archive.debian.org/debian-security/ stretch/updates main contrib non-free' >> /etc/apt/sources.list && \
    echo 'Acquire::Check-Valid-Until "false";' > /etc/apt/apt.conf.d/99no-check-valid-until

# Actualizar la lista de paquetes, actualizar el sistema e instalar Apache y PHP 7.0
RUN apt-get update && apt-get -y upgrade && DEBIAN_FRONTEND=noninteractive apt-get -y install \
    apache2 php7.0 php-mysql php-gd php-ldap php-curl libapache2-mod-php curl php-xml php-mbstring apache2-utils php7.0-gd


RUN a2enmod php7.0
RUN a2enmod rewrite
RUN a2enmod ssl

RUN sed -i "s/short_open_tag = Off/short_open_tag = On/" /etc/php/7.0/apache2/php.ini
RUN sed -i "s/error_reporting = .*$/error_reporting = E_ERROR | E_WARNING | E_PARSE/" /etc/php/7.0/apache2/php.ini
RUN sed -i 's/;extension=php_openssl.dll/extension=php_openssl.dll/' /etc/php/7.0/apache2/php.ini
RUN sed -i 's/max_execution_time = 30/max_execution_time = 120/' /etc/php/7.0/apache2/php.ini
RUN sed -i 's/max_input_time = 60/max_input_time = 120/' /etc/php/7.0/apache2/php.ini
RUN sed -i 's/max_file_uploads = 20/max_file_uploads = 200/' /etc/php/7.0/apache2/php.ini
RUN sed -i 's/max_input_vars = 1000/max_input_vars = 10000/' /etc/php/7.0/apache2/php.ini
RUN sed -i 's/memory_limit = 128M/memory_limit = 512M/' /etc/php/7.0/apache2/php.ini
RUN sed -i 's/post_max_size = 8M/post_max_size = 200M/' /etc/php/7.0/apache2/php.ini
RUN sed -i 's/upload_max_filesize = 2M/upload_max_filesize = 200M/' /etc/php/7.0/apache2/php.ini


#ENV APACHE_RUN_USER www-data

#ENV APACHE_RUN_GROUP www-data
#ENV APACHE_LOG_DIR /var/log/apache2
#ENV APACHE_LOCK_DIR /var/lock/apache2
#ENV APACHE_PID_FILE /var/run/apache2.pid
RUN echo "ServerSignature Off" >> /etc/apache2/apache2.conf
RUN echo "ServerTokens Prod" >>  /etc/apache2/apache2.conf
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf
RUN apt-get update && apt-get -y upgrade
# Limpiar la cache de APT para reducir el tamaño de la imagen
RUN apt-get clean && rm -rf /var/lib/apt/lists/*
EXPOSE 80
VOLUME ["/sitio/"]


WORKDIR /sitio/
ADD 000.conf /etc/apache2/sites-enabled/000-default.conf

CMD ["apache2ctl","-D FOREGROUND"]
