# **NLP : Classifying AG News Using RNN with LSTM Architecture - Maulana Zulfikar Aziz**

![](https://github.com/Maulanaaz/ML-Project/blob/main/AG%20News%20Classification/Images/Cover_MD.jpg?raw=true "Picture : freepik.com")



## **Project Overview**
Berita dapat didefinisikan sebagai suatu informasi aktual yang menarik dan akurat serta dianggap penting bagi sejumlah besar pembaca, pendengar, maupun penonton [1]. Seiring dengan perkembangan digitalisasi, kita dapat dengan mudah memperoleh berita melalui berbagai platform online, seperti media sosial dan website. Banyaknya berita yang beredar di platform online kadangkala menyebabkan para pembaca kesulitan untuk mencari kategori berita yang sesuai dengan preferensi mereka. Ditambah isi dari suatu berita yang seringkali memuat lebih dari satu topik menjadikan suatu berita sulit untuk diklasifikasikan.

Klasifikasi teks merupakan salah satu bagian penting dalam ranah Natural Language Processing (NLP). Klasifikasi teks dapat diaplikasikan ke dalam berbagai bidang, seperti analisis sentimen, klasifikasi dokumen, kategorisasi teks dan penggalian informasi [2]. Salah satu pendekatan populer yang dapat digunakan untuk menyelesaikan permasalahan klasifikasi teks dalam NLP adalah Neural Network [2].

Dalam project ini, saya menggunakan Recurrent Neural Network dengan arsitektur LSTM (Long Short Term Memory) untuk menyelesaikan permasalahan klasifikasi berita dengan multi-kategori.

## **Business Understanding**
### Problem Statements

Bagaimana cara membuat sebuah model Natural Language Processing (NLP) menggunakan RNN dengan arsitektur LSTM untuk mengklasifikasikan jenis berita?

### Goals

Membuat sebuah model Natural Language Processing (NLP) menggunakan RNN dengan arsitektur LSTM untuk mengklasifikasikan jenis berita.

### Solution Approach

Untuk membuat sebuah model Natural Language Processing (NLP) menggunakan RNN dengan arsitektur LSTM, kita perlu melakukan beberapa tahapan, yaitu :    

1. Data Understanding

   Tahapan ini mencakup penjelasan awal tentang data, data loading, dan Exploratory Data Analysis (EDA).
  
2. Data Cleaning

   Setelah dilakukan proses eksplorasi data, tahap selanjutnya adalah membersihkan data yaitu menghilangkan noise pada data.

3. Data Preparation

   Sebelum melalui tahap pemodelan, data perlu disiapkan terlebih dahulu. Persiapan yang perlu dilakukan adalah membagi dataset menjadi data train dan data validasi, melakukan One-Hot Encoding pada data, melakukan tokenisasi, dan melakukan padding.

4. Modelling & Result

   Pada tahap ini, kita membuat sebuah model RNN dengan arsitektur LSTM lalu kita fit dengan menggunakan data train yang telah kita prepare sebelumnya.

5. Evaluation

   Model dievaluasi dengan menggunakan data test untuk mengetahui performa model jika diterapkan terhadap data yang belum pernah dilihat oleh model.
## **Data Understanding**

### About Data

Data yang digunakan pada project ini merupakan data AG News Classification Dataset yang dapat diunduh di : https://www.kaggle.com/datasets/amananandrai/ag-news-classification-dataset

AG adalah sekumpulan artikel berita yang berjumlah lebih dari 1 juta. Artikel berita tersebut telah dikumpulkan dari 2000 lebih sumber oleh ComeToMyHead dalam waktu lebih dari 1 tahun. ComeToMyHead adalah mesin pencari berita akademik yang telah berdiri sejak bulan Juli 2004.

Adapun kolom-kolom yang terdapat dalam dataset tersebut yaitu :     

`Class Index` :  Klasifikasi berita, dikategorikan menjadi 4 macam (1 = World, 2 = Sport, 3 = Business, 4 = Sci/Tech)

`Title` : Judul artikel berita

`Description` : Deskripsi artikel berita

Data training berukuran 120.000 baris dengan masing-masing kelas memiliki 40.000 baris. Sedangkan data test berukuran 7600 baris dengan masing-masing kelas memiliki 1900 baris.

## **Exploratory Data Analysis**
### General Information

![](https://github.com/Maulanaaz/ML-Project/blob/main/AG%20News%20Classification/Images/data_head.png?raw=true)

![](https://github.com/Maulanaaz/ML-Project/blob/main/AG%20News%20Classification/Images/Class_unique.png?raw=true)

Terlihat bahwa dataset memiliki 3 kolom, yaitu `Class Index`, `Title`, dan `Description`. Untuk kolom `Class Index` diganti namanya menjadi `Class` untuk efisiensi nama. Terlihat bahwa kelas dalam kolom target berjumlah 4.

### Checking Distribution

Sebelum melangkah lebih lanjut, sebaiknya kita harus tahu terlebih dahulu bagaimana bentuk distribusi setiap kelasnya, untuk mengecek apakah terjadi kasus distribusi kelas yang tidak seimbang.

![](https://github.com/Maulanaaz/ML-Project/blob/main/AG%20News%20Classification/Images/dist_class.png?raw=true)

Terlihat bahwa distribusi antar kelas sudah seimbang, jadi tidak perlu dilakukan upsampling/downsampling.

Selanjutnya, akan dicek distribusi jumlah kata untuk setiap item di dalam kolom `Description` berdasarkan kolom `Class`

![](https://github.com/Maulanaaz/ML-Project/blob/main/AG%20News%20Classification/Images/dist_kata2.png?raw=true)
![](https://github.com/Maulanaaz/ML-Project/blob/main/AG%20News%20Classification/Images/dist_kata.png?raw=true)

Terlihat bahwa rata-rata jumlah kata per-kelas adalah sebanyak kurang lebih 200 kata.

Selanjutnya, akan dilakukan pengecekan 10 kata (tidak termasuk stopwords) yang paling sering muncul untuk setiap kelasnya.

![](https://github.com/Maulanaaz/ML-Project/blob/main/AG%20News%20Classification/Images/cek_kata1.png?raw=true)
![](https://github.com/Maulanaaz/ML-Project/blob/main/AG%20News%20Classification/Images/cek_kata2.png?raw=true)
![](https://github.com/Maulanaaz/ML-Project/blob/main/AG%20News%20Classification/Images/cek_kata3.png?raw=true)
![](https://github.com/Maulanaaz/ML-Project/blob/main/AG%20News%20Classification/Images/cek_kata4.png?raw=true)

Terlihat ada banyak kata yang tidak diperlukan selain stopwords, yaitu tanda baca. Masalah ini akan diselesaikan pada bagian Data Cleaning.

## **Data Cleaning**
Setelah dilakukan proses eksplorasi data, tahap selanjutnya adalah membersihkan data. Pada kasus ini, Data Cleaning dilakukan dengan menghilangkan tanda baca dan stopwords pada kolom `Description`.

## **Data Preparation**
Dalam bagian ini, data akan diproses agar bisa digunakan untuk modelling. Persiapan yang perlu dilakukan adalah sebagai berikut :

1. Mengaplikasikan One-Hot Encoding pada data
   One-Hot Encoding adalah salah satu metode encoding yang merepresentasikan data bertipe kategori sebagai vektor biner yang bernilai integer, 0 dan 1, dimana semua elemen akan bernilai 0 kecuali satu elemen yang bernilai 1, yaitu elemen yang memiliki nilai kategori tersebut.

2. Membagi dataset menjadi data train dan data validasi
   Dataset dibagi menjadi dua bagian, yaitu data train dan data validasi dengan rasio 80:20.

3. Melakukan Tokenisasi
   Tokenisasi merupakan proses mengonversi kata-kata ke dalam bilangan numerik. Dalam project ini, parameter `num_words` diset menjadi 2000, artinya hanya 2000 kata yang muncul terbanyak yang akan ditokenisasi. 

4. Melakukan Padding
   Padding adalah proses untuk mengubah setiap sequence agar memiliki panjang yang sama. Dalam project ini, panjang maksimal untuk setiap sequence diset menjadi 150.

## **Modelling and Result**
Dalam project ini, saya menggunakan model RNN dengan arsitektur LSTM. RNN merupakan merupakan suatu metode dalam deep learning yang digunakan untuk memproses data sekuensial dengan pemanggilan berulang. RNN digunakan dengan arsitektur LSTM yang memiliki memory cells untuk dapat menyimpan informasi dengan jangka waktu yang panjang sehingga dapat mencegah terjadinya kasus vanishing gradient. Berikut adalah model Sequential yang dibangun :
![](https://github.com/Maulanaaz/ML-Project/blob/main/AG%20News%20Classification/Images/seq_model.png?raw=true)

Selain itu, saya juga menggunakan 3 jenis callbacks, yaitu Checkpoint, Early Stopping, dan Reduce Learning Rate on Plateau. Fungsi loss yang digunakan untuk train model adalah Categorical Crossentrophy, dengan optimizer Adaptive Moment Estimation (ADAM) dan matriks evaluasi Accuracy.

Model dilatih dengan `batch_size` 64 pada data training. Dari hasil training, didapatkan model terbaik diperoleh di Epoch 5 :
![](https://github.com/Maulanaaz/ML-Project/blob/main/AG%20News%20Classification/Images/Epochs5.png?raw=true)

## **Evaluation**
![](https://github.com/Maulanaaz/ML-Project/blob/main/AG%20News%20Classification/Images/eval.png?raw=true)

Terlihat bahwa model yang dibuat menghasilkan akurasi sekitar 88% pada data validasi.

### Plotting Accuracy and Loss of Model
![](https://github.com/Maulanaaz/ML-Project/blob/main/AG%20News%20Classification/Images/plot_akurasi.png?raw=true)
Dari grafik, terlihat bahwa semakin banyak epochs yang dijalankan, akurasi model pada data train meningkat akan tetapi akurasi pada data validasi menurun. Hal ini mengindikasikan adanya Overfitting pada model, callback Checkpoint telah membantu untuk mengambil model yang terbaik, yaitu pada Epoch ke-5.

![](https://github.com/Maulanaaz/ML-Project/blob/main/AG%20News%20Classification/Images/plot_loss.png?raw=true)

Berdasarkan grafik, terlihat bahwa semakin banyak Epochs yang dijalankan, loss dari data train semakin turun dan loss dari data validasi semakin naik. Didapatkan kesimpulan yang sama, yaitu model mengalami Overfitting.

### Evaluation on Data Test
Sebelum dilakukan evaluasi model terhadap data test, terlebih dahulu dilakukan data preparation pada test yang mencakup One-Hot Encoding, Tokenization, dan Padding. Berikut tampilan dari data test yang sudah melalui tahap data preparation :
![](https://github.com/Maulanaaz/ML-Project/blob/main/AG%20News%20Classification/Images/data%20test%20overview.png?raw=true)

Berikut adalah hasil evaluasi model pada data test :
![](https://github.com/Maulanaaz/ML-Project/blob/main/AG%20News%20Classification/Images/eval_datatest.png?raw=true)
Model yang telah dibuat menghasilkan akurasi sebesar 87% pada data test.

## **Conclusion**
Sebuah model Natural Language Processing (NLP) menggunakan RNN dengan arsitektur LSTM untuk mengklasifikasikan jenis berita berhasil dibuat dengan akurasi sebesar 88% pada data validasi dan 87% pada data test. Besaran akurasi ini sudah cukup tinggi, akan tetapi akan lebih bagus lagi jika berada di atas 90%. Metode-metode lain terkait klasifikasi teks dalam NLP mungkin saja bisa digunakan untuk meningkatkan akurasi pada model.

## **References**
1. Alfando and R. Hayami, "Klasifikasi Teks Berbahasa Indonesia Menggunakan Machine Learning dan Deep Learning : Studi Literatur," Jurnal Mahasiswa Teknik Informatika, vol. 7, no. 1, pp. 681-686, 2023. 

2. W. K. Sari, D. P. Rini, R. F. Malik and I. S. B. Azhar, "Sequential Models for Text Classification Using Recurrent Neural Network," Advances in Intelligent Systems Research, vol. 172, pp. 333-340, 2020. 

3. http://groups.di.unipi.it/~gulli/AG_corpus_of_news_articles.html

4. Y. Fauziyah, R. Ilyaz and F. Kasyidi, "Mesin Penterjemah Bahasa Indonesia-Bahasa Sunda Menggunakan Recurrent Neural Networks," Jurnal Teknoninfo, vol. 16, no. 2, pp. 313-322, 2022. 

5. https://ilmudatapy.com/one-hot-encoding-di-python/
