> apt-get install php-opcache php-cli php-gd php-mysql php-curl php-mbstring php-xml php-gmp php-zip php-imagick<br>
> apt-get install php-pear php-dev<br>
> apt-get install gcc<br>
> apt-get install make<br>
> apt-get install autoconf<br>
> pecl install swoole<br>

> php -i | grep php.ini<br>
> echo "extension=swoole.so" >> /etc/php/7.4/cli/php.ini<br>
> php -m | grep swoole<br>
