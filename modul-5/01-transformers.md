# 01. Transformers

## Konsep

![Transformer](./images/transformer-umume.png)

Arsitektur **Transformer** adalah revolusi terbesar dalam AI dekade terakhir. Diperkenalkan pertama kali oleh Google pada tahun 2017 lewat *paper* **"Attention Is All You Need"**, arsitektur ini sepenuhnya menggantikan pendekatan RNN/LSTM yang mendominasi sebelumnya.

Jika RNN membaca teks kata demi kata (sekuensial), Transformer memiliki kemampuan murni untuk **memproses seluruh kata secara paralel sekaligus** dan "melihat" hubungan antar setiap kata di seluruh kalimat tanpa dihalangi oleh memori yang meluruh seiring panjangnya teks. Ini adalah dasar utama dari *semua* Large Language Models (LLM) modern saat ini, mulai dari ChatGPT hingga DeepSeek.

---

## Pilar Utama

Dua pilar teori matematis di balik Transformer adalah *Self-Attention* dan *Positional Encoding*.

### 1. Mekanisme Self-Attention

Inti dari Transformer adalah *Self-Attention*. Jika terdapat kalimat "Kucing itu mengejar tikus karena ia lapar", kata "ia" merujuk ke siapa? Mekanisme attention memberikan bobot komputasi agar "ia" lebih berfokus (memberi nilai *attention* tinggi) kepada kata "Kucing".

Secara matematis, setiap *token* diproyeksikan ke dalam tiga vektor: **Query (Q)**, **Key (K)**, dan **Value (V)**.
- **Query:** Apa yang sedang saya cari?
- **Key:** Apa identitas informasi yang saya punya?
- **Value:** Apa informasi aktual yang saya bawa?

Pusat komputasi dihitung dengan persamaan *Scaled Dot-Product Attention*:

$$ \text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V $$

Perkalian *dot product* antara Query dari kata saat ini dengan Key dari semua kata lain pada kalimat ($QK^T$) menghasilkan "skor kecocokan" seberapa relevan antarkata. Nilai ini dibagi dengan $\sqrt{d_k}$ untuk menstabilkan gradien, sebelum dimasukkan ke dalam fungsi `softmax` agar nilainya berada antara 0 hingga 1. Terakhir, bobot tersebut digunakan untuk mengalikan Value aslinya.

### 2. Positional Encoding

![Positional Encoding](./images/positional-encoding.png)

Karena Transformer memproses semua kata secara paralel, model tersebut benar-benar **buta terhadap urutan posisi kata**. Kata "Iwan memukul Budi" akan terlihat sama probabilitasnya dengan "Budi memukul Iwan". 

Oleh karena itu, sebelum vektor kata masuk ke model, kita wajib "menyuntikkan" suatu sinyal tambahan yang menunjukkan urutan kata. Cara klasik adalah menggunakan **Sinusoidal Positional Encoding**, di mana disematkan gelombang sinus dan kosinus pada setiap vektor. Model-model modern saat ini banyak menggunakan varian lanjutan yang disebut **RoPE (Rotary Positional Embedding)**.

**Sinusoidal Positional Encoding**

$$\begin{aligned} PE(pos, 2i) &= \sin\left(\frac{pos}{10000^{2i/d_{model}}}\right) \\ PE(pos, 2i+1) &= \cos\left(\frac{pos}{10000^{2i/d_{model}}}\right) \end{aligned}$$

Keterangan:

* (pos) → posisi token dalam urutan (0,1,2,3,…)
* (i) → indeks dimensi embedding
* (d_{model}) → dimensi embedding model (misalnya 512 atau 1024)

Dimensi **genap** menggunakan fungsi **sin**, sedangkan dimensi **ganjil** menggunakan **cos**.

**Rotary Positional Encoding (RoPE)**

$$\mathbf{R}_{\Theta, m}^d \mathbf{x} = \begin{pmatrix} x_1 \\ x_2 \\ \vdots \\ x_{d-1} \\ x_d \end{pmatrix} \otimes \begin{pmatrix} \cos(m\theta_1) \\ \cos(m\theta_1) \\ \vdots \\ \cos(m\theta_{d/2}) \\ \cos(m\theta_{d/2}) \end{pmatrix} + \begin{pmatrix} -x_2 \\ x_1 \\ \vdots \\ -x_d \\ x_{d-1} \end{pmatrix} \otimes \begin{pmatrix} \sin(m\theta_1) \\ \sin(m\theta_1) \\ \vdots \\ \sin(m\theta_{d/2}) \\ \sin(m\theta_{d/2}) \end{pmatrix}$$

---

## Arsitektur

![Transformer Architecture](./images/transformer.png)

Secara historis, arsitektur penuh Transformer terdiri dari dua sisi yang bekerja sama: **Encoder** (kiri) dan **Decoder** (kanan). Di dalam sisian ini, teori-teori matematis di atas dirakit membentuk pipeline komponen berikut:

### A. Sisi Encoder 
Bertugas membaca input teks mentah, memroses relasi seluruh kata, dan merangkumnya menjadi representasi vektor yang kaya makna kontekstual.
1. **Input Embedding:** Mengubah data teks (token) menjadi angka representasi matriks agar bisa diolah secara relasional oleh algoritma komputer.
2. **Positional Encoding:** Menjalankan rumusan *Positional Encoding*. Matriks gelombang matematis ditambahkan ke matriks kata agar komputer tahu di mana letak urutan sintaksnya (misal: "makan" adalah subjek yang muncul di urutan-2).
3. **Multi-Head Attention:** Mengeksekusi rumusan matematis *Self-Attention*. Daripada menjalankan perkalian *attention (Q,K,V)* satu kali lurus, prosesnya dipecah jadi banyak "kepala paralel" (*multiple heads*) secara bersamaan yang masing-masing menyelidiki aspek berbeda (satu head fokus ke konteks tenses waktu, head lain berfokus ke sentimen relasi emosi bahasanya dan lain lain).
4. **Add & Norm (Residual + Layer Normalization):** Mekanisme yang menambahkan kembali input asli ke hasil layer lalu menormalkan nilainya sehingga informasi tetap terjaga dan training model yang sangat dalam tetap stabil. Residual menjaga informasi tetap mengalir, LayerNorm menjaga nilai tetap stabil. Keduanya penting agar gradien tidak hilang saat melakukan *Backpropagation*.

5. **Feed Forward Network (FFN):** Jaringan neural kecil yang memproses setiap token secara terpisah setelah *attention*, untuk memperkaya dan mentransformasikan representasi maknanya. FFN melakukan transformasi non-linear pada setiap token secara individual, sehingga model bisa mempelajari fungsi yang lebih kompleks.

*Notes* :

Peran *Attention* dan FFN berbeda, gambaran dapat dicek melalui berikut ini

Attention → menentukan hubungan antar kata

FFN → mengolah makna setiap kata setelah hubungan itu diketahui

### B. Sisi Decoder (Penghasil Teks)
Bertugas menghasilkan (*generate*) respons kata-demi-kata (bersifat *autoregressive*). Di dalam Decoder, anatominya mengulang semua step 1 hingga 5, namun disisipkan komponen **Dua Tipe Interaksi Attention yang Berbeda**:

6. **Masked Multi-Head Attention:** Sama seperti multi-head attention reguler, namun sengaja "ditutup matanya" (diberi pinalti matematis infinit *-infinity* di matriks skor) memblokir intipan kata masa depan yang posisinya berkanan. Decoder dilarang keras mencontek "jawaban" langkah ke-n+1.
7. **Encoder-Decoder Attention / Cross-Attention:** Fitur "melengong dan buka catatan". Di mana *Query (Q)* bersumber murni dari layer internal Decoder, tapi suplay wawasan *Key (K)* & *Value (V)* ditarik langsung dari output kompresi stasiun stasiun si **Encoder**. Di sinilah jembatan proses penjawaban terjadi.
8. **Linear + Softmax Output:** Fase puncak. *Linear classifier* yang mentransformasi kedalaman ruang vektor kembali membengkak seluas himpunan kamus bank kata model (hingga jutaan set). *Softmax* mengubah sekuens skor kaku tadi jadi murni probabilitas persenan (0.0~1.0). Skor paling montok melesat menjadi *Predicted Token* di kursor layar Anda!

> Note: Banyak model LLM saat ini melepaskan komponen Encoder sama sekali dan hanya murni menyusun lapisan **Decoder-only**.

---

## Model Modern

Perkembangan mutakhir tidak mengubah banyak logika dasar *attention*, melainkan mengoptimalkan efisiensi komputasi dan penskalaannya. 

Beberapa wujud *frontier* arsitektur Transformer saat ini (2026):
*   **GPT-5 / Claude-4 / Gemini-3:** Implementasi sistem Transformer masif.
*   **Qwen3.5 / DeepSeek-V3:** Mulai banyak memakai arsitektur **Mixture-of-Experts (MoE)** yang dinamis di dalam *Feed Forward Network*-nya. Artinya, tidak semua miliaran parameter diforward secara pasif di tiap lapisan, melainkan dirutekan secara hemat hanya kepada "Expert" sub-network kecil yang relevan, meningkatkan efisiensi komputasi secara masif.
*   Riset terbaru juga secara masif memperbarui mekanisme optimalisasi dasar, seperti ditemukannya efisiensi tinggi pada penggunaan **Muon/MuonClip Optimizer** guna menjaga kelangsungan model stabil berskala triliunan parameter tanpa jeda *crash* *training*. (Cek [technical report Kimi K2](https://arxiv.org/abs/2507.20534) untuk contoh implementasinya).

---

## Notebook

Cara kerja logis *attention* dan komponen pembangun sistem Transformer ini dapat dilihat melalui notebook berikut: 

1. [Notebook Implementasi Arsitektur Transformer Dasar](./code/transformer.ipynb)
2. [Notebook Deep-Dive: Self-Attention Mechanism](./code/attention_mechanism.ipynb)

---

## Informasi Tambahan

Sebuah kerangka pikir arsitektur yang sangat superior juga tak luput dari kelemahan mendadak. Berikut adalah *trade-off* yang harus dihadapi:

1.  **Complexitas Kuadratik $O(n^2)$**: Rumus *self-attention* kita mengharuskan perkalian antara semua token terhadap semua token di seluruh dokumen. Jika panjang teks menjadi $2x$ lipat, beban *memory* & kalkulasi komputasional naik menjadi $4x$ lipat. Inilah tantangan fundamental yang menjegal Transformer saat memproses sangat banyak data historis, melahirkan bidang spesifik yang memelajari optimisasi konteks (*Long Context Models*).
2.  **Beban Data & Compute**: Model dengan arsitektur ini secara harfiah "pintar" jika difasilitasi dengan volume data teks yang teramat masif. Hal tersebut menyebabkan *training cost* membengkak dan dapat mencapai ratusan triliun rupiah.
