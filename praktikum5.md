# Dynamic Route dan Middleware

Untuk praktikum ini, kita dapat menggunakan aplikasi dari modul sebelumnya. <br>

## Dynamic Route

Dynamic route adalah route yang dapat berubah-ubah, contohnya pada saat kita membuka suatu halaman web, kadang kita melihat `/users/1` atau `/users/2`, hal ini yang dinamakan dynamic routes.
Untuk menambahkan dynamic routes pada aplikasi lumen kita, kita dapat menggunakan syntax berikut,

  ```
  $router->get('/user/{id}', function ($id) {
      return 'User Id = ' . $id;
  });
  ```

  ![Hasil Screenshot](/Gambar_Praktikum5/1.png)<br>
  ![Hasil Screenshot](/Gambar_Praktikum5/2.png)<br>

Saat menambahkan parameter pada routes, kita tidak terbatas pada 1 variable saja, namun kita dapat menambahkan sebanyak yang diperlukan seperti kode berikut,

  ```
  $router->get('/post/{postId}/comments/{commentId}', function ($postId, $commentId) {
      return 'Post ID = ' . $postId . ' Comments ID = ' . $commentId;
  });
  ```

![Hasil Screenshot](/Gambar_Praktikum5/3.png)<br>
![Hasil Screenshot](/Gambar_Praktikum5/4.png)<br>

Pada dynamic routes kita juga bisa menambahkan optional routes, yang mana optional routes tidak mengharuskan kita untuk memberi variable pada endpoint kita, namun saat kita memanggil endpoint, dapat menggunakan parameter variable ataupun tidak, seperti pada kode dibawah ini,

  ```
  $router->get('/users[/{userId}]', function ($userId = null) {
      return $userId === null ? 'Data semua users' : 'Data user dengan id ' . $userId;
  });
  ```

![Hasil Screenshot](/Gambar_Praktikum5/5.png)<br>
Ketika mengakses endpoint /users


![Hasil Screenshot](/Gambar_Praktikum5/6.png)<br>
Ketika mengakses endpoint /users/1


![Hasil Screenshot](/Gambar_Praktikum5/7.png)<br>

## Aliases Route

Aliases Route digunakan untuk memberi nama pada route yang telah kita buat, hal ini dapat membantu kita, saat kita ingin memanggil route tersebut pada aplikasi kita. Berikut syntax untuk menambahkan aliases route

  ```
  $router->get('/auth/login', ['as' => 'route.auth login', function (...) {...}])
  ...
  $router->get('/profile', function (Request $request) {
    if ($request->isLoggedIn) {
      return redirect()->route('route.auth.login');
    }
  });
  ```

![Hasil Screenshot](/Gambar_Praktikum5/8.png) <br>
Jika kita masuk ke endpoint /profile, akan redirect ke endpoint /auth/login <br>
![Hasil Screenshot](/Gambar_Praktikum5/9.png) <br>
![Hasil Screenshot](/Gambar_Praktikum5/10.png)<br>

## Group Route

Pada lumen, kita juga dapat memberikan grouping pada routes kita agar lebih mudah pada saat penulisan route pada web.php. Kita dapat melakukan grouping dengan menggunakan syntax berikut,

  ```
  $router->group(['prefix' => 'users'], function () use ($router) {
    $router->get('/', function () { 
      // menjadi /users/, /users => prefix, / => path
      return "GET /users";
    });
  });
  ```

![Hasil Screenshot](/Gambar_Praktikum5/11.png)<br>
![Hasil Screenshot](/Gambar_Praktikum5/12.png)<br>

Selain dapat mengelompokkan prefix, kita juga dapat mengelompokkan middleware dan
namespace pada kelompok routes kita.

## Middleware

Middleware adalah penengah antara komunikasi aplikasi dan client. Middleware biasanya digunakan untuk membatasi siapa yang dapat berinteraksi dengan aplikasi kita dan semacamnya, kita dapat menambahkan middleware dengan menambahkan file pada folder `app/Http/Middleware`. Pada folder tersebut terdapat file `ExampleMiddleware`, kita dapat mencopy file tersebut untuk membuat middleware baru. Pada praktikum kali ini akan dibuat middleware Age dengan isi,

  ```
  class AgeMiddleware{
      /**
      * Handle an incoming request.
      *
      * @param \Illuminate\Http\Request $request
      * @param \Closure $next
      * @return mixed
      */
      public function handle($request, Closure $next){
          if ($request->age < 17)
              return redirect('/fail');
          return $next($request);
      }
  }
  ```

![Hasil Screenshot](/Gambar_Praktikum5/13.png)<br>

Setelah menambahkan filter pada AgeMiddleware , kita harus mendaftarkan
AgeMiddleware pada aplikasi kita, pada file `bootstrap/app.php` seperti berikut ini,

```
  ...

  // $app->middleware([
  //      App\Http\Middleware\ExampleMiddleware::class
  // ]);
  
  // $app->routeMiddleware([
  //    'auth' => App\Http\Middleware\Authenticate::class,
  //    'age' => App\Http\Middleware\AgeMiddleware::class
  // ]);

  ...
  ```

Pada baris 65 terdapat comment mengenai proses mendaftarkan suatu middleware dalam aplikasi kita. Untuk menambahkan middleware pada aplikasi kita, kita dapat men-uncomment baris 75 hingga 77, kemudian menambahkan age middleware ke dalamnya.Namun, karena kita hanya ingin menambahkan middleware pada route tertentu, kita akan menghapus comment pada baris 79 hingga 81, kemudian menambahkan middleware age di
dalamnya.

![Hasil Screenshot](/Gambar_Praktikum5/14.png) <br>

Kita dapat menambahkan middleware pada routes kita dengan menambahkan opsi middleware pada salah satu route, contohnya,

  ```
  $router->get('/admin/home/', ['middleware' => 'age', function () {
      return 'Dewasa';
  }]);

  $router->get('/fail', function () {
      return 'Dibawah umur';
  });
  ```

![Hasil Screenshot](/Gambar_Praktikum5/15.png)<br>
Ketika 'age' kurang dari 17 tahun<br>
![Hasil Screenshot](/Gambar_Praktikum5/17.png)<br>
Ketika 'age' 17 tahun ke atas<br>
![Hasil Screenshot](/Gambar_Praktikum5/16.png)<br>