# WS-04: Research Question & Hypothesis

> **Bab 4 — Research Question, Contribution & Hypothesis**

---

## Ringkasan Materi

### RQ Bukan Pertanyaan Biasa

Research Question yang baik secara implisit mengandung cetak biru eksperimen: subjek, baseline, metrik, domain, dataset.

| Kualitas | Contoh |
|----------|--------|
| **Buruk** | "Bagaimana pengaruh deep learning terhadap deteksi malware?" |
| **Baik** | "Apakah CNN menghasilkan F1-Score lebih tinggi dari RF pada CIC-MalMem-2022?" |

Perbedaan: RQ yang baik menyebutkan **metode spesifik**, **metrik terukur**, **baseline**, dan **dataset**.

### Tiga Jenis RQ

| Jenis | Pola | Kebutuhan |
|-------|------|-----------|
| **Comparison** | A vs B → mana lebih baik? | ≥ 2 metode, metrik sama |
| **Improvement** | A' vs A → modifikasi lebih baik? | Pre/post, bukti perbaikan |
| **Exploratory** | Faktor X₁...Xₙ → pengaruh terhadap Y? | Multi-variabel, korelasi/regresi |

### Contribution Statement

Tiga jenis kontribusi: **Improvement** (metode terbukti lebih baik), **Comparison** (perbandingan sistematis yang belum ada), **Novel Approach** (pendekatan baru). Kontribusi harus terhubung langsung dengan gap — kontribusi tanpa gap = klaim tanpa justifikasi.

### Hypothesis H₀ / H₁

- **H₀** (Null) = Tidak ada perbedaan signifikan — asumsi default, harus dibuktikan salah
- **H₁** (Alternative) = Ada perbedaan signifikan — diterima hanya jika H₀ ditolak
- Harus **falsifiable**, mengandung **metrik terukur**, dirumuskan **SEBELUM eksperimen**

### Rantai Operasionalisasi

```
RQ → Variable → Metric → Data → Analysis
```

Jika rantai ini tidak lengkap, RQ belum mature. Bi-directional: RQ yang tidak bisa jadi hipotesis testable harus direvisi mundur.

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan pertanyaan | Apa yang harus dibangun? | Apa yang harus dibuktikan? |
| Bentuk jawaban | Sistem yang berfungsi | Bukti empiris terukur |
| Sukses diukur oleh | User satisfaction, uptime | Signifikansi statistik, effect size |
| Jika gagal | Debug dan perbaiki | Laporkan, analisis mengapa |

### Istilah Penting

- **Research Question (RQ)** — Pertanyaan spesifik: variabel terukur + metrik + konteks
- **Contribution Statement** — Apa yang diketahui setelah riset selesai yang sebelumnya belum ada
- **H₀ / H₁** — Null vs Alternative Hypothesis
- **Falsifiability** — Kondisi hipotesis ditolak harus bisa didefinisikan sebelum eksperimen
- **Operationalization** — Proses mewujudkan konsep abstrak menjadi variabel terukur

---

## Template A.4 — RQ-Contribution-Hypothesis

```
RQ-CONTRIBUTION-HYPOTHESIS

Gap Statement  : Belum ada studi yang secara langsung membandingkan
                rule-based SIEM dengan ML-based UEBA menggunakan
                metrik yang sama (detection rate, false positive rate,
                waktu deteksi) untuk kasus insider threat di sistem
                enterprise secara real-time.

Research Question:
  Tipe         : [ V ] Comparison  [ ] Improvement  [ ] Exploratory
  Formulasi    : Apakah metode ML-based UEBA (Random Forest)
              menghasilkan detection rate lebih tinggi dan
              false positive rate lebih rendah dibandingkan
              rule-based SIEM dalam mendeteksi insider threat
              pada CERT Insider Threat Dataset?
  Variabel IV  : Metode deteksi (rule-based SIEM vs
                Random Forest berbasis UEBA)
  Variabel DV  : Detection rate (%), false positive
                rate (%), waktu deteksi (detik)
  Metrik       : Detection rate, false positive rate,
                waktu deteksi rata-rata
  Dataset      : CERT Insider Threat Dataset r5.2 / r6.2
  Baseline     : Rule-based SIEM (threshold & correlation
                rules standar)

Quality Check RQ:
  [ V ] Variabel spesifik
  [ V ] Metrik jelas
  [ V ] Baseline ada
  [ V ] Konteks disebutkan
  [ V ] Memerlukan eksperimen (bukan hanya survei literatur)

Contribution Statement:
  Apa yang baru diketahui : Bukti empiris kuantitatif
  tentang seberapa besar perbedaan performa rule-based
  SIEM vs ML-based UEBA dalam mendeteksi insider threat,
  diukur dengan metrik yang seragam dan dapat direplikasi
  Jenis kontribusi        : [ ] Improvement  [ V ] Comparison  [ ] Novel approach
  Gap yang diisi          : Belum adanya perbandingan langsung
  antara kedua pendekatan dengan metrik yang sama di
  konteks enterprise

Hypothesis Pair:
  H₀ : Tidak ada perbedaan signifikan antara detection
       rate dan false positive rate dari rule-based SIEM
       dan ML-based UEBA (Random Forest) dalam mendeteksi
       insider threat pada CERT Insider Threat Dataset
  H₁ : ML-based UEBA (Random Forest) menghasilkan
       detection rate yang lebih tinggi dan false positive
       rate yang lebih rendah secara signifikan dibanding
       rule-based SIEM pada dataset yang sama
  Threshold              : α = 0.05
  Justifikasi threshold  : Threshold 0.05 adalah standar
    umum dalam penelitian bidang keamanan siber dan
    informatika, digunakan juga oleh Alzaabi & Mehmood
    (2024) dan Sherje et al. (2024)
```

---

## Latihan 1 — Dari Gap ke RQ

Gunakan gap yang ditemukan di WS-03. Transformasikan menjadi Research Question.

**Gap dari WS-03:** Belum ada studi yang head-to-head bandingin rule-based SIEM vs ML-based UEBA pakai metrik yang sama untuk insider threat detection di enterprise

**RQ versi pertama (tulis bebas):**
> Apakah machine learning lebih baik dari SIEM untuk deteksi insider threat?

**Evaluasi RQ:**

| Komponen | Ada? | Isi |
|----------|------|-----|
| Metode spesifik | Ya | Rule-based SIEM vs Random Forest (ML-based UEBA) |
| Metrik terukur | Ya | Detection rate, false positive rate, waktu deteksi |
| Baseline | Ya | Rule-based SIEM standar enterprise |
| Dataset/konteks | Ya | CERT Insider Threat Dataset r5.2/r6.2 |

**Tipe RQ:** [ V ] Comparison / [ ] Improvement / [ ] Exploratory

**RQ versi revisi (setelah evaluasi):**
> Apakah ML-based UEBA menggunakan Random Forest menghasilkan detection rate lebih tinggi dan false positive rate lebih rendah dibandingkan rule-based SIEM dalam mendeteksi insider threat pada CERT Insider Threat Dataset r5.2?

---

## Latihan 2 — Hypothesis Pair

Rumuskan pasangan hipotesis dari RQ di Latihan 1.

| Komponen | Isi |
|----------|-----|
| H₀ | Tidak ada perbedaan signifikan antara detection rate dan false positive rate dari rule-based SIEM dan ML-based UEBA (Random Forest) pada CERT Insider Threat Dataset |
| H₁ | ML-based UEBA (Random Forest) menghasilkan detection rate lebih tinggi dan false positive rate lebih rendah secara signifikan dibanding rule-based SIEM |
| Metrik | Detection rate (%), false positive rate (%), waktu deteksi (detik) |
| Threshold | α = 0.05 |
| Justifikasi threshold | Standar umum riset informatika dan keamanan siber — dipakai Alzaabi & Mehmood (2024) dan Sherje et al. (2024) untuk evaluasi model yang serupa |

**Apakah hipotesis ini falsifiable?** [ V ] Ya / [ ] Tidak
> Bagaimana cara membuktikannya salah? Cara membuktikannya salah: kalau hasil eksperimen menunjukkan p-value > 0.05, atau detection rate RF tidak lebih tinggi dari SIEM secara statistik, maka H₁ ditolak dan H₀ tetap berlaku — artinya ML nggak terbukti lebih baik



---

## Latihan 3 — Rantai Operasionalisasi

Lengkapi rantai dari RQ hingga metode analisis.

| Tahap | Isi |
|-------|-----|
| RQ | Apakah ML-based UEBA (Random Forest) lebih baik dari rule-based SIEM dalam mendeteksi insider threat pada CERT Insider Threat Dataset? |
| Variable (IV) | Metode deteksi: rule-based SIEM vs Random Forest berbasis UEBA |
| Variable (DV) | Detection rate (%), false positive rate (%), waktu deteksi rata-rata (detik) |
| Metric | Detection rate, false positive rate, F1-score, average detection time |
| Data source | CERT Insider Threat Dataset r5.2 — dataset publik, dipakai mayoritas paper insider threat (Alzaabi & Mehmood 2024; Sherje et al. 2024) |
| Analysis method | Uji statistik perbandingan (Mann-Whitney U atau t-test) untuk cek apakah perbedaan performa signifikan pada α = 0.05 |

**Apakah rantai lengkap?** [ V ] Ya / [ ] Tidak
> Jika tidak, tahap mana yang perlu direvisi? ______________

---

## Refleksi

> Ambil satu judul skripsi/paper yang pernah dibaca. Coba ekstrak RQ-nya. Apakah RQ tersebut memenuhi semua komponen (metode, metrik, baseline, konteks)? Jika tidak, apa yang hilang?

**Judul:** "Insider Threat Detection Using Machine Learning Approach" — Alzaabi & Mehmood, IEEE Access 2024
**RQ yang diekstrak:** Algoritma ML mana (RF, SVM, LSTM, Isolation Forest) yang paling efektif untuk deteksi insider threat pada CERT Dataset?
**Komponen yang hilang:** Nggak ada baseline non-ML (rule-based SIEM) sebagai pembanding — jadi kita nggak tau seberapa jauh ML lebih baik dari sistem yang sekarang dipakai di industri. Ini justru yang jadi gap dan kita isi di penelitian ini.
