> apt-get update<br>
> apt-get install php5-ldap<br>

insert into php.ini

    extension=ldap.so

> composer require adldap2/adldap2-laravel<br>

edit /config/app.php<br>

    'providers' => [
            ................................
            Adldap\Laravel\AdldapServiceProvider::class,
            Adldap\Laravel\AdldapAuthServiceProvider::class,
     ],
     
    'aliases' => [
            ...............................
            'Adldap' => Adldap\Laravel\Facades\Adldap::class,
     ],

> php artisan vendor:publish --tag="adldap"<br>

edit /config/auth.php<br>

    'providers' => [
        'users' => [
            'driver' => 'adldap',
            'model' => App\User::class,
        ],
     ]  

edit /config/adldap.php<br>

    'base_dn' => env('ADLDAP_BASEDN', 'dc=example,dc=com'),
    ..............................
    'admin_username' => env('ADLDAP_ADMIN_USERNAME', env('ADLDAP_CN').','.env('ADLDAP_BASEDN')),
    'admin_password' => env('ADLDAP_ADMIN_PASSWORD', 'Passw0rd'),
    ..............................          

edit /config/adldap_auth.php<br>

    ..............................
    'usernames' => [
        'ldap' => env('ADLDAP_USER_ATTRIBUTE', 'userprincipalname'),    
        'eloquent' => 'username'
     ],
    ..............................
    'sync_attributes' => [
         'username' => 'uid',
         'name' => 'cn',
     ],

edit /.env<br>

    ADLDAP_CONNECTION=default
    ADLDAP_CONTROLLERS=192.168.0.13
    ADLDAP_BASEDN=dc=example,dc=com
    ADLDAP_CN=cn=admin
    ADLDAP_USER_ATTRIBUTE=uid
    ADLDAP_USER_FORMAT=uid=%s
     
    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=homestead
    DB_USERNAME=homestead
    DB_PASSWORD=secret

edit /database/migrations/2014_10_12_000000_create_users_table.php<br>

```php
<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateUsersTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('users', function (Blueprint $table) {
            $table->increments('id');
            $table->string('name');
            $table->string('username',20)->unique(); // was 'email'
            $table->string('password');
            $table->rememberToken();
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('users');
    }
}
```

> rm /migrations/2014_10_12_100000_create_password_resets_table.php<br>
> php artisan migrate<br>

edit /app/User.php<br>

```php
<?php

namespace App;

use Illuminate\Notifications\Notifiable;
use Illuminate\Foundation\Auth\User as Authenticatable;

/**
 * App\User
 *
 * @property-read \Illuminate\Notifications\DatabaseNotificationCollection|\Illuminate\Notifications\DatabaseNotification[] $notifications
 * @mixin \Eloquent
 */
class User extends Authenticatable
{
    use Notifiable;

    /**
     * The attributes that are mass assignable.
     *
     * @var array
     */
    protected $fillable = [
        'name', 'username', 'password',
    ];

    /**
     * The attributes that should be hidden for arrays.
     *
     * @var array
     */
    protected $hidden = [
        'password', 'remember_token',
    ];
}
```

> php artisan make:auth<br>

edit /resources/views/layouts/app.blade.php<br>

    <!-- remove the line !>
    <li><a href="{{ route('register') }}">Register</a></li>

edit /resources/views/welcome.blade.php<br>

    <!-- remove the line !>
    <a href="{{ url('/register') }}">Register</a>

edit /resources/views/auth/login.blade.php<br>

    <!-- replace 'email' with 'username' !>
    <div class="form-group{{ $errors->has('username') ? ' has-error' : '' }}">
        <label for="username" class="col-md-4 control-label">Username</label>
        <div class="col-md-6">
            <input id="username" type="text" class="form-control" name="username" value="{{ old('username') }}" required autofocus>
            @if ($errors->has('username'))
                <span class="help-block">
                    <strong>{{ $errors->first('username') }}</strong>
                </span>
            @endif
        </div>
    </div>

edit /app/Http/Controllers/Auth/LoginController.php<br>

```php
<?php

namespace App\Http\Controllers\Auth;

use Adldap\AdldapInterface;
use Adldap\Laravel\AdldapAuthServiceProvider;
use Adldap\Laravel\AdldapServiceProvider;
use Adldap\Laravel\Traits\UsesAdldap;
use App\User;
use Carbon\Carbon;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Auth;
use Adldap\Laravel\Facades\Adldap;
use App\Http\Controllers\Controller;
use Illuminate\Foundation\Auth\AuthenticatesUsers;
use Illuminate\Support\Facades\Log;

class LoginController extends Controller
{
    /*
    |--------------------------------------------------------------------------
    | Login Controller
    |--------------------------------------------------------------------------
    |
    | This controller handles authenticating users for the application and
    | redirecting them to your home screen. The controller uses a trait
    | to conveniently provide its functionality to your applications.
    |
    */

    use AuthenticatesUsers;

    /**
     * Where to redirect users after login.
     *
     * @var string
     */
    protected $redirectTo = '/home';

    /**
     * Create a new controller instance.
     *
     * @return void
     */
    public function __construct()
    {
        $this->middleware('guest', ['except' => 'logout']);
    }

    public function username()
    {
        return config('adldap_auth.usernames.eloquent');
    }

    protected function validateLogin(Request $request)
    {
        $this->validate($request, [
            $this->username() => 'required|string|regex:/^\w+$/',
            'password' => 'required|string',
        ]);
    }

    protected function attemptLogin(Request $request)
    {
        $credentials = $request->only($this->username(), 'password');
        $username = $credentials[$this->username()];
        $password = $credentials['password'];

        $user_format = env('ADLDAP_USER_FORMAT') . ',' . env('ADLDAP_CN') . ',' . env('ADLDAP_BASEDN');
        $userdn = sprintf($user_format, $username);


        if (Adldap::auth()->attempt($userdn, $password, $bindAsUser = true)) {
            // the user exists in the LDAP server, with the provided password
            $user = User::where($this->username(), $username)->first();

            if (!$user) {
                // the user doesn't exist in the local database
                $user = new User();
                $user->name = $username;
                $user->username = $username;
                $user->password = '';

                $current_time = Carbon::now()->toDateTimeString();
                $user->setCreatedAt($current_time);
                $user->setUpdatedAt($current_time);
            }
            // by logging the user we create the session so there is no need to login again (in the configured time)
            $this->guard()->login($user, true);
            return true;
        }

        // the user doesn't exist in the LDAP server or the password is wrong
        return false;
    }
}
```

[http://127.0.0.1](http://127.0.0.1)<br>

### REFERECE

[PHP操作LDAP訪問AD域進行認證登陸](https://ddnews.me/world/hbqfcayb.html)<br>
[LDAP Authentication & Management for Laravel](https://github.com/Adldap2/Adldap2-Laravel)<br>
[Howto: adminless LDAP authentification in Laravel](https://github.com/jotaelesalinas/laravel-simple-ldap-auth)<br>
