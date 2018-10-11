>/bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024<br>
>/sbin/mkswap /var/swap.1<br>
>/sbin/swapon /var/swap.1<br>

>cd /var/www/laravel<br>
>composer require barryvdh/laravel-ide-helper --dev<br>
>composer require doctrine/dbal --dev<br>

>vim config/app.php<br>

    'providers' => [
    
            ..................................
            Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider::class,
    
            ..................................
    
        ],

>php artisan vendor:publish --provider="Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider" --tag=config<br>

>vim config/ide-helper.php<br>

    'include_fluent' => true,
    'include_helpers' => true,

>php artisan clear<br>
>php artisan clear-compiled<br>
>php artisan config:clear<br>
>php artisan ide-helper:generate<br>
>php artisan optimize<br>

>vim composer.json<br>

    "scripts":{
          "post-update-cmd": [
                  "Illuminate\\Foundation\\ComposerScripts::postUpdate",
                  "php artisan ide-helper:generate",
                  "php artisan ide-helper:meta",
                  "php artisan optimize"
          ]
    },

>php artisan ide-helper:meta<br>
>php artisan ide-helper:models<br>

### Refernce
[Laravel 5 IDE Helper Generator](https://github.com/barryvdh/laravel-ide-helper)<br>
