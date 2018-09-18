>echo 'deb http://ftp.de.debian.org/debian jessie main non-free' | tee -a /etc/apt/sources.list.d/sources.list<br>
>echo 'deb-src http://ftp.de.debian.org/debian jessie main non-free' | tee -a /etc/apt/sources.list.d/sources.list<br>
>apt-get update<br>
>apt-get install php7.3<br>
>apt-get install php7.3-cgi php7.3-cli php7.3-gd php7.3-mysql php7.3-curl php7.3-mbstring php7.3-xml<br>
>apt-get install libapache2-mod-php7.3<br>
>apt-get install libapache2-mod-fastcgi<br>
>a2dismod php5.6<br>
>a2enmod php7.3<br>
>a2enmod fastcgi<br>
>systemctl restart apache2<br>
>update-alternatives --set php /usr/bin/php7.3<br>
