### Laravel API install in laravel 11
default api not install on laravel version 11 so install API. following commands.
### 1. Install api
```bash
php artisan install:api
```
### 2. Authentication Exception Handles
Replace this code on your `bootstrap/app.php`
```php
<?php

use Illuminate\Http\Request;
use Illuminate\Http\Response;
use Illuminate\Foundation\Application;
use Illuminate\Foundation\Configuration\Exceptions;
use Illuminate\Foundation\Configuration\Middleware;
use Illuminate\Auth\Sanctum\AuthenticationException;
use Illuminate\Auth\Sanctum\UnauthorizedHttpException;
use Symfony\Component\HttpKernel\Exception\NotFoundHttpException;
use Symfony\Component\HttpKernel\Exception\MethodNotAllowedHttpException;
use Symfony\Component\HttpKernel\Exception\RouteNotFoundException;
use Illuminate\Support\Str;

return Application::configure(basePath: dirname(__DIR__))
    ->withRouting(
        web: __DIR__.'/../routes/web.php',
        api: __DIR__.'/../routes/api.php',
        commands: __DIR__.'/../routes/console.php',
        health: '/up',
    )
    ->withMiddleware(function (Middleware $middleware) {
        // middlewares
    })
    ->withExceptions(function (Exceptions $exceptions) {
        // Not found exception handle return error
        $exceptions->render(function (NotFoundHttpException $e, Request $request) {
            if ($request->is('api/*')) {
                return response()->json([
                    'code' => 404,
                    'message' => 'Record not found.'
                ], 404);
            }
        });
        
        // Http method exception return json
        $exceptions->render(function (MethodNotAllowedHttpException $e, Request $request) {
            return response()->json([
                'code' => 405,
                'message' => 'Method Not Allowed'
            ], 405);
        });
        
        // authentication exception
        $exceptions->render(function (Throwable $e, Request $request) {
            if($request->is('api/*')) {
                if(Str::contains($e->getMessage(), 'login')) {
                    return response()->json([
                        'code' => 401,
                        'message' => 'Unauthenticated.'
                    ], 401);
                }
            }
        });
    })->create();
```
### 3. Configure Model
open your modal and add this important `HasApiTokens` and extends `Authenticatable`
```php
<?php
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;
use Laravel\Sanctum\HasApiTokens;
use Illuminate\Database\Eloquent\Concerns\HasUuids;

class Admin extends Authenticatable
{
    use HasApiTokens, HasFactory, HasUuids, Notifiable;

    protected $fillable = [
        'avatar',
        'name',
        'email',
        'password',
        'role',
        'status',
    ];

    protected $hidden = [
        'password',
        'remember_token',
    ];

    protected function casts(): array
    {
        return [
            'email_verified_at' => 'datetime',
            'password' => 'hashed',
        ];
    }
}
```

### 4. Auth guard
add new `gurads`, `providers`, `passwords` and values edit file `config/auth.php`
```php
<?php

return [

    'defaults' => [
        'guard' => env('AUTH_GUARD', 'web'),
        'passwords' => env('AUTH_PASSWORD_BROKER', 'users'),
    ],

    'guards' => [
        'web' => [
            'driver' => 'session',
            'provider' => 'users',
        ],
        'admin' => [
            'driver' => 'session',
            'provider' => 'admins',
        ]
    ],

    'providers' => [
        'users' => [
            'driver' => 'eloquent',
            'model' => env('AUTH_MODEL', App\Models\User::class),
        ],
        
        'admins' => [
            'driver' => 'eloquent',
            'model' => env('AUTH_MODEL', App\Models\Admin::class),
        ],
    ],

    'passwords' => [
        'users' => [
            'provider' => 'users',
            'table' => env('AUTH_PASSWORD_RESET_TOKEN_TABLE', 'password_reset_tokens'),
            'expire' => 60,
            'throttle' => 60,
        ],
        'admins' => [
            'provider' => 'admins',
            'table' => env('AUTH_PASSWORD_RESET_TOKEN_TABLE', 'password_reset_tokens'),
            'expire' => 60,
            'throttle' => 60,
        ],
    ],

    'password_timeout' => env('AUTH_PASSWORD_TIMEOUT', 10800),
];
```
### 5. Add Middleware on routes
simply add middleware on protected routes `routes/api.php` guard `auth:` after custom **guard** name
```php
<?php

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;

Route::group(['prefix' => 'v1', 'namespace' => 'App\Http\Controllers\Api\V1'], function () {
    // normal User table
    Route::middleware(['auth:sanctum'])->group(function () {
        Route::controller(AuthController::class)->group(function () {
            Route::get('/protected', 'protected');
        });
    });

    // add custom guard 
    Route::middleware(['auth:admin'])->group(function () {
        Route::controller(AuthController::class)->group(function () {
            Route::get('/protected', 'protected');
        });
    });
});

```