# WS-03: Literature Mapping & Gap

> **Bab 3 — Literature Review, Research Gap & Baseline**

---

## Ringkasan Materi

### Literature Review = Positioning, Bukan Ringkasan

Literature review bukan merangkum paper satu per satu. Pendekatan yang benar adalah **concept-centric** — organisasi berdasarkan tema, metode, atau variabel. Tujuan: menemukan **pola, kontradiksi, dan gap**.

### Empat Jenis Research Gap

| Jenis Gap | Deskripsi | Contoh |
|-----------|----------|--------|
| **Performance Gap** | Performa belum memadai | Akurasi deteksi hanya 78% pada kasus tertentu |
| **Method Gap** | Pendekatan belum diterapkan | Belum ada yang pakai transformer untuk task ini |
| **Data Gap** | Dataset terbatas/tidak representatif | Semua studi pakai dataset sintetis |
| **Context Gap** | Belum diuji pada konteks berbeda | Belum ada evaluasi di negara berkembang |

Gap terkuat = kombinasi 2+ jenis.

### Systematic Search Strategy

1. **Database**: IEEE Xplore, ACM DL, Scopus, Google Scholar
2. **Boolean query** yang terdokumentasi eksplisit
3. **Snowballing**: backward (telusuri referensi) + forward (cari yang mengutip)
4. Klaim "belum ada penelitian" harus didukung **bukti pencarian**

### Baseline Selection — 3 Kriteria

| Kriteria | Pertanyaan |
|----------|-----------|
| **Relevan** | Apakah menyelesaikan masalah yang sama? |
| **Representatif** | Apakah mewakili common practice? |
| **State-of-the-Art** | Apakah terbaru/terbaik? |

Membandingkan deep learning 2024 dengan decision tree sederhana tanpa justifikasi = **straw man comparison** (perbandingan tidak jujur).

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan baca literatur | Mencari solusi yang sudah ada | Memahami apa yang belum terjawab |
| Cara membaca paper | Tutorial, how-to | Metode, limitasi, gap |
| Baseline | Framework terpopuler | State-of-the-art yang rigorous |
| Dokumentasi pencarian | Tidak diperlukan | Wajib (reproducible) |

### Istilah Penting

- **Concept-centric** — Organisasi literatur berdasarkan konsep/metode, bukan per penulis
- **Snowballing** — Backward (telusuri referensi) + Forward (cari yang mengutip paper kunci)
- **Research Position** — Pernyataan eksplisit posisi riset terhadap studi sebelumnya
- **Straw man comparison** — Memilih baseline lemah agar metode sendiri terlihat lebih baik

---

## Template A.3 — Literature Mapping & Gap Identification

```
LITERATURE MAPPING

Topik      : Deteksi Insider Threat pada Sistem Enterprise
Database   : Google Scholar, IEEE Xplore, Springer, MDPI
Query      : ("insider threat detection") AND ("machine learning"
           OR "deep learning" OR "UEBA") AND ("enterprise"
           OR "SIEM") NOT ("medical")
Tahun      : 2020–2024
Hasil awal : sekitar 120 paper → Screening → 5 paper final yang benar-benar relevan

Literature Matrix (concept-centric):

| Study | Tahun | Method | Data | Result | Limitation |
|-------|-------|--------|------|--------|------------|
|       |       |        |      |        |            |

Pola yang ditemukan:
  Metode dominan     : Random Forest dan LSTM paling sering dipakai;
                   tren terbaru mulai ke Transformer dan GNN

  Dataset umum       : CERT Insider Threat Dataset (r5.2 dan r6.2)
                   dipakai hampir semua studi
  Limitasi berulang  : Hampir nggak ada paper yang bandingin
                   rule-based SIEM vs ML secara langsung
                   pakai metrik yang sama

GAP IDENTIFICATION

Gap 1: [Jenis: performance / METHOD / data / context]
  Deskripsi    : Belum ada studi yang khusus bandingin
                rule-based SIEM vs ML-based UEBA pakai
                metrik yang seragam (detection rate,
                false positive rate, waktu deteksi)
  Bukti        : Dari 5 paper yang direview, semuanya
                fokus improve ML-nya aja — nggak ada
                yang jadiin rule-based SIEM sebagai
                baseline pembanding langsung
  Signifikansi : Tim SOC di enterprise masih pakai
                rule-based SIEM karena belum ada bukti
                empiris yang cukup untuk beralih ke ML

Gap 2: [Jenis: Performance Gap]
  Deskripsi    : Rule-based SIEM diketahui punya
                detection lag tinggi dan false positive
                banyak, tapi belum pernah diukur
                selisihnya secara kuantitatif vs ML
  Bukti        : Transformer-GNN (IJMRAI, 2025) nunjukin
                false positive turun 40% vs rule-based,
                tapi nggak ada studi yang fokus khusus
                ngukur gap performa ini secara sistematis
  Signifikansi : Tanpa angka perbandingan yang jelas,
                susah buat organisasi justifikasi
                investasi ke sistem ML-based UEBA

Baseline Selection:
| Baseline | Relevansi | Representatif | Source |
|----------|-----------|---------------|--------|
|          |           |               |        |
```

---

## Latihan 1 — Concept-Centric Literature Table

Gunakan topik riset dari WS-02. Cari minimal 5 paper relevan menggunakan Google Scholar atau database lain.

**Topik riset:**  Deteksi Insider Threat pada Sistem Enterprise
**Query pencarian:** ("insider threat detection") AND ("machine learning" OR "UEBA") AND ("SIEM" OR "enterprise")
**Database:** Google Scholar, IEEE Xplore, MDPI, Springer

| # | Study | Tahun | Method | Dataset | Result | Limitasi |
|---|-------|-------|--------|---------|--------|----------|
| 1 | Villarreal-Vasquez et al. (IEEE TDSC) | 2023 | LSTM | Log jaringan komersial nyata (38,9 juta event, 30 komputer) | LSTM terbukti bisa mendeteksi anomali di jaringan enterprise sungguhan | Cuma diuji di satu jaringan, nggak ada bandingannya sama rule-based SIEM |
| 2 | Alzaabi & Mehmood (IEEE Access) | 2024 | Review: RF, SVM, LSTM, Isolation Forest | CERT Insider Threat Dataset | RF dan LSTM paling bagus; RF unggul kalau fiturnya lengkap | Kebanyakan studi masih pakai dataset sintetis, belum coba di lingkungan enterprise asli |
| 3 | Sherje et al. (Springer) | 2024 | RF, SVM, LSTM, Isolation Forest, Autoencoder | CERT Insider Threat Dataset | RF tetap terbaik; pemilihan fitur pakai RFE bikin F1-score naik lumayan | Nggak ada yang bandingkan langsung sama rule-based SIEM |
| 4 | Hassan et al. — ZenGuard (Nature) | 2025 | Zero Trust + SIEM + UEBA + ML | Simulasi enterprise (CERT, UNSW-NB15) | Lebih akurat dari rule-based, bisa deteksi real-time | Implementasinya ribet, belum dicoba di enterprise skala besar yang sesungguhnya |
| 5 | Transformer-GNN Ensemble (IJMRAI) | 2025 | Transformer + GNN vs. rule-based SIEM | CERT, UNSW-NB15, TON_IoT | F1-score 0.90, false positive turun 40%, waktu triage turun 78% dibanding rule-based | Butuh GPU, susah direplikasi di lingkungan dengan sumber daya terbatas |

**Pola yang terlihat — Metode dominan:** Random Forest dan LSTM paling sering muncul. Belakangan mulai banyak yang pakai Transformer dan Graph Neural Network
**Limitasi yang berulang:** Hampir nggak ada paper yang benar-benar membandingkan rule-based SIEM dengan ML secara head-to-head pakai metrik yang sama

---

## Latihan 2 — Gap Identification

Berdasarkan tabel di Latihan 1, identifikasi gap.

| Jenis Gap | Ditemukan? | Gap Statement |
|-----------|-----------|---------------|
| Performance Gap | [ V ] Ya / [ ] Tidak | Rule-based SIEM diketahui sering kecolongan dan banyak false alarm, tapi belum ada yang ngukur selisihnya secara kuantitatif dibanding ML |
| Method Gap | [ V ] Ya / [ ] Tidak | Belum ada studi yang khusus bandingin rule-based vs ML-based UEBA pakai metrik yang sama: detection rate, false positive rate, dan waktu deteksi |
| Data Gap | [ V ] Ya / [ ] Tidak | Hampir semua pakai CERT Insider Threat Dataset yang sifatnya sintetis — belum ada yang pakai log enterprise nyata secara terbuka |
| Context Gap | [ V ] Ya / [ ] Tidak | Penelitian yang ada hampir semuanya dari konteks US atau Eropa, belum ada yang coba di organisasi enterprise negara berkembang |

**Gap utama yang dipilih:** Method Gap + Performance Gap
**Mengapa gap ini penting (bukan sekadar "belum ada yang meneliti")?**
> Bukan cuma soal "belum ada yang neliti". Masalahnya adalah tim keamanan di perusahaan sekarang masih pakai rule-based SIEM karena belum ada bukti yang cukup kuat untuk beralih ke ML. Kalau kita bisa kasih data perbandingan yang jelas — berapa persen lebih akurat, berapa kali lebih cepat — itu langsung bisa jadi dasar keputusan yang nyata buat tim IT Security.

---

## Latihan 3 — Baseline Selection

Pilih 2 baseline dari literatur yang sudah dibaca.

| # | Baseline | Mengapa Relevan | Mengapa Representatif | Apakah SOTA? | Sumber |
|---|----------|----------------|----------------------|-------------|--------|
| 1 | Rule-based SIEM | Ini yang lagi dipakai di enterprise sekarang, jadi wajar dijadiin pembanding utama | Hampir semua organisasi enterprise masih pakai ini sebagai standar | Bukan SOTA, tapi ini "kenyataan di lapangan" yang penting untuk dibandingkan | Hassan et al., 2025; IJMRAI 2025 |
| 2 | Random Forest + behavioral features | Tugasnya sama: deteksi insider threat dari log perilaku | Muncul di 3 dari 5 paper yang direview, dianggap ML baseline yang solid | Bukan yang terbaru, tapi cukup kuat sebagai titik tengah sebelum ke deep learning | Alzaabi & Mehmood, 2024; Sherje et al., 2024 |

**Apakah pemilihan baseline ini bisa dianggap straw man?** [ ] Ya / [ ] Tidak
> Justifikasi: Keduanya dipilih bukan supaya metode kita keliatan keren — tapi karena memang itu yang paling relevan. Rule-based SIEM karena itu realita industri, Random Forest karena itu benchmark ML yang konsisten di literatur. Kalau kita menang atas keduanya, hasilnya lebih bisa dipercaya.

---

## Refleksi

> Apa perbedaan antara "belum ada yang meneliti ini" (klaim tanpa bukti) dengan research gap yang valid? Bagaimana cara membuktikan bahwa sebuah gap benar-benar ada?

**Jawaban:**
> Bedanya "belum ada yang neliti ini" sama research gap yang beneran itu ada di buktinya. Kalau cuma bilang "belum ada", reviewer bisa langsung tanya: "kamu udah cari di mana aja? query-nya apa?". Gap yang valid itu bisa dijelasin: paper A neliti X, paper B neliti Y, tapi nggak ada yang gabungin X dan Y di konteks Z. Itu baru bisa disebut gap — karena ada jejak pencariannya, ada alasannya, dan jelas posisi riset kita di mana.
