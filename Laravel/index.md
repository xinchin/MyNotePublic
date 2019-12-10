# Laravel 雜記

## OAuth 2.0 Laravel Passport 密碼模式

**Reference**

https://www.bilibili.com/video/av74879198/?p=4

https://www.bilibili.com/video/av74879198/?p=5

https://www.bilibili.com/video/av74879198/?p=6


**建立新專案**

```
composer create-project --prefer-dist laravel/laravel demo_1911181040
```

**設定：.env**

```
DB_DATABASE=demo_1911181040
DB_USERNAME=
DB_PASSWORD=
```

**AppServiceProvider.php**
```php
public function boot()
{
  Schema::defaultStringLength(191);
}
```

**建立 Database**
```
demo_1911181040
```

**migrate**
```
php artisan migrate
```

**make:request**
```
php artisan make:request BaseRequest
```

**BaseRequest.php**
```php
public function wantsJson()
{
  return true;
}

public function expectsJson()
{
  return true;
}
```
**make:controller**

> PassportController.php

```
php artisan make:controller PassportController
```
**install passport**
```
composer require laravel/passport
```

**創建表來存儲客戶端和 access_token...**
```
php artisan migrate
```

**生成加密 access_token 的 key、密碼授權客戶端、個人訪問客戶端...**

```
php artisan passport:install
```

**result**
```
Encryption keys generated successfully.
Personal access client created successfully.
Client ID: 1
Client secret:
s6zKr6MBLQ8ABnYJJJATDTVwGVEenIXcKVA9de0I
Password grant client created successfully.
Client ID: 2
Client secret:
hbLymP6HV8rGUC0fZsHutBftO0FVtNN9LGAVeGgh
```

**提供一些輔助函數檢查已認證用戶的令牌和使用範圍**

> User.php

```
原本
use Notifiable;

新增
use Notifiable, HasApiTokens;
```

**install guzzlehttp （偽造 Http 請求）**

```
composer require guzzlehttp/guzzle
```

**配置 passport 認證**

> config/auth.php

```
defaults' => [

    'guard' => 'api',

    'passwords' => 'users',

],
```
```
'guards' => [

    'web' => [

        'driver' => 'session',

        'provider' => 'users',

    ],

    'api' => [

        'driver' => 'passport',

        'provider' => 'users',

        'hash' => false,

    ],

],
```

**註冊路由**

```
Route::post('/oauth/token', '\Laravel\Passport\Http\Controllers\AccessTokenController@issueToken');
Route::post('/register', 'PassportController@register');
Route::post('/login', 'PassportController@login');
Route::post('/logout', 'PassportController@logout');
Route::post('/refresh', 'PassportController@refresh');
```

**設置一個需驗證的測試用路由**

```
Route::get('test', function(){
  return 'ok';
})->middleware('auth');
````

**使用 scope**
> AppServiceProvider.php

```
Passport::tokensCan([
  'test1'=>'for test',
  'test2'=>'for test2',
]);
```

**註冊中間件**
> Kernel.php
```
'scopes' => \Laravel\Passport\Http\Middleware\CheckScopes::class, // and
'scope' => \Laravel\Passport\Http\Middleware\CheckForAnyScope::class, // or
```

**使用**
```
->middleware('scope:test1');
->middleware('scope:test2');
->middleware('scope:test1,test2');
if(auth()->user()->tokenCan('test1')){
  $scopeIds = Passport::scopes();
}
```

**其它操作**
```
$scopeIds = Passport::scopeIds();
$scopes = Passport::scopes();
$scopesFor = Passport::scopesFor(['test1','check-status','test2']);
$hasScope = Passport::hasScope('showshow');
```

------