# WS-07: Experimental Design & Validity

> **Bab 7 — Experimental Design & Validity**

---

## Ringkasan Materi

### Correlation ≠ Causality

Kausalitas membutuhkan 3 syarat:
1. **Covariance** — X dan Y bergerak bersama
2. **Temporal precedence** — X berubah sebelum Y
3. **Elimination of alternatives** — Tidak ada faktor lain yang menjelaskan Y

Controlled experiment adalah satu-satunya metode yang bisa membuktikan kausalitas.

### Empat Jenis Validitas

| Jenis | Pertanyaan | Ancaman Umum |
|-------|-----------|-------------|
| **Internal** | Apakah hubungan IV→DV nyata? | Confounding variable, selection bias |
| **External** | Apakah bisa digeneralisasi? | Dataset terlalu spesifik |
| **Construct** | Apakah mengukur konsep yang benar? | Metrik tidak sesuai |
| **Conclusion** | Apakah kesimpulan statistik valid? | Sample size kecil, uji salah |

Internal dan external validity sering berkonflik: semakin terkontrol (internal kuat) → semakin artificial (external lemah).

### Tiga Tipe Eksperimen dalam Riset TI

| Tipe | Deskripsi | Kapan Digunakan |
|------|----------|----------------|
| **Comparison Study** | Metode A vs B pada kondisi identik | Membandingkan pendekatan berbeda |
| **Ablation Study** | Full system → lepas komponen satu per satu | Mengukur kontribusi tiap komponen |
| **Parameter Study** | Variasikan satu parameter, amati dampak | Uji sensitifitas/robustness |

### Fairness dalam Perbandingan

Perbandingan yang adil = **kondisi identik** untuk semua metode: dataset sama, preprocessing sama, tuning effort sebanding, environment sama, metrik sama.

Contoh tidak adil: Transformer (30 fitur tambahan + Bayesian optimization) vs RF (default params) → hasilnya misleading.

### Threats to Validity = Diidentifikasi Sebelum Eksperimen

Ancaman validitas harus diidentifikasi **sebelum** eksperimen dan mitigasinya dirancang sebagai bagian dari desain — bukan ditulis sebagai boilerplate setelah selesai.

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan testing | Memastikan sistem memenuhi requirement | Membuktikan hubungan kausal antar variabel |
| Baseline | Versi sebelumnya (last release) | Metode tervalidasi dari literatur |
| Kegagalan | Bug → fix → release | H₀ tidak ditolak → tetap kontribusi ilmiah |
| Sukses | 100% test pass | Evidence valid — mendukung atau menolak hipotesis |

### Istilah Penting

- **Causality** — Hubungan sebab-akibat (covariance + temporal + elimination)
- **Controlled Experiment** — Ubah satu variabel, kontrol sisanya, amati efek
- **Fairness** — Semua metode diuji pada kondisi yang benar-benar identik
- **Threats to Validity** — Faktor yang bisa melemahkan kesimpulan jika tidak dimitigasi
- **Conclusion Validity** — Validitas statistik: power, sample size, uji yang tepat

---

## Template A.7 — Desain Eksperimen Lengkap

```
EXPERIMENT DESIGN

Research Question : Apakah Random Forest berbasis UEBA menghasilkan detection rate lebih tinggi dan false positive rate lebih rendah dibandingkan rule-based SIEM dalam
                    mendeteksi insider threat tipe data exfiltration pada CERT Insider Threat Dataset r5.2?
Hypothesis        : H0: Tidak ada perbedaan signifikan antara detection rate dan false positive rate dari rule-based SIEM dan Random Forest berbasis UEBA pada CERT r5.2
                    H1: Random Forest berbasis UEBA menghasilkan detection rate lebih tinggi dan false positive rate lebih rendah secara signifikan dibanding rule-based SIEM
Tipe Eksperimen   : [ V ] Comparison  [ ] Ablation  [ ] Parameter

Kondisi Eksperimen:
| Kondisi | Deskripsi | IV Value | CV Settings |
|---------|-----------|----------|-------------|
| Control | Rule-based SIEM dengan threshold & correlation rules standar | model_type:siem | Dataset: CERT r5.2, split 80:20, seed: 42, threshold: sama, preprocessing: identik, runs: 5 |
| Treatment | Random Forest berbasis UEBA dengan fitur perilaku pengguna | model_type: rf | Dataset: CERT r5.2, split 80:20, seed: 42, threshold: sama, preprocessing: identik, runs: 5 |

Fairness Checklist:
  [✓] Dataset identik — sama-sama CERT r5.2, split dan seed dikunci
  [✓] Preprocessing setara — pipeline yang sama dijalankan sebelum kedua kondisi
  [✓] Tuning effort setara — RF pakai default params scikit-learn, tidak di-optimize lebih dari rule-based SIEM
  [✓] Environment identik — dijalankan di mesin yang sama, library yang sama
  [✓] Metrik evaluasi sama — detection rate, FPR, waktu deteksi, F1-Score dihitung dengan formula yang persis sama di kedua kondisi

Threat Analysis:
| Threat Type | Ancaman Spesifik | Mitigasi |
|-------------|-----------------|----------|
| Internal    | Data leakage antara train dan test set RF bisa bocor | Gunakan stratified split, validasi tidak ada overlap antara train dan test |
| External    | CERT r5.2 adalah dataset sintetis — belum tentu mewakili enterprise nyata | Acknowledge sebagai limitasi, rekomendasikan validasi lanjut di enterprise nyata |
| Construct   | Waktu deteksi punya definisi beda di SIEM vs RF | Definisikan waktu deteksi secara eksplisit sebelum eksperimen dan pakai definisi yang sama di kedua kondisi |
| Conclusion  | Sample size mungkin tidak cukup untuk uji statistik yang kuat | Jalankan 5 kali eksperimen, laporkan rata-rata dan standar deviasi, gunakan Mann-Whitney U yang tidak mengasumsikan distribusi normal |

Statistical Plan:
  Uji statistik  : Mann-Whitney U test (primary), paired t-test (secondary jika distribusi normal)
  Justifikasi    : Mann-Whitney U dipilih karena tidak mengasumsikan distribusi normal — cocok untuk metrik performa yang distribusinya belum tentu normal
  Alpha          : 0.05
  Effect size    : Cohen's d minimum 0.5 (medium effect) — perbedaan yang lebih kecil dari ini dianggap tidak praktis signifikan untuk implementasi nyata
```

---

## Latihan 1 — Desain Eksperimen

Susun desain eksperimen berdasarkan RQ, variabel, dan sistem dari WS-04 sampai WS-06.

**RQ:** Apakah Random Forest berbasis UEBA menghasilkan detection rate lebih tinggi dan false positive rate lebih rendah dibandingkan rule-based SIEM dalam mendeteksi insider threat tipe data exfiltration pada CERT Insider Threat Dataset r5.2?
**Tipe eksperimen:** [ V ] Comparison / [ ] Ablation / [ ] Parameter

| Kondisi | Deskripsi | IV Value | CV Settings |
|---------|-----------|----------|-------------|
| Control | Rule-based SIEM dengan threshold dan correlation rules standar — ini yang sekarang dipakai di enterprise | model_type: siem | CERT r5.2, split 80:20, seed: 42, threshold: identik, preprocessing: pipeline yang sama, runs: 5 |
| Treatment | Random Forest berbasis UEBA dengan fitur perilaku pengguna yang diekstrak dari log | model_type: rf | CERT r5.2, split 80:20, seed: 42, threshold: identik, preprocessing: pipeline yang sama, runs: 5 |

---

## Latihan 2 — Fairness Checklist

Evaluasi apakah desain eksperimen di Latihan 1 sudah fair.

| Kriteria | Status | Detail |
|----------|--------|--------|
| Dataset identik | ✅ | Kedua kondisi pakai CERT Insider Threat Dataset r5.2 yang sama persis, dengan split 80:20 dan seed 42 yang dikunci di config |
| Preprocessing setara | ✅ | Pipeline preprocessing yang sama dijalankan sebelum kedua kondisi — missing value handling, normalisasi, dan ekstraksi fitur dilakukan identik |
| Tuning effort setara | ✅ | RF menggunakan default parameters dari scikit-learn tanpa hyperparameter tuning tambahan, sama seperti rule-based SIEM yang juga pakai rule set standar tanpa optimasi khusus |
| Environment identik | ✅ | Eksperimen dijalankan di mesin yang sama, versi Python dan scikit-learn yang sama, tidak ada proses lain yang berjalan bersamaan |
| Metrik evaluasi sama | ✅ | Detection rate, FPR, waktu deteksi, dan F1-Score dihitung dengan formula yang persis sama dari modul evaluasi yang sama di kedua kondisi |

**Ada yang tidak fair?** [ ] Ya / [V] Tidak

---

## Latihan 3 — Threat Analysis

Identifikasi ancaman validitas untuk desain eksperimen ini.

| Threat Type | Ancaman Spesifik | Mitigasi |
|-------------|-----------------|----------|
| Internal | Data leakage — model RF bisa "kenal" data test kalau split-nya tidak hati-hati, sehingga performanya terlihat lebih bagus dari yang seharusnya | Gunakan stratified split dengan seed yang dikunci, validasi tidak ada overlap antara train dan test set sebelum eksperimen jalan |
| External | CERT r5.2 adalah dataset sintetis yang dibuat berdasarkan simulasi enterprise AS — belum tentu merepresentasikan perilaku insider di enterprise nyata, apalagi di konteks yang berbeda | Acknowledge secara eksplisit sebagai limitasi di bagian kesimpulan, rekomendasikan replikasi di dataset enterprise nyata sebagai penelitian lanjutan |
| Construct | Waktu deteksi punya definisi yang berbeda di SIEM vs RF — SIEM kasih alert saat rule match, RF kasih output setelah semua data diproses — kalau nggak didefinisikan sama, hasil waktu deteksi jadi nggak apple-to-apples | Definisikan waktu deteksi secara eksplisit sebelum eksperimen: waktu dari event pertama masuk ke sistem sampai output klasifikasi keluar, dan terapkan definisi yang sama di kedua kondisi |
| Conclusion | Menjalankan eksperimen hanya sekali bisa menghasilkan kesimpulan yang tidak stabil — satu run yang kebetulan bagus atau jelek bisa menyesatkan | Jalankan 5 kali eksperimen, laporkan rata-rata dan standar deviasi, gunakan Mann-Whitney U test pada alpha 0.05 untuk membuktikan signifikansi secara statistik |

**Ancaman mana yang paling sulit dimitigasi?** External validity
**Mengapa?**
> Karena satu-satunya cara yang beneran bisa mengatasi external validity adalah dengan eksperimen di lingkungan enterprise nyata — dan itu butuh akses ke log perusahaan asli yang sangat susah didapat karena isu privasi dan keamanan data. Semua mitigasi yang bisa kita lakukan dari dalam penelitian ini — seperti mengakui limitasinya dan merekomendasikan penelitian lanjutan — hanya bisa mengurangi dampaknya di mata reviewer, bukan menghilangkan ancamannya secara fundamental.

---

## Refleksi

> Sebuah paper melaporkan "metode kami mengalahkan semua baseline." Apa 3 pertanyaan pertama yang harus diajukan untuk mengevaluasi klaim ini?

**Jawaban:**
1. Baseline-nya dipilih dengan jujur atau sengaja dipilih yang lemah?
2. Kondisi eksperimennya fair untuk semua metode?
3. Apakah perbedaannya signifikan secara statistik dan bukan cuma kebetulan?
