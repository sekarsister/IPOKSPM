```markdown
# 🔴 IPO SENTIMENT & BANDARMOLOGY SCORE – COMPLETE WORKSHEET  
### *“Baca Ombak Sebelum Diterjang”*  

Panduan lengkap untuk mengukur **sentimen media sosial, jejak bandar, dan rekomendasi analis** dari berbagai platform (X/Twitter, TikTok, Stockbit, Telegram, Reddit, forum) guna menangkap *hype* serta potensi jebakan psikologis pada saham IPO.  
Skor akhir (setelah normalisasi) berkisar **0–100**.

---

## 📌 STRUKTUR SKOR  

| Kode | Indikator | Bobot | Pertanyaan Kunci |
|:----:|-----------|:----:|------------------|
| **MMI** | Market Maker Influence | 25% | Apakah bandar sedang mendorong beli atau jual? |
| **SC** | Sentimen Konten | 15% | Narasi dominan di postingan/video? |
| **SK** | Sentimen Komentar | 15% | Bagaimana respons massa di kolom komentar? |
| **ALIGN** | Alignment (Keselarasan) | 10% | Apakah komentar selaras dengan konten? |
| **D** | Indeks Keraguan Forum | 15% | Seberapa banyak keraguan di forum diskusi? |
| **SA** | Sentimen Analis | 20% | Rekomendasi analis di platform khusus (Stockbit/vlbury). |

---

## 🌐 PLATFORM & METODE SCRAPING  

### 1. X (Twitter)  

| Metode | Tools | Keterangan |
|--------|-------|------------|
| **API v2 (Academic/Pro)** | Tweepy, requests | Rate limit tinggi, filter by keyword, lang:id. |
| **Scraping tanpa API** | `snscrape` (Python library) | Tidak perlu key, bisa ambil tweet historis dengan query. |

**Query keyword:** `$KODE IPO`, `#IPO2026`, `nama emiten IPO`.  
**Data diambil:** teks, like, retweet, reply, followers akun, verified status.

### 2. TikTok  

| Metode | Tools | Keterangan |
|--------|-------|------------|
| **Scraping web** | Selenium + BeautifulSoup | Buka halaman pencarian, scroll, ambil HTML. |
| **Third-party API** | `TikTokApi` (unofficial) | Rentan perubahan, perlu maintenance. |
| **Manual sampling** | Download video kompilasi | Untuk analisis sentimen jika data sulit diambil otomatis. |

**Data diambil:** Judul/deskripsi video, hashtag, komentar, like, share, follower akun.  
**Klasifikasi sentimen** bisa dari *speech-to-text* (opsional) atau analisis teks deskripsi & komentar.

### 3. Stockbit  

| Metode | Tools | Keterangan |
|--------|-------|------------|
| **Web scraping** | Selenium + BeautifulSoup | Login jika perlu, buka *stream* kode saham, scroll. |
| **API tidak resmi** | Reverse-engineering endpoint | Risiko diblokir. |

**Data diambil:** Postingan (teks), like, komentar, user (follower, verified/analis).

### 4. Telegram  

| Metode | Tools | Keterangan |
|--------|-------|------------|
| **API resmi (MTProto)** | `Telethon` (Python) | Perlu `api_id` & `api_hash`, bergabung di grup publik. |

**Grup target:** Cari grup saham/IPO dengan kata kunci “saham”, “IPO”, “bandar”.  
**Data diambil:** Pesan teks, reaksi (jika ada), views, pengirim.

### 5. Reddit  

| Metode | Tools | Keterangan |
|--------|-------|------------|
| **API resmi** | `praw` (Python) | Perlu client_id & secret, rate limit 60/menit. |

**Subreddit:** `r/indonesia`, `r/finansial`, `r/stockmarket`.  
**Data diambil:** Judul, selftext, komentar, upvotes.

### 6. Forum Kaskus / Lainnya  

| Metode | Tools | Keterangan |
|--------|-------|------------|
| **Web scraping** | `requests` + `BeautifulSoup` | Situs statis, mudah di-parse. |
| **Selenium** (jika dinamis) | Selenium | Untuk infinite scroll. |

**Data diambil:** Thread title, post content, reply, timestamp.

---

## 🧹 PREPROCESSING TEKS  

1. **Cleaning:** Hapus URL, mention, hashtag (bisa disimpan terpisah), emoji (konversi ke teks atau hapus).  
2. **Case folding:** Semua lowercase.  
3. **Tokenisasi:** Pecah kalimat menjadi kata.  
4. **Stopword removal:** Pakai `Sastrawi` (bahasa Indonesia).  
5. **Slang normalizer:** Tambahkan kamus manual untuk istilah pasar modal:  
   - *cuan* → untung  
   - *nyangkut* → rugi / tertahan  
   - *lambo* → keuntungan besar  
   - *gorengan* → saham yang dimanipulasi  
   - *bandar* → pemain besar  
   - *auto lotre* → pasti untung  
   - *FOMO* → fear of missing out  

---

## 🧠 KLASIFIKASI SENTIMEN  

### Model Utama  

- **IndoBERT** yang di-*fine-tune* pada dataset sentimen pasar modal Indonesia.  
  - Label: `buy` (+1), `netral` (0), `sell` (–1).  
  - Library: `transformers` (HuggingFace).

### Fallback Rule-Based  

Jika model tidak tersedia, gunakan daftar kata:  

- **Buy:** *beli, masuk, potensi cuan, target harga, rekomendasi beli, auto lotre, pasti naik, IPO murah*  
- **Sell:** *jual, hindari, nyangkut, turun, rugi, overvalued, mahal, hati-hati*  
- **Netral:** kata selain itu, atau skor sentimen mendekati nol.

---

## 🐺 MMI – Market Maker Influence (25%)  

**Tujuan:** Mengidentifikasi akun‑akun “bandar” besar/kecil dari berbagai platform dan arah sentimennya.

### Klasifikasi Akun Bandar  

| Tipe | Follower (X/TikTok) | Ciri Tambahan |
|------|---------------------|---------------|
| 🦈 **Big Shark** | >100.000 | Sering dikutip, bisa jadi corong opini. |
| 🐟 **Small Shark** | 1.000–100.000 | Engagement rate tinggi (>5%), sering viral. |
| 🐠 **Piranha** | <1.000 (viral) | Postingan mendadak viral, suara “rakyat” yang bisa jadi boneka. |

Untuk platform tanpa follower (mis. Telegram), gunakan **jumlah views/reaksi** sebagai proksi pengaruh.

### Rumus MMI  

```

MMI = ───────────────────────────
Σ ( w_size × w_eng )

```

- **S** = sentimen postingan: +1 (buy), 0 (netral), –1 (sell)  
- **w_size** = log₁₀(followers + 1)  
- **w_eng** = (like + retweet + komentar + share) / followers → cap di 1,0  

### Cara Pengisian  

1. Kumpulkan semua postingan/video terkait IPO dari semua platform selama periode pengamatan (1–7 hari sebelum listing).  
2. Filter akun dengan followers >1.000 atau engagement rate >5% (atau views >100.000 untuk TikTok).  
3. Labeli sentimen tiap postingan (pakai model NLP atau manual sampling).  
4. Hitung MMI dengan formula di atas.

### 🧾 Worksheet MMI  

| No | Platform | Username | Followers | Eng. (like+RT+reply) | w_size | w_eng | S | w_size×w_eng×S |
|----|----------|----------|-----------|-----------------------|--------|-------|---|----------------|
| 1  | X        | @traderA | 250.000   | 5.200                 | 5,40   | 0,02  | +1| 0,108          |
| 2  | TikTok   | @cuanID  | 500.000   | 120.000               | 5,70   | 0,24  | +1| 1,368          |
| 3  | Stockbit | analisX  | 12.500    | 2.500                 | 4,10   | 0,20  | 0 | 0              |
| 4  | Telegram | PumpGrup | (views 50k)| 50.000              | 4,70*  | 1,0   | -1| –4,70          |
| ...|          |          |           |                       |        |       |   |                |
| **Total** | | | | | Σ w_size×w_eng = ___ | | | Σ atas = ___ |

*Untuk Telegram tanpa follower, gunakan log₁₀(views+1) sebagai w_size, w_eng = reaksi/views.

**MMI = Σ (w_size×w_eng×S) / Σ (w_size×w_eng) = ___**

> **Bandarmology Insight:**  
> - MMI > 0,8 dari sedikit akun besar → *gorengan* sengaja.  
> - MMI positif tinggi dengan banyak akun kecil → akumulasi organik, lebih sehat.  
> - MMI negatif kuat → bandar tebar takut, potensi *mark‑down*.

---

## 📣 SC, SK, ALIGN – Sentimen & Keselarasan (40% gabungan)  

**Tujuan:** Memisahkan suara konten (postingan/video) dan respons massa (komentar), lalu menguji keselarasannya.

### 1. Sentimen Konten (SC) – 15%  

```

SC = (N_buy – N_sell) / N_total

```

Kumpulkan **semua postingan/video** dari semua platform yang relevan. Labeli sebagai `buy` (+1), `netral` (0), `sell` (–1).  
Dataset yang sama seperti di MMI.

### 2. Sentimen Komentar (SK) – 15%  

```

SK = (N_buy_komentar – N_sell_komentar) / N_total_komentar

```

Ambil semua komentar langsung dari postingan/video yang sudah dikumpulkan. Labeli dengan model yang sama.

### 3. Alignment (ALIGN) – 10%  

```

ALIGN = (1/N) × Σ keselarasan_i

```

Untuk setiap **postingan**, bandingkan sentimen kontennya dengan sentimen rata‑rata komentarnya.  
- Jika sama (buy vs buy, sell vs sell) → skor 1  
- Jika salah satu netral → skor 0,5  
- Jika bertolak belakang (buy vs sell) → skor 0  

Kemudian rata‑ratakan semua skor postingan.

### 🧾 Worksheet SC, SK, ALIGN  

| Platform | Postingan Buy | Postingan Netral | Postingan Sell | Total Postingan |
|----------|---------------|------------------|----------------|-----------------|
| X        | ___           | ___              | ___            | ___             |
| TikTok   | ___           | ___              | ___            | ___             |
| Stockbit | ___           | ___              | ___            | ___             |
| Lainnya  | ___           | ___              | ___            | ___             |
| **Total**| **___**       | **___**           | **___**         | **___**          |

**SC = (Total Buy – Total Sell) / Total Postingan = ___**

| Platform | Komentar Buy | Komentar Netral | Komentar Sell | Total Komentar |
|----------|---------------|-----------------|---------------|----------------|
| X        | ___           | ___             | ___           | ___            |
| TikTok   | ___           | ___             | ___           | ___            |
| Stockbit | ___           | ___             | ___           | ___            |
| Lainnya  | ___           | ___             | ___           | ___            |
| **Total**| **___**       | **___**          | **___**        | **___**         |

**SK = (Total Buy Komentar – Total Sell Komentar) / Total Komentar = ___**

**ALIGN** (estimasi):  
- % komentar selaras = ___% → ALIGN ≈ ___

> **Divergensi berbahaya:** SC tinggi (konten gembar‑gembor buy) tapi SK rendah dan ALIGN < 0,5 → *barang lagi disebar, ombak buatan.* Sebaliknya, konten netral tapi komentar penuh buy → keyakinan tumbuh dari bawah.

---

## 🌫️ D – Indeks Keraguan Forum (15%)  

**Tujuan:** Menangkap keraguan tersembunyi di forum diskusi yang lebih bebas dan mendalam (Telegram grup, Reddit, Kaskus).

### Kata Kunci Keraguan (dapat disesuaikan)  

*“ragu”, “wait and see”, “jebakan”, “bandar nyebar”, “hati‑hati”, “belum yakin”, “takut nyangkut”, “kemahalan”, “overvalued”, “jangan dulu”, “mending lewat”, “gak dulu”, “tunggu dulu”*

### Rumus  

```

D = Jumlah komentar/pesan mengandung keraguan / Total komentar/pesan terkait IPO

```

**Keyakinan Pasar = 1 – D**

### 🧾 Worksheet D  

| Sumber Forum | Total Pesan IPO | Pesan Ragu |
|--------------|-----------------|------------|
| Telegram Grup | ___             | ___        |
| Reddit       | ___             | ___        |
| Kaskus       | ___             | ___        |
| **Total**    | **___**         | **___**    |

**D = Total Pesan Ragu / Total Pesan = ___**

**1 – D = ___**

> **Bandarmology Insight:**  
> - D tinggi saat awal masa penawaran → bandar mungkin belum selesai akumulasi.  
> - D turun drastis mendekati listing → euforia puncak, siap‑siap *distribusi*.  
> - D tetap tinggi hingga listing → pasar tidak percaya, IPO berisiko sepi peminat.

---

## 🎓 SA – Sentimen Analis (20%)  

**Platform:** Stockbit (postingan analis terverifikasi/populer), vlbury, rekomendasi resmi dari sekuritas.

### Konversi Rekomendasi  

| Rekomendasi | Skor |
|-------------|:----:|
| **Buy** tegas (target harga jelas, tanpa banyak syarat) | +1 |
| **Buy** dengan catatan (buy on weakness, spekulatif) | +0,5 |
| **Hold** / Netral | 0 |
| **Sell** / Kurangi | –1 |

### Cara Pengisian  

1. Scrape postingan analis dari Stockbit (cari yang memiliki badge “Analis” atau follower >50k) dan vlbury.  
2. Baca rekomendasi mereka, konversi ke skor.  
3. Rata‑ratakan.

### 🧾 Worksheet SA  

| Analis / Sumber | Rekomendasi | Skor |
|-----------------|-------------|:----:|
| 1. @analisA (Stockbit) | Buy, TP 500 | +1 |
| 2. @investorB (vlbury) | Hold dulu | 0 |
| 3. Sekuritas XYZ | Buy dengan catatan | +0,5 |
| ... | | |
| **Rata‑rata** | | **SA = ___** |

> **Catatan:** Jika mayoritas analis seragam *buy* tanpa dasar fundamental kuat, waspadai *pom‑pom* bandar. Analis independen biasanya lebih seimbang.

---

## 🧮 FORMULA SENTIMEN & NORMALISASI  

### 1. Hitung Sentimen Mentah  

```

Sentiment_Raw = 0,25×MMI + 0,15×SC + 0,15×SK + 0,10×ALIGN + 0,15×(1–D) + 0,20×SA

```

Nilai ini berkisar antara –1 (sangat bearish) hingga +1 (sangat bullish).

### 2. Normalisasi ke Skala 0–100  

```

Sentiment_Norm = (Sentiment_Raw + 1) / 2 × 100

```

### 3. Interpretasi  

| Skor Norm | Kategori | Arti |
|:---------:|----------|------|
| **> 75** | 🟢 **Bullish Kuat** | Bandar mendorong, konten/komentar selaras, forum yakin, analis merekomendasikan beli. Konfirmasi banyak pihak. |
| **50 – 75** | 🟡 **Bullish Moderat** | Optimisme ada, tetapi ada sedikit keraguan atau pertentangan. |
| **25 – 50** | 🟠 **Mixed / Wait & See** | Sinyal campur. Banyak keraguan atau divergensi antara konten dan komentar. |
| **< 25** | 🔴 **Bearish / Hindari** | Pasar takut, bandar mungkin mendistribusikan, forum penuh peringatan. |

---

## 🧪 CONTOH PERHITUNGAN LENGKAP  
*(Data fiktif 3 hari sebelum listing PT. Teknologi Maju Bersama)*  

### Data Dikumpulkan  

| Indikator | Nilai |
|-----------|:-----:|
| MMI | 0,87 |
| SC | 0,50 |
| SK | 0,05 |
| ALIGN | 0,70 |
| 1–D | 0,70 |
| SA | 0,80 |

### Perhitungan  

```

Sentiment_Raw = 0,25×0,87 + 0,15×0,50 + 0,15×0,05 + 0,10×0,70 + 0,15×0,70 + 0,20×0,80
= 0,2175 + 0,075 + 0,0075 + 0,07 + 0,105 + 0,16
= 0,635

Sentiment_Norm = (0,635 + 1) / 2 × 100 = 81,75  🟢 Bullish Kuat

```

### Analisis Singkat  

- **MMI tinggi (0,87):** Banyak akun besar dan kecil mendorong beli.  
- **SC = 0,50:** Konten cukup positif.  
- **SK rendah (0,05):** Komentar cenderung netral, tidak sekuat konten.  
- **ALIGN 0,70:** Komentar cukup selaras, tapi ada sebagian yang tidak setuju.  
- **1–D = 0,70:** Forum masih ada sedikit keraguan (30% ragu).  
- **SA 0,80:** Analis rata‑rata merekomendasikan beli.  

**Kesimpulan:** Secara sentimen, IPO ini disambut positif. Namun waspadai divergensi kecil antara konten dan komentar – mungkin ada segelintir yang sudah mulai ragu. Secara umum, sinyal **Bullish Kuat**.

---

## 🛠️ TOOLS & TEKNIKAL IMPLEMENTASI  

| Tugas | Tools / Library |
|-------|-----------------|
| **Scraping X** | `snscrape`, Tweepy (API v2) |
| **Scraping TikTok** | Selenium + BeautifulSoup, `TikTokApi` (unofficial) |
| **Scraping Stockbit** | Selenium + BeautifulSoup |
| **Scraping Telegram** | `Telethon` |
| **Scraping Reddit** | `praw` |
| **Scraping Forum** | `requests` + `BeautifulSoup` |
| **Preprocessing Teks** | `re`, `Sastrawi`, custom slang dictionary |
| **Klasifikasi Sentimen** | `transformers` (IndoBERT), `scikit-learn` (jika pakai model tradisional) |
| **Analisis Data** | `pandas`, `numpy` |
| **Dashboard** | `Streamlit`, `Grafana` |
| **Notifikasi Otomatis** | Telegram Bot, email |

---

## ⚠️ CATATAN PENTING  

1. **Bahasa:** Model NLP harus sering di‑update dengan slang pasar modal terbaru.  
2. **Etika Scraping:** Patuhi `robots.txt`, jangan DDoS, gunakan rate limiting.  
3. **Validasi Manual:** Sentimen dari TikTok sering butuh verifikasi manual karena banyak ironi/sarkasme.  
4. **Bobot Dinamis:** Lakukan backtest dengan regresi logistik menggunakan data IPO historis untuk mengkalibrasi ulang bobot MMI, SC, SK, dll.  

---

*“Sentiment is the wind, bandarmology is the current. Navigate both, and you’ll never sail blind into an IPO.”*
```