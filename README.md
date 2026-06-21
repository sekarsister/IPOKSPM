# 🧠 **IPO MASTER SCORE**  
### *Fundamental × Sentiment & Bandarmology*  
> **Interactive Decision Framework** – klik setiap pilar untuk detailnya.

---

## 📌 **STRUKTUR SKOR**

| Komponen | Bobot | Fokus |
|----------|:----:|-------|
| 🔵 **Fundamental Score** | **60%** | Seberapa yakin dana IPO akan jadi mesin laba? |
| 🔴 **Sentiment & Bandarmology** | **40%** | Apakah pasar & bandar selaras mendukung? |
| 🎯 **Master Score** | **100%** | Keputusan akhir: timing + kualitas |

---

# 🔵 BAGIAN 1: FUNDAMENTAL SCORE (60%)

### 🏗️ Enam Pilar Fundamental – klik untuk membuka

<details>
<summary><strong>🅰️ Pilar A – Kualitas Penggunaan Dana IPO (30%)</strong></summary>

| Sub | Komponen | Skor Maks | Kriteria |
|:---:|----------|:--------:|----------|
| A1 | Ekspansi Produktif (capex, R&D, akuisisi) | 40 | Porsi >50% dari total dana IPO |
| A2 | Modal Kerja untuk dorong penjualan | 30 | Disertai proyeksi pertumbuhan konkret |
| A3 | Pelunasan Utang berbunga tinggi | 20 | Bunga >10% → langsung angkat laba |
| A4 | Dana Cadangan / Tidak Jelas | -10 | Pengurang jika tanpa rencana |
| A5 | Target Kuantitatif Spesifik | +10 | Bonus jika ada proyeksi waktu & kapasitas |

> **Skor A** = (A1+A2+A3+A4+A5) / 100 → normalisasi 0–1

</details>

<details>
<summary><strong>🅱️ Pilar B – Kesehatan Keuangan Historis (20%)</strong></summary>

| Rasio | Skor Maks | Ambang Ideal |
|-------|:--------:|---------------|
| B1. Pertumbuhan Pendapatan (CAGR 3th) | 25 | >15% |
| B2. Margin Laba Bersih | 25 | >10% |
| B3. ROE | 20 | >15% |
| B4. DER | 15 | <0.5× |
| B5. Arus Kas Operasi / Laba Bersih | 15 | >1.0× |

> **Skor B** = Total / 100

</details>

<details>
<summary><strong>🅲️ Pilar C – Prospek Industri & Posisi Kompetitif (20%)</strong></summary>

| Sub | Komponen | Skor Maks | Kriteria |
|:---:|----------|:--------:|----------|
| C1 | Pertumbuhan Industri (CAGR 5th) | 30 | >10% → 30 |
| C2 | Market Share & Peringkat | 25 | Top 3 → 25 |
| C3 | Barrier to Entry (Moat) | 25 | Paten, regulasi, brand, jaringan |
| C4 | Diversifikasi Produk/Pelanggan | 10 | Tidak bergantung >30% pada satu |
| C5 | Jumlah Kompetitor Besar | 10 | <5 → 10 |

> **Skor C** = Total / 100

</details>

<details>
<summary><strong>🅳️ Pilar D – Valuasi Harga IPO (15%)</strong></summary>

| P/E IPO vs Rata-rata Industri | Skor |
|-------------------------------|:----:|
| < 0.8× | 100 |
| 0.8 – 1.0× | 80 |
| 1.0 – 1.2× (wajar) | 60 |
| 1.2 – 1.5× | 40 |
| 1.5 – 2.0× | 20 |
| > 2.0× atau perusahaan rugi | 0 |

> **Skor D** = skor / 100

</details>

<details>
<summary><strong>🅴️ Pilar E – Kualitas Manajemen & Tata Kelola (10%)</strong></summary>

| Sub | Komponen | Skor Maks |
|:---:|----------|:--------:|
| E1 | Pengalaman & Rekam Jejak | 30 |
| E2 | Kepemilikan Manajemen Pasca-IPO (>30%) | 25 |
| E3 | Independensi Dewan Komisaris (>50%) | 20 |
| E4 | Transparansi Prospektus | 25 |

> **Skor E** = Total / 100

</details>

<details>
<summary><strong>🅵️ Pilar F – Risiko & Mitigasi (5%)</strong></summary>

| Sub | Komponen | Skor Maks |
|:---:|----------|:--------:|
| F1 | Identifikasi Risiko Lengkap | 40 |
| F2 | Strategi Mitigasi Konkret | 40 |
| F3 | Tanpa Ketergantungan >20% | 20 |

> **Skor F** = Total / 100

</details>

### 🔢 **Formula Fundamental**

$$
\text{Fundamental Score} = 0.30A + 0.20B + 0.20C + 0.15D + 0.10E + 0.05F
$$

| Skor | Kategori |
|:----:|----------|
| **> 75** | 🟢 High Conviction |
| **60–75** | 🟡 Moderate |
| **40–60** | 🟠 Wait & See |
| **< 40** | 🔴 Avoid |

---

# 🔴 BAGIAN 2: SENTIMENT & BANDARMOLOGY SCORE (40%)

<details>
<summary><strong>🐺 Market Maker Influence (MMI) – 25%</strong></summary>

**Rumus:**

$$
MMI = \frac{ \sum ( w_{\text{size}} \times w_{\text{eng}} \times S ) }{ \sum ( w_{\text{size}} \times w_{\text{eng}} ) }
$$

- **S** = sentimen (+1 buy, 0 netral, -1 sell)  
- **w_size** = log10(followers+1)  
- **w_eng** = (like+retweet+komentar)/followers (cap 1)

| Tipe Akun | Follower | Bobot |
|-----------|----------|:-----:|
| 🦈 Big Shark | >100k | tinggi |
| 🐟 Small Shark | 1k–100k | sedang |
| 🐠 Piranha | <1k (viral) | rendah |

> **Bandarmology insight:** MMI >0.8 dari sedikit akun besar → *gorengan*. MMI tinggi dengan banyak akun kecil → akumulasi organik.

</details>

<details>
<summary><strong>📣 Sentimen Konten (SC), Komentar (SK), Alignment (ALIGN)</strong></summary>

| Indeks | Rumus | Bobot |
|--------|-------|:----:|
| **SC** | (N_buy – N_sell) / N_total | 15% |
| **SK** | (N_buy – N_sell) / N_total | 15% |
| **ALIGN** | Proporsi komentar selaras konten (1=setuju, 0.5=netral, 0=bertolak) | 10% |

> **Divergensi:** SC tinggi + SK rendah + ALIGN < 0.5 → *barang disebar, ombak buatan.*

</details>

<details>
<summary><strong>🌫️ Indeks Keraguan Forum (D) – 15%</strong></summary>

$$
D = \frac{\text{Jumlah komentar ragu}}{\text{Total komentar IPO}}
$$

Kata kunci: *“ragu”, “wait and see”, “jebakan”, “bandar nyebar”, “belum yakin”*

**Keyakinan Pasar = 1 – D**

</details>

<details>
<summary><strong>🎓 Sentimen Analis (SA) – 20%</strong></summary>

| Rekomendasi | Skor |
|-------------|:----:|
| Buy tegas | +1 |
| Buy dengan catatan | +0.5 |
| Hold | 0 |
| Sell | -1 |

> **SA** = rata-rata skor semua analis di Stockbit/vlbury.

</details>

### 🔢 **Formula Sentimen (mentah → normalisasi)**

$$
Sentiment_{raw} = 0.25\cdot MMI + 0.15\cdot SC + 0.15\cdot SK + 0.10\cdot ALIGN + 0.15\cdot(1-D) + 0.20\cdot SA
$$

$$
Sentiment_{norm} = (Sentiment_{raw} + 1) / 2
$$

| Skor Norm | Kategori |
|:---------:|----------|
| **> 0.75** | 🟢 Bullish Kuat |
| **0.50–0.75** | 🟡 Bullish Moderat |
| **0.25–0.50** | 🟠 Mixed |
| **< 0.25** | 🔴 Bearish |

---

# ⚖️ **GABUNGAN: IPO MASTER SCORE**

### **Formula Akhir**

$$
\text{Master Score} = 0.60 \times \text{Fundamental} + 0.40 \times \text{Sentiment}_{norm}
$$

| Master Score | Sinyal | Aksi |
|:------------:|--------|------|
| **> 80** | 🟢 **STRONG BUY** | Konfirmasi fundamental + sentimen. Entry aman. |
| **65 – 80** | 🟡 **BUY WITH CAUTION** | Prospek bagus, akumulasi bertahap. |
| **45 – 65** | 🟠 **HOLD / WAIT** | Tunggu kejelasan pasca-IPO. |
| **< 45** | 🔴 **AVOID** | Jangan masuk. |

---

## 🧪 **CONTOH PERHITUNGAN INTERAKTIF**  
*PT. Teknologi Maju Bersama Tbk. (Fiktif)*

<details>
<summary><strong>🔵 Fundamental Score – klik untuk lihat detail</strong></summary>

| Pilar | Skor Mentah | Normalisasi |
|-------|:----------:|:-----------:|
| A | 90/100 | 0.90 |
| B | 100/100 | 1.00 |
| C | 100/100 | 1.00 |
| D | 80/100 | 0.80 |
| E | 100/100 | 1.00 |
| F | 100/100 | 1.00 |

**Fundamental Score** = 0.30×0.90 + 0.20×1.00 + 0.20×1.00 + 0.15×0.80 + 0.10×1.00 + 0.05×1.00 = **0.940 (94.0)** 🟢

</details>

<details>
<summary><strong>🔴 Sentiment Score – klik untuk lihat detail</strong></summary>

| Indikator | Nilai |
|-----------|:-----:|
| MMI | 0.87 |
| SC | 0.50 |
| SK | 0.05 |
| ALIGN | 0.70 |
| 1-D (Keyakinan) | 0.70 |
| SA | 0.80 |

**Sentiment Raw** = 0.25×0.87 + 0.15×0.50 + 0.15×0.05 + 0.10×0.70 + 0.15×0.70 + 0.20×0.80 = **0.635**  
**Sentiment Norm** = (0.635+1)/2 = **0.8175** 🟢

</details>

### 🎯 **Master Score**

$$
0.60 \times 0.940 + 0.40 \times 0.8175 = \mathbf{0.891 \ (89.1)} \quad \text{🟢 STRONG BUY}
$$

> **Kesimpulan:** Fundamental kokoh, bandar & sentimen mendukung. Dana IPO diyakini akan mengangkat laba, dan pasar merespons positif.

---

## 🛠️ **TEKNIKAL IMPLEMENTASI**

| Tugas | Tools |
|-------|-------|
| Scraping | snscrape, Selenium, BeautifulSoup, telethon, praw |
| NLP | IndoBERT (fine-tuned), Sastrawi |
| Fundamental | Prospektus parsing, analisis manual/otomatis |
| Backend | Python (pandas, scikit-learn) |
| Dashboard | Streamlit / Grafana |

---

*“What gets measured, gets managed. Now you measure both fundamentals and market psychology before every IPO.”*  
