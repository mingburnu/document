>apt-get update && apt-get upgrade<br>
>apt-get install software-properties-common<br>
>echo 'deb http://ftp.utexas.edu/dotdeb/ stable all' | tee /etc/apt/sources.list.d/dotdeb.list<br>
>echo 'deb-src http://ftp.utexas.edu/dotdeb/ stable all' | tee -a /etc/apt/sources.list.d/dotdeb.list<br>
>wget https://www.dotdeb.org/dotdeb.gpg<br>
>apt-key add dotdeb.gpg<br>
>apt-get update<br>
>apt-get install redis-server<br>
>vim /etc/redis/redis.conf<br>

    databases 16
    requirepass Passw0rd
    appendonly yes

>service redis-server restart<br>
>sysctl vm.overcommit_memory=1<br>
>echo 'vm.overcommit_memory = 1' | tee -a /etc/sysctl.conf<br>
>redis-server<br>
>redis-cli<br>
>auth Passw0rd<br>
>select 3
>ping<br>
>ping test<br>
>set 'a' 1<br>
>get 'a'<br>
>exit<br>

>apt-get install curl<br>
>curl -sL https://deb.nodesource.com/setup_10.x -o nodesource_setup.sh<br>
>bash nodesource_setup.sh<br>
>apt-get install nodejs<br>
>rm nodesource_setup.sh<br>
>node -v<br>

>cd /var/www/laravel<br>
>composer require predis/predis<br>
>npm install express socket.io ioredis --save<br>
>npm install socket.io-client --save<br>
[edit package.json](https://github.com/mingburnu/broadcast/blob/master/package.json)<br>

    ...............  
    "scripts": {
        "dev": "cross-env NODE_ENV=development node_modules/webpack/bin/webpack.js --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js",
        "watch": "cross-env NODE_ENV=development node_modules/webpack/bin/webpack.js --watch --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js",
        "hot": "cross-env NODE_ENV=development node_modules/webpack-dev-server/bin/webpack-dev-server.js --inline --hot --config=node_modules/laravel-mix/setup/webpack.config.js",
        "production": "cross-env NODE_ENV=production node_modules/webpack/bin/webpack.js --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js"
    ..............
    
    "devDependencies": {
        .............
        "webpack": "2.2.1",
        "laravel-mix": "^0.8.1",
        .............
      }

>npm install<br>

[edit .env](https://github.com/mingburnu/broadcast/blob/master/.env)<br>

    BROADCAST_DRIVER=redis
    QUEUE_DRIVER=redis
    REDIS_PASSWORD=Passw0rd

[edit config/database.php](https://github.com/mingburnu/broadcast/blob/master/config/database.php)

            'default' => [
                'host' => env('REDIS_HOST', '127.0.0.1'),
                'password' => env('REDIS_PASSWORD','Passw0rd'),
                'port' => env('REDIS_PORT', 6379),
                'database' => 0, // 0-15
            ],

>php artisan queue:listen<br>

[edit  socket.js](https://github.com/mingburnu/broadcast/blob/master/socket.js)<br>
>node socket.js<br>
>php artisan make:event PushNotification<br>

[edit app/Events/PushNotification.php](https://github.com/mingburnu/broadcast/blob/master/app/Events/PushNotification.php)<br>

>php artisan tinker<br>
>event(new App\Events\PushNotification('Hello', 'World!'))<br>
>exit<br>

[edit resources/assets/js/broadcast.js](https://github.com/mingburnu/broadcast/blob/master/resources/assets/js/broadcast.js)<br>

    let io = require('socket.io-client');
    let notification = io.connect('http://192.168.1.133:3001');
    notification.on('notification', function(message) {
        console.log(message);
        $('div').html(message);
    });

[edit resources/assets/js/app.js](https://github.com/mingburnu/broadcast/blob/master/resources/assets/js/app.js)<br>

    require('./broadcast');

>npm run dev<br>

[edit app/Console/Kernel.php](https://github.com/mingburnu/broadcast/blob/master/app/Console/Kernel.php)

    $schedule->call(
                function () {
                    event(new PushNotification('time', Carbon::now()->format('h:i:s')));
                }
            )->cron('* * * * *');

>vim /etc/crontab<br>

    * * * * * root php /var/www/laravel/artisan schedule:run >> /dev/null 2>&1

[edit resources/views/broadcast.blade.php](https://github.com/mingburnu/broadcast/blob/master/resources/views/broadcast.blade.php)<br>

    <!DOCTYPE html>
    <html lang="en">
        <head>
            <meta name="csrf-token" content="{{ csrf_token() }}">
            <script src="{{asset('js/app.js')}}"></script>
        </head>
        <body>
            <div></div>
        </body>
    </html>

[edit routes/web.php](https://github.com/mingburnu/broadcast/blob/master/routes/web.php)

    Route::get('/broadcast', function () {
        return view('broadcast');
    });

[http://APP_URL/broadcast](http://APP_URL/broadcast)

### Refernce
[Broadcasting](https://laravel.com/docs/5.4/broadcasting)<br>
[How To Install Node.js on Debian 8](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-debian-8)<br>
[How to Install a Redis Server on Ubuntu or Debian 8](https://www.linode.com/docs/databases/redis/how-to-install-a-redis-server-on-ubuntu-or-debian8/)<br>
[Laravel 5.4 推播功能](https://hackmd.io/s/SknZva8jb)<br>
[在 laravel 5 實作瀏覽器推播通知](https://jigsawye.com/2015/12/22/push-notification-to-user-in-laravel-5/)<br>
[Laravel 5.4 Mix执行 npm run dev时报错](https://juejin.im/post/5a7b9e686fb9a0635a654f83)
