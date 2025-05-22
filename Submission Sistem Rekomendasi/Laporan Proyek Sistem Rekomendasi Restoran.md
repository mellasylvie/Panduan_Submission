# Laporan Proyek Sistem Rekomendasi Restoran - Mellania Permata
---
## Project Overview

Dalam era digital saat ini, informasi mengenai berbagai pilihan restoran tersedia dalam jumlah yang sangat besar, baik melalui aplikasi mobile, situs web, maupun media sosial. Meskipun hal ini memberikan banyak alternatif bagi pengguna, justru seringkali menimbulkan kebingungan dalam menentukan pilihan terbaik sesuai preferensi pribadi. Oleh karena itu, dibutuhkan suatu sistem yang mampu memberikan rekomendasi secara cerdas dan personal. Salah satu solusi yang efektif adalah penerapan sistem rekomendasi.

Dalam proyek ini, dibangun sebuah sistem rekomendasi restoran dengan menggabungkan dua pendekatan utama, yaitu Content-Based Filtering dan Collaborative Filtering. Pendekatan Content-Based Filtering berfokus pada karakteristik restoran seperti jenis masakan, lokasi, harga, atau rating, dan mencocokkannya dengan preferensi pengguna. Sementara itu, Collaborative Filtering menganalisis pola interaksi pengguna lain yang memiliki kesamaan dalam memberikan penilaian, untuk menemukan restoran yang mungkin juga disukai oleh pengguna target.

### Referensi

1. Alfian, R., & Nurhayati, N. (2023). Pemodelan sistem rekomendasi restoran berdasarkan preferensi rasa, harga, dan rating. Jurnal JEPIN (Jurnal Elektronik Pendidikan Informatika), 11(3), 146–153. https://jurnal.untan.ac.id/index.php/jepin/article/view/74008

2. Ramadhan, F., & Prasetya, I. (2023). Rest-Rec: Restaurant recommender system based on model-based collaborative filtering. International Journal of Intelligent Systems and Applications in Engineering (IJISAE), 11(2), 188–194. https://www.ijisae.org/index.php/IJISAE/article/view/6300

---

## Business Understanding

## Problem Statements
Berdasarkan kondisi yang telah diuraikan sebelumnya, perusahaan akan mengembangkan sebuah sistem rekomendasi restoran untuk menjawab permasalahan berikut.

- Berdasarkan data mengenai pengguna, bagaimana membuat sistem rekomendasi yang dipersonalisasi dengan teknik content-based filtering?
- Dengan data rating yang Anda miliki, bagaimana perusahaan dapat merekomendasikan restoran lain yang mungkin disukai dan belum pernah dikunjungi oleh pengguna?

## Goals
Untuk menjawab pertanyaan di atas, maka tujuan atau goals sebagai berikut:

- Menghasilkan sejumlah rekomendasi restoran yang dipersonalisasi untuk pengguna dengan teknik content-based filtering.
- Menghasilkan sejumlah rekomendasi restoran yang sesuai dengan preferensi pengguna dan belum pernah dikunjungi sebelumnya dengan teknik collaborative filtering.

## Solution Statement

Solusi yang digunakan untuk menjawab permasalahan di atas, yaitu :

- Menggunakan pendekatan Content-Based filtering untuk membuat sistem rekomendasi berdasarkan jenis masakan
- Menggunakan pendekatan Collaborative Filtering untuk menemukan pola kesamaan di antara pengguna lain yang memiliki selera serupa, guna merekomendasikan restoran yang mungkin juga disukai oleh pengguna tersebut.

---

## Data Understanding

### Deskripsi Data

Dataset yang digunakan bernama Restaurant Data with Consumer Ratings. Data ini memiliki 9 file terpisah mengenai restoran, konsumen, dan rating.

### Link Download Dataset

Dataset ini dapat diunduh melalui : [Kaggle](https://www.kaggle.com/datasets/uciml/restaurant-data-with-consumer-ratings)

### Uraian Variabel Dataset

Variabel-variabel pada Restaurant UCI dataset adalah sebagai berikut:

- accepts : merupakan jenis pembayaran yang diterima pada restoran tertentu.
- cuisine : merupakan jenis masakan yang disajikan pada restoran.
- hours : merupakan jadwal buka dan tutupnya restoran.
- parking : merupakan ketersediaan tempat parkir pada restoran.
- geo : merupakan letak restoran.
- usercuisine : merupakan jenis masakan dari data pengguna.
- payment : merupakan jenis pembayaran yang dipakai pengguna.
- profile : merupakan data profil pengguna

---
## Data Preparation

### Data Preparation (Content-Based Filtering)

1. Menggabungkan Restoran

   Menggabungkan seluruh placeID pada kategori Restaurant dengan menggunakan np.concatenate(). Data yang digabungkan meliputi accepts, cuisine, hours, parking, geo. Kemudian mengurutkan data dan menghapus data yang sama.

2. Menggabungkan Seluruh User

   Menggabungkan seluruh userID dengan np.concatenate(). Data yang digabungkan meliputi usercuisine, payment dan profile. Kemudian mengurutkan data dan menghapus data yang sama.

3. Mengetahui Jumlah Rating

   Untuk mengetahui jumlah seluruh rating dari berbagai file maka digabungkan file accepts, geo, parking, hours ke dalam dataframe resto_info. Kemudian Menggabungkan dataframe rating dengan resto_info berdasarkan nilai placeID.

4. Menggabungkan Data dengan Fitur Nama Resto

   Mendefinisikan dataframe rating ke dalam variabel all_resto_rate. Menggabungkan all resto_rate dengan dataframe geo berdasarkan placeID.

5. Menggabungkan Data dengan Fitur masakan Resto

   Menggabungkan dataframe cuisine dengan all_resto_name dan memasukkannya ke dalam variabel all_resto.

6. Mengatasi Missing Value

   Pada dataframe all_resto dilakukan penghapusan missing value dengan fungsi dropna()

7. Menyamakan Jenis masakan

   Mengurutkan resto berdasarkan PlaceID kemudian memasukkannya ke dalam variabel fix_resto, Mengecek kategori masakan pada nama restoran KFC, Mengubah nama kategori masakan ‘Game’ menjadi ‘American’. LaluMembuat variabel preparation yang berisi dataframe fix_resto kemudian mengurutkan berdasarkan placeID dan Membuang data duplikat pada variabel preparation.

8. Konversi Data Series menjadi List

   Dilakukan konversi data series ‘placeID’, ‘Name’, ‘Rcuisine’ menjadi dalam bentuk list. Kemudian membuat dictionary untuk data ‘resto_id’, ‘resto_name’, dan ‘cuisine’.

9. Ekstraksi Fitur dengan TF-IDF Vectorizer

   Dilakukan inisialisasi TfidfVectorizer, melakukan perhitungan idf pada data cuisine, dan Mapping array dari fitur index integer ke fitur nama. Melakukan fit lalu ditransformasikan ke bentuk matrix dan Mengubah vektor tf-idf dalam bentuk matriks dengan fungsi todense().

### Data Preparation (Collaborative Filtering)

1. Encoding Fitur User dan PlaceID

   Mengambil nilai unik dari masing-masing kolom userID dan placeID, kemudian mengonversinya ke bentuk numerik menggunakan teknik enumerasi.

2. Memetakan userID dan placeID

   Mapping userID ke dataframe user dan Mapping placeID ke dataframe resto dengan menggunakan fungsi map()

3. Mengubah rating menjadi nilai float

   Mengecek beberapa hal dalam data seperti jumlah user, jumlah resto, kemudian mengubah nilai rating menjadi float.

4. Pembagian Data
    
    Dataset dibagi menjadi 80% data training dan 20% data testing. Kemudian data ini akan diacak.

--- 
## Modeling and Result

### Pendekatan Content-Based Filtering
Pendekatan Content-Based Filtering dalam sistem ini bertujuan untuk merekomendasikan restoran yang memiliki karakteristik serupa dengan restoran yang telah disukai pengguna sebelumnya. Sistem membandingkan restoran satu dengan yang lain berdasarkan atribut yang dimiliki, seperti nama restoran dan jenis masakan (cuisine), menggunakan metrik kesamaan Cosine Similarity.

Pertama, nilai cosine similarity dihitung untuk setiap pasangan restoran, dan hasilnya disimpan dalam bentuk matriks simetri dua dimensi. Matriks ini dikonversi menjadi sebuah DataFrame bernama cosine_sim_df, di mana baris dan kolom merepresentasikan nama-nama restoran. Setiap nilai dalam matriks menunjukkan tingkat kemiripan antara dua restoran—semakin tinggi nilainya, semakin mirip kedua restoran tersebut berdasarkan fitur yang dianalisis.

Untuk menghasilkan rekomendasi, digunakan fungsi resto_recommendations() yang menerima nama restoran sebagai input. Fungsi ini akan mencari restoran lain dengan nilai kemiripan tertinggi berdasarkan matriks cosine similarity, kemudian mengembalikan sejumlah restoran teratas (top-k) sebagai rekomendasi. Fungsi ini juga mengecualikan restoran yang sedang dicari agar tidak muncul dalam daftar hasil.

#### Hasil Rekomendasi Content-Based Filtering
Berikut hasil rekomendasinya :
| resto_name         | cuisine      |
|--------------------|--------------|
| VIPS               | American     |
| tacos los volcanes | American     |
| Pizzeria Julios    | American     |
| Sirlone            | International|
| McDonalds Centro   | American     |

### Pendekatan Collaborative Filtering
Untuk membangun sistem rekomendasi berbasis collaborative filtering, digunakan pendekatan pembelajaran mendalam (deep learning) dengan membangun model kustom bernama RecommenderNet menggunakan arsitektur dari tf.keras.Model. Tujuan utama dari model ini adalah mempelajari representasi laten (latent features) dari pengguna dan restoran untuk memprediksi preferensi pengguna terhadap restoran tertentu.

#### Arsitektur Model

Model RecommenderNet terdiri dari beberapa komponen utama:

- User Embedding: Layer embedding user_embedding memetakan setiap userID ke dalam representasi vektor berdimensi 50 (dalam kasus ini). Layer ini dilatih untuk menangkap preferensi pengguna.

- User Bias: Layer user_bias memberikan bobot bias untuk setiap pengguna, yang membantu menangkap kecenderungan umum seorang pengguna dalam memberikan rating tinggi atau rendah.

- Restaurant Embedding: Layer resto_embedding melakukan hal yang sama seperti user embedding, tetapi untuk restoran (placeID). Layer ini menangkap karakteristik laten restoran.

- Restaurant Bias: Layer resto_bias memberikan bias spesifik untuk tiap restoran.

- Dot Product: Vektor embedding pengguna dan restoran dikalikan secara dot product untuk menghasilkan skor prediksi awal, lalu dijumlahkan dengan bias pengguna dan restoran.

- Activation: Output dari model melewati fungsi aktivasi sigmoid untuk menormalkan nilai antara 0 dan 1, cocok untuk memprediksi probabilitas ketertarikan.

#### Parameter Model
- num_users: jumlah total pengguna unik dalam dataset.

- num_resto: jumlah total restoran unik.

- embedding_size: jumlah dimensi representasi laten yang digunakan untuk user dan resto embeddings (dalam hal ini 50).

- embeddings_initializer: bobot awal menggunakan he_normal, yang cocok untuk aktivasi non-linear.

- embeddings_regularizer: regulasi L2 dengan nilai 1e-6 digunakan untuk mencegah overfitting.

#### Proses Training
Model dikompilasi menggunakan:

- Loss Function: BinaryCrossentropy, karena target berupa skor yang telah dinormalisasi antara 0 dan 1.

- Optimizer: Adam dengan learning rate 0.001, yang efisien untuk training deep learning model.

- Metric: RootMeanSquaredError digunakan untuk mengevaluasi seberapa jauh prediksi model dari nilai aktual.

Model dilatih selama 100 epoch dengan ukuran batch 8, menggunakan data training x_train, y_train, serta data validasi x_val, y_val.

#### Hasil Rekomendasi Collaborative Filtering

##### Resto with high ratings from user `U1045`

| Resto Name                      | Cuisine         |
|--------------------------------|-----------------|
| puesto de tacos                | Mexican         |
| La Cantina Restaurante         | Bar_Pub_Brewery |
| Restaurante Marisco Sam        | Seafood         |
| Tortas Locas Hipocampo         | Fast_Food       |


##### Top 10 Resto Recommendation

| Resto Name                      | Cuisine         |
|--------------------------------|-----------------|
| La Estrella de Dimas           | Mexican         |
| La Posada del Virrey           | International   |
| cafe punta del cielo           | Cafeteria       |
| emilianos                      | Bar_Pub_Brewery |
| Restaurant Las Mananitas       | International   |
| Kiku Cuernavaca                | Japanese        |
| El Oceano Dorado               | Mexican         |
| El Rincon de San Francisco     | Mexican         |
| Preambulo Wifi Zone Cafe       | Cafeteria       |
| Michiko Restaurant Japones     | Japanese        |

---
## Evaluasi

### Evaluasi Content-Based Filtering
Precision adalah metrik evaluasi yang digunakan untuk mengukur seberapa relevan item yang direkomendasikan oleh sistem. Dalam konteks sistem rekomendasi, precision dihitung dengan membandingkan jumlah item yang relevan dengan total item yang direkomendasikan. 

Berdasarkan hasil rekomendasi content-based filtering yang ditampilkan, sistem merekomendasikan lima restoran, di mana empat di antaranya memiliki kategori "American" yang sesuai dengan preferensi pengguna (misalnya karena pengguna menyukai restoran KFC yang juga berjenis "American"). Dengan demikian, precision dapat dihitung sebagai 4 dibagi 5, yaitu 0.8 atau 80%. Ini menunjukkan bahwa 80% dari rekomendasi yang diberikan sistem relevan dengan selera pengguna.
### Evaluasi Collaborative

Nilai Root Mean Squared Error (RMSE) merupakan ukuran yang digunakan untuk mengukur tingkat kesalahan rata-rata antara nilai prediksi model dengan nilai sebenarnya. Pada hasil yang diberikan, RMSE pada data training sebesar 0,2385 menunjukkan bahwa secara rata-rata, prediksi model memiliki kesalahan sebesar 0,2385 terhadap nilai asli di data training. Sedangkan pada data validasi, RMSE sebesar 0,3343 menunjukkan bahwa kesalahan rata-rata prediksi pada data yang belum pernah dilihat oleh model sedikit lebih besar dibandingkan data training, yaitu sekitar 0,3343. Perbedaan nilai RMSE antara data training dan validasi ini menunjukkan bahwa model memiliki performa yang baik dan cukup stabil, meskipun terdapat sedikit peningkatan kesalahan saat menghadapi data baru. Secara umum, semakin kecil nilai RMSE, maka model dianggap semakin akurat dalam melakukan prediksi. Oleh karena itu, hasil ini menandakan bahwa model yang dibuat cukup efektif dalam memprediksi data, walaupun masih ada ruang untuk peningkatan agar kesalahan pada data validasi bisa lebih mendekati kesalahan pada data training.

