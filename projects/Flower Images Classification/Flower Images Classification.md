# **CNN : Classifying Flower Images Using Transfer Learning with MobileNetV2 Architecture - Maulana Zulfikar Aziz**

![](https://github.com/Maulanaaz/ML-Project/blob/main/Flower%20Images%20Classification/Images/ML%20PROJECT.png?raw=true)

[File Notebook](https://github.com/Maulanaaz/ML-Project/blob/main/Flower%20Images%20Classification/Flower_Image_Classifications.ipynb)


## **Project Overview**
Bunga merupakan salah satu bagian dari tanaman yang berupa modifikasi suatu tunas (batang dan daun) dimana bentuk, warna, dan susunannya disesuaikan dengan kepentingan tumbuhan, salah satu fungsinya yaitu sebagai tempat terjadinya peristiwa penyerbukan dan pembuahan yang nantinya akan menghasilkan buah [1].

Saat kita berada di lingkungan yang penuh dengan pepohonan dan tanaman, seringkali kita melihat suatu bunga akan tetapi kita tidak mengetahui nama dari bunga tersebut. Salah satu cara untuk menyelesaikan permasalahan ini adalah dengan membuat suatu model Machine Learning yang bisa digunakan untuk mengklasifikasikan nama bunga berdasarkan bentuknya yang nantinya bisa di-deploy ke dalam suatu device.

Dalam project ini, saya menggunakan metode Transfer Learning dengan arsitektur MobileNetV2 untuk menyelesaikan permasalahan klasifikasi 14 jenis bunga.
## **Project Understanding**
### Problem Statements

1. Bagaimana cara membangun model Machine Learning Convolutional Neural Network untuk mengklasifikasikan jenis bunga berdasarkan gambarnya?

### Goals

1. Membangun model Machine Learning Convolutional Neural Network untuk mengklasifikasikan jenis bunga berdasarkan gambarnya.

### Solution Statements
Untuk membuat sebuah model Convolutional Neural Network dengan bantuan metode Transfer Learning menggunakan arsitektur MobileNetV2, kita perlu melakukan beberapa tahapan, yaitu :    

1. Data Understanding
   Tahapan ini mencakup penjelasan awal tentang data, data loading, data exploration, dan data visualization

2. Data Preparation
   Sebelum melalui tahap pemodelan, data perlu disiapkan terlebih dahulu. Persiapan yang perlu dilakukan adalah normalisasi nilai pixel pada gambar agar berada di rentang 0 sampai 1.

3. Modelling & Result
   Pada tahap ini, kita membuat sebuah model CNN dengan metode Transfer Learning menggunakan arsitektur MobileNetV2 lalu kita fit dengan menggunakan data train yang telah kita prepare sebelumnya.

4. Evaluation
   Model dievaluasi dengan menggunakan data test untuk mengetahui performa model jika diterapkan terhadap data yang belum pernah dilihat oleh model.

5. Save the Model
   Model yang telah dibuat, disimpan dengan format TF-Lite agar nantinya bisa digunakan untuk proses deployment ke mobile device.

## **Data Understanding**

### About Data
Data yang digunakan pada project ini merupakan data kumpulan 14 jenis gambar bunga yang dapat diunduh di : https://www.kaggle.com/datasets/amananandrai/ag-news-classification-dataset

Data ini mencakup 2 folder, yaitu `train` dan `val`. Total gambar yang dapat digunakan untuk proses training adalah 13618 buah gambar dengan total gambar validasi sebesar 98 buah gambar.

Adapun jenis-jenis bunga dalam data ini mencakup : carnation, iris, bluebells, golden english, roses, fallen nephews, tulips, marigolds, dandelions, chrysanthemums, black-eyed daisies, water lilies, sunflowers, dan daisies.
### Data Exploration

![](https://github.com/Maulanaaz/ML-Project/blob/main/Flower%20Images%20Classification/Images/2_explor_1.png?raw=true)

Terlihat bahwa terdapat 14 jenis kelas dalam data. Ini berarti dalam project ini, akan dibangun sebuah model untuk mengklasifikan 14 jenis bunga yang berbeda. 

![](https://github.com/Maulanaaz/ML-Project/blob/main/Flower%20Images%20Classification/Images/2_explor_2.png?raw=true)

Gambar diatas menunjukkan jumlah gambar yang ada di dalam folder `train` dari masing-masing kelas dan total gambar train keseluruhan yaitu sebanyak 13642 buah gambar.

![](https://github.com/Maulanaaz/ML-Project/blob/main/Flower%20Images%20Classification/Images/2_explor_3.png?raw=true)

Gambar diatas menunjukkan jumlah gambar dari folder `val` dari masing-masing kelas dan total gambar train keseluruhan yaitu sebanyak 98 buah gambar. Terlihat bahwa masing-masing kelas memiliki jumlah gambar yang sama, yaitu sebanyak 7 buah gambar.

### Data Visualization

Sebelum melanjutkan ke tahap pemodelan, alangkah lebih baiknya kita mengetahui terlebih dahulu beberapa informasi tentang data kita melalui visualisasi.

![](https://github.com/Maulanaaz/ML-Project/blob/main/Flower%20Images%20Classification/Images/2_explor_5.png?raw=true)

Gambar diatas merupakan salah satu gambar yang terdapat pada folder `train`. Dari gambar diatas, kita dapat mengetahui bahwa gambar pada kita berukuran 256 x 256. 

Sekarang kita tahu mengenai dimensi dari gambar pada data kita. Sekarang akan dilihat 9 sampel gambar dalam data kita, untuk mencari tahu bagaimana sudut pemotretan dari masing-masing gambar.

![](https://github.com/Maulanaaz/ML-Project/blob/main/Flower%20Images%20Classification/Images/2_explor_6.png?raw=true)

Ternyata sudut pemotretan dalam gambar pada data kita tidak seragam, artinya beberapa gambar ada yang dipotret dari jarak jauh dan ada yang dipotret dari jarak dekat, adapula yang dipotret dari samping dan dari atas. Oleh karena itu, pada tahap **Data Preparation** tidak akan dilakukan proses data augmentation dengan mengubah-ubah bentuk dari gambar.
## **Data Preparation**

Setelah mengetahui informasi mengenai gambar pada data kita, saatnya kita _prepare_ data kita untuk keperluan pemodelan. Dalam tahap ini, hanya akan dilakukan rescaling, yaitu menormalisasi nilai pixel pada gambar agar berada pada rentang 0 sampai 1, berikut adalah hasilnya :

![](https://github.com/Maulanaaz/ML-Project/blob/main/Flower%20Images%20Classification/Images/2_explor_7.png?raw=true)
![](https://github.com/Maulanaaz/ML-Project/blob/main/Flower%20Images%20Classification/Images/2_explor_8.png?raw=true)
Terlihat bahwa gambar train kita berjumlah 13642 buah gambar dan gambar val kita berjumlah 98 gambar, hal ini sama dengan yang telah didapatkan pada bagian **Data Exploration**. Dalam project ini, _batch size_ yang digunakan adalah 32.

## **Modelling**

MobileNetV2 merupakan salah satu arsitektur CNN yang memiliki performa baik di _mobile device_. MobileNetV2 berbasis pada struktur residual terbalik dimana koneksi residual berada di antara lapisan-lapisan _bottleneck_. Lapisan ekspansi intermediate menggunakan konvolusi depthwise ringan untuk menyaring fitur sebagai sumber non-linieritas. Secara keseluruhan, arsitektur MobileNetV2 berisi lapisan _initial fully convolutional_ layer dengan 32 filter, diikuti oleh 19 lapisan _bottleneck_ residual. [2]

Berikut adalah model yang dibangun pada project ini.
![](https://github.com/Maulanaaz/ML-Project/blob/main/Flower%20Images%20Classification/Images/2_explor_9.png?raw=true)

Setelah model dibangun, tahap selanjutnya adalah mendefinisikan _Callbacks_. Adapun _Callbacks_ yang digunakan dalam project ini adalah _Early Stopping_, _Checkpoint_, dan _Reduce Learning Rate on Plateau_.

Loss yang akan digunakan pada model ini adalah _Categorical Crossentrophy_ dengan _Optimizer_ Adam dan _Metrics_ _Accuracy_.

Setelah semua persiapan selesai, waktunya _fitting_ model menggunakan data train. Dan didapatkan model dengan _weight_ terbaik terdapat pada epoch ke-12.

![](https://github.com/Maulanaaz/ML-Project/blob/main/Flower%20Images%20Classification/Images/2_explor_10.png?raw=true)

## **Evaluation**

Setelah model berhasil dibuat, tahap selanjutnya adalah mengevaluasi proses konvergensi model dengan menggunakan visualisasi _accuracy_ plot dan _loss_ plot.

![](https://github.com/Maulanaaz/ML-Project/blob/main/Flower%20Images%20Classification/Images/2_explor_11.png?raw=true)
![](https://github.com/Maulanaaz/ML-Project/blob/main/Flower%20Images%20Classification/Images/2_explor_12.png?raw=true)

Terlihat bahwa semakin meningkat epoch, semakin besar akurasi dari data train dan semakin kecil juga _loss_ dari data train. Akan tetapi tidak begitu dengan data val, akurasi dari data val tidak selalu meningkat dan _loss_ dari data val juga tidak selalu menurun seiring dengan pertambahan epoch. Inilah kegunaan dari _Callbacks_ yang telah didefinisikan tadi, yaitu untuk menghindarkan model dari keadaan _Overfitting_.

## **Save the Model**

Setelah model dibangun, ini saatnya model disimpan dalam format TF-Lite, agar nantinya model klasifikasi bunga yang telah dibuat bisa di-_deploy_ ke dalam _mobile device_.

![](https://github.com/Maulanaaz/ML-Project/blob/main/Flower%20Images%20Classification/Images/2_explor_13.png?raw=true)
![](https://github.com/Maulanaaz/ML-Project/blob/main/Flower%20Images%20Classification/Images/2_explor_14.png?raw=true)

## **Conclusion**

Model yang telah dibuat menggunakan metode Transfer Learning dengan arsitektur MobileNetV2 menghasilkan akurasi pada data train sebesar 90.02% dan akurasi pada data validasi sebesar 93.88%. Akurasi yang lebih besar mungkin bisa didapatkan dengan menggunakan arsitektur yang lain atau dengan proses hyperparameter tuning pada arsitektur MobileNetV2. Model ini dapat digunakan untuk mengklasifikan 14 jenis bunga dan dengan disimpan ke dalam format TF-Lite, model dapat di-deploy ke dalam _mobile device_.

## **Daftar Referensi**
[1] E. Palupi, Syafrizal and N. Hariani, "Studi Morfologi Polen Tanaman Pekarangan di Perumahan Gn. Dubbs Balikpapan," Bioprospek, pp. 16-21, 2018.

[2] A. G. Howard et al., “MobileNets: Efficient Convolutional Neural Networks for Mobile Vision Applications.” arXiv, 2017. doi: 10.48550/ARXIV.1704.04861.