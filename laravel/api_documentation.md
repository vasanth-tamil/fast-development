# API Documentations
API documentation create laravel
### 1. install l5-swagger
```bash
composer require zircote/swagger-php
composer require darkaonline/l5-swagger
```
### 2. publish swagger configuration
```bash
php artisan vendor:publish --provider  "L5Swagger\L5SwaggerServiceProvider"
```
### 3. configuration file on `config/l5-swagger.php`
```bash
L5_SWAGGER_CONST_HOST=http://localhost
```
### 6.important controller information
```php
<?php

namespace App\Http\Controllers;

use Illuminate\Support\Facades\Schema;
use Illuminate\Support\Facades\Validator;

/**
 * @OA\Info(
 *     title="Project Mangement API",
 *     description="Simple project management api for commercial purpose",
 *     version="cosdb beta v1.0.0"
 * )
 * @OA\SecurityScheme(
 *     type="http",
 *     securityScheme="bearerAuth",
 *     scheme="bearer"
 * )
 */

abstract class Controller{
    // common controller code here.
}
```
### 5. add must one endpoint documentation
## GET REQUEST
```php
<?php

namespace App\Http\Controllers\Api\v1;

use App\Http\Controllers\Controller;

class AuthController extends Controller {
    /**
     * @OA\Get(
     *     path="/api/v1/post-request",
     *     summary="test GET request",
     *     tags={"Request"},
     *     @OA\Response(
     *         response=200,
     *         description="sign in successfully",    
     *     ),
     * )
     */
    public function get_request(Request $request) {}
    
    /**
     * @OA\Get(
     *     path="/api/v1/post-request",
     *     summary="test POST request",
     *     tags={"Request"},
     *     @OA\Response(
     *         response=200,
     *         description="sign in successfully",    
     *     ),
     * )
     */
    public function post_request(Request $request) {}
    
    /**
     * @OA\Put(
     *     path="/api/v1/put-request",
     *     summary="test PUT request",
     *     tags={"Request"},
     *     @OA\Response(
     *         response=200,
     *         description="sign in successfully",    
     *     ),
     * )
     */
    public function put_request(Request $request) {}
    
    /**
     * @OA\Delete(
     *     path="/api/v1/delete-request",
     *     summary="test DELETE request",
     *     tags={"Request"},
     *     @OA\Response(
     *         response=200,
     *         description="sign in successfully",    
     *     ),
     * )
     */
    public function delete_request(Request $request) {}
}
```
### 5. generate api documentation file
```bash
php artisan l5-swagger:generate
```