# Deep Learning for Text Classification

> Pada modul kelima, kita akan mempelajari deep learning part 2 untuk text classification. Kita akan mempelajari langkah demi langkah gimana caranya melakukan text classifiation. So let's get started, shall we?

![Wordcloud2](./images/Wordcloud2.png)

## Daftar Isi

- [Deep Learning (Text)](#deep-learning-text-classification)
  - [Daftar Isi](#daftar-isi)
  - [1. Pendahuluan](#1-pendahuluan)
    - [Apa Itu Text Classification](#apa-itu-text-classification)
    - [Penggunaan](#penggunaan)
  - [2. Preprocessing Text](#2-preprocessing-text)
    - [Tokenization and Text Normalization](#tokenization-normalization)
    - [Padding and Truncation](#padding-truncate)
    - [One Hot Encoding vs Word Embeddings](#Encoding-Embeding)
  - [3. Recurrent Neural Network (RNN)](#3-recurrent-neural-network-rnn)
    - [Basic RNN Structure and Workflow](#basic-rnn)
    - [Forward pass and backpropagation through time (BPTT)](#BPTT)
    - [LSTM](#lstm)
    - [GRU](#gru)
  - [Referensi](#referensi)

## 1. Pendahuluan

### Apa itu Natural Languange Processing (NLP)?
Natural Language Processing atau Pemrosesan Bahasa Alami adalah cabang dari Kecerdasan Buatan (Artificial Intelligence) yang menjembatani komunikasi antara bahasa manusia (bahasa alami) dengan pemahaman mesin.

![NLP](./images/nlp.jpg)

Tujuan utama NLP adalah untuk memungkinkan komputer memahami, menafsirkan, memanipulasi, dan menghasilkan bahasa manusia dengan cara yang bermakna. NLP mengombinasikan keilmuan Linguistik Komputasional dengan model statistik, Machine Learning, dan Deep Learning.

Implementasi dalam Dunia Nyata:
1. Machine Translation: Seperti Google Translate yang menerjemahkan antar bahasa secara instan.
2. Virtual Assistants: Seperti Siri, Google Assistant, atau Alexa yang memproses perintah suara.
3. **Text Classification**: (Topik modul kita) Yang digunakan untuk memilah email spam atau melakukan analisis sentimen.
4. Information Extraction: Mengambil poin-poin penting secara otomatis dari sebuah dokumen kontrak yang panjang.

### Apa itu text classification?
Text Classification atau klasifikasi teks merupakan salah satu bidang dari 
Natural Language Processing yang mengotomasikan pengklasifikasian teks ke satu 
atau lebih kategori yang tepat berdasarkan isinya dengan membangun model 
menggunakan data latih. Ada beberapa strategi umum dalam penggunaan klasifikasi teks, yaitu text preprocessing, feature extraction, modeling menggunakan teknik pembelajaran mesin yang sesuai, serta training dan testing pada classifier

### Penggunaan

Klasifikasi teks diterapkan dalam berbagai konteks contohnya:
1. Analisis Sentimen: Apakah sentimen atau review dari suatu produk itu positif atau negatif
2. Klasifikasi Topik: Untuk mengetahui topik tersebut merupakan topik Olahraga, Sinema atau Teknologi
3. Spam detection: Apakah email ini merupakan email spam atau tidak

---
## 2. Preprocessing Text
Merupakan tahap awal dalam metode NLP untuk dokumen yang berupa teks (NLP for Text). Text Preprocessing mempersiapkan teks yang tidak terstruktur menjadi data yang baik dan siap untuk diolah. Ada berbagai proses yang dapat digunakan dalam tahap Text Preprocessing. Tidak ada aturan yang baku mengenai proses apa saja serta urutan yang digunakan dalam tahap Text Preprocessing. Semua tergantung dari output yang kita inginkan dari data tersebut. Kali ini, kita akan mencoba melakukan Text Preprocessing menggunakan bahasa pemrograman Python dengan library **Natural Language Toolkit (NLTK). **

Contoh kalimat yang akan digunakan adalah **"Barangnya oke, penjualnya juga ramah dan respon cepat. Mantaplah pokoknya, very good!"**

```python
import nltk
import re
from nltk.tokenize import word_tokenize
from nltk.corpus import stopwords
from Sastrawi.Stemmer.StemmerFactory import StemmerFactory # Library tambahan untuk Stemming Bahasa Indonesia

# Download resource NLTK
nltk.download('punkt')
nltk.download('stopwords')

# Kalimat contoh
raw_text = "Barangnya oke, penjualnya juga ramah dan respon cepat. Mantaplah pokoknya, very good!"
```

### a. Tokenization 
<img src="./images/tokenization.png" width="50%">

Tokenizing atau disebut juga tahap Lexical Analysis adalah proses pemotongan teks menjadi bagian-bagian yang lebih kecil, yang disebut token. Pada proses ini juga dilakukan penghilangan angka, tanda baca dan karakter lain yang dianggap tidak memiliki pengaruh terhadap pemrosesan teks. 

```python
# Tokenization
tokens = word_tokenize(text_cleaned)

print(f"Hasil Tokenization: \n{tokens}")
```

#### Outputnya adalah **['barangnya','oke', 'penjualnya', 'juga', 'ramah', 'dan', 'respon', 'cepat', 'mantaplah', 'pokoknya', 'very', 'good']**

### b. Case Folding
![casefolding](./images/casefolding.png)

Case Folding merupakan proses untuk mengkonversi teks ke dalam format huruf kecil (lowercase). Hal ini bertujuan untuk memberikan bentuk standar pada teks. 

```python
# Case Folding
text_lowercase = raw_text.lower()

# Menghapus tanda baca dan angka menggunakan Regex
text_cleaned = re.sub(r'[^a-zA-Z\s]', '', text_lowercase)

print(f"Hasil Case Folding: \n{text_cleaned}")
```

#### Outputnya adalah: **"barangnya oke, penjualnya juga ramah dan respon cepat. mantaplah pokoknya, very good!"**

### c. Stemming
<img src="./images/stemming.png" width="50%">

Stemming adalah proses pengubahan bentuk kata menjadi kata dasar atau tahap mencari root dari tiap kata. Karena NLTK belum optimal untuk root word Indonesia, kita menggunakan **library PySastrawi.**
```python
# Inisialisasi Stemmer Sastrawi
factory = StemmerFactory()
stemmer = factory.create_stemmer()

# Menggabungkan token kembali menjadi kalimat untuk proses stemming
text_for_stemming = " ".join(filtered_tokens)
stemmed_text = stemmer.stem(text_for_stemming)

# Jika ingin dalam bentuk list kembali
final_tokens = stemmed_text.split()

print(f"Hasil Stemming: \n{final_tokens}")
```

#### Outputnya adalah: **"barang oke jual juga ramah dan respon cepat mantap pokok very good"**

### d. Filtering
<img src="./images/stopword.png" width="50%">

Tahap Filtering atau Stopword Removal adalah tahap pemilihan kata-kata yang dianggap penting. Terdapat dua metode yang dapat digunakan dalam tahap ini, yaitu: 
1. Stoplist:
Pada metode ini, kita menyiapkan kumpulan kata yang tidak deskriptif (tidak penting) yang disebut stoplist/stopword. Kata yang termasuk ke dalam stoplist akan dibuang dan tidak digunakan pada proses selanjutnya.
2. Wordlist:
Kebalikan dari stoplist, pada metode ini kita menyiapkan kumpulan kata yang deskriptif (penting) yang disebut wordlist. Hanya kata yang termasuk ke dalam wordlist yang akan digunakan pada proses selanjutnya, sementara kata lainnya akan dibuang.

```python
# Mengambil daftar stopword Bahasa Indonesia dari NLTK
stop_words = set(stopwords.words('indonesian'))

# Menambahkan stopword tambahan jika perlu
stop_words.extend(['oke', 'very', 'good']) 

# Proses Filtering
filtered_tokens = [word for word in tokens if word not in stop_words]

print(f"Hasil Filtering: \n{filtered_tokens}")
```

---
## 3. Recurrent Neural Network (RNN)
Recurrent neural network (RNN) adalah sistem algoritma tertua yang telah dikembangkan sejak tahun 1980-an. Sebagai sebuah sistem algoritma, Recurrent neural network dapat mengingat input dan selanjutnya memberikan output sesuai dengan yang diinginkan. Memori internal menjadi poin penting dalam Recurrent neural network karena dapat memprediksi hal berikutnya. Sehingga, Recurrent neural network sangat cocok untuk diaplikasikan pada deret waktu, mesin pencarian, teks, audio, video, bahkan mesin keuangan.

![RNN](./images/rnn.jpg)

#### Basic RNN vs FNN
![RNN vs FNN](./images/RNN-vs-FNN-660.png)

**a. Feedforward Neural Networks (FNN)** memproses data dalam satu arah dari input ke output tanpa menyimpan informasi dari input sebelumnya. Hal ini membuat mereka cocok untuk tugas-tugas dengan input independen seperti klasifikasi gambar. Namun, FNN tidak cocok untuk data berurutan karena kekurangan memori.

**b. Recurrent Neural Networks (RNNs)** mengatasi hal ini dengan memasukkan loop yang memungkinkan informasi dari langkah sebelumnya untuk diumpankan kembali ke dalam jaringan. Umpan balik ini memungkinkan RNN untuk mengingat input sebelumnya sehingga ideal untuk tugas-tugas yang membutuhkan konteks.

```python
import tensorflow as tf
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Embedding, SimpleRNN, Dense

# Parameter Model
vocab_size = 5000    # Jumlah kosakata unik
embedding_dim = 64   # Dimensi vektor kata
max_length = 100     # Panjang maksimal kalimat

# Membangun Arsitektur RNN
model = Sequential([
    # 1. Layer Embedding: Mengubah angka (token) menjadi vektor padat
    Embedding(vocab_size, embedding_dim, input_length=max_length),
    
    # 2. Layer SimpleRNN: Inti dari pemrosesan sekuensial
    # units=32 adalah jumlah hidden neurons
    SimpleRNN(units=32, dropout=0.2, recurrent_dropout=0.2),
    
    # 3. Dense Layer: Untuk klasifikasi akhir
    Dense(16, activation='relu'),
    
    # 4. Output Layer: Sigmoid digunakan untuk klasifikasi biner (Positif/Negatif)
    Dense(1, activation='sigmoid')
])

# Kompilasi Model
model.compile(
    optimizer='adam',
    loss='binary_crossentropy',
    metrics=['accuracy']
)

model.summary()
```

##### a. Embedding
Mengubah representasi angka (ID kata) menjadi vektor yang menangkap hubungan semantik antar kata.

##### b. SimpleRNN
Layer yang melakukan perulangan (loop) melalui urutan kata. Argumen dropout digunakan untuk mencegah overfitting.

##### c. Dense
Lapisan saraf standar untuk melakukan pengolahan fitur hasil dari RNN.

##### d. Output (Sigmoid)
Menghasilkan nilai antara 0 dan 1. Jika $> 0.5$, teks diklasifikasikan sebagai kategori 1 (misal: Positif).

### Forward pass and backpropragation through time (BPTT)
Backpropagation through time (BPTT) adalah metode yang digunakan Recurrent Neural Network (RNN) untuk melatih jaringan dengan merambatkan kesalahan melalui waktu. Dalam FNN, data mengalir melalui jaringan dalam satu arah, dari lapisan input melalui lapisan tersembunyi ke lapisan output. Namun, dalam RNN, ada koneksi antara node dalam langkah waktu yang berbeda, yang berarti bahwa output dari jaringan pada satu langkah waktu tergantung pada input pada langkah waktu tersebut dan juga langkah waktu sebelumnya.

![BPTT](./images/image.png)

### LSTM
Long Short-Term Memory (LSTM) adalah versi pengembangan dari Recurrent Neural Network (RNN) yang dirancang oleh Hochreiter & Schmidhuber. LSTM mampu menangkap dependensi jangka panjang dalam data sekuensial, menjadikannya ideal untuk tugas-tugas seperti terjemahan bahasa, pengenalan suara, dan peramalan time series.

Tidak seperti RNN tradisional yang menggunakan hidden state tunggal yang dilewatkan seiring waktu, LSTM memperkenalkan memory cell yang menyimpan informasi selama periode yang lebih lama, mengatasi tantangan dalam mempelajari dependensi jangka panjang.


![LSTM](./images/LSTM.jpg)

### GRU
GRU adalah singkatan dari Gated Recurrent Unit, yaitu jenis arsitektur recurrent neural network (RNN) yang mirip dengan LSTM (Long Short-Term Memory).

Seperti LSTM, GRU dirancang untuk memodelkan data sekuensial dengan memungkinkan informasi diingat atau dilupakan secara selektif seiring waktu. Namun, GRU memiliki arsitektur yang lebih sederhana daripada LSTM, dengan lebih sedikit parameter, yang membuatnya lebih mudah dilatih dan lebih efisien secara komputasi.

Perbedaan utama antara GRU dan LSTM adalah cara mereka menangani memory cell state. Dalam LSTM, memory cell state dipertahankan secara terpisah dari hidden state dan diperbarui menggunakan tiga gate: input gate, output gate, dan forget gate. Dalam GRU, memory cell state digantikan dengan "candidate activation vector", yang diperbarui menggunakan dua gate: reset gate dan update gate.

Reset gate menentukan seberapa banyak hidden state sebelumnya untuk dilupakan, sedangkan update gate menentukan seberapa banyak candidate activation vector untuk digabungkan ke dalam hidden state yang baru.
![GRU](./images/GRU.jpg)

---
### Referensi
