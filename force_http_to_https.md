    if ($server_port = 8080) {
      rewrite ^(/.*)$ https://$host$2:8443$request_uri permanent;
    }

    if ($server_port = 80) {
      rewrite ^(/.*)$ https://$host$1 permanent;
    }
