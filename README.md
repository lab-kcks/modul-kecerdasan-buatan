# Modul Praktikum Kecerdasan Buatan 2026

Repositori ini berisi kumpulan modul praktikum dan *Jupyter Notebook* untuk mata kuliah AI tahun 2026.

## Daftar Modul

- [**Modul 0: Preprocessing Tabular, Text and Image**](./modul-0/)
  > Preprocessing tabular, teks, dan citra termasuk *feature-label-target*, *data split*, dan transformasi dasar agar data siap dimodelkan.

-[**Modul 1: Supervised Learning**](./modul-1/)
  > Regression, Classification, Ensemble Methods, Metrics, serta pengantar PCA sebagai tahap *feature engineering*.

- [**Modul 2: Unsupervised Learning & Metrics**](./modul-2/)
  > Mengeksplorasi struktur data serta penggunaan metrik *unsupervised* untuk mengevaluasi kualitas hasil secara objektif.

- [**Modul 3: Deep Learning Image**](./modul-3/)
  > *Deep learning* untuk *image* berbasis CNN beserta evaluasinya.

-[**Modul 4: Deep Learning Text**](./modul-4/)
  > *Deep learning* untuk teks berbasis RNN/LSTM/GRU untuk *task* NLP dasar.
  
- [**Modul 5: Generative AI**](./modul-5/)
  > *Generative AI* dengan Transformers (*attention, pretraining, finetuning*), dilanjutkan *Multimodal Learning* dan RAG.

---

## Cara Menjalankan Environment & Run Notebook

1. **Prasyarat Instalasi**
   Sebelum mencoba Notebook, pastikan Anda telah menginstal:
   - [uv package manager](https://docs.astral.sh/uv/getting-started/installation/)
   - Ekstensi **Jupyter** di VS Code (atau Jupyter Notebook *standalone*).

2. **Sinkronisasi Dependencies**
   Buka terminal di *root* repositori ini, lalu jalankan perintah berikut untuk membuat *virtual environment* (`.venv`) dan menginstal seluruh *library* yang dibutuhkan secara otomatis:
   ```sh
   uv sync