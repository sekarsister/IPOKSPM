Tentu, ini versi yang lebih keren dengan sentuhan visual dan struktur yang rapi.

```markdown
# 🐋 Analisis IPO via Social Scraping × Bandarmology  
### *“Baca ombak sebelum diterjang.”*

---

## 🧭 FRAMEWORK SINGKAT  
**Lima Langkah dari Data Mentah ke Keputusan Trading**

| Langkah | Nama | Deskripsi Singkat |
|--------|------|-------------------|
| **1** | 🧹 **Scraping & Preprocessing** | Kumpulkan cuitan, postingan, komentar dari Twitter/X, Stockbit, vlbury, forum (Telegram/Reddit/Kaskus). Bersihkan teks dan tokenisasi khusus bahasa Indonesia + slang saham. |
| **2** | 🧠 **Klasifikasi Sentimen** | Pakai model **IndoBERT** yang di-*fine-tune* pada sentimen pasar modal. Setiap teks dilabeli `buy`, `netral`, atau `sell`. |
| **3** | 📊 **Empat Pilar Indeks** | Hitung **MMI** (jejak bandar), **SC & SK** (sentimen konten & komentar), **ALIGN** (keselarasan), **D** (keraguan forum), **SA** (rekomendasi analis). |
| **4** | ⚖️ **Skor Akhir IPO** | Gabungkan semua indeks dengan bobot hasil *backtest*. Formula di bawah. |
| **5** | 🟢🟡🔴 **Keputusan** | Konversi skor ke sinyal: **>0.5** = kuat beli, **0.2–0.5** = spekulatif, **<0.2** = hindari. |

---

## 🧮 FORMULA UTAMA

$$
\text{IPO\_Score} = 0.25\cdot MMI + 0.15\cdot SC + 0.15\cdot SK + 0.10\cdot ALIGN + 0.15\cdot(1-D) + 0.20\cdot SA
$$

> ⚠️ Bobot di atas adalah *default*. Lakukan **backtest** terhadap data IPO historis untuk mengoptimalkan koefisien (misal dengan regresi logistik).

---

## 🐺 MENGAPA BANDARMOLOGY?
Di pasar modal Indonesia, **bandar** adalah dalang di balik layar. Mereka menciptakan ombak – entah itu *mark-up* euforia atau *mark-down* ketakutan – lewat akun-akun digital yang terlihat “organik”. Social scraping biasa hanya melihat sentimen permukaan, tapi kita harus membedah **siapa yang bicara** dan **apakah pesannya tulus**.

---

## 🔍 1. MARKET MAKER INFLUENCE (MMI)
### *Jejak digital sang bandar*

- **Big Shark** 🦈 – Akun dengan pengikut >100k, sering jadi corong narasi.  
- **Small Shark** 🐟 – Akun 1k–100k pengikut, *engagement rate* tinggi, sering dipakai buat kesan organik.  
- **Piranha** 🐠 – Akun kecil mendadak viral, suara “rakyat” yang bisa jadi boneka.

**Rumus MMI:**

$$
MMI = \frac{ \sum_{i} \left( w_{\text{size},i} \times w_{\text{eng},i} \times S_i \right) }{ \sum_{i} \left( w_{\text{size},i} \times w_{\text{eng},i} \right) }
$$

- \( S_i \) = sentimen postingan bandar \( i \) (+1 buy, 0 netral, -1 sell).  
- \( w_{\text{size}} = \log_{10}(\text{follower}+1) \).  
- \( w_{\text{eng}} = \frac{\text{like+retweet+komentar}}{\text{follower}} \) (dicap di 1).

> 🧠 **Bandarmology Insight:**  
> - MMI > 0.8 tapi hanya dari 2–3 akun besar → waspada *gorengan*.  
> - MMI tinggi ditopang banyak akun kecil dengan engagement alami → euforia lebih sehat, akumulasi bisa jadi.  
> - MMI negatif kuat → bandar menebar ketakutan, potensi *mark-down* atau akumulasi murah.

---

## 📣 2. SENTIMEN KONTEN (SC), KOMENTAR (SK), & ALIGNMENT
### *Uji kejujuran opini publik*

- **SC** = \( \frac{N_{buy} - N_{sell}}{N_{total}} \) dari seluruh konten terkait IPO.  
- **SK** = sama, tetapi dari komentar langsung.  
- **ALIGN** = proporsi komentar yang “selaras” dengan sentimen kontennya (1 = setuju, 0.5 = salah satu netral, 0 = bertolak belakang).

> 🧠 **Bandarmology Insight:**  
> - **Divergensi berbahaya:** SC tinggi (konten gembar-gembor *buy*) tapi SK rendah dan ALIGN < 0.5 → komentar penuh keraguan/tolak. *“Barang lagi disebar, ombak buatan.”*  
> - **Konfirmasi kuat:** SC netral tapi SK positif tinggi dan ALIGN tinggi → keyakinan tumbuh dari bawah, bandar mungkin sudah selesai akumulasi.

---

## 🌫️ 3. INDEKS KERAGUAN FORUM (D)
### *“Wait and see” yang tersembunyi*

Forum (Kaskus, Reddit, Telegram grup) sering jadi tempat diskusi lebih jujur. Kata kunci keraguan: *“ragu”, “takut nyangkut”, “jangan-jebakan”, “wait and see”, “bandar nyebar”*.

$$
D = \frac{\text{Jumlah komentar ragu}}{\text{Total komentar IPO}}
$$

- \( 1-D \) = **Tingkat Keyakinan Pasar**.  
- Semakin kecil D, semakin yakin massa. Tapi jika D mendekati 0 dan MMI tinggi, justru bisa jadi puncak *blow-off* – semua sudah naik kapal.

> 🧠 **Bandarmology Insight:**  
> - D masih tinggi saat fase awal IPO → bandar belum selesai mengumpulkan.  
> - D turun drastis → euforia mencapai puncak, siap-siap *distribusi*.

---

## 🎓 4. SENTIMEN ANALIS (SA)
### *Corong resmi yang harus difilter*

Scrape rekomendasi analis di Stockbit atau vlbury. Konversi ke skor:

| Rekomendasi | Skor |
|-------------|------|
| Buy tegas | +1 |
| Buy dengan catatan | +0.5 |
| Hold | 0 |
| Sell | -1 |

\( SA = \text{rata-rata skor semua analis} \).

> 🧠 **Bandarmology Insight:**  
> - Analis seragam *buy* tanpa alasan fundamental → kemungkinan *pom-pom* bandar.  
> - SA tinggi tapi MMI rendah & D tinggi → analis terlalu dini atau ada informasi asimetris; jangan terjebak.

---

## ⚡ 5. SKOR AKHIR & KEPUTUSAN

Gabungkan semua dengan bobot:

$$
IPO\_Score = 0.25\cdot MMI + 0.15\cdot SC + 0.15\cdot SK + 0.10\cdot ALIGN + 0.15\cdot(1-D) + 0.20\cdot SA
$$

| Skor | Sinyal | Tindakan |
|------|--------|----------|
| **> 0.5** | 🟢 Kuat beli | Probabilitas naik tinggi, *entry* di hari listing. |
| **0.2 – 0.5** | 🟡 Spekulatif netral | Tunggu konfirmasi volume/aksi harga. |
| **< 0.2** | 🔴 Hindari | Pasar dingin atau jebakan, *wait and see* aman. |

> 📌 **Backtest:** Latih bobot di atas dengan regresi logistik menggunakan data IPO 2–3 tahun terakhir agar sesuai dengan karakter pasar.

---

## 🧪 CONTOH PERHITUNGAN CEPAT

Data 1 hari sebelum listing:
- **Bandar:** 3 Big Shark (semua *buy*), 10 Small Shark (8 buy, 2 netral) → **MMI = 0.87**  
- **Konten:** 60 buy, 30 netral, 10 sell → **SC = 0.50**  
- **Komentar:** 80 buy, 50 netral, 70 sell → **SK = 0.05**  
- **Alignment:** 60% komentar setuju → **ALIGN = 0.70**  
- **Forum:** 30% ragu → **D = 0.3 → (1-D) = 0.70**  
- **Analis:** Rata-rata skor +0.8 → **SA = 0.80**

$$
\begin{aligned}
IPO\_Score &= 0.25(0.87) + 0.15(0.50) + 0.15(0.05) + 0.10(0.70) + 0.15(0.70) + 0.20(0.80) \\
           &= 0.2175 + 0.075 + 0.0075 + 0.07 + 0.105 + 0.16 \\
           &= \mathbf{0.635} \quad \Rightarrow \quad \text{🟢 Sinyal Kuat Beli}
\end{aligned}
$$

---

## 🛠️ TOOLS & STACK
- **Scraping:** `snscrape`, `Selenium`, `BeautifulSoup`, `telethon`, `praw`  
- **NLP:** `transformers` (IndoBERT), `Sastrawi`, `vaderSentiment` (modifikasi)  
- **Analisis:** `pandas`, `scikit-learn` (regresi logistik), `matplotlib`  
- **Dashboard:** Streamlit / Grafana (opsional)

---

> *“Jika semua orang sudah di atas kapal, kapal akan tenggelam.”*  
> **Social scraping + Bandarmology** membantumu tahu kapan harus ikut berlayar dan kapan harus menepi. Selamat membaca ombak! 🌊
```