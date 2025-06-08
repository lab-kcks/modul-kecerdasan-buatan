# **Prompt Engineering**

Prompt engineering adalah sebuah teknik pada perancangan dan penyusunan input (prompt) untuk berinteraksi secara efektif dengan LLM. Tujuan utamanya adalah untuk memandu model agar menghasilkan output yang akurat, relevan, dan sesuai dengan kebutuhan. Terdapat tiga teknik dasar berdasarkan jumlah contoh yang diberikan dalam prompt, yaitu zero-shot, one-shot, dan few-shot prompting.

## Basic Prompt Engineering

### Zero Shot Prompt

Zero-shot prompting adalah teknik memberikan instruksi kepada LLM untuk melakukan sebuah tugas tanpa menyertakan contoh apa pun. Teknik ini sepenuhnya mengandalkan `pure` kemampuan model untuk memahami dan mengeksekusi perintah berdasarkan pengetahuan yang diperoleh. Biasanya digunakan dalam task seperti:

- Bersifat umum dan sederhana (misalnya, penerjemahan, rangkuman, jawaban atas pertanyaan umum).
- Mendapatkan respons yang cepat dan tidak memerlukan format spesifik.
- Model yang digunakan memiliki kapabilitas tinggi dan pemahaman konteks yang luas.

Contoh:

```text
Klasifikasikan sentimen dari teks berikut sebagai positif, negatif, atau netral:

Teks: "Kualitas kamera ponsel ini sangat mengesankan, tetapi daya tahan baterainya kurang."
```

### One Shot Prompt

One-shot prompting adalah teknik menyertakan satu contoh lengkap (input dan output yang diharapkan) di dalam prompt sebelum memberikan instruksi tugas yang sebenarnya. Contoh ini berfungsi sebagai petunjuk atau acuan bagi model mengenai format, gaya, atau struktur output yang diinginkan. Biasanya digunakan dalam:

- Output harus berada pada format atau pola tertentu.
- Mengarahkan output model agar lebih spesifik dan terstruktur.

Contoh:

```text
Ekstrak nama entitas dari sebuah kalimat.

Kalimat: "Apple Inc. mengumumkan iPhone terbaru pada acara di Cupertino."
Entitas: Apple Inc., iPhone, Cupertino

Kalimat: "Cristiano Ronaldo mencetak gol kemenangan untuk Manchester United di Old Trafford."
Entitas:
```

### Few Shot Prompt

Few-shot prompting adalah pengembangan dari teknik one-shot dengan menyertakan beberapa contoh. Dengan lebih banyak contoh, model mendapatkan konteks yang lebih untuk mempelajari pola dan batasan dari tugas yang diberikan, sehingga meningkatkan akurasi dan konsistensi output. Biasanya digunankan dalam:

- Bersifat kompleks, ambigu, atau memerlukan penalaran pola yang mendalam.
- Memastikan tingkat akurasi dan konsistensi output yang tinggi.

Contoh:

```text
Ubah kalimat percakapan menjadi format JSON.

Teks: "Tolong pesankan saya tiket ke Bali untuk tanggal 25 Desember."
JSON: {"tujuan": "Bali", "tanggal": "25 Desember", "aksi": "pesan_tiket"}

Teks: "Setel alarm untuk jam 7 pagi besok."
JSON: {"aksi": "setel_alarm", "waktu": "07:00", "hari": "besok"}

Teks: "Berapa cuaca di Jakarta hari ini?"
JSON:
```

## Advanced Prompt Engineering

### Chain of Though (CoT)

Chain of Thought (CoT) adalah sebuah teknik di mana model diinstruksikan untuk "berpikir langkah demi langkah" sebelum memberikan jawaban akhir. Model diminta untuk menguraikan proses penalarannya, memecah masalah kompleks menjadi bagian-bagian yang lebih kecil dan dapat ditrace pemikirannya. Pendekatan ini meniru cara manusia memecahkan masalah yang memerlukan logika atau perhitungan. Biasanya digunakan dalam:

- masalah matematika, soal logika, teka-teki, dan tugas apa pun yang memerlukan penalaran multi-langkah.
- Meningkatkan akurasi dan keandalan model pada tugas-tugas kompleks dengan mengurangi kemungkinan "halusinasi" atau kesalahan dalam penalaran.

Contoh tanpa CoT:

```text
Roger memiliki 5 bola tenis. Dia membeli 2 kaleng bola tenis lagi. Setiap kaleng berisi 3 bola tenis. Berapa jumlah total bola tenis yang dia miliki sekarang?
```

Tanpa CoT, model mungkin langsung menjawab 11, tetapi bisa juga salah hitung di operasi yang lebih sulit.

Contoh dengan CoT:

```text
Roger memiliki 5 bola tenis. Dia membeli 2 kaleng bola tenis lagi. Setiap kaleng berisi 3 bola tenis. Berapa jumlah total bola tenis yang dia miliki sekarang?

Mari kita pikirkan langkah demi langkah untuk menyelesaikan masalah ini.
```

Output:

```text
1. Roger memulai dengan 5 bola tenis.
2. Dia membeli 2 kaleng bola tenis.
3. Setiap kaleng berisi 3 bola. Jadi, jumlah bola dari kaleng adalah 2 * 3 = 6 bola.
4. Jumlah total bola adalah bola awal ditambah bola baru: 5 + 6 = 11 bola. Jawaban akhirnya adalah 11.
```

### ReAct (Reason and Act)

ReAct adalah teknik  yang mengintegrasikan kemampuan penalaran (Reasoning) seperti pada CoT dengan kemampuan untuk bertindak (Acting). "Bertindak" berarti model dapat menggunakan alat eksternal—paling umum adalah melakukan pencarian di internet—untuk mengumpulkan informasi yang tidak dimilikinya. Prosesnya bersifat iteratif: model berpikir, memutuskan tindakan, mengamati hasil tindakan, lalu melanjutkan proses berpikirnya dengan informasi baru tersebut. Biasanya digunakan seperti:

- Menjawab pertanyaan yang memerlukan informasi terkini (real-time), seperti "Siapa pemenang Piala Dunia FIFA 2026?" atau "Berapa harga saham perusahaan X hari ini?".
- Mengatasi keterbatasan pengetahuan LLM yang statis (terbatas pada data saat pelatihan) dan memungkinkan pembuatan AI Agent yang dapat berinteraksi dengan environmentnya.

Contoh:

```text
Prompt: "Siapa sutradara film dengan pendapatan kotor tertinggi di tahun 2024 dan berapa total pendapatannya?"

Output:
1. Thinking: Saya perlu mencari daftar film dengan pendapatan tertinggi di tahun 2024 dan nama sutradaranya.
2. Action: Search("film pendapatan tertinggi 2024")
3. Observe/Overview: Hasil pencarian menunjukkan bahwa film 'XYZ Movie' adalah yang tertinggi. Informasi sutradaranya adalah 'Jane Doe'. Total pendapatan kotornya adalah $1.5 Miliar.
4. Thinking: Saya sudah memiliki semua informasi yang dibutuhkan. Sutradaranya adalah Jane Doe dan pendapatannya adalah $1.5 Miliar. Sekarang saya bisa merumuskan jawaban akhir.
5. Output: Sutradara film dengan pendapatan kotor tertinggi di tahun 2024 adalah Jane Doe dengan filmnya 'XYZ Movie', yang meraih total pendapatan $1.5 Miliar.
```