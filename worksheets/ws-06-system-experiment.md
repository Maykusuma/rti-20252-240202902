# WS-06: System-Experiment Mapping

> **Bab 6 — System Design sebagai Experimental Artifact**

---

## Ringkasan Materi

### Sistem = Instrumen Pengujian, Bukan Produk

Seorang engineer bertanya "apakah sistem bekerja?" — seorang peneliti bertanya "apa yang bisa dibuktikan sistem ini?" Sistem dalam riset adalah **artifact** — objek yang sengaja dibuat untuk menguji klaim spesifik.

### System as Experiment Model

```
RQ → Variable → System Component → Experimental Setup → Output
```

Setiap komponen sistem harus bisa ditelusuri ke variabel riset (top-down), dan setiap pengukuran harus menjawab RQ (bottom-up).

### Mapping Variabel ke Komponen

| Tipe Variabel | Peran di Sistem | Contoh |
|---------------|----------------|--------|
| **IV** (Independent) | Modul yang bisa di-toggle/swap | Algoritma A vs B |
| **DV** (Dependent) | Modul pengukuran | Logger, metrics collector |
| **CV** (Control) | Config yang dikunci | Dataset, parameter tetap |

Jika variabel tidak bisa di-map ke komponen apapun → arsitektur perlu didesain ulang.

### 4 Prinsip Desain Eksperimental

| Prinsip | Pertanyaan Kunci |
|---------|-----------------|
| **Traceability** | Komponen ini melayani variabel yang mana? |
| **Modularity** | Bisakah IV diubah tanpa memengaruhi yang lain? |
| **Controllability** | Apakah CV dieksternalisasi ke config file? |
| **Measurability** | Apakah sistem otomatis menghasilkan data yang dibutuhkan? |

### Variable Isolation melalui Arsitektur

- **Modular architecture** — Pisahkan berdasarkan variabel
- **Configuration-driven** — Ubah config (YAML/JSON), bukan code
- **Feature toggles** — On/off flag untuk ablation study

  Contoh config YAML dengan feature toggles:
  ```yaml
  model:
    type: cnn          # IV: ganti "rf" untuk kondisi baseline
  features:
    use_temporal: true  # toggle komponen temporal
    use_normalization: true  # toggle preprocessing
  experiment:
    seed: 42
    runs: 5
  ```
  Dengan pendekatan ini, berbeda kondisi eksperimen = berbeda satu baris config, **tanpa mengubah kode**.

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan sistem | Memenuhi kebutuhan user | Menguji hipotesis, menghasilkan bukti |
| Arsitektur | Optimasi performa & skalabilitas | Optimasi isolasi variabel & reprodusibilitas |
| Konfigurasi | Sering hardcoded | Dieksternalisasi ke config file |
| Fitur tambahan | Menambah nilai user | Menambah noise jika tidak terkait RQ |

### Istilah Penting

- **Artifact** — Objek yang sengaja dibuat untuk memecahkan masalah atau menguji proposisi
- **Traceability** — Kemampuan menelusuri hubungan RQ → variabel → komponen → output
- **Variable Isolation** — Mengubah hanya satu variabel sambil menahan yang lain konstan
- **Ablation Study** — Menguji kontribusi tiap komponen dengan melepasnya satu per satu
- **Configuration-driven Execution** — Semua parameter di config file, bukan hardcoded

---

## Template A.6 — Mapping RQ ke Arsitektur Sistem

```
SYSTEM-EXPERIMENT MAPPING

Research Question: Apakah Random Forest berbasis UEBA menghasilkan detection rate lebih tinggi dan false positive rate lebih rendah dibandingkan rule-based SIEM dalam mendeteksi insider threat tipe data exfiltrationpada CERT Insider Threat Dataset r5.2?

Variable → Component Mapping:
| Variabel | Tipe | Komponen Sistem | Cara Manipulasi/Pengukuran |
|----------|------|-----------------|---------------------------|
|          | IV   |                 |                           |
|          | DV   |                 |                           |
|          | CV   |                 |                           |

4 Prinsip Desain:
  [✓] Traceability    — Setiap komponen bisa ditelusuri ke variabel riset
  [✓] Variable Isolation   — IV (metode deteksi) bisa di-swap hanya dengan ganti satu baris config tanpa ubah modul lain
  [✓] Measurement Integration     — Modul evaluasi built-in, langsung hitung confusion matrix & metrik otomatis setiap run
  [✓] Reproducibility — Semua parameter ada di config file, seed ditetapkan, split data dikunci

Experimental Setup:
  Input data   : CERT Insider Threat Dataset r5.2, sudah dipreprocess (missing value dibersihkan, fitur dinormalisasi, split 80:20
                train/test dikunci)
  Parameter    : model_type, threshold, random_seed: 42, train_test_split: 0.8, n_runs: 5
  Output format: CSV per run berisi confusion matrix + detection rate + FPR + waktu deteksi + F1-Score
```

---

## Latihan 1 — Variable-to-Component Mapping

Gunakan RQ dan variabel dari WS-05. Petakan ke komponen sistem.

**RQ:** Apakah Random Forest berbasis UEBA menghasilkan detection rate lebih tinggi dan false positive rate lebih rendah dibandingkan rule-based SIEM dalam mendeteksi insider threat tipe data exfiltration pada CERT Insider Threat Dataset r5.2?

| Variabel | Tipe | Komponen Sistem | Cara Manipulasi / Pengukuran |
|----------|------|-----------------|---------------------------|
| *Metode deteksi (SIEM vs RF)* | *IV* | *Modul Classifier — bisa di-swap antara rule-based SIEM dan Random Forest* | *Ganti satu baris di config: model_type: siem atau model_type: rf`* |
| *Detection rate* | DV | *Modul Evaluasi — Metrics Collector* | *Dihitung otomatis dari TP/(TP+FN) setiap eksperimen selesai* |
| *False positive rate* | DV | *Modul Evaluasi — Metrics Collector* | *Dihitung otomatis dari FP/(FP+TN) setiap eksperimen selesai* |
| *Waktu deteksi* | *DV* | *Modul Evaluasi — Timer* | *Dicatat otomatis dari timestamp event masuk sampai alert keluar* |
| *Ukuran dataset* | *CV* | *Config File (experiment.yaml)* | *Dikunci — jumlah record sama di kedua kondisi* |
| *Nilai threshold* | *Cv* | *Config File (experiment.yaml)* | *Dikunci — threshold sama di kedua kondisi, tidak boleh diubah* |
| *Random seed & split* | *CV* | *Config File (experiment.yaml)* | *seed: 42, split: 80/20 dikunci agar eksperimen bisa direplikasi* |
**Apakah semua variabel bisa di-map?** [ V ] Ya / [ ] Tidak

---

## Latihan 2 — 4 Prinsip Desain

Evaluasi desain sistem terhadap 4 prinsip.

| Prinsip | Status | Bukti / Penjelasan |
|---------|--------|-------------------|
| Traceability | *✅* | Setiap komponen punya label variabel yang jelas — Modul Classifier = IV, Modul Evaluasi = DV, Config File = CV. Kalau ada pertanyaan "komponen ini buat apa?", langsung bisa dijawab dengan variabelnya |
| Modularity | *✅* | *IV bisa di-swap hanya dengan ganti satu baris config (model_type) tanpa menyentuh Modul Evaluasi atau preprocessing sama sekali — persis seperti prinsip yang dicontohkan di materi* |
| Controllability | *✅* | *Semua CV dieksternalisasi ke experiment.yaml — threshold, seed, split, jumlah run semuanya ada di satu file config yang dikunci sebelum eksperimen dimulai* |
| Measurability | *✅* | *Modul Evaluasi built-in dan jalan otomatis setiap run selesai — output langsung berupa CSV berisi semua metrik yang dibutuhkan tanpa perlu hitung manual* |

**Prinsip mana yang paling sulit dipenuhi?** Measurability untuk waktu deteksi
**Strategi untuk mengatasinya:**
> Waktu deteksi di rule-based SIEM dan Random Forest punya definisi yang sedikit beda — SIEM kasih alert berbasis rule match, RF kasih prediksi setelah seluruh data diproses. Solusinya: pasang timer yang mulai jalan dari event pertama masuk ke sistem sampai output klasifikasi keluar, dan pakai definisi yang sama persis di kedua kondisi. Dokumentasikan definisi ini sebelum eksperimen jalan biar nggak ada ambiguitas.

---

## Latihan 3 — Ablation Study Planning

Jika sistem memiliki 3 komponen utama, rencanakan ablation study.

> **Panduan jumlah kondisi:** Untuk 3 komponen (A, B, C), kondisi minimal yang direkomendasikan:
> Full + (-A) + (-B) + (-C) = **4 kondisi dasar**. Jika waktu memungkinkan, tambahkan kombinasi ganda: (-A,-B), (-A,-C), (-B,-C) = **7 kondisi**. Sesuaikan dengan *computational cost* dan tenggat waktu penelitian.

| Kondisi | Komponen A | Komponen B | Komponen C | Hasil yang Diharapkan |
|---------|-----------|-----------|-----------|----------------------|
| Full | *✅ RF* | *✅ UEBA features* | *✅ Normalisasi* | *Performa terbaik RF — ini kondisi B utama* |
| – A | *❌ Ganti ke rule-based SIEM* | ✅ | ✅ | *Kondisi A baseline — seberapa jauh RF lebih baik dari SIEM* |
| – B | *✅RF* | *❌ Tanpa UEBA features (pakai fitur raw saja)* | *✅* | *Seberapa besar kontribusi feature engineering berbasis UEBA terhadap performa RF* |
| – C | *✅RF* | ✅ | *❌ tanpa normalisasi* | Seberapa besar preprocessing mempengaruhi performa RF |

**Komponen mana yang diprediksi paling berkontribusi?** Komponen B — Feature Engineering berbasis UEBA
**Mengapa?**
> Karena inti dari UEBA itu justru ada di fiturnya — cara sistem "belajar" pola perilaku normal pengguna ditentukan oleh seberapa informatif fitur yang dimasukkan ke model. Kalau fitur UEBA-nya dicopot dan diganti fitur raw biasa, Random Forest jadi kehilangan "kecerdasannya" dalam membedakan perilaku normal vs mencurigakan, dan performanya diprediksi akan turun signifikan. Ini juga yang belum pernah dieksplorasi di paper-paper yang kita review sebelumnya.

---

## Refleksi

> Apa risiko jika sistem dibangun seperti produk (monolitik, fitur lengkap) lalu baru dilakukan eksperimen? Mengapa arsitektur modular penting untuk riset?

**Jawaban:**
> Kalau sistem dibangun dulu sebagai produk yang monolitik — fitur lengkap, semua komponen nyambung satu sama lain, hardcoded di mana-mana — terus baru mau dicoba eksperimen, masalahnya adalah nggak bisa ganti satu hal tanpa ngaruh ke yang lain. Mau swap algoritmanya? Harus ubah kode di banyak tempat. Mau coba tanpa normalisasi? Harus korek-korek kode lagi. Hasilnya jadi susah dipercaya karena nggak bisa yakin perubahan yang terjadi itu akibat variabel yang mana. Makanya arsitektur modular itu penting banget di riset. Bukan soal kode yang rapi, tapi soal bisa buktiin bahwa perbedaan hasil itu beneran berasal dari variabel yang kamu manipulasi, bukan dari hal lain yang nggak sengaja ikut berubah.