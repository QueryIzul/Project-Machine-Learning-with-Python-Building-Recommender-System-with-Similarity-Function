Introduction
Membuat sebuah sistem rekomendasi berdasarkan konten dari sebuah film. 

Pengenalan Project
Masih ingat dengan project Building Recommender System using Python?

Pada bagian sebelumnya kita telah melihat bagaimana recommender system dibuat hanya dengan menggunakan average rating, dengan mengurutkan score yang terdapat komponen average rating secara descending, kita dapat mengetahui (secara estimasi) film mana yang menurut para audience paling menarik.

Kali ini, kita akan membuat recommender system yang menggunakan Content/feature dari film/entitas tersebut, kemudian melakukan perhitungan terhadap kesamaannya satu dan yang lain sehingga ketika kita menunjuk ke satu film, kita akan mendapat beberapa film lain yang memiliki kesamaan dengan film tersebut. Hal ini biasa kita sebut sebagai Content Based Recommender System.

Dengan membandingkan kesamaan plot yang ada dan genre yang ada, ketika audience lebih menyukai film Narnia, maka content based recommender system ini akan juga merekomendasikan film seperti Harry Potter atau The Lords of The Rings yang memiliki genre yang mirip.

Task 1 - Unloading and Checking Datasets
Pada bagian ini akan dilakukan import library yang dibutuhkan dan pembacaan dataset yang digunakan. 

Import Basics Library and File Unloading
Langkah pertama yang harus kita lakukan adalah melakukan import library yang dibutuhkan untuk pengerjaan project ini dan melakukan pembacaan dataset.

Notes :

Library yang akan kita gunakan adalah pandas (as pd) dan numpy (as np)
Dataset yang akan digunakan adalah movie_rating_df.csv
Akses dataset :

movie_rating_df.csv = https://storage.googleapis.com/dqlab-dataset/movie_rating_df.csv 

Menampilkan 5 data teratas dan info data
Setelah sebelumnya kita sudah menyimpan dataset pada variabel movie_rating_df, hal selanjutnya yang akan kita lakukan adalah menampilkan lima baris teratas dari dataset tersebut dan menampilkan info mengenai tipe data dan jumlah non-null value dari tiap kolom yang ada pada dataset. 

Add Actors Dataframe
Dari output yang sudah dihasilkan sebelumnya, kita dapat memperoleh list film dengan beberapa metadata seperti isAdult, runtimeMinutes, dan genres nya

Selanjutnya, kita akan menambahkan metadata lain seperti aktor/aktris yang bermain di film tersebut, kita akan menggunakan dataframe lain kemudian akan melakukan join dengan dataframe movie_rating_df. 

Dataset yang akan digunakan adalah actor_name.csv

Akses dataset: https://storage.googleapis.com/dqlab-dataset/actor_name.csv

Add Directors and Writers Dataframe
Dataframe yang akan ditambahkan selanjutnya adalah dataframe yang berisi directors dan writers dari film.

Dataset yang akan digunakan adalah directors_writers.csv

Akses dataset : https://storage.googleapis.com/dqlab-dataset/directors_writers.csv

Convert into List
Setelah menampilkan informasi mengenai dataframe directors_writer, dapat dilihat bahwa tidak ada nilai NULL pada dataset tersebut. Hal selanjutnya yang akan kita lakukan adalah mengubah director_name dan writer_name dari string menjadi list

Setelah itu, tampilkan 5 baris teratas dari dataframe director_writers

Task 2 - Cleaning and Processing Table Cast
Membuang kolom-kolom yang tidak digunakan dan menghapus nilai NULL 

Update name_df
Kita hanya akan membutuhkan kolom nconst, primaryName, dan knownForTitles pada name_df untuk mencocokkan aktor/aktris ini dengan film yang ada. 

Movies per Actor
Hal selanjutnya yang ingin kita ketahui adalah mengenai variasi dari jumlah film yang dapat dibintangi oleh seorang aktor.

Tentunya seorang aktor dapat membintangi lebih dari 1 film, bukan? maka akan diperlukan untuk membuat table yang mempunyai relasi 1-1 ke masing-masing title movie tersebut. Kita akan melakukan unnest terhadap table tersebut. 

Pekerjaan selanjutnya yang harus kita lakukan adalah :

Melakukan pengecekan variasi jumlah film yang dibintangi oleh aktor.
Mengubah kolom 'knownForTitles' menjadi list of list.

Korespondensi 1 - 1
Karena pada data sebelumnya dapat dilihat bahwa seorang aktor dapat membintangi 1 sampai 4 film, diperlukan untuk membuat table yang mempunyai relasi 1-1 dari aktor ke masing-masing title movie tersebut. 

contoh table yang tidak korespondensi 1-1

Contoh table korespondensi 1-1

Task 3 - Nesting primaryName group by knownForTitles
Melakukan grouping kembali pada kolom player karena yang kita inginkan adalah level movie untuk melakukan movie recommendation

Mengelompokkan primaryName menjadi list group by knownForTitles
Selanjutnya, kita akan melakukan grouping kembali pada kolom player karena yang kita inginkan adalah level movie untuk melakukan movie recommendation

Task 4 - Joining with Movie Table

Join table
Sekarang kita akan melakukan :

join antara movie table dan cast table  ( field knownForTitles dan tconst)
join antara base_df dengan director_writer table (field tconst dan tconst)

Cleaning data
Setelah melakukan join table sebelumnya, sekarang hal yang akan kembali kita lakukan adalah melakukan cleaning pada data yang sudah dihasilkan. 

Reformat table base_df
Hal selanjutnya yang akan kita lakukan adalah melakukan reformat pada table base_df yang beberapa kolomnya sudah didrop.

Petunjuk: 

Rename-lah kolom berikut:

primaryTitle -> title
titleType -> type
startYear -> start
runtimeMinutes -> duration
averageRating -> rating
numVotes -> votes

Task 5 - Creating Content-based Recommender System

Klasifikasi Metadata
kita akan klasifikasikan berdasarkan metadata genres, primaryName (cast name), director name, dan writer_name

Pertanyaan 1: Bagaimana cara membuat fungsi untuk strip spaces dari setiap row dan setiap elemennya?
Lengkapilah function sanitize yang digunakan untuk melakukan strip spaces dari setiap row dan setiap elemennya

Pertanyaan 2: Bagaimana cara membuat fungsi untuk membuat metadata soup (menggabungkan semua feature menjadi 1 bagian kalimat) untuk setiap judulnya?
Lengkapi function soup_feature yang digunakan untuk menggabungkan semua feature  menjadi 1 bagian 

Pertanyaan 3: Cara menyiapkan CountVectorizer (stop_words = english) dan fit dengan soup yang kita buat di atas
CountVectorizer adalah tipe paling sederhana dari vectorizer. Supaya lebih mudah akan dijelaskan melalui contoh di bawah ini:

bayangkan terdapat 3 text A, B, dan C, dimana text nya adalah

A: The Sun is a star
B: My Love is like a red, red rose
C : Mary had a little lamb
Sekarang kita harus konversi text-text ini menjadi bentuk vector menggunakan CountVectorizer. Langkah-langkahnya adalah: menghitung ukuran dari vocabulary. Vocabulary adalah jumlah dari kata unik yang ada dari text tersebut.


Oleh sebab itu, vocabulary dari set ketiga text tersebut adalah: the, sun, is, a, star, my, love, like, red, rose, mary, had, little, lamb. Secara total, ukuran vocabulary adalah 14.


Tetapi, biasanya kita tidak include stop words (english), seperti as, is, a, the, dan sebagainya karena itu adalah kata yang sudah common sekali.


Dengan mengeliminasi stop words, maka clean size vocabulary kita adalah like, little, lamb, love, mary, red, rose, sun, star (sorted alphabet ascending)
Maka, dengan menggunakan CountVectorizer, maka hasil yang kita dapatkan adalah sebagai berikut:


A : (0,0,0,0,0,0,0,1,1), terdiri atas sun:1, star:1
B : (1,0,0,1,0,2,1,0,0), terdiri atas like:1, love:1, red:2, rose:1
C : (0,1,1,0,1,0,0,0,0), terdiri atas little:1, lamb:1, mary:1

Pertanyaan 4: Cara membuat model similarity antara count matrix
Pada langkah ini, kita akan menghitung score cosine similarity dari setiap pasangan judul (berdasarkan semua kombinasi pasangan yang ada, dengan kata lain kita akan membuat 675 x 675 matrix, dimana cell di kolom i dan j menunjukkan score similarity antara judul i dan j. kita dapat dengan mudah melihat bahwa matrix ini simetris dan setiap elemen pada diagonal adalah 1, karena itu adalah similarity score dengan dirinya sendiri

 
Cosine Similarity
pada bagian ini, kita akan menggunakan formula cosine similarity untuk membuat model. Score cosine ini sangatlah berguna dan mudah untuk dihitung.

formula untuk perhitungan cosine similarity antara 2 text, adalah sebagai berikut:

$cosine(x,y)=\frac{x.y^T}{||x||.||y||}$
output yang didapat antara range -1 sampai 1. Score yang hampir mencapai 1 artinya kedua entitas tersebut sangatlah mirip sedangkan score yang hampir mencapai -1 artinya kedua entitas tersebut adalah beda

Pertanyaan 5: Cara membuat content based recommender system
Task selanjutnya yang harus dilakukan adalah reverse mapping dengan judul sebagai index nya