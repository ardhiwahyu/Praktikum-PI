# Model, Controller, dan Request-Response Header

* ## Model
  Model merupakan bagian yang bertugas untuk menyiapkan, mengatur, memanipulasi, dan mengorganisasikan data yang ada di database. Model merepresentasikan kolom apa saja yang ada pada database, termasuk relasi dan primary key dapat didefinisikan di dalam model. Dengan menggunakan perintah Artisan, pembuatan model pada Laravel dapat dilakukan dengan satu perintah menggunakan<br>
  ```
    php artisan make:model nama_model
  ```
  Namun karena perintah Artisan yang terbatas pada Lumen, pembuatan model harus dilakukan secara manual. Langkah-langkah untuk membuat model adalah sebagai berikut:<br><br>
<tb>1. Pastikan terdapat tabel users yang dibuat menggunakan migration pada bab sebelumnya.<br>
![Screenshot model](/Gambar_Praktikum6/1.png) <br>
<tb>2. Bersihkan isi ```User.php``` yang ada sebelumnya dan isi dengan baris kode berikut<br>
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
  ![Screenshot model](/Gambar_Praktikum6/2.png) <br>
  
* ## Controller
  Controller merupakan bagian yang menjadi tempat berkumpulnya logika pemrograman yang digunakan untuk memisahkan organisasi data pada database. Dalam beberapa kasus, controller menjadi penghubung antara model dan view pada arsitektur MVC. Langkah-langkah untuk membuat Controller adalah sebagai berikut:<br><br>
  <tb>1. Buatlah salinan **ExampleController.php** pada folder ```app/Http/Controllers``` dengan nama **HomeController.php** dan buatlah fungsi ```index()``` yang berisi<br>
  ```
    <?php
      namespace App\Http\Controllers;
      class HomeController extends Controller
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
        // Pembuatan fungsi index() //
        public function index()
        {
          return 'Hello, from lumen!';
        }
        // Pembuatan fungsi index() //
        //
      }
  ```
  ![Screenshot controller](/Gambar_Praktikum6/3.png) <br>
  
  <tb>2. Ubah route ```/``` pada file ```routes/web.php``` menjadi seperti ini<br>
  ```
    # Sebelum,
    $router->get('/', function () use ($router) {
      return $router->app->version();
    });
    
    # Setelah,
    $router->get('/', ['uses' => 'HomeController@index']);
  ```
  ![Screenshot controller](/Gambar_Praktikum6/4.png) <br>
  
  <tb>3. Jalankan aplikasi<br>
  ![Screenshot controller](/Gambar_Praktikum6/5.png) <br>
  
* ## Request Handler
  Request handler adalah fungsi yang digunakan untuk berinteraksi dengan request yang datang. Request handler dapat digunakan untuk melihat apa saja yang dikirimkan oleh user seperti parameter, query, dan body. Langkah untuk membuat request handler adalah sebagai berikut:<br>
  <tb>1. Lakukan import library Request dengan menambahkan baris berikut di bagian atas file ```HomeController.php```<br>
  ```  
      // Import Library Request
      use Illuminate\Http\Request;
  ```
  ![Screenshot request handler](/Gambar_Praktikum6/6.png) <br>
  
  <tb>2. Ubah fungsi index menjadi<br>
  ```
        // Pembuatan fungsi index
        public function index (Request $request)
        {
        return 'Hello, from lumen! We got your request from endpoint: ' . $request->path();
        }
        // Pembuatan fungsi index
        //
  ```
  ![Screenshot request handler](/Gambar_Praktikum6/7.png) <br>
  
  <tb>3. Jalankan aplikasi<br>
  ![Screenshot request handler](/Gambar_Praktikum6/8.png) <br>
  
* ## Response Handler
  Response handler adalah fungsi yang digunakan untuk membentuk output yang diharapkan kepada user dan beberapa properti selain data seperti status code dan header. Berikut adalah langkah-langkah untuk membuat response handler:<br>
  <tb>1. Lakukan import library Response dengan menambahkan baris berikut di bagian atas file ```HomeController.php```<br>
  ```
    <?php
      namespace App\Http\Controllers;
      
      use Illuminate\Http\Request;
      use Illuminate\Http\Response; // import library Response
  ```
  ![Screenshot response handler](/Gambar_Praktikum6/9.png) <br>
  
  <tb>2. Buatlah fungsi ```hello()``` yang berisi<br>
  ```
    <?php
      namespace App\Http\Controllers;
      
      use Illuminate\Http\Request;
      use Illuminate\Http\Response;
  
      class HomeController extends Controller
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
  
        public function index (Request $request)
        {
        return 'Hello, from lumen! We got your request from endpoint: ' . $request->path();
        }
        
        // Pembuatan fungsi hello
        public function hello()
        {
        $data['status'] = 'Success';
        $data['message'] = 'Hello, from lumen!';
        return (new Response($data, 201))
            ->header('Content-Type', 'application/json');
        }
      }
  ```
  ![Screenshot response handler](/Gambar_Praktikum6/10.png) <br>
  
  <tb>3. Tambahkan route ```/hello``` pada file ```routes/web.php```<br>
  ```
    <?php
      $router->get('/', ['uses' => 'HomeController@index']);
      $router->get('/hello', ['uses' => 'HomeController@hello']); // route hello
  ```
  ![Screenshot response handler](/Gambar_Praktikum6/11.png) <br>
  
  <tb>4. Jalankan aplikasi pada route ```/hello```<br>
  ![Screenshot response handler](/Gambar_Praktikum6/12.png) <br>
  
* ## Penerapan
  <tb>1. JLakukan import model User dengan menambahkan baris berikut di bagian atas file ```HomeController.php```<br>
  ```
    <?php
      namespace App\Http\Controllers;
      
      use App\Models\User; // import model User
      use Illuminate\Http\Request;
      use Illuminate\Http\Response;
  ```
  ![Screenshot penerapan](/Gambar_Praktikum6/13.png) <br>
  
  <tb>2. Tambahkan ketiga fungsi berikut di ```HomeController.php```<br>
  ```
    <?php
      namespace App\Http\Controllers;
      
      use App\Models\User; // import model User
      use Illuminate\Http\Request;
      use Illuminate\Http\Response;
      
      class HomeController extends Controller
      {
        ...
        // Tiga Fungsi
        public function defaultUser()
        {
            $user = User::create([
                'name' => 'Nahida',
                'email' => 'nahida@akademiya.ac.id',
                'password' => 'smol'
              ]);
            return response()->json([
              'status' => 'Success',
              'message' => 'default user created',
              'data' => [
                  'user' => $user,
              ]
          ],200);
      }
      
      public function createUser(Request $request)
        {
            $name = $request->name;
            $email = $request->email;
            $password = $request->password;
        
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
      
      public function getUsers()
        {
            $users = User::all();
        
            return response()->json([
              'status' => 'Success',
              'message' => 'all users grabbed',
              'data' => [
                  'users' => $users,
              ]
          ],200);
        }
      // Tiga Fungsi
      }
  ```
  ![Screenshot penerapan](/Gambar_Praktikum6/14.png) <br>
  ![Screenshot penerapan](/Gambar_Praktikum6/15.png) <br>
  
  <tb>3. Tambahkan ketiga route pada file ```routes/web.php``` menggunakan group route<br>
  ```
    $router->get('/', ['uses' => 'HomeController@index']);
    $router->get('/hello', ['uses' => 'HomeController@hello']);
  
    // Tiga Route
    $router->group(['prefix' => 'users'], function () use ($router) {
        $router->post('/default', ['uses' => 'HomeController@defaultUser']);
        $router->post('/new', ['uses' => 'HomeController@createUser']);
        $router->get('/all', ['uses' => 'HomeController@getUsers']);
    });
  ```
  ![Screenshot penerapan](/Gambar_Praktikum6/16.png) <br>
  
  <tb>4. Jalankan aplikasi pada route ```/users/default``` menggunakan Postman<br>
  ![Screenshot penerapan](/Gambar_Praktikum6/17.png) <br>
  
  ***Hasil eksekusi pada phpmyadmin***<br>
  ![Screenshot penerapan](/Gambar_Praktikum6/18.png) <br>
  
  <tb>5. Jalankan aplikasi pada route ```/users/new``` dengan mengisi body sebagai berikut<br>
  ![Screenshot penerapan](/Gambar_Praktikum6/19.png) <br>
  
  ***Hasil eksekusi pada phpmyadmin***<br>
  ![Screenshot penerapan](/Gambar_Praktikum6/20.png) <br>
  
  <tb>6. Jalankan aplikasi pada route ```/users/all```<br>
  ![Screenshot penerapan](/Gambar_Praktikum6/21.png) <br>