# Register, Authentication, dan Authorization

* ## Register
  <tb>1. Pastikan terdapat tabel ```users``` yang dibuat menggunakan migration pada bab 3 ___Basic Routing dan Migration___. Beikut informasi kolom yang harus ada <br>
  id |
  --- |
  createdAt |
  updatedAt |
  name |
  email |
  password |

![Screenshot register](/Gambar_Praktikum8/1.png) <br>

  <tb>2. Pastikan terdapat model ```User.php``` yang digunakan pada bab 5 ___Model, Controller, dan Request-Response Handler___. Berikut baris kode yang harus ada <br>
  ```
    <?php

    namespace App\Models;
    use Illuminate\Database\Eloquent\Model;
    class User extends Model

    {
        /**
        * The attributes that are mass assignable.
        *
        * @var array
        */

        protected $fillable = [
            'name', 'email', 'password'
        ];

        /**
        * The attributes excluded from the model's JSON form.
        *
        * @var array
        */

        protected $hidden = [];
    }
  ```

  ![Screenshot register](/Gambar_Praktikum8/2.PNG) <br>

  
  <tb>3. Buatlah file ```AuthController.php``` dan isilah dengan baris kode berikut <br>
  ```
    <?php
      namespace App\Http\Controllers;
  
      use App\Models\User;
      use Illuminate\Http\Request;
      use Illuminate\Support\Facades\Hash;
  
      class AuthController extends Controller
      {
        /**
        * Create a new controller instance.
        *
        * @return void
        */
        public function __construct()
        {
          //
        }
  
        //
        public function register(Request $request)
        {
          $name = $request->name;
          $email = $request->email;
          $password = Hash::make($request->password);
  
          $user = User::create([
            'name' => $name,
            'email' => $email,
            'password' => $password
          ]);
  
          return response()->json([
            'status' => 'Success',
            'message' => 'new user created',
            'data' => [
              'user' => $user,
            ]
          ],200);
        }
      }
  ```

  ![Screenshot register](/Gambar_Praktikum8/3.PNG) <br>

  <tb>4. Tambahkan baris berikut pada ```routes/web.php``` <br>
  ```
    <?php
    ...
    $router->group(['prefix' => 'auth'], function () use ($router) {
      $router->post('/register', ['uses'=> 'AuthController@register']);
    });
  ```
  ![Screenshot register](/Gambar_Praktikum8/4.PNG) <br>
  <tb>5. Jalankan aplikasi pada endpoint ```/auth/register``` dengan body berikut <br>
  ```
    {
      "name": "Scaramouche",
      "email": "scaramouche@fatui.org",
      "password": "wanderer"
    }
  ```

  ![Screenshot register](/Gambar_Praktikum8/5.PNG) <br>

  **Hasil pada PHPMyAdmin**<br>
  
  ![Screenshot register](/Gambar_Praktikum8/6.PNG) <br>
  
* ## Authentication
  <tb>1. Buatlah fungsi ```login(Request $request)``` pada file ```AuthController.php``` <br>
  ```
    <?php
      namespace App\Http\Controllers;
  
      use App\Models\User;
      use Illuminate\Http\Request;
      use Illuminate\Support\Facades\Hash;
  
      class AuthController extends Controller
      {
        ...
        public function login(Request $request)
        {
            $email = $request->email;
            $password = $request->password;

            $user = User::where('email', $email)->first();

            if (!$user) {
                return response()->json([
                    'status' => 'Error',
                    'message' => 'user not exist',
                ],404);
            }

            if (!Hash::check($password, $user->password)) {
                return response()->json([
                    'status' => 'Error',
                    'message' => 'wrong password',
                ],400);
            }
  
            return response()->json([
                'status' => 'Success',
                'message' => 'successfully login',
                'data' => [
                    'user' => $user,
                ]
            ],200);
        }
    }
  ```

  ![Screenshot authentication](/Gambar_Praktikum8/7.PNG) <br>

  
  <tb>2. Tambahkan baris berikut pada ```routes/web.php``` <br>
  ```
    <?php
    ...
    $router->group(['prefix' => 'auth'], function () use ($router) {
      $router->post('/register', ['uses'=> 'AuthController@register']);
      $router->post('/login', ['uses'=> 'AuthController@login']); // route login
    });
  ```

  ![Screenshot authentication](/Gambar_Praktikum8/8.PNG) <br>

  
  <tb>3. Jalankan aplikasi pada endpoint ```/auth/login``` dengan body berikut <br>
  ```
    {
      "email": "scaramouche@fatui.org",
      "password": "wanderer"
    }
  ```

  ![Screenshot authentication](/Gambar_Praktikum8/9.PNG) <br>

  
  > [!NOTE]
  > Opsional: lakukan percobaan dengan menyalahkan email dan password dan amati responnya

  **Hasil ketika password salah**<br>

  ![Screenshot authentication](/Gambar_Praktikum8/10.PNG) <br>

  **Hasil ketika email salah**<br>

  ![Screenshot authentication](/Gambar_Praktikum8/11.PNG) <br>

  
* ## Token
  <tb>1. Jalankan perintah berikut untuk membuat migrasi baru <br>
  ```
    php artisan make:migration add_column_token_to_users
  ```

  ![Screenshot token](/Gambar_Praktikum8/12.PNG) <br>

  
  <tb>2. Tambahkan baris berikut pada migration yang baru terbuat <br>
  ```
    <?php

      use Illuminate\Database\Migrations\Migration;
      use Illuminate\Database\Schema\Blueprint;
      use Illuminate\Support\Facades\Schema;
      
      class AddColumnTokenToUsers extends Migration
      {
          /**
           * Run the migrations.
           *
           * @return void
           */
          public function up()
          {
              Schema::table('users', function (Blueprint $table) {
                  $table->string('token', 72)->unique()->nullable(); //
              });
          }
      
          /**
           * Reverse the migrations.
           *
           * @return void
           */
          public function down()
          {
              Schema::table('users', function (Blueprint $table) {
                  $table->dropIfExists('token'); //
              });
          }
      }

  ```

  ![Screenshot token](/Gambar_Praktikum8/13.PNG) <br>

  
  <tb>3. Tambahkan atribut token di ```$fillable``` pada ```User.php``` <br>
  ```
    <?php

    namespace App\Models;
    use Illuminate\Database\Eloquent\Model;
    class User extends Model

    {
        /**
        * The attributes that are mass assignable.
        *
        * @var array
        */

        protected $fillable = [
            'name', 'email', 'password',
            'token' //
        ];

        /**
        * The attributes excluded from the model's JSON form.
        *
        * @var array
        */

        protected $hidden = [];
    }
  ```

  ![Screenshot token](/Gambar_Praktikum8/14.PNG) <br>

  
  <tb>4. Tambahkan baris berikut pada file ```AuthController.php``` <br>
  ```
    <?php

    namespace App\Http\Controllers;
    use App\Models\User;
    use Illuminate\Http\Request;
    use Illuminate\Support\Facades\Hash;
    use Illuminate\Support\Str;

    class AuthController extends Controller
    {
        /**
        * Create a new controller instance.
        *
        * @return void
        */
        public function __construct()
        {
        //
        }

        public function register(Request $request)
        {
            $name = $request->name;
            $email = $request->email;
            $password = Hash::make($request->password);

            $user = User::create([
                'name' => $name,
                'email' => $email,
                'password' => $password
            ]);
            
            return response()->json([
                'status' => 'Success',
                'message' => 'new user created',
                'data' => [
                    'user' => $user,
                ]
            ],200);
        }

        public function login(Request $request)
        {
            $email = $request->email;
            $password = $request->password;

            $user = User::where('email', $email)->first();

            if (!$user) {
                return response()->json([
                    'status' => 'Error',
                    'message' => 'user not exist',
                ],404);
            }

            if (!Hash::check($password, $user->password)) {
                return response()->json([
                    'status' => 'Error',
                    'message' => 'wrong password',
                ],400);
            }

            $user->token = Str::random(36); //
            $user->save(); //

            return response()->json([
                'status' => 'Success',
                'message' => 'successfully login',
                'data' => [
                    'user' => $user,
                ]
            ],200);
        }
    }
  ```

  ![Screenshot token](/Gambar_Praktikum8/15.PNG) <br>

  
  <tb>5. Jalankan perintah di bawah untuk menjalankan migrasi terbaru <br>
  ```
    php artisan migrate
  ```

  ![Screenshot token](/Gambar_Praktikum8/16.PNG) <br>

  
  <tb>6. Jalankan aplikasi pada endpoint ```/auth/login``` dengan body berikut. Salinlah token yang didapat ke notepad<br>
  ```
    {
      "email": "scaramouche@fatui.org",
      "password": "wanderer"
    }
  ```

  ![Screenshot token](/Gambar_Praktikum8/17.PNG) <br>

  
* ## Authorization
  <tb>1. Buatlah file ```Authorization.php``` pada folder ```App/Http/Middleware``` dan isilah dengan baris berikut<br>
  ```
    <?php
  
      namespace App\Http\Middleware;
      
      use App\Models\User;
      use Closure;
      
      class Authorization
      {
          /**
          * Handle an incoming request.
          *
          * @param \Illuminate\Http\Request $request
          * @param \Closure $next
          * @return mixed
          */
          public function handle($request, Closure $next)
          {
              $token = $request->header('token') ?? $request->query('token');
              if (!$token) {
                  return response()->json([
                      'status' => 'Error',
                      'message' => 'token not provided',
                  ],400);
              }
      
              $user = User::where('token', $token)->first();
              if (!$user) {
                  return response()->json([
                      'status' => 'Error',
                      'message' => 'invalid token',
                  ],400);
              }
              $request->user = $user;
              return $next($request);
          }
      }
  ```

  ![Screenshot authorization](/Gambar_Praktikum8/18.PNG) <br>

  
  <tb>2. Tambahkan middleware yang baru dibuat pada ```bootstrap/app.php```.<br>
  ```
    /*
    |--------------------------------------------------------------------------
    | Register Middleware
    |--------------------------------------------------------------------------
    |
    | Next, we will register the middleware with the application. These can
    | be global middleware that run before and after each request into a
    | route or middleware that'll be assigned to some specific routes.
    |
    */
  
    // $app->middleware([
    // App\Http\Middleware\ExampleMiddleware::class
    // ]);
  
    $app->routeMiddleware([
      'auth' => App\Http\Middleware\Authorization::class, //
    ]);
  ```

  ![Screenshot authorization](/Gambar_Praktikum8/19.PNG) <br>

  
  <tb>3. Buatlah fungsi ```home()``` pada ```HomeController.php```<br>
  ```
    <?php
    namespace App\Http\Controllers;
  
    use App\Models\User; // import model User
    use Illuminate\Http\Request;
    use Illuminate\Http\Response;
  
    class HomeController extends Controller
    {
    ...
      public function home(Request $request)
      {
        $user = $request->user;
    
        return response()->json([
          'status' => 'Success',
          'message' => 'selamat datang ' . $user->name,
        ],200);
      }
    }
  ```

  ![Screenshot authorization](/Gambar_Praktikum8/20.PNG) <br>

  
  <tb>4. Tambahkan baris berikut pada ```routes/web.php```<br>
  ```
    <?php

    $router->get('/', ['uses' => 'HomeController@index']);
    $router->get('/hello', ['uses' => 'HomeController@hello']);
    $router->get('/home', ['middleware' => 'auth','uses' => 'HomeController@home']); //
    ...
  ```

  ![Screenshot authorization](/Gambar_Praktikum8/21.PNG) <br>

  
  <tb>5. Jalankan aplikasi pada endpoint ```/home``` dengan melampirkan nilai token yang didapat setelah login pada header<br>
  
  ![Screenshot authorization](/Gambar_Praktikum8/22.PNG) <br>