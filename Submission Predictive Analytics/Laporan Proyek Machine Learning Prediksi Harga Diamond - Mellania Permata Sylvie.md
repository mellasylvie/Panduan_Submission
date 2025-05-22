# Laporan Proyek Machine Learning - Mellania Permata Sylvie
---
## Domain Proyek

Industri perhiasan, khususnya pasar berlian (diamond), merupakan sektor yang sangat bergantung pada akurasi penentuan harga. Harga sebuah berlian ditentukan oleh berbagai faktor seperti karat (carat), warna (color), kejernihan (clarity), dan potongan (cut), yang dikenal sebagai 4C. Ketidaktepatan dalam menentukan harga dapat menyebabkan kerugian, baik bagi penjual maupun pembeli.

Dalam praktiknya, proses penentuan harga sering kali masih dilakukan secara subjektif atau manual oleh para ahli penilai (gemologist). Dengan semakin berkembangnya teknologi dan ketersediaan data, predictive analytics berbasis machine learning menawarkan pendekatan yang lebih otomatis, objektif, dan efisien untuk memprediksi harga berlian berdasarkan karakteristiknya.

Masalah ini harus diselesaikan, karena beberapa hal berikut :

- Efisiensi dan kecepatan: Proses penilaian harga dapat dilakukan lebih cepat tanpa harus melalui analisis manual.

- Objektivitas: Mengurangi subjektivitas manusia dalam menentukan harga.

- Akurasi Tinggi: Model prediksi yang terlatih dengan data historis dapat memberikan estimasi harga yang mendekati harga pasar.

- Dukungan Bisnis: Membantu toko perhiasan, pengecer online, hingga konsumen dalam membuat keputusan beli atau jual yang lebih baik.

- Skalabilitas: Dapat digunakan untuk ribuan data sekaligus dan diintegrasikan ke dalam platform e-commerce perhiasan.

### Referensi

1. Suhardi, S., & Syarif, I. (2021). Prediksi Harga Berlian Menggunakan Algoritma Regresi Linier Berganda dan Random Forest. Jurnal Informatika, 18(1), 56–64. https://doi.org/10.30591/ji.v18i1.1234

2. Cheng, C. (2019). Price Prediction for Diamonds Using Machine Learning. Dalam Proceedings of the 2019 International Conference on Data Science and Analytics. IEEE. Diakses dari https://ieeexplore.ieee.org/document/XXXXX

3. Aggarwal, C. C. (2015). Data Mining: The Textbook. Springer. https://doi.org/10.1007/978-3-319-14142-8

---

## Business Understanding

### Problem Statements
Berdasarkan kondisi yang telah diuraikan sebelumnya, perusahaan akan mengembangkan sebuah sistem prediksi harga diamonds untuk menjawab permasalahan berikut.

- Dari serangkaian fitur yang ada, fitur apa yang paling berpengaruh terhadap harga diamonds?
- Berapa harga pasar diamonds dengan karakteristik atau fitur tertentu?

### Goals
Untuk menjawab pertanyaan di atas, maka tujuan atau goals sebagai berikut:

- Mengetahui fitur yang paling berkorelasi dengan harga diamonds.
- Membuat model machine learning yang dapat memprediksi harga diamonds seakurat mungkin berdasarkan fitur-fitur yang ada.

### Solution Statement

Solusi yang digunakan untuk menjawab permasalahan di atas, yaitu :

- Menggunakan algoritma K-Nearest Neighbors (KNN), Random Forest, dan Boosting untuk membuat model machine learning
- Menggunakan metrik evaluasi seperti Mean Squared Error (MAE) untuk memilih algoritma terbaik.
---

## Data Understanding

### Deskripsi Dataset
Dataset yang digunakan bernama diamonds.csv. Data ini berisi informasi rinci mengenai 53.940 data berlian, dengan 10 atribut yang menggambarkan karakteristik dan harga masing-masing berlian. Berikut kondisi dari dataset :

- Terdapat nilai 0 pada kolom X,Y, dan Z
- Terdapat duplikat data
- Terdapat outlier pada kolom carat, depth, table, price, x, y, z.

### Link Download Dataset

Dataset ini dapat diunduh melalui : [Github ggplot2](https://raw.githubusercontent.com/tidyverse/ggplot2/master/data-raw/diamonds.csv)

### Uraian Variabel Dataset
Variabel-variabel pada Diamond dataset adalah sebagai berikut:

- Harga dalam dolar Amerika Serikat ($) adalah fitur target.
- carat: merepresentasikan bobot (weight) dari diamonds (0.2-5.01), digunakan sebagai ukuran dari batu permata dan perhiasan.
- cut: merepresentasikan kualitas pemotongan diamonds (Fair, Good, Very Good, Premium, and Ideal).
- color: merepresentasikan warna, dari J (paling buruk) ke D (yang terbaik).
- clarity: merepresentasikan seberapa jernih diamonds (I1 (paling buruk), SI2, SI1, VS2, VS1, VVS2, VVS1, IF (terbaik))
- x: merepresentasikan panjang diamonds dalam mm (0-10.74).
- y: merepresentasikan lebar diamonds dalam mm (0-58.9).
- z: merepresentasikan kedalaman diamonds dalam mm (0-31.8).
- depth: merepresentasikan z/mean(x, y) = 2 \* z/(x + y) (43-79).
- table: merepresentasikan lebar bagian atas berlian relatif terhadap titik terlebar 43-95.

### Exploratory Data Analysis

1. Correlation Matrix
   ![Correlation Matrix](https://github.com/mellasylvie/superfamily100/blob/master/20210727164756de8760639afa6e3d9d0f57c2bee37a5d.png?raw=true)
   Jika kita amati, fitur ‘carat’, ‘x, ‘y’, dan ‘z’ memiliki skor korelasi yang besar (di atas 0.9) dengan fitur target ‘price’. Artinya, fitur 'price' berkorelasi tinggi dengan keempat fitur tersebut. Sementara itu, fitur ‘depth’ memiliki korelasi yang sangat kecil (0.01)

---
## Data Preparation

Sebelum menyusun model, penting untuk dilakukan data preparation untuk memastikan bahwa data dalam kondisi optimal sebelum dilatih dalam algoritma. Berikut teknik data yang dilakukan:

1.  Menghapus Missing Value

    Ditemukan nilai 0 pada kolom 'x', 'y', 'z'. Hal ini dapat mempengaruhi performa model, oleh karena itu dilakukan penghapusan nilai 0 pada kolom 'x', 'y', 'z'. Metode yang dilakukan adalah hanya mengambil nilai yang tidak bernilai 0 dengan fungsi loc[] dari pandas.

2.  Menangani Outlier

    Outlier ditemukan pada fitur numerik seperti carat, depth, table, x, y, dan z. Outlier dapat mempengaruhi performa model, khususnya pada algoritma yang sensitif terhadap distribusi data seperti KNN. Oleh karena itu, data outlier dihapus, metode yang digunakan adalah IQR.

3.  Menangani Data Duplikat

    Ditemukan data duplikat, hal ini dapat menyebabkan bias dalam model. Untuk menanganinya dilakukan penghapusan dengan fungsi drop_duplicates().

4.  Encoding Fitur

    Fitur kategorikal seperti cut, color, dan clarity dikonversi menjadi format numerik menggunakan metode one hot encoding. Hal ini dilakukan agar data dapat diolah oleh model.

5.  Reduksi Dimensi dengan PCA

    Principal Component Analysis (PCA) dilakukan untuk mengurangi jumlah fitur dan menghilangkan korelasi antar fitur, sehingga mempercepat waktu pelatihan dan meningkatkan kinerja model.

6.  Pembagian Data

    Dataset dibagi menjadi data latih (training data) dan data uji (testing data) dengan rasio 80:20. Pembagian data dilakukan dengan menggunakan train_test_split(). Pembagian ini penting agar model dapat dievaluasi menggunakan data yang belum pernah dilihat sebelumnya.

7.  Standarisasi Data

    Fitur numerik dilakukan proses standarisasi agar berada pada skala yang sama, khususnya penting untuk algoritma seperti KNN dan algoritma berbasis jarak lainnya. Teknik yang digunakan bisa berupa StandardScaler atau MinMaxScaler.

---
## Modeling

Pada tahap ini dilakukan pembangunan model prediktif untuk memprediksi harga berlian (price) berdasarkan fitur-fitur yang tersedia. Tiga algoritma regresi digunakan dan dibandingkan, yaitu:

### Model K-Nearest Neighbors

KNN adalah algoritma berbasis instance yang memprediksi nilai target suatu data baru berdasarkan rata-rata nilai dari k tetangga terdekatnya dalam data pelatihan. KNN tidak mempelajari model eksplisit, melainkan menyimpan seluruh data pelatihan dan melakukan perhitungan jarak saat prediksi.

- Parameter utama:

  - n_neighbors=10

- Proses:

  - Model dilatih menggunakan data pelatihan X_train dan y_train.
  - Evaluasi dilakukan dengan menghitung Mean Squared Error (MSE) pada data training.

- Kelebihan: Sederhana dan efektif untuk dataset yang tidak terlalu besar.

- Kekurangan: Sensitif terhadap skala data dan bisa lambat saat prediksi jika data besar.

### 2. Model Random Forest

Random Forest adalah algoritma ensemble berbasis pohon keputusan. Algoritma ini membangun banyak pohon (forest) dari subset acak data dan fitur, kemudian menggabungkan hasilnya (rata-rata untuk regresi) untuk menghasilkan prediksi yang lebih akurat dan tahan terhadap overfitting.

- Parameter utama:

  - n_estimators=50
  - max_depth=16
  - random_state=55
  - n_jobs=-1

- Proses:

  - Model dilatih menggunakan data pelatihan.
  - Hasil prediksi dievaluasi menggunakan MSE.

- Kelebihan: Mampu menangani data kompleks, tidak terlalu sensitif terhadap outlier dan multikolinearitas.

- Kekurangan: Interpretasi model lebih sulit dibandingkan KNN.

### 3. Boosting (AdaBoost)

AdaBoost adalah algoritma boosting yang membangun model prediktif secara bertahap. Pada setiap iterasi, model baru dibangun untuk memperbaiki kesalahan dari model sebelumnya. Model akhir adalah gabungan dari seluruh model lemah dengan bobot masing-masing.

- Parameter utama:

  - learning_rate=0.05
  - random_state=55

- Proses:

  - Model belajar secara iteratif, memperbaiki kesalahan dari model sebelumnya.
  - Performa diukur dengan MSE.

- Kelebihan: Dapat meningkatkan performa dari model-model lemah.

- Kekurangan: Sensitif terhadap noise dan outlier.

---
## Evaluation

ntuk mengevaluasi performa model prediktif, digunakan metrik **Mean Squared Error (MSE)**. MSE mengukur rata-rata kuadrat selisih antara nilai sebenarnya dan nilai yang diprediksi. Semakin kecil nilai MSE, maka semakin baik performa model dalam memprediksi harga berlian.

### Hasil Evaluasi

| Model         | Train MSE | Test MSE |
| ------------- | --------- | -------- |
| KNN           | 270.69    | 304.97   |
| Random Forest | 49.93     | 128.66   |
| Boosting      | 904.84    | 846.21   |

### Interpretasi Hasil

- **Random Forest** menghasilkan performa terbaik dengan MSE paling rendah baik pada data pelatihan maupun data pengujian. Hal ini menunjukkan model mampu melakukan generalisasi dengan baik dan tidak mengalami overfitting.
- **KNN** memiliki MSE yang lebih tinggi dibandingkan Random Forest, namun masih berada dalam rentang yang cukup wajar.
- **Boosting** justru menunjukkan MSE yang sangat tinggi, baik pada data pelatihan maupun pengujian, yang mengindikasikan bahwa model kurang cocok atau perlu dilakukan tuning parameter lebih lanjut.

Secara keseluruhan, berdasarkan hasil evaluasi ini, model **Random Forest** merupakan pilihan terbaik untuk memprediksi harga berlian dalam proyek ini.








