>apt-get install ca-certificates apt-transport-https<br>
>wget -q https://packages.sury.org/php/apt.gpg -O- | apt-key add -<br>
>echo "deb https://packages.sury.org/php/ jessie main" | tee /etc/apt/sources.list.d/php.list<br>
>apt-get update<br>
>apt-get install php7.3<br>
>apt-get install php7.3-fpm php7.3-opcache php7.3-cgi<br>
>apt-get install php7.3-cli php7.3-gd php7.3-mysql php7.3-curl php7.3-mbstring php7.3-xml<br>
>apt-get install libapache2-mod-php7.3<br>

>update-alternatives --set php /usr/bin/php7.3<br>

>a2dismod php5.6<br>
>a2enmod php7.3<br>

>a2dismod php7.3<br>
>a2dismod mpm_prefork<br>
>a2enmod  mpm_event<br>
>a2enmod  proxy_fcgi setenvif<br>
>a2enconf php7.3-fpm<br>

>vim /etc/apache2/ports.conf

    Listen 880

>nano /etc/apache2/sites-available/your_vh.conf<br>

    <VirtualHost *:880>
        .........................
        <FilesMatch \.php$>
                # Apache 2.4.10+ can proxy to unix socket
                #SetHandler "proxy:unix:/var/run/php/php5.6-fpm.sock|fcgi://localhost/"
                SetHandler "proxy:unix:/var/run/php/php7.3-fpm.sock|fcgi://localhost/"
        </FilesMatch>
    </VirtualHost>

>service apache2 restart<br>
