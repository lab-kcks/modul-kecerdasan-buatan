# Generative AI

Pada modul ini, kita akan beralih dari model yang bersifat prediktif (discriminative) menuju model yang mampu menciptakan data baru (generative).

## Pengantar Generative AI

*Artificial Intelligence* secara umum dapat dibagi menjadi dua paradigma utama berdasarkan cara pandang model terhadap data:

1.  **Discriminative Models:** Model-model ini (seperti CNN untuk klasifikasi citra atau RNN untuk sentimen teks) belajar membedakan atau mengkategorikan data. Secara intuitif, model belajar memetakan $X \rightarrow Y$ (mengubah input menjadi label). Contoh: "Apakah gambar ini anjing atau kucing?"
2.  **Generative Models:** Model-model ini belajar menangkap distribusi probabilitas dari data itu sendiri, $P(X)$ atau $P(X|Y)$, sehingga mereka mampu **menghasilkan (generate)** sampel data baru yang mirip dengan data latih aslinya. Contoh: "Buatkan saya gambar seekor kucing."

Generative AI modern sebagian besar didorong oleh arsitektur **Transformer**, yang mampu memproses konteks dalam skala masif secara paralel. Kesuksesan arsitektur ini meledakkan tren Large Language Models (LLM) dan dengan cepat merambah ke modalitas lain (*multimodal*).

### Jenis Model Generative AI & Frontier Models (2026)

Lanskap AI generatif berkembang sangat pesat. Berikut adalah kategori utama dan jenis beserta contoh model *frontier* saat ini:

| Jenis Generative AI | Penjelasan | Contoh Model |
| :--- | :--- | :--- |
| **Large Language Models (LLM)** | Model generatif berbasis teks yang dilatih dengan skala triliunan token. Mampu memahami instruksi, coding, merangkum atau hal yang berkaitan dengan komunikasi tekstual. | GPT-5.2, Gemini 3, Claude Opus 4.6, GLM-5|
| **Vision-Language Model (VLM)** | Model yang mampu menerima dan menghasilkan kombinasi modalitas (misal: prompt teks + gambar -> output teks). | Qwen-VL |
| **Image Generation** | Model yang menghasilkan gambar fotorealistik atau ilustrasi berdasarkan deskripsi teks (text-to-image). | Stable Diffusion 3/XL, Midjourney v7, DALL-E 3/4, Nano Banana |
| **Audio & Music Generation** | Menghasilkan suara manusia yang natural (TTS), efek suara, atau bahkan musik komplit. | Whisper (speech recognition), MusicGen, ElevenLabs |
| **Video Generation** | Menghasilkan rekaman video yang konsisten secara temporal berdasarkan instruksi prompt. | Sora, Runway Gen-3 |

---

## Daftar Isi

1.  **[01. Transformers: Arsitektur Dasar Mutakhir](01-transformers.md)**

    Mempelajari *Self-Attention* dan mekanisme dasar *Transformer* yang menjadi tulang punggung revolusi AI modern.
2.  **[02. Large Language Models (LLM)](02-llm.md)**

    Memahami properti LLM (Tokens, Parameters, Context Window, dan *Mixture-of-Experts*).
3.  **[03. Reasoning Models](03-reasoning-models.md)**

    Mempelajari bagaimana model berevolusi dari sekadar menebak kata berikutnya menjadi entitas yang mampu mempertimbangkan *multi-step logic* (melalui mekanisme *Reinforcement Learning*).
4.  **[04. Long Context Models](04-long-context-models.md)**
    
    Mempelajari teknik yang memungkinkan model saat ini untuk dapat memproses hingga jutaan rentetan token sekaligus.
5.  **[05. Multimodal Learning](05-multimodal-learning.md)**
    
    Memahami bagaimana AI mulai mengawinkan modalitas bahasa dengan visual (citra, video) beserta intuisinya (*Contrastive Learning*).
6.  **[06. Retrieval-Augmented Generation (RAG)](06-rag.md)**

    Teknik yang sering digunakan industri untuk "mencangkokkan" pengetahuan/database eksternal langsung ke sebuah LLM agar jawaban relevan dan tervalidasi berdasarkan dataset lokal tanpa perlu melakukan *fine-tuning*.

---
**Catatan Penting:** 
*   *Semua skrip (*pseudocode* maupun implementasi notebook) dapat ditemukan di [direktori code/](./code).*