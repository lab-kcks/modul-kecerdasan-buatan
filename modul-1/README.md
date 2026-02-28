
# <b>Supervised Learning</b>
<a name="supervised-learning"></a>
<p>Supervised learning adalah salah satu bidang machine learning di mana model belajar dari data pelatihan berlabel. Selama training, pengguna memberikan data input kepada algoritma serta label output yang terkait. Berdasarkan data input, algoritma mempelajari pemetaan dari input ke output dan menghasilkan prediksi atau penilaian. Ada banyak penerapan yang menggunakan supervised learning, termasuk penyaringan spam, pengenalan suara, penerjemahan mesin, dan periklanan online.</p>

Sebelum menggunakan model Supervised Learning, sebaiknya dilakukan eksplorasi data (Exploratory Data Analysis/EDA) terlebih dahulu untuk memahami karakteristik, pola, dan kualitas data yang digunakan. Seperti yang telah dilakukan pada [modul 1](../modul-1/Modul_1.ipynb)

src: https://www.datacamp.com/blog/supervised-machine-learning

## <b>Daftar Isi</b>
- [Supervised Learning](#supervised-learning)
    1. [Regresi](#regresi)
    2. [Klasifikasi](#klasifikasi)

<p align="center">
    <img src="./images/supervised_desc.png" width='750'>
</p>

## <b>Regresi</b>
<a name="1. regresi"></a>
<p>Regresi adalah salah satu jenis metode Supervised Learning di mana variabel targetnya berupa nilai yang bersifat kontinu. Contoh penggunaannya termasuk memprediksi berat badan, usia, harga, dan sebagainya.</p>

<p align="center">
    <img src="./images/regresi_desc.png" width='750'>
</p>

[Kode Implementasi Regresi](code/regresi.ipynb) 

## <b>Klasifikasi</b>
<a name="2. klasifikasi"></a>
<p>Klasifikasi adalah salah satu metode dalam Supervised Learning di mana algoritma belajar dari data berlabel untuk memprediksi kategori atau kelas dari data baru di masa mendatang. Metode ini digunakan untuk membedakan data ke dalam beberapa kelompok berdasarkan fitur-fitur tertentu.</p>

<p align="center">
    <img src="./images/klasifikasi_example.png" width='750'>
</p>

[Kode Implementasi Klasifikasi](code/klasifikasi.ipynb)

## <b>Metode</b>
### <b>Linear Models</b>
<a name="3. Linear"></a>
<p>Linear models adalah metode *supervised learning* yang membuat prediksi berdasarkan hubungan linear antar fitur. 
    
Di **regression**, model digunakan untuk memprediksi nilai numerik seperti harga atau jumlah, sedangkan pada **classification** model digunakan untuk memisahkan data ke dalam beberapa kelas menggunakan batas keputusan linear. 
    
Contohnya adalah Linear Regression dan Logistic Regression.</p>

### <b>Tree-based Models</b>
<a name="4. tree"></a>
<p>Untuk Tree-based model, metode yang membuat prediksi lewat serangkaian aturan keputusan berbentuk pohon. 

Pada **regression**, model memprediksi nilai numerik dengan membagi data ke beberapa kelompok. Sedangkan di **classification** model, menentukan kelas berdasarkan kondisi tertentu dari fitur. 
    
Contohnya adalah Decision Tree, Random Forest, dan Gradient Boosting.</p>

## <b>Ensemble Methods</b>
<a name="5. ensemble"></a>
<p>Dengan menggabungkan beberapa model sekaligus untuk menghasilkan prediksi yang lebih akurat. 
    
Ide dasarnya: daripada mengandalkan satu model yang bisa saja salah, kita menggabungkan banyak model supaya kesalahannya bisa saling menutupi. Ensemble biasanya dibangun dari model-model dasar seperti decision tree atau model lain yang sudah dipelajari sebelumnya. 
    
Contoh metode ensemble yang sering digunakan adalah Random Forest dan Gradient Boosting, yang dikenal punya performa bagus di banyak kasus.</p>

## <b>Metode Evaluasi Supervised Learning</b>
<a name="6. metrics"></a>
<p>Metode evaluasi adalah cara untuk mengukur seberapa baik model bekerja pada data baru. 

Evaluasi biasanya dilakukan dengan membagi data menjadi data train dan data test, lalu menghitung metrik tertentu. Untuk regression digunakan metrik seperti MAE atau RMSE, sedangkan untuk classification digunakan metrik seperti accuracy atau F1-score.</p>

<p align="center">
    <img src="./images/meme.png" width='75%'>
