# Relasi One-to-Many dan Many-to-Many

* ## Pembuatan Tabel
  Tabel yang digunakan adalah sebagai berikut<br>

  posts | comments | tags | post_tag |
  --- | --- | --- | --- |
  id | id | id | postId |
  content (STRING) | review (STRING) | name | tagId |

  <tb>1. Sebelum membuat migrasi database atau membuat tabel pastikan server database
aktif kemudian pastikan sudah membuat database dengan nama ```lumenpost```<br>
  ![Screenshot pembuatan tabel](/Gambar_Praktikum7/1.png) <br>
  
  <tb>2. Kemudian ubah konfigurasi database pada file ```.env``` menjadi<br>
  ```
    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=lumenpost
    DB_USERNAME=root
    DB_PASSWORD=
  ```
  ![Screenshot pembuatan tabel](/Gambar_Praktikum7/2.png) <br>
  
  <tb>3. Setelah mengubah konfigurasi pada file .env, kita juga perlu menghidupkan beberapa library bawaan dari lumen dengan membuka file ```app.php``` pada folder ```bootstrap``` dan mengubah baris berikut<br>
  ```
    // $app->withFacades();
    // $app->withEloquent();
  ```
  menjadi seperti dibawah ini
  ```
    $app->withFacades();
    $app->withEloquent();
  ```
  ![Screenshot pembuatan tabel](/Gambar_Praktikum7/3.png) <br>
  
  <tb>4. Setelah itu jalankan command berikut untuk membuat file ```migration``` <br>
  ```
    php artisan make:migration create_posts_table
    php artisan make:migration create_comments_table
    php artisan make:migration create_tags_table
    php artisan make:migration create_post_tag_table
  ```
  ![Screenshot pembuatan tabel](/Gambar_Praktikum7/4.png) <br>
  ![Screenshot pembuatan tabel](/Gambar_Praktikum7/5.png) <br>
  
  <tb>5. Ubah fungsi ```up()``` pada file ```migrasi create_posts_table```<br>
  ```
    public function up()
    {
      Schema::create('posts', function (Blueprint $table) {
        $table->id();
        $table->timestamps();
        $table->string('content');
      });
    }
    ...
  ```
  ![Screenshot pembuatan tabel](/Gambar_Praktikum7/6.png) <br>
  
  <tb>6. Ubah fungsi ```up()``` pada file ```create_comments_table```<br>
  ```
    public function up()
    {
      Schema::create('comments', function (Blueprint $table) {
        $table->id();
        $table->timestamps();
        $table->string('review');
        $table->foreignId('postId')->unsigned();
      });
    }
    ... 
  ```
  ![Screenshot pembuatan tabel](/Gambar_Praktikum7/7.png) <br>
  
  <tb>7. Ubah fungsi ```up()``` pada file ```create_tags_table```<br>
  ```
    public function up()
    {
      Schema::create('tags', function (Blueprint $table) {
        $table->id();
        $table->timestamps();
        $table->string('name');
      });
    }
    ...
  ```
  ![Screenshot pembuatan tabel](/Gambar_Praktikum7/8.png) <br>
  
  <tb>8. Ubah fungsi ```up()``` pada file ```create_post_tag_table```<br>
  ```
    public function up()
    {
      Schema::create('post_tag', function (Blueprint $table) {
        $table->id();
        $table->timestamps();
        $table->foreignId('postId')->unsigned();
        $table->foreignId('tagId')->unsigned();
      });
    }
    ...
  ```
  ![Screenshot pembuatan tabel](/Gambar_Praktikum7/9.png) <br>
  
  <tb>9. Kemudian jalankan command dibawah ini<br>
  ```
    php artisan migrate
  ```
  ![Screenshot pembuatan tabel](/Gambar_Praktikum7/10.png) <br>
  ![Screenshot pembuatan tabel](/Gambar_Praktikum7/11.png) <br>
  
* ## Pembuatan Model
  <tb>1. Buatlah file dengan nama ```Post.php``` dan isi dengan baris kode berikut<br>
  ```
    <?php
    namespace App\Models;
  
    use Illuminate\Database\Eloquent\Model;
  
    class Post extends Model
    {
      /**
      * The attributes that are mass assignable.
      *
      * @var string[]
      */
      protected $fillable = [
        'content'
      ];
  
      /**
      * The attributes excluded from the model's JSON form.
      *
      * @var string[]
      */
      protected $hidden = [];
    }
  ```
  ![Screenshot pembuatan model](/Gambar_Praktikum7/12.png) <br>
  
  <tb>2. Buatlah file dengan nama ```Comment.php``` dan isi dengan baris kode berikut<br>
  ```
    <?php
    namespace App\Models;
  
    use Illuminate\Database\Eloquent\Model;
  
    class Comment extends Model
    {
      /**
      * The attributes that are mass assignable.
      *
      * @var string[]
      */
      protected $fillable = [
        'review'
      ];

      /**
      * The attributes excluded from the model's JSON form.
      *
      * @var string[]
      */
      protected $hidden = [];
    }
  ```
  ![Screenshot pembuatan model](/Gambar_Praktikum7/13.png) <br>
  
  <tb>3. Buatlah file dengan nama ```Tag.php``` dan isi dengan baris kode berikut<br>
  ```
    <?php
    namespace App\Models;
  
    use Illuminate\Database\Eloquent\Model;
  
    class Tag extends Model
    {
      /**
      * The attributes that are mass assignable.
      *
      * @var string[]
      */
      protected $fillable = [
        'name'
      ];
  
      /**
      * The attributes excluded from the model's JSON form.
      *
      * @var string[]
      */
      protected $hidden = [];
    }
   ```
  ![Screenshot pembuatan model](/Gambar_Praktikum7/14.png) <br>
  
* ## Relasi One-to-Many
  <tb>1. Tambahkan fungsi ```comments()``` pada file ```Post.php```<br>
  ```
   
      // fungsi comments
      public function comments()
      {
        return $this->hasMany(Comment::class, 'postId');
      }
    
  ```
  ![Screenshot relasi](/Gambar_Praktikum7/15.png) <br>
  
  <tb>2. Tambahkan fungsi ```post()``` dan atribut postId pada ```$fillable``` pada file ```Comment.php```<br>
  ```
      public function post()
      {
        return $this->belongsTo(Post::class, 'postId');
      }
  ```
  ![Screenshot relasi](/Gambar_Praktikum7/16.png) <br>
  
  <tb>3. Buatlah file ```PostController.php``` dan isilah dengan baris kode berikut<br>
  ```
    <?php
    namespace App\Http\Controllers;
  
    use App\Models\Post;
    use Illuminate\Http\Request;

    class PostController extends Controller
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
      public function createPost(Request $request)
      {
        $post = Post::create([
          'content' => $request->content,
        ]);
  
        return response()->json([
          'success' => true,
          'message' => 'New post created',
          'data' => [
            'post' => $post
          ]
        ]);
      }
  
      public function getPostById(Request $request)
      {
        $post = Post::find($request->id);
  
        return response()->json([
          'success' => true,
          'message' => 'All post grabbed',
          'data' => [
            'post' => [
              'id' => $post->id,
              'content' => $post->content,
              'comments' => $post->comments,
            ]
          ]
        ]);
      }
    }
  ```
  ![Screenshot relasi](/Gambar_Praktikum7/17.png) <br>
  
  <tb>4. Buatlah file ```CommentController.php``` dan isilah dengan baris kode berikut<br>
  ```
    <?php
  
    namespace App\Http\Controllers;

    use App\Models\Comment;
    use Illuminate\Http\Request;
  
    class CommentController extends Controller
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
      public function createComment(Request $request)
      {
        $comment = Comment::create([
          'review' => $request->review,
          'postId' => $request->postId,
        ]);
  
        return response()->json([
          'success' => true,
          'message' => 'New comment created',
          'data' => [
            'comment' => $comment
          ]
        ]);
      }
    }
  ```
  ![Screenshot relasi](/Gambar_Praktikum7/18.png) <br>
  
  <tb>5. Tambahkan baris berikut pada ```routes/web.php```<br>
  ```
    $router->group(['prefix' => 'posts'], function () use ($router) {
      $router->post('/', ['uses' => 'PostController@createPost']);
      $router->get('/{id}', ['uses' => 'PostController@getPostById']);
    });
  
    $router->group(['prefix' => 'comments'], function () use ($router) {
      $router->post('/', ['uses' => 'CommentController@createComment']);
    });
  ```
  ![Screenshot relasi](/Gambar_Praktikum7/19.png) <br>
  
  <tb>6. Buatlah satu post menggunakan Postman<br>
  ![Screenshot relasi](/Gambar_Praktikum7/20.png) <br>

  ***Hasil pada PHPMyAdmin:***
  ![Screenshot relasi](/Gambar_Praktikum7/21.png) <br>
  
  <tb>7. Buatlah satu comment menggunakan Postman<br>
  ![Screenshot relasi](/Gambar_Praktikum7/22.png) <br>

  ***Hasil pada PHPMyAdmin:***
  ![Screenshot relasi](/Gambar_Praktikum7/23.png) <br>
  
  <tb>8. Tampilkan post menggunakan Postman<br>
  ![Screenshot relasi](/Gambar_Praktikum7/24.png) <br>
  
* ## Relasi Many-to-Many
  <tb>1. Tambahkan fungsi ```tags()``` pada file ```Post.php```<br>
  ```
      public function tags()
      {
        return $this->belongsToMany(Tag::class, 'post_tag', 'postId', 'tagId');
      }
  ```
  ![Screenshot relasi](/Gambar_Praktikum7/25.PNG) <br>
  
  <tb>2. Tambahkan fungsi ```posts()``` pada file ```Tag.php```<br>
  ```
      public function posts()
      {
        return $this->belongsToMany(Post::class, 'post_tag', 'tagId', 'postId');
      }
  ```
  ![Screenshot relasi](/Gambar_Praktikum7/26.PNG) <br>
  
  <tb>3. Buatlah file ```TagController.php``` dan isilah dengan baris kode berikut<br>
  ```
    <?php
    namespace App\Http\Controllers;
  
    use App\Models\Tag;
    use Illuminate\Http\Request;
  
    class TagController extends Controller
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
      public function createTag(Request $request)
      {
        $tag = Tag::create([
          'name' => $request->name
        ]);
  
        return response()->json([
          'success' => true,
          'message' => 'New tag created',
          'data' => [
            'tag' => $tag
          ]
        ]);
      }
    }
  ```
  ![Screenshot relasi](/Gambar_Praktikum7/27.PNG) <br>
  
  <tb>4. Tambahkan fungsi ```addTag``` dan response tags pada ```PostController.php```<br>
  ```
      public function getPostById(Request $request)
      {
        $post = Post::find($request->id);
  
        return response()->json([
          'success' => true,
          'message' => 'All post grabbed',
          'data' => [
            'post' => [
              'id' => $post->id,
              'content' => $post->content,
              'comments' => $post->comments,
              'tags' => $post->tags, //response tags
            ]
          ]
        ]);
      }

      public function addTag(Request $request)
      {
        $post = Post::find($request->id);
  
        $post->tags()->attach($request->tagId);
  
        return response()->json([
          'success' => true,
          'message' => 'Tag added to post',
        ]);
      }
  ```
  ![Screenshot relasi](/Gambar_Praktikum7/28.PNG) <br>
  
  <tb>5. Tambahkan baris berikut pada ```routes/web.php```<br>
  ```
    $router->group(['prefix' => 'posts'], function () use ($router) {
      $router->post('/', ['uses' => 'PostController@createPost']);
      $router->get('/{id}', ['uses' => 'PostController@getPostById']);
      $router->put('/{id}/tag/{tagId}', ['uses' => 'PostController@addTag']); //
    });
  
    ...
    $router->group(['prefix' => 'tags'], function () use ($router) {
      $router->post('/', ['uses' => 'TagController@createTag']);
    });
  ```
  ![Screenshot relasi](/Gambar_Praktikum7/29.PNG) <br>
  
  <tb>6. Buatlah satu tag menggunakan Postman<br>
  ![Screenshot relasi](/Gambar_Praktikum7/30.PNG) <br>

  ***Hasil di PHPMyAdmin:***
  ![Screenshot relasi](./Gambar_Praktikum7/31.PNG) <br>
  
  <tb>7. Tambahkan tag **“jadul”** pada post **“disana engkau berdua”** <br>
  ![Screenshot relasi](/Gambar_Praktikum7/32.PNG) <br>
  
  <tb>8. Tampilkan post **“disana engkau berdua”** menggunakan Postman<br>
  ![Screenshot relasi](/Gambar_Praktikum7/33.PNG) <br>
  
  <tb>9. Buatlah postingan **“tanpamu apa artinya”** menggunakan Postman<br>
  ![Screenshot relasi](/Gambar_Praktikum7/34.PNG) <br>
  
  <tb>10. Tambahkan tag **“jadul”** pada postingan **“tanpamu apa artinya”** <br>
  ![Screenshot relasi](/Gambar_Praktikum7/35.PNG) <br>
  
  <tb>11. Buatlah tag **“lagu”** menggunakan Postman<br>
  ![Screenshot relasi](/Gambar_Praktikum7/36.PNG) <br>
  
  <tb>12. Tambahkan tag **“lagu”** pada postingan **“tanpamu apa artinya”** <br>
  ![Screenshot relasi](/Gambar_Praktikum7/37.PNG) <br>
  
  <tb>13. Tampilkan post pertama<br>
  ![Screenshot relasi](/Gambar_Praktikum7/38.PNG) <br>
  
  <tb>14. Tampilkan post kedua<br>
  ![Screenshot relasi](/Gambar_Praktikum7/39.PNG) <br>
  
  > [!NOTE]
  > Tag "jadul" yang berada pada dua post menunjukkan **satu** tag dapat berada di **banyak** post

  > [!NOTE]
  > Post "tanpamu apa artinya" yang memiliki dua tag menunjukkan **satu** post dapat memiliki banyak **tag**
