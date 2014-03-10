# Use the official Ubuntu Docker image
FROM ubuntu

# Put the nginx signing key into the container for use
ADD http://nginx.org/keys/nginx_signing.key /usr/local/nginx_signing.key

# Define our preferred mirror, add the nginx server, and install desired packages
RUN apt-key add /usr/local/nginx_signing.key
RUN echo "deb http://mirrors.xmission.com/ubuntu precise nginx" > /etc/apt/sources.list
RUN echo "deb http://nginx.org/packages/ubuntu precise nginx" > /etc/apt/sources.list.d/nginx.list
RUN apt-get update
RUN apt-get install -y nginx php5-fpm php-apc mysql-server php5-mysql

# A little prep work for nginx
RUN rm -rf /etc/nginx/conf.d
RUN mkdir /etc/nginx/sites-available
RUN mkdir /etc/nginx/sites-enabled

# Put nginx configuration files into place
ADD etc/nginx/fastcgi_params /etc/nginx/fastcgi_params
ADD etc/nginx/nginx.conf /etc/nginx/nginx.conf
ADD etc/nginx/sites-available/localhost /etc/nginx/sites-available/localhost
RUN chown -R root:root /etc/nginx

# Enable the localhost server in nginx
WORKDIR /etc/nginx/sites-enabled
RUN ln -s ../sites-available/localhost

# Move PHP-FPM files into place
ADD etc/php5/fpm/php.ini /etc/php5/fpm/php.ini
ADD etc/php5/fpm/php-fpm.conf /etc/php5/fpm/php-fpm.conf
ADD etc/php5/fpm/pool.d/www.conf /etc/php5/fpm/pool.d/www.conf
ADD etc/php5/conf.d/apc.ini /etc/php5/conf.d/apc.ini
RUN chown -R root:root /etc/php5

# Do a little cleanup for PHP-FPM logs
RUN mkdir /var/log/php5-fpm
RUN mv /var/log/php5-fpm.log /var/log/php5-fpm/fpm.log
RUN touch /var/log/php5-fpm/error.log
RUN chown www-data:www-data /var/log/php5-fpm/error.log

EXPOSE 22 80 443

ENTRYPOINT /usr/sbin/sshd -D
