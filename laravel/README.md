## Laravel

Add forignkey

```php
$table->bigInteger("category_id")->unsigned()->index()->nullable(False);
$table->foreign("category_id")->references("id")->on("categories")->onDelete('cascade');Drop foreignkey, index and column
```

```php
$table->dropForeign('lists_user_id_foreign');
$table->dropIndex('lists_user_id_index');
$table->dropColumn('user_id');
```

**debug bar**

```textile
composer require barryvdh/laravel-debugbar --dev
```

edit this file `config/app.php`

```php
'Debugbar' => Barryvdh\Debugbar\Facades\Debugbar::class,
```

**publish**

```textile
php artisan vendor:publish --provider="Barryvdh\Debugbar\ServiceProvider"
```

**with errors**

```php
return Redirect::back()->withErrors(['msg' => 'The Message']);
```

```php
@if($errors->any())
<h4>{{$errors->first()}}</h4>
@endif
```

```php
@if ($errors->has('username'))                    
    <div class="invalid-feedback">
        {{ $errors->first('username') }}
     </div>
@endif
```

```php
@if($errors->any())
    <div class="alert alert-danger">
        @foreach ($errors->all() as $error)
            <li>{{ $error }}</li>
        @endforeach 
    </div>
@endif
```

**validator**

```php
use Validator;

$validator = Validator::validate($request->all(), [
    "name": "required|string|max:20",
    "description": "required|max:255",
    "email": "required|unique:users,name"
]);
```

**auth middleware**

```php
class administrator
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
    public function handle($request, Closure $next)
    {
        if ( Auth::check() && Auth::user()->isAdmin() )
        {
            return $next($request);
        }

        return redirect('signin');
    }
}
```

```php
Route::get('/', 'Dashboard@dashboard')->middleware(['administrator']);
```

```php
public function isAdmin()
{
    return $this->admin;
}
```

**middleware**

```php
//register new user
Route::post('/create-account', [AuthenticationController::class, 'createAccount']);
//login user
Route::post('/signin', [AuthenticationController::class, 'signin']);
//using middleware
Route::group(['middleware' => ['auth:sanctum']], function () {
    Route::get('/profile', function(Request $request) {
        return auth()->user();
    });
    Route::post('/sign-out', [AuthenticationController::class, 'logout']);
});
```

**auth.php**

```php
return [
    'defaults' => [
        'guard' => 'web',
        'passwords' => 'users',
    ],
    'guards' => [
        'web' => [
            'driver' => 'session',
            'provider' => 'users',
        ],
        'api' => [
            'driver' => 'token',
            'provider' => 'users',
        ],
        'admin' => [
            'driver' => 'session',
            'provider' => 'admins',
        ],
        'admin-api' => [
            'driver' => 'token',
            'provider' => 'admins',
        ],
    ],
    'providers' => [
        'users' => [
            'driver' => 'eloquent',
            'model' => App\User::class,
        ],
        'admins' => [
            'driver' => 'eloquent',
            'model' => App\Admin::class,
        ],
    ],
    'passwords' => [
        'users' => [
            'provider' => 'users',
            'table' => 'password_resets',
            'expire' => 60,
        ],
        'admins' => [
            'provider' => 'admins',
            'table' => 'password_resets',
            'expire' => 15,
        ],
    ],
];
```

```php
if(auth()->guard('admin')->attempt($request->only('email','password')))
{
    return auth()->guard('admin')->user();
}
```

**middleware**

```php
Route::middleware(['auth:admin'])->get('/user', function(){
    return auth()->guard('admin')->user();
}
```

**create token**

```php
$user->createToken('token')->plainTextToken;
```

**token check**

```php
auth()->user()->tokenCan('key')
```

https://stackoverflow.com/questions/61170647/laravel-sanctum-can-be-use-multiauth-guard
**push template**

```php
@stack("script_1")
```

```php
@push("script_1")
    // code here
@endpush()
```

**password confirm**

```php
$request->validate([
    'password' => 'required|min:6|confirmed',
]);
```

parse variables like this

```text
password
password_confirmation
```

https://laravel.com/docs/4.2/validation#rule-confirmed

laravel excel import and export

https://www.positronx.io/laravel-import-expert-excel-and-csv-file-tutorial-with-example/

**library**

```
composer require maatwebsite/excel
```

**laravel speed optimization**

```
https://www.cloudways.com/blog/laravel-performance-optimization/
```