    
    <VirtualHost *:*>
    ProxyPreserveHost On
    ProxyPass / http://192.168.1.31:1985/
    ProxyPassReverse / http://192.168.1.31:1985/
    ServerName localhost        
    </VirtualHost>
