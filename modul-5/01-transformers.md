# 01. Transformers

## Konsep

![Transformer](./images/transformer-umume.png)

Arsitektur **Transformer** adalah revolusi terbesar dalam AI dekade terakhir. Diperkenalkan pertama kali oleh Google pada tahun 2017 lewat *paper* **"Attention Is All You Need"**, arsitektur ini sepenuhnya menggantikan pendekatan RNN/LSTM yang mendominasi sebelumnya.

Jika RNN membaca teks kata demi kata (sekuensial), Transformer memiliki kemampuan murni untuk **memproses seluruh kata secara paralel sekaligus** dan "melihat" hubungan antar setiap kata di seluruh kalimat tanpa dihalangi oleh memori yang meluruh seiring panjangnya teks. Ini adalah dasar utama dari *semua* Large Language Models (LLM) modern saat ini, mulai dari ChatGPT hingga DeepSeek.

---

## Teori

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

$$\mathbf{R}_{\Theta, m}^d \mathbf{x} = \begin{pmatrix} x_1 \\ x_2 \\ \vdots \\ x_{d-1} \\ x_d \end{pmatrix} \otimes \begin{pmatrix} \cos(m\theta_1) \\ \cos(m\theta_1) \\ \vdots \\ \cos(m\theta_{d/2}) \\ \cos(m\theta_{d/2}) \end{pmatrix} + \begin{pmatrix} -x_2 \\ x_1 \\ \vdots \\ -x_d \\ x_{d-1} \end{pmatrix} \otimes \begin{pmatrix} \sin(m\theta_1) \\ \sin(m\theta_1) \\ \vdots \\ \sin(m\theta_{d/2}) \\ \sin(m\theta_{d/2}) \end{pmatrix}$$

---

## Arsitektur

![Transformer Architecture](./images/transformer.png)

Secara historis, arsitektur penuh Transformer terdiri dari dua sisi yang bekerja sama:

1.  **Encoder:** Bertugas membaca input teks mentah, memroses seluruh relasi kata menggunakan *Self-Attention*, dan merangkumnya menjadi representasi vektor padat (*continuous representation*).
2.  **Decoder:** Bertugas menghasilkan (men-*generate*) respons kata-demi-kata (bersifat *autoregressive*). Ia menggunakan informasi vektor yang dihasilkan oleh *Encoder*, tapi hanya bisa melihat token-token yang telah ia generasikan di masa lalu (melalui mekanisme *Masked Attention*).

Sebuah blok Transformer Layer umumnya berisi komponen berikut:

Input $\rightarrow$ **Multi-Head Attention** (menjalankan banyak *attention* paralel untuk banyak ranah perspektif) $\rightarrow$ Penambahan (Residual) + **LayerNorm** $\rightarrow$ **Feed Forward Network (FFN)** $\rightarrow$ Penambahan (Residual) + **LayerNorm** $\rightarrow$ Output.

> Note: Banyak model LLM saat ini melepaskan komponen Encoder sama sekali dan hanya murni menyusun lapisan **Decoder-only**.

---

## Model Modern

Perkembangan mutakhir tidak mengubah banyak logika dasar *attention*, melainkan mengoptimalkan efisiensi komputasi dan penskalaannya. 

Beberapa wujud *frontier* arsitektur Transformer saat ini (2026):
*   **GPT-5 / Claude-4 / Gemini-3:** Implementasi sistem Transformer masif.
*   **Llama-4 / DeepSeek-V3:** Mulai banyak memakai arsitektur **Mixture-of-Experts (MoE)** yang dinamis di dalam *Feed Forward Network*-nya. Artinya, tidak semua miliaran parameter diforward secara pasif di tiap lapisan, melainkan dirutekan secara hemat hanya kepada "pakar" sub-network kecil yang relevan, meningkatkan efisiensi komputasi secara radikal.
*   Riset terbaru juga secara masif memperbarui mekanisme optimalisasi dasar, seperti ditemukannya efisiensi tinggi pada penggunaan **Muon/MuonClip Optimizer** guna menjaga kelangsungan model stabil berskala triliunan parameter tanpa jeda *crash* *training*.

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
