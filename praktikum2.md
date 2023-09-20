# CRUD MongoDB Compass dan Shell

* ## MongoDB Compass
  MongoDB Compass adalah tool berbasis Graphical User Interface (GUI) yang digunakan untuk melakukan aktivitas dasar CREATE, READ, UPDATE, dan DELETE (CRUD) tanpa commad line pada MongoDB. Berikut adalah langkah-langkah untuk melakukan CRUD pada MongoDB Compass: <br>
  a. Sebelum mengoneksikan MongoDB, jalankan terlebih dahulu MongoDB Server melalui terminal dengan command 
  > ```brew services start mongodb-community@7.0```<br>

    ![Screenshot connection](Gambar_Praktikum2/1.png) <br>
  b. Lakukan koneksi ke MongoDB menggunakan connection string <br>
    ![Screenshot connection](Gambar_Praktikum2/2.png) <br>
  c. Buat database baru dengan menekan tanda plus "+", maka MongoDB Compass akan menampilkan input form seperti di bawah <br>
     ![Screenshot connection](Gambar_Praktikum2/3.png) <br>
  d. Isi informasi yang ada dan klik "Create Database". Database baru akan dibuat oleh MongoDB Compass <br>
     ![Screenshot connection](Gambar_Praktikum2/4.png) <br>

     ![Screenshot connection](Gambar_Praktikum2/5.png) <br>
  e. Pada percobaan ini, proses CREATE dilakukan dengan insert buku pertama dengan melakukan klik "Add Data", pilih "Insert Document", <br>
     ![Screenshot connection](Gambar_Praktikum2/6.png) <br>
  isi dengan data yang diinginkan dan klik "Next" <br>
     ![Screenshot connection](Gambar_Praktikum2/7.png) <br>
  f. Lakukan insert buku kedua dengan cara yang sama <br>
     ![Screenshot connection](Gambar_Praktikum2/8.png) <br>
  g. Proses READ dilakukan dengan cara mencari buku dengan author "Osamu Dazai" dengan mengisi filter yang diinginkan dan klik "Find" <br>
     ![Screenshot connection](Gambar_Praktikum2/9.png) <br>

  h. Sedangkan untuk proses UPDATE, dilakukan perubahan summary pada buku "No Longer Human" menjadi "Buku yang bagus (NAMA,NIM) dengan melakukan klik "Edit Document" (berlambang pensil), mengisi nilai summary yang baru, dan melakukan klik "Update" <br>
     ![Screenshot connection](Gambar_Praktikum2/10.png) <br>
     ![Screenshot connection](Gambar_Praktikum2/11.png) <br>
  i. Proses DELETE, dilakukan dengan cara penghapusan pada buku "I Am a Cat" dengan melakukan klik "Remove Document" (berlambang tong sampah) dan melakukan klik "Delete" <br>
     ![Screenshot connection](Gambar_Praktikum2/12.png) <br>
     ![Screenshot connection](Gambar_Praktikum2/13.png) <br>
     
* ## MongoDB Shell
  MongoDB Shell merupakan tool untuk melakukan aktivitas CRUD yang berbasis Command Line Interface (CLI). MongoDB Shell dapat diakses langsung dari MongoDB Compass atau menggunakan perintah mongosh pada Command Prompt. Berikut adalah langkah-langkah untuk melakukan CRUD pada MongoDB Shell: <br>
  a. Lakukan koneksi ke MongoDB dengan menjalankan command dibawah ini pada terminal yang ada di OS
    >```mongosh```
   



     ![Screenshot connection](Gambar_Praktikum2/14.png) <br>
  b. Lihat list database yang ada di server dengan menjalankan command 
    >```show dbs```



    
     ![Screenshot connection](Gambar_Praktikum2/15.png)<br>
    Untuk berpindah ke database "bookstore" gunakan command dibawah ini dan memastikan database telah berpindah dengan melihat tulisan sebelum tanda ">" <br> 
   > ```use bookstore```



     ![Screenshot connection](Gambar_Praktikum2/16.png) <br>
  c. Proses CREATE pada MongoDB Shell dapat dilakukan secara satu per satu atau langsung membuat banyak. Untuk membuat objek baru, dilakukan insert buku "Overlord I" dengan menggunakan command 
  
  >```db.books.insertOne(data)```



     ![Screenshot connection](Gambar_Praktikum2/17.png) <br>
  d. Sementara untuk membuat objek baru lebih dari satu, dilakukan insert buku "The Setting Sun" dan "Hujan" dengan insert many dengan menggunakan command 
  >```db.books.insertMany(data)```



     ![Screenshot connection](Gambar_Praktikum2/18.png) <br>
  e. Aktivitas READ, dilakukan dengan pencarian buku dengan menggunakan command dibawah ini untuk melakukan pencarian semua buku
  >```db.books.find()``` 


     ![Screenshot connection](Gambar_Praktikum2/19.png) <br>
  f. Tampilkan seluruh buku dengan author "Osamu Dazai" dengan mengisi argument pada find() dengan menggunakan command dibawah ini.
  > ```db.books.find({filter})``` 



     ![Screenshot connection](Gambar_Praktikum2/20.png) <br>
  g. Proses UPDATE dapat dilakukan dengan melakukan perubahan summary pada buku "Hujan" menjadi "Buku yang bagus (NAMA, NIM) dengan menggunakan command
  >```db.books.updateOne({filter}, {$set: {data yang ingin diupdate}})``` 



     ![Screenshot connection](Gambar_Praktikum2/21.png) <br>
  h. Lakukan perubahan publisher menjadi "Yen Press" pada semua buku "Osamu Dazai" dengan menggunakan command dibawah ini.
  >```db.books.updateMany({filter}, {$set: {data yang ingin diupdate}})``` 


    ![Screenshot connection](Gambar_Praktikum2/22.png) <br>
  i. Proses DELETE, dilakukan dengan penghapusan pada buku "Overlord I" dengan menggunakan command dibawah ini 
  >```db.books.deleteOne({argument})```


     ![Screenshot connection](Gambar_Praktikum2/23.png) <br>
  j. Lakukan penghapusan pada semua buku "Osamu Dazai" dengan menggunakan command dibawah ini. 
  >```db.books.deleteMany({argument})```


    ![Screenshot connection](Gambar_Praktikum2/24.png) <br>
Untuk melihat hasil dari proses CRUD di atas, dapat dilakukan pencarian seluruh buku menggunakan command dibawah ini.
    >```db.books.find()```

    ![Screenshot connection](Gambar_Praktikum2/25.png) <br>