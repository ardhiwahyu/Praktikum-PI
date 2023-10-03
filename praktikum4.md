# Basic Routing dan Migration

* ## GET
  Tambahkan baris berikut untuk membuat endpoint method GET pada file **web.php** pada folder routes. <br><br>
  ```
    ...
    $router->get('/get', function () {
      return 'GET';
    });
  ```
  ![Screenshot method GET](/Gambar_Praktikum4/1.png) <br>
  Jalankan aplikasi dengan command di bawah pada terminal<br><br>
  ```
    php -S localhost:8000 -t public
  ```


  Setelah aplikasi berhasil dijalankan, buka browser dengan url ```http://localhost:8000/get``` dan browser akan menghasilkan tampilan seperti berikut<br><br>
  ![Screenshot method GET](/Gambar_Praktikum4/2.png) <br>

  ![Screenshot method GET](/Gambar_Praktikum4/3.png) <br>
  
  ![Screenshot method GET](/Gambar_Praktikum4/4.png) <br>
  
* ## POST, PUT, PATCH, DELETE, dan OPTIONS
  Tambahan method POST, PUT, PATCH, DELETE, dan OPTIONS pada file **web.php** dengan kode berikut<br><br>
  ```
    ...
    $router->post('/post', function () {
      return 'POST';
    });
  
    $router->put('/put', function () {
      return 'PUT';
    });
  
    $router->patch('/patch', function () {
      return 'PATCH';
    });
  
    $router->delete('/delete', function () {
      return 'DELETE';
    });
  
    $router->options('/options', function () {
    return 'OPTIONS';
    });
  ```
  ![Screenshot other method](/Gambar_Praktikum4/5.png) <br>
  
  Jalankan kembali server seperti pada saat percobaan GET. Pada percobaan ini, request ke server akan dilakukan menggunakan extensions pada VSCode yaitu **_Thunder Client_** <br>
  a. Buka Aplikasi **Postman** <br><br>
  ![Screenshot Postman](/Gambar_Praktikum4/6.png) <br><br>
  b. Buat **New Request** pada ekstensi <br><br>
  ![Screenshot Postman](/Gambar_Praktikum4/7.png) <br><br>
  c. Masukkan method dan url yang dituju <br><br>
  d. Akses url yang baru ditambahkan pada aplikasi dengan methodnya <br>

  ### Method Get
  ![Screenshot method post](/Gambar_Praktikum4/8.png) <br><br>
  ### Method post
  ![Screenshot method post](/Gambar_Praktikum4/9.png) <br><br>
  ### Method put
  ![Screenshot method put](/Gambar_Praktikum4/10.png) <br><br>
  ### Method patch
  ![Screenshot method patch](/Gambar_Praktikum4/11.png) <br><br>
  ### Method delete
  ![Screenshot method delete](/Gambar_Praktikum4/12.png) <br><br>
  ### Method options
  ![Screenshot method options](/Gambar_Praktikum4/13.png) <br><br>
  
* ## Migrasi Database
  a. Pastikan server database aktif dan buat database dengan nama ```lumenapi``` sebelum melakukan migrasi database <br><br>
  ![Screenshot migrasi database](/Gambar_Praktikum4/14.png) <br><br>

  ![Screenshot migrasi database](/Gambar_Praktikum4/15.png) <br><br>
  b. Ubah konfigurasi database pada file **.env** menjadi seperti ini <br><br>
  ```
    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=lumenapi
    DB_USERNAME=root
    DB_PASSWORD=<<password masing-masing>>
  ```
  ![Screenshot migrasi database](/Gambar_Praktikum4/16.png) <br><br> <br><br>
  c. Hidupkan library bawaan dari lumen dengan membuka file **app.php** pada folder bootstrap dengan menghilangkan tanda miring (//). Sehingga kode menjadi seperti ini <br><br>
  ```
    $app->withFacades();
    $app->withEloquent();
  ```
  ![Screenshot migrasi database](/Gambar_Praktikum4/17.png) <br><br> <br><br>
  d. Buat file migration dengan menjalankan command <br><br>
  ```
    php artisan make:migration create_users_table # membuat migrasi untuk tabel users
    php artisan make:migration create_products_table # membuat migrasi untuk tabel products
  ```
  ![Screenshot migrasi database](/Gambar_Praktikum4/18.png) <br><br> <br><br>
  ![Screenshot migrasi database](/Gambar_Praktikum4/19.png) <br><br> <br><br>
  
  Setelah menjalankan 2 syntax diatas akan terbuat 2 file pada folder
    database/migrations dengan format ```YYYY_MM_DD_HHmmss_nama_migrasi```. Pada
    file migrasi kita akan menemukan fungsi up() dan fungsi down(), fungsi up() akan
    digunakan pada saat kita melakukan migrasi, fungsi down() akan digunakan saat
    kita ingin me-rollback migrasi
  
  e. Ubah fungsi up pada file migrasi ```create_users_table```<br><br>
  ```
    ...
    public function up()
    {
      Schema::create('users', function (Blueprint $table) {
        $table->id();
        $table->timestamps();
        $table->string('name');
        $table->string('email');
        $table->string('password');
      });
    }
    ...
  ```
  ![Screenshot migrasi database](/Gambar_Praktikum4/20.png) <br><br> <br><br>
  f. Ubah fungsi up pada file migrasi ```create_products_table```<br><br>
  ```
    ...
    public function up()
    {
      Schema::create('products', function (Blueprint $table) {
        $table->id();
        $table->timestamps();
        $table->string('name');
        $table->integer('category_id');
        $table->string('slug');
        $table->integer('price');
        $table->integer('weight');
        $table->text('description');
      });
    }
    ...
  ```
  ![Screenshot migrasi database](/Gambar_Praktikum4/21.png) <br><br> <br><br>
  g. Jalankan command <br><br>
  ```
    php artisan migrate
  ```
  ![Screenshot migrasi database](/Gambar_Praktikum4/22.png) <br><br> <br><br>
  h. Cek database **lumenapi** pada database masing-masing. Disini terlihat terdapat 2 tabel, yaitu tabel users dan tabel products <br><br>
  ![Screenshot migrasi database](/Gambar_Praktikum4/23.png) <br><br> <br><br>