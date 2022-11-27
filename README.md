# Capstone-Modul-3

# Problem Statement

Pada suatu perusahaan tingginya pelanggan yang melakukan *Churn* merupakan suatu indikator tingkat kegagalan suatu perusahaan telekomunikasi. Sehingga diperlukan upaya untuk mengurangi persentase pelanggan yang melakukan *Churn*. Seperti disebutkan sebelumnya bahwa untuk mencari pelanggan lebih dibutuhkan biaya yang lebih besar dibandingkan mempertahankan pelanggan dimana dibutuhkan **5 kali lebih besar** biaya dibandingkan mempertahankan pelanggan( [sumber](https://www.outboundengine.com/blog/customer-retention-marketing-vs-customer-acquisition-marketing/).). Menurut [sumber](https://www.woopra.com/blog/churn-rate-vs-retention-rate#:~:text=Customer%20churn%20rate%20is%20the,up%20and%20stay%20with%20you.) )apabila kita melakukan suatu *ads* untuk menarik pelanggan baru dgn biaya sebesar 100 dolar perbulan untuk mendapatkan pelanggan baru dan biaya perpelanggan perbulan sebesar 50 dolar, maka setidaknya setiap bulan harus ada 2 pelanggan untuk menutup biaya pelanggan baru tersebut. Salah satu cara perusahaan telekomunikasi mempertahankan pelanggannya agar tidak melakukan Churn yaitu memberikan insentif retensi terhadap pelanggan. Insentif retensi terdiri dari berbagai macam seperti memberikan potongan harga, memberikan paket layanan yang menarik, memberikan prioritas pelayanan dan lain-lain. Namun, kebijakan pemberian insentif retensi belum dilakukan secara efektif. Kebijakan tersebut sering ditemui berbagai kendala bahkan membuat
perusahaan semakin merugi. 

## Goals

Sehingga dari permasalahan yang ada, perusahaan ingin memiliki kemampuan untuk dilakukannya prediksi kemungkinan seorang pelanggan akan berhenti berlangganan atau tidak. Untuk memfokuskan upaya retensi(tetap berelanggan) pada pelanggan yang terindikasi akan melakukan *Churn*. Selain itu juga agar perusahaan mengetahui faktor yang memengaruhi pelanggan bertahan agar strategi bisnis yang dilakukan tepat dengan keinginan pelanggan untuk menurunkan tingkat dari pelanggan yang *Churn*.

## Analytics Approach

Akan dilakukan analisa bagaimana faktor pembeda dari pelanggan yang melakukan *Churn* dan tidak. selanjutnya akan dibuat model klasifikasi untuk memprediksi probabilitas dari pelanggan akan melakukan *Churn* atau tidak.

# Metric Evaluation

Target utama dalam masalah ini adalah pelanggan yang berhenti berlangganan (Churn), seperti target yang sudah disebutkan pada context sebelumnya yaitu:

Target:
* 0 : Tidak berhenti berlangganan 
* 1 : Berhenti berlangganan (Churn)

False Positive (FP) yaitu pelanggan yang aktualnya tidak tetapi diprediksi churn.
konsekuensi : tidak efektif dalam pemberian insentif. <br>
False negative(FN) yaitu pelanggan yang aktualnya churn tetapi diprediksi tidak akan churn. konsekuensi kehilangan pelanggan.

Berdasarkan konsekuensi yang ada, akan dibuat model yang akan mengurangi resiko kehilangan pelanggan karena untuk mendapatkan pelanggan baru membutuhkan biaya lebih banyak dibandingkan kita mempertahankan pelanggan yang ada. Sehingga kita akan fokus pada nilai FN dengan mendapatkan nilai **recall** yang tinggi dan tetap membandingkan nilai **precision** agar tidak terlalu jauh. Kita ingin recall dan precisionya seimbang sehingga digunakan metric **f1_score** dengan data yang *imbalance*. Dilakukan juga Sampling *Undersampling (NearMiss)* dan *Oversampling(SMOTE)*. Selanjutnya akan dibandingkan model mana yang paling cocok untuk digunakan pada kasus ini.

# Data Preprocessing
feature:<br>
X='Dependents,OnlineSecurity,'OnlineBackup', 'InternetService', 'DeviceProtection', 'TechSupport', 'Contract', 'PaperlessBilling',Tenure,Monthly charge
<br>
Y= Churn

## Scaling

Pada permodelan machine learning akan dicoba menggunakan algoritma *Logistic Regression* dan *KNN* maka akan dilakukan scalling pada data kita. permodelan diharapkan memiliki skala yang sama sehingga akan maksimal dalam target. jika tidak dilakukan scaling maka variabel dengan skala besar akan mendominasi yang kecil. Pada data numerikal juga tidak memiliki outlier (**Tenure dan MonthlyCharges**) sehingga dapat digunakan MinMaxScaler().

## Sampling

Karena Data yang kita miliki tidak seimbang untuk mengatasinya sehingga kita akan menggunakan metode resampling agar data memiliki distribusi kelas yang lebih seimbang.

Akan dilakukan Ujicoba Oversampling dan UnderSampling dan akan dilihat parameter mana yang lebih maksimal dalam permodelan kita. Dimana pada oversampling kita akan menggunakan : <br>
SMOTE()

Dan pada data Undersampling :<br>
Nearmiss()

## Data Splitting

pada permodelan digunakan train size = 0.7 sehingga data training kita adalah sebesar 70% dari dataset. kemudian karena menggunakan klasifikasi untuk memprediksi data yang sifatnya kategorik, kita menggunakan stratify =y agar proporsi y_train dan y_test sama.

## Data Transformer (Encoding)

1. Merubah feature **Dependent,OnlineSecurity,'OnlineBackup', 'InternetService', 'DeviceProtection', 'TechSupport', 'PaperlessBilling'** karena fitur ini jumlah unique nya hanya sedikit dan tidak memiliki kelas/urutan(ordinal) sehingga kita encode menggunakan **One Hot Encoding**.

2. Merubah feature **Contract** menggunakan **Ordinal Encoder** karena datanya memiliki kelas yaitu Month-to-month menjadi kelas 3, One Year menjadi kelas 2, two year menjadi kelas 1.

Berdasarkan hasil train dan test pada data. pada kasus ini didapat hasil paling bagus pada model **XGBoost** sebesar 66% diikuti model model **Gradient Boost**, **Ada Boost**  yang nilainya cukup mirip sehingga pada ketiga model ini dilakukan tuning untuk melihat bagaimana performa dari ketiga model tersebut untuk mencari model yang memiliki perfoma paling maksimal.

Dari hasil tuning didapatkan bahwa model terbaik itu didapatkan bahwa model **XGBoost** memiliki performa paling baik diantara yang lain dengan menggunakan parameter learning rate :0.25, max_depth 3, dan n_estimator: 50.

Evaluasi terhadap biaya yang akan dikeluarkan jika kita menggunakan model **XGBoost** yang sudah kita buat seperti yang sudah di sebutkan pada bagian **Problem Statement** bahwa untuk mendapatkan pelanggan baru memerlukan biaya **5 kali lebih besar** dibandingkan kita mempertahankan pelanggan kita( [sumber](https://www.woopra.com/blog/churn-rate-vs-retention-rate#:~:text=Customer%20churn%20rate%20is%20the,up%20and%20stay%20with%20you.)). Sehingga dapat kita analisa sebegai berikut:

1. Pelanggan yang *True Negative* memerlukan biaya **0**.
2. Pelanggan yang *False negative* memerlukan biaya yang lebih banyak yaitu harus mengeluarkan biaya yang besar yaitu untuk iklan dan lain lain. Perlu mengeluarkan biaya sebesar **5** * biaya.
3. Pelanggan yang teridentifikasi churn ( *True Positive dan False Positif* ) kita akan mengeluarkan **1 * biaya untuk memberikan biaya retensi** misalnya berupa voucher diskon untuk mempertahankan pelanggan ini 


Hasil :
1. Kondisi Paling Buruk <br>
Perusahaan tidak menggunakan model untuk memprediksi pelanggan yang akan Churn. padahal terdapat 395 pelanggan yang akan churn sehingga memerlukan biaya 5 * 395 pelanggan = 1975 (p)

2. Fokus pemberian kupon pada setiap pelanggan <br>
Memberikan kupon diskon kepada setiap pelanggan yaitu sebesar 1 * 1479 pelanggan = **1479 (p)**

3. Kondisi Paling Baik<br>
Perusahaan mempunyai prediksi pelanggan yang akan Churn dan akan diberikan kupon diskon dengan biaya : 1* 395 = **395(p)**

4. Menggunakan Model yang sekarang <br>
Menggunakan model XGBoost akan mengeluarkan biaya sebesar :
(5 * 87 pelanggan) + (1* 557) = **992 (p)**


* Berdasarkan hasil plotting diatas dapat kita simpulkan bahwa kita dapat menekan biaya pengeluaran perusahaan dengan menggunakan model yang sudah kita buat dengan rincian sebagai berikut:
    1. Menghemat biaya sebesar **50 %** dari skenario kondisi terburuk perusahaan.
    2. Mengehemat biaya sebesar **33 %** dari skenario memberikan voucher diskon kepada setiap pelanggan.

Sehingga dengan menggunakan model yang sudah dibuat, perusahaan dapat menghemat 50 % dari total pengeluarannya.
   
# Kesimpulan

**Kesimpulan** :

1. Dari analisa yang sudah dilakukan model yang memiliki performa paling maksimal yaitu menggunakan model **XGBoost Classifier** dengan dilakukan hyperparameter tuning.
2. Parameter yang paling baik menggunakan parameter : **learning rate : 0.25, max_depth : 3, dan n_estimator: 50.**
3. Nilai Recall dan Precision dari kelas positif yaitu masing-masing sebesar **78% dan 55%**
4. Fitur paling berpengaruh pada model yaitu **Contract, Internet Service Fiber Optic dan Internet Service No**
5. Dengan menggunakan model ini dapat menghemat 50 % total pengeluaran perusahaan dibandingkan dengan perusahan tidak menggunakan model

**Rekomendasi** :
1. Dapat menggunakan algoritma Machine Learning lain seperti CatBoost, LightLgbm.
2. Menambahkan Feature lain seperti Jenis Pembayaran, Paket kecepatan Internet dan lainnya yang berhungan dengan Perusahaan Telco
3. Menggunakan sumber lain dalam perhitungan biaya Customer Acquisition dan mempertahankan pelanggan.
4. Data yang di analisa tidak seimbang (Imbalance) sehingga dapat menggunakan algoritma statistik lain.
5. Berdasarkan analisa Feature Importance, fitur yang paling berpengaruh adalah **Contract, Internet Service Fiber Optic dan Internet Service No** sehingga :

        1. pada kontrak pelanggan dapat memberikan hadiah kepada pelanggan yang mau beralih dari Month-to Month yang sifatnya jangka pendek untuk beralih ke kontrak 1 tahun dan 2 tahun yang bersifat jangka panjang
        2. memberikan harga yang lebih murah terhadap layanan internet fiber optic karena dengan harga yang tinggi juga dapat menjadi pemicu pelanggan tersebut melakukan churn.
        3. memperluas layanan internet perusahaan sehingga dapat menggunakan jasa internet perusahaan.

   
