# Instalasi LumenAPI, MongoDB, dan Konfigurasi APP Key

* ## Percobaan Instalasi Composer
    Instalasi composer khususnya pada arsitketur M1 (Apple Silicon) bisa dilakukan dengan mengakses link https://getcomposer.org/doc/00-intro.md 

    Lalu setelah mengakses link tersebut, gunakan command 
    > *mv composer.phar /usr/local/bin/composer* 

    Namun pada tahap ini, composer telah terinstall sehingga langkah ini dilewati. <br>
    ![Screenshot composer](Gambar_Praktikum1/1.png) 

    * ## Percobaan Instalasi MongoDB Server dan MongoDB Compass
    * ### Instalasi MongoDB Server
    Instalasi MongoDB pada arsitektur M1 bisa dilakukan dengan mengakses link https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-os-x/  atau bisa dilakukan lewat terminal dengan menggunakan command sebagai berikut : 
    1. >xcode-select --install

    ![Screenshot mongoDB](Gambar_Praktikum1/2.png)
    Jika sudah terinstall, makan akan ada tulisan seperti gambar diatas. 

    2. >brew tap mongodb/brew
    
    Untuk mengunduh Homebrew Formula untuk MongoDB dan database tools. 

    3. >brew update

    Untuk mengupdate homebrew

    4. >brew install mongodb-community@7.0

    Lalu untuk menginstall mongoDB via homebrew, bisa menggunakan commad diatas.

    ![Screenshot mongoDB](Gambar_Praktikum1/3.png)

    5. >brew services start mongodb-community@7.0

    Lalu untuk memulai server mongoDB, bisa menggunakan command diatas.

    ![Screenshot mongoDB](Gambar_Praktikum1/4.png)

    Pada gambar tersebut, server mongoDB berhasil dijalankan.

    * ### Instalasi MongoDB Compass
    Untuk menginstal GUI dari mongoDB, kita bisa menggunakan aplikasi MongoDB Compass. Tahapannya yaitu : <br>
    1. Buka website MongoDB atau bisa melalui link https://www.mongodb.com/try/download/shell <br>
    2. Lalu pilih versi dan platform yang sesuai dengan device kalian.
    ![Screenshot Mongodb](../Gambar_Praktikum1/5.png)
    <br>
    disini saya menggunakan platform MacOS M1 dengan versi 1.10.6 lalu klik *download*
    3. Setelah Download, lalu ekstrak dan jalankan aplikasi MongoDB compass
    4. Lalu Mongodb Compass berhasil dijalankan.
    ![Screenshot MongoDB Compass](../Gambar_Praktikum1/6.png)

    * ## Percobaan Instalasi Lumen
    Untuk menginstal lumen, kita dapat menggunakan visual studio code. Untuk cara yaitu : <br>
    1. Buat Folder terlebih dahulu dengan menggunakan command
        >mkdir tesLumenAPI

        ![Screenshot Lumen](Gambar_Praktikum1/8.png)

    2. Setelah membuat folder, kita masuk kedalam folder lalu kita menginstal lumen dengan menggunakan command 
        >composer create-project --prefer-dist laravel/lumen lumenapi

        ![Screenshot Lumen](Gambar_Praktikum1/7.png)

    3.  Setelah itu, buka Visual Studio Code, lalu buka folder lumenAPI yang sudah terinstal.
        ![Screenshot Lumen](Gambar_Praktikum1/9.png)

    4. Lalu, buka folder routes dan kemudian buka file web.php lalu masukan syntax berikut kedalam file web.php
        >$router->get('/key', function () { <br>
    return Str::random(32); <br>
});

        ![Screenshot Lumen](Gambar_Praktikum1/10.png)


    5. Setelah itu, jalankan terminal pada Visual Studio Code lalu gunakan command dibawah ini untuk menjalankan server.
        >php -S localhost:8000 -t public


        ![Screenshot Lumen](Gambar_Praktikum1/11.png)


    6. Untuk mengetahui apakah lumen berhasil di instal atau tidak, jalankan "http://localhost:8000" pada browser
        ![Screenshot Lumen](Gambar_Praktikum1/12.png)

    7. Untuk mendapatkan appKey, jalanka "http://localhost:8000/key"
        ![Screenshot Lumen](Gambar_Praktikum1/13.png)

    8. Setelah mendapatkan appKey, maka instalasi lumen dan konfigurasi appKey berhasil di jalankan.






