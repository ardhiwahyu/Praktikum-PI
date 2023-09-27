# Integrasi MongoDB dan Express

* ## Percobaan Instalasi NodeJS
  a. Dikarenakan sudah menginstalasi NodeJS, maka jalankan command ```node -v``` untuk memeriksa versi dari NodeJs <br>
  ![Screenshot node.js](/Gambar_Praktikum3/1.png) <br>
  b. Jika belum menginstall, maka bisa mendownload dulu pada link https://nodejs.org/en 

  
* ## Inisiasi Project Express dan Pemasangan Package
  Express.js adalah sebuah framework web app untuk Node.js yang ditulis dengan bahasa pemrograman JavaScript. Framework ini dirancang secara fleksibel dan minimalis untuk membantu tahap pengembangan aplikasi back-end. <br>

  Mongoose adalah sebuah pustaka atau library JavaScript yang digunakan untuk memudahkan pemodelan data pada MongoDB. Mongoose memungkinkan pengembang untuk membuat skema atau blueprint untuk dokumen MongoDB dan menyediakan berbagai fitur seperti validasi data, middleware, dan query builder. <br>

  ### Inisiasi project Express.js dapat dilakukan dengan cara: 
  a. Lakukan pembuatan folder dengan nama express-mongodb dan masuk ke dalam folder tersebut lalu buka melalui text editor masing-masing <br>
  ![Screenshot express](/Gambar_Praktikum3/2.png) <br>
  b. Lakukan npm init untuk mengenerate file package.json dengan menggunakan command ```npm init -y``` <br>
  ![Screenshot express](/Gambar_Praktikum3/3.png) <br>
  c. Lakukan instalasi express, mongoose, dan dotenv dengan menggunakan command ```npm i express mongoose dotenv``` <br>
  ![Screenshot express](/Gambar_Praktikum3/4.png) <br>
  
* ## Koneksi Express ke MongoDB
  a. Buatlah file **index.js** pada root folder <br>
  ![Screenshot Index.js](/Gambar_Praktikum3/5.png)
  b. Masukkan kode berikut pada file **index.js** <br>
    ```
    require('dotenv').config();
    const express = require('express');
    const mongoose = require('mongoose');

    const app = express();

    app.use(express.json());

    app.get('/', (req, res) => {
        res.status(200).json({
        message: '<nama>,<nim>'
        })
    })

    const PORT = 8000;
    app.listen(PORT, () => {
        console.log(`Running on port ${PORT}`);
    })
    ```
  ![Screenshot koneksi](/Gambar_Praktikum3/6.png) <br>
  c. Setelah itu coba jalankan dengan command ```node index.js``` <br>
  > [!NOTE]
  > Lakukan langkah ini untuk melakukan restart server node setiap kali melakukan perubahan pada file
  >
  ![Screenshot koneksi](/Gambar_Praktikum3/7.png) <br>
  d. Lakukan pembuatan file **.env** dan masukkan baris berikut <br>
  ```
  PORT=5000
  ```
  ![Screenshot koneksi](/Gambar_Praktikum3/8.png) <br>
  ![Screenshot koneksi](/Gambar_Praktikum3/9.png)<br>
  e. Ubah kode pada listening port menjadi berikut dan coba jalankan aplikasi kembali <br>
    ```
    ...

    const PORT = process.env.PORT || 8000;
    app.listen(PORT, () => {
        console.log(`Running on port ${PORT}`);
    })
    ```
  ![Screenshot koneksi](/Gambar_Praktikum3/10.png) <br>
  f. Copy connection string yang terdapat pada compas atau atlas dan paste kan pada **.env** seperti berikut <br>
    ```
    MONGO_URI=<Connection string masing-masing>
    ```
  ![Screenshot koneksi](/Gambar_Praktikum3/13.png) <br>
  g. Tambahkan baris kode berikut pada file **index.js** <br>
    ```
    require('dotenv').config();
    const express = require('express');
    const mongoose = require('mongoose');

    mongoose.connect(process.env.MONGO_URI);
    const db = mongoose.connection;

    db.on('error', (error) => {
        console.log(error);
    });

    db.once('connected', () => {
        console.log('Mongo connected');
    })

    ...
    ```
  ![Screenshot koneksi](/Gambar_Praktikum3/14.png) <br>
  h. Jalankan aplikasi kembali seperti langkah c <br>
  ![Screenshot koneksi](/Gambar_Praktikum3/15.png) <br>
  
* ## Pembuatan Routing
  Routing adalah mekanisme yang digunakan oleh sebuah aplikasi web untuk merespons permintaan dari client atau browser ke endpoint atau URL tertentu dengan spesifik. alam Express.js, routing dapat diatur menggunakan metode app yang akan merespons setiap permintaan berbentuk HTTP seperti GET, POST, PUT, dan DELETE. Routing juga dapat digunakan untuk membuat endpoint pada RESTful API. alam routing, terdapat beberapa parameter seperti METHOD, PATH, dan HANDLER yang digunakan untuk menentukan jenis permintaan, path atau URL pada server, dan fungsi yang dijalankan ketika route sesuai atau match. <br>

  Routing dapat dibuat dengan langkah berikut: <br>
  a. Lakukan pembuatan direktori **routes** di tingkat yang sama dengan index.js <br>
  b. Buat file **book.route.js** di dalamnya <br>
  ![Screenshot routing](/Gambar_Praktikum3/16.png) <br>
  c. Tambahkan baris kode berikut untuk fungsi ```getAllBooks``` <br>
    ```
    const router = require('express').Router();
    
    router.get('/', function getAllBooks(req, res) {
        res.status(200).json({
        message: 'mendapatkan semua buku'
        })
    })
    module.exports = router;
    ```
  ![Screenshot routing](/Gambar_Praktikum3/17.png) <br>
    d. Lakukan hal yang sama untuk ```getOneBook```, ```createBook```, ```updateBook```, dan ```deleteBook``` <br>
    ```
    const router = require('express').Router();
    
    ...
    
    router.get('/:id', function getOneBook(req, res) {
        const id = req.params.id;
        res.status(200).json({
        message: 'mendapatkan satu buku',
        id,
        })
    })

    router.post('/', function createBook(req, res) {
        res.status(200).json({
        message: 'membuat buku baru'
        })
    })

    router.put('/:id', function updateBook(req, res) {
        const id = req.params.id;
        res.status(200).json({
        message: 'memperbaharui satu buku',
        id,
        })
    })
    
    router.delete('/:id', function deleteBook(req, res) {
    const id = req.params.id;
        res.status(200).json({
        message: 'menghapus satu buku',
        id,
        })
    })
    
    module.exports = router;
    ```
  ![Screenshot routing](/Gambar_Praktikum3/18.png) <br>
    e. Lakukan import book.route.js pada file index.js dan tambahkan baris kode berikut <br>
    ```
    require('dotenv').config();
    const express = require('express');
    const mongoose = require('mongoose');
    
    const bookRoutes = require('./routes/book.route'); //
    
    ...
    
    app.get('/', (req, res) => {
        res.status(200).json({
        message: '<nama>,<nim>'
        })
    })
    
    app.use('/books', bookRoutes); //
    
    const PORT = process.env.PORT || 8000;
    app.listen(PORT, () => {
        console.log(`Running on port ${PORT}`);
    })
    ```
  ![Screenshot routing](/Gambar_Praktikum3/19.png) <br>
    f. Uji salah satu endpoint dengan Postman <br>
    ![Screenshot routing](/Gambar_Praktikum3/20.png) <br>
    
* ## Pembuatan Controller
Dalam konsep Model-View-Controller (MVC), controller adalah bagian dari aplikasi yang bertanggung jawab untuk mengontrol alur kerja aplikasi. Controller menerima permintaan dari pengguna dan mengambil data dari model untuk kemudian menampilkan data tersebut pada view. Dengan adanya controller, pengembang dapat memisahkan tugas-tugas pada aplikasi web menjadi tiga bagian yang terdiri dari model, view, dan controller, sehingga memudahkan dalam pengembangan dan perawatan aplikasi.  <br>

Controller dapat dibuat dengan cara: <br>
a. Lakukan pembuatan direktori **controllers** di tingkat yang sama dengan index.js <br>
b. Buatlah file **book.controller.js** di dalamnya <br>
![Screenshot controller](/Gambar_Praktikum3/21.png) <br>
c. Salin baris kode dari routes untuk fungsi ```getAllBooks``` <br>
```
function getAllBooks(req, res) {
  res.status(200).json({
    message: 'mendapatkan semua buku'
  })
};

module.exports = {
  getAllBooks,
}
```
![Screenshot controller](/Gambar_Praktikum3/22.png) <br>
  d. Lakukan hal yang sama untuk ```getOneBook```, ```createBook```, ```updateBook```, dan ```deleteBook``` <br>
```
...

function getOneBook(req, res) {
  const id = req.params.id;
  res.status(200).json({
    message: 'mendapatkan satu buku',
    id,
  })
}

function createBook(req, res) {
  res.status(200).json({
    message: 'membuat buku baru'
  })
}

function updateBook(req, res) {
  const id = req.params.id;
  res.status(200).json({
    message: 'memperbaharui satu buku',
    id,
  })
}

function deleteBook(req, res) {
  const id = req.params.id;
  res.status(200).json({
    message: 'menghapus satu buku',
    id,
  })
}

module.exports = {
getAllBooks,
getOneBook, //
createBook, //
updateBook, //
deleteBook //
}
```
![Screenshot controller](/Gambar_Praktikum3/23.png) <br>
  e. Lakukan import book.controller.js pada file book.route.js <br>
```
  const router = require('express').Router();
  const book = require('../controllers/book.controller'); //

  ...

  module.exports = router;
```
![Screenshot controller](/Gambar_Praktikum3/24.png) <br>
  f. Lakukan perubahan pada fungsi agar dapat memanggil fungsi dari book.controller.js <br>
```
const router = require('express').Router();
const book = require('../controllers/book.controller');

router.get('/', book.getAllBooks);
router.get('/:id', book.getOneBook);
router.post('/', book.createBook);
router.put('/:id', book.updateBook);
router.delete('/:id', book.deleteBook);

module.exports = router;
```
![Screenshot controller](/Gambar_Praktikum3/24.png) <br>
  g. Lakukan pengujian kembali, pastikan response tetap sama <br>
  ![Screenshot controller](/Gambar_Praktikum3/25.png) <br>
  
* ## Pembuatan Model
  Dalam konsep Model-View-Controller (MVC), model adalah bagian dari aplikasi yang bertanggung jawab untuk menyiapkan, mengatur, memanipulasi, dan mengorganisasikan data yang ada di database. Dalam alur kerja MVC, model akan menerima permintaan dari controller untuk mengambil data dari database, kemudian mengirimkan data tersebut ke controller untuk ditampilkan pada view. <br>

  Model dapat dibuat dengan cara: <br>
  a. Lakukan pembuatan direktori **models** di tingkat yang sama dengan index.js <br>
  b. Buat file **book.model.js** di dalamnya <br>
  ![Screenshot model](/Gambar_Praktikum3/26.png) <br>
  c. Tambahkan baris kode berikut <br>
  ```
  const mongoose = require('mongoose');
  
  const bookSchema = new mongoose.Schema({
    title: {
      type: String
    },
    author: {
      type: String
    },
    year: {
      type: Number
    },
    pages: {
      type: Number
    },
    summary: {
      type: String
    },
    publisher: {
      type: String
    }
  })
  
  module.exports = mongoose.model('book', bookSchema);
  ```
  ![Screenshot model](/Gambar_Praktikum3/27.png) <br>
  
* ## Operasi CRUD
  a. Hapus semua data pada collection books yang telah dibuat pada praktikum sebelumnya ( https://afifahrahma22.github.io/praktikum-pemin/praktikum/praktikum2 ) <br>
  ![Screenshot CRUD](/Gambar_Praktikum3/28.png) <br>
  b. Lakukan import book.model.js pada file book.controller.js <br>
  ```
  const Book = require('../models/book.model');
  
  ...
  ```
  ![Screenshot CRUD](/Gambar_Praktikum3/29.png) <br>
    c. Lakukan perubahan pada fungsi ```createBook``` <br>
  ```
  const Book = require('../models/book.model');
  
  ...
  
  async function createBook(req, res) {
    const book = new Book({
      title: req.body.title,
      author: req.body.author,
      year: req.body.year,
      pages: req.body.pages,
      summary: req.body.summary,
      publisher: req.body.publisher,
    })

    try {
      const savedBook = await book.save();
      res.status(200).json({
        message: 'membuat buku baru',
        book: savedBook,
      })
    } catch (error) {
      res.status(500).json({
        message: 'kesalahan pada server',
        error: error.message,
      })
    }
  }
  
  ...
  ```
  ![Screenshot CRUD](/Gambar_Praktikum3/30.png) <br>
    d. Buatlah dua buah buku dengan data di bawah ini dengan Postman <br>
  ```
    {
      "title": "Dilan 1990",
      "author": "Pidi Baiq",
      "year": 2014,
      "pages": 332,
      "summary": "Mirea, anata wa utsukushī",
      "publisher": "Pastel Books"
    }
  ```
  ```
    {
      "title": "Dilan 1991",
      "author": "Pidi Baiq",
      "year": 2015,
      "pages": 344,
      "summary": "Watashi ga kare o aishite iru to ittara",
      "publisher": "Pastel Books"
    }
  ```
  ![Screenshot CRUD](/Gambar_Praktikum3/31.png) <br>
  ![Screenshot CRUD](/Gambar_Praktikum3/32.png) <br>
    e. Lakukan perubahan pada fungsi ```getAllBooks``` <br>
  ```
    const Book = require('../models/book.model');
  
    async function getAllBooks(req, res) {
      try {
        const books = await Book.find();
        res.status(200).json({
          message: 'mendapatkan semua buku',
          books,
        })
      } catch (error) {
        res.status(500).json({
          message: 'kesalahan pada server',
          error: error.message,
        })
      }
    }
  
    ...
  ```
  ![Screenshot CRUD](/Gambar_Praktikum3/33.png) <br>
    f. Lakukan perubahan pada fungsi ```getOneBook``` <br>
  ```
    const Book = require('../models/book.model');
  
    ...
  
    async function getOneBook(req, res) {
      const id = req.params.id;
  
      try {
        const book = await Book.findById(id);
        res.status(200).json({
          message: 'mendapatkan satu buku',
          book,
        })
      } catch (error) {
        res.status(500).json({
          message: 'kesalahan pada server',
          error: error.message,
        })
      }
    }
  
    ...
  ```
  ![Screenshot CRUD](/Gambar_Praktikum3/34.png) <br>
    g. Tampilkan semua buku dengan Postman <br>
    ![Screenshot CRUD](/Gambar_Praktikum3/35.png) <br>
    h. Tampilkan buku Dilan 1990 dengan Postman <br>
    ![Screenshot CRUD](/Gambar_Praktikum3/36.png) <br>
    i. Lakukan perubahan pada fungsi ```updateBook``` <br>
  ```
    const Book = require('../models/book.model');
  
    ...
  
    async function updateBook(req, res) {
      const id = req.params.id;
  
      try {
        const book = await Book.findByIdAndUpdate(
          id, req.body, { new: true }
        )
        res.status(200).json({
          message: 'memperbaharui satu buku',
          book,
        })
      } catch (error) {
        res.status(500).json({
          message: 'kesalahan pada server',
          error: error.message,
        })
      }
    }
  
    ...
  ```
  ![Screenshot CRUD](/Gambar_Praktikum3/37.png) <br>
    j. Ubah judul buku Dilan 1991 menjadi “NAMA PANGGILAN 1991” dengan Postman <br>
    ![Screenshot CRUD](/Gambar_Praktikum3/38.png) <br>
    k. Lakukan perubahan pada fungsi ```deleteBook``` <br>
  ```
    const Book = require('../models/book.model');
  
    ...
  
    async function deleteBook(req, res) {
      const id = req.params.id;
  
      try {
        const book = await Book.findByIdAndDelete(id);
        res.status(200).json({
          message: 'menghapus satu buku',
          book,
        })
      } catch (error) {
        res.status(500).json({
          message: 'kesalahan pada server',
          error: error.message,
        })
      }
    }
  
    ...
  ```
  ![Screenshot CRUD](/Gambar_Praktikum3/39.png) <br>
    l. Hapus buku Dilan 1990 dengan Postman <br>
    ![Screenshot CRUD](/Gambar_Praktikum3/40.png) <br>
    ![Screenshot CRUD](/Gambar_Praktikum3/41.png) <br>