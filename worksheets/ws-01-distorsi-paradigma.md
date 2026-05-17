# WS-01: Distorsi & Paradigma

> **Bab 1 — Research Mindset in IT**

---

## Ringkasan Materi

### Research Trust Model

Pengetahuan ilmiah tidak muncul langsung dari kenyataan. Ia melewati **6 tahap transformasi** yang masing-masing rawan distorsi:

```
Reality → Data → Processing → Analysis → Inference → Knowledge
```

Etika mencegah distorsi yang disengaja (fabrikasi, cherry-picking). Validitas mendeteksi distorsi yang tidak disengaja (confounding variable, sampling bias).

### Tiga Jenis Validitas

| Jenis | Pertanyaan | Contoh Ancaman |
|-------|-----------|----------------|
| **Internal Validity** | Apakah hubungan kausal benar ada? | Confounding variable |
| **External Validity** | Apakah bisa digeneralisasi? | Dataset terlalu homogen |
| **Construct Validity** | Apakah mengukur hal yang benar? | Metrik tidak sesuai klaim |

### Paradigma Riset

Mata kuliah ini menggunakan pendekatan **Positivist** (fenomena TI bisa diukur objektif melalui eksperimen terkontrol) diperkuat **Design Science Research** (DSR). Penting untuk membedakan keduanya:

| Paradigma | Cara Kerja | Contoh di TI |
|-----------|-----------|---------------|
| **Positivis** | Uji hipotesis dengan eksperimen terkontrol | Apakah CNN lebih akurat dari RF pada dataset X? |
| **Design Science Research** | Bangun artefak (sistem/model/framework) untuk menguji proposisi | Dapatkah arsitektur hybrid CNN+LSTM membuktikan peningkatan recall ≥5%? |
| **Interpretivis** | Pahami makna melalui konteks & kualitatif | Bagaimana peneliti manafsirkan anomali data sensor IoT? |

Dalam DSR, artefak **bukan tujuan akhir** — ia adalah instrumen untuk menghasilkan pengetahuan. Pertanyaan riset tetap harus difalsifikasi.

### Mode Berpikir Peneliti

**Curious** (mempertanyakan fenomena) → **Critical** (mengevaluasi klaim berdasarkan bukti) → **Systematic** (merancang investigasi terstruktur dan reproducible).

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan | Membuat sistem yang bekerja | Menghasilkan pengetahuan yang valid |
| Pertanyaan khas | "Bagaimana membuatnya jalan?" | "Apakah klaim ini benar?" |
| Ukuran sukses | Sistem berfungsi, client puas | Hipotesis terjawab, temuan tervalidasi |
| Kegagalan | Harus dihindari | Harus dilaporkan (negative result = kontribusi) |

### Istilah Penting

- **Research Mindset** — Pola pikir yang menuntut bukti dan mempertanyakan asumsi
- **Research Ethics** — Prinsip perilaku: kejujuran, objektivitas, keterbukaan, akuntabilitas
- **HARKing** — Hypothesizing After Results are Known — merumuskan hipotesis setelah melihat data
- **Falsifiability** — Hipotesis harus bisa dibuktikan salah

---

## Template A.1 — Research Mindset Self-Assessment

```
Nama Peneliti    : Mayang Nur Annisa Kusuma
Tanggal          : 1 Mei 2026

1. Ketika membaca klaim "metode X 95% akurat":
   - Pertanyaan pertama saya:  95% itu diukur di dataset apa? Kondisinya kayak gimana?
     Dibandingin sama sistem yang mana?
   - Data yang dibutuhkan untuk verifikasi: Dataset yang dipakai (ukuran, sumber, seimbang atau tidak antara kelas normal vs anomali), metode     baseline yang jadi pembanding, dan hasil confusion matrix-nya (bukan cuma akurasi doang)
2. Posisi paradigma:
   - Pendekatan: [V] Positivis  [ ] Interpretivis  [V] Design Science  [ ] Mixed
   - Alasan: Penelitian ini ngukur performa dua metode secara objektif pakai eksperimen terkontrol (Positivis), sekaligus membangun setup         perbandingan sebagai artefak untuk menguji klaim (Design Science)

3. Identifikasi distorsi:
   - Asumsi tersembunyi:  CERT Dataset dianggap representatif untuk semua enterprise, padahal itu dataset simulasi bukan log enterprise nyata
   - Sumber bias potensial: Pemilihan threshold di rule-based SIEM bisa mempengaruhi hasil — kalau threshold-nya diset terlalu longgar, SIEM       keliatan buruk; kalau terlalu ketat, banyak false positive
   - Langkah mitigasi: Samain threshold di kedua kondisi eksperimen, dokumentasikan semua keputusan preprocessing, dan acknowledge keterbatasan    dataset di bagian kesimpulan

4. Komitmen etika:
   - Data yang tidak akan dimanipulasi: Label ground truth dari CERT Dataset tidak akan diubah; hasil confusion matrix dilaporkan apa adanya meski hasilnya tidak sesuai harapan
   - Batasan yang diakui sejak awal: Dataset bersifat sintetis (bukan log enterprise nyata), eksperimen dilakukan dalam kondisi terkontrol sehingga belum tentu hasilnya sama di lingkungan enterprise sesungguhnya
```

---

## Latihan 1 — Identifikasi Distorsi

Pilih satu paper riset di bidang TI yang mengklaim "metode X meningkatkan performa." Telusuri setiap tahap Research Trust Model.

> **Panduan pencarian paper:** Gunakan [IEEE Xplore](https://ieeexplore.ieee.org), [ACM Digital Library](https://dl.acm.org), atau Google Scholar. Pilih paper **tahun 2020 ke atas**, di topik yang Anda minati: deteksi anomali, klasifikasi citra, NLP, keamanan siber, IoT, dsb.
>
> **Contoh domain TI:** "Deteksi anomali lalu-lintas jaringan menggunakan CNN — akurasi meningkat 94% vs baseline SVM 87%." Distorsi potensial: apakah dataset normal/anomali seimbang? Apakah hanya diuji pada satu vendor traffic?

**Paper yang dipilih:**
> Judul: A Review of Recent Advances, Challenges, and Opportunities in Malicious Insider Threat Detection Using Machine Learning Methods
> Penulis (Tahun): Alzaabi & Mehmood (2024)
> Sumber/Link DOI: IEEE Access — DOI: 10.1109/ACCESS.2024.3369906

| Tahap | Apa yang Dilakukan | Potensi Distorsi |
|-------|-------------------|-----------------|
| Reality → Data | Mengumpulkan hasil eksperimen dari berbagai paper insider threat yang ada | Hanya ambil paper yang hasilnya positif — paper yang hasilnya jelek jarang dipublikasikan (publication bias) |
| Data → Processing | Mengelompokan metode ML berdasarkan kategori: RF, SVM, LSTM, dll | Cara ngelompokinnya bisa subjektif — metode yang mirip bisa dikategoriin beda-beda tergantung penelitinya |
| Processing → Analysis | Bandingin performa antar metode berdasarkan angka yang dilaporkan masing-masing paper | Dataset yang dipakai tiap paper beda-beda, jadi sebenernya nggak apple-to-apples — RF di dataset A belum tentu sama performanya kalau dipakai di dataset B |
| Analysis → Inference | Menyimpulkan RF dan LSTM paling bagus secara umum | Kesimpulan ini terlalu general — bisa jadi RF bagus di dataset tertentu tapi jelek di kondisi lain |
| Inference → Knowledge | Diklaim sebagai panduan pemilihan metode untuk insider threat detection | Belum tentu berlaku di enterprise nyata karena semua paper yang direview pakai dataset sintetis (CERT) |

**Distorsi paling besar di tahap:** Analysis → Inference

**Dua distorsi spesifik yang teridentifikasi:**
1. Pertama, perbandingan antar metode tidak apple-to-apples karena setiap paper yang direview pakai dataset dan kondisi eksperimen yang berbeda-beda, tapi hasilnya dijadikan satu kesimpulan seolah sebanding. Kedua, tidak ada satupun paper dalam review ini yang jadikan rule-based SIEM sebagai baseline pembanding, jadi klaim "ML lebih baik" sebenernya belum bisa dibuktikan karena nggak ada pembanding dari sistem yang sekarang dipakai di industri.

---

## Latihan 2 — Analisis Kasus Etika

Skenario: Seorang peneliti menemukan bahwa jika 3 data point outlier dihapus, hasil eksperimennya menjadi signifikan. Dengan outlier, hasilnya tidak signifikan.

| Perspektif | Analisis |
|------------|---------|
| Kejujuran ilmiah | Wajib laporkan kedua versi — dengan outlier dan tanpa outlier. Kalau cuma lapor yang signifikan, itu namanya cherry-picking dan termasuk fabrikasi data |
| Transparansi | Jelaskan alasan kenapa outlier itu ada dan apa yang menyebabkannya. Kalau outlier-nya memang error teknis (misal sensor rusak), boleh dihapus tapi harus dijelaskan. Kalau outlier-nya data valid, tidak boleh dihapus cuma karena bikin hasil jadi tidak signifikan |
| Peer review | Reviewer punya hak tahu seluruh data mentah dan keputusan preprocessing. Menyembunyikan outlier dari reviewer adalah pelanggaran etika riset yang serius |

**Keputusan akhir dan justifikasi:**
> Laporkan kedua hasil secara transparan — sertakan analisis dengan outlier sebagai hasil utama, dan analisis tanpa outlier sebagai analisis tambahan dengan penjelasan lengkap kenapa outlier itu dihapus. Kalau hasilnya memang tidak signifikan dengan data lengkap, itu tetap temuan yang valid dan harus dilaporkan apa adanya. Negative result bukan kegagalan dalam riset — justru itu kontribusi yang jujur.

---

## Latihan 3 — Posisi Paradigma

**Topik riset:** Perbandingan Rule-Based SIEM vs Random Forest berbasis UEBA untuk Deteksi Insider Threat

> **Skala 1–5:** 1 = tidak sesuai sama sekali dengan topik ini, 5 = sangat sesuai dan dominan digunakan pada riset bertopik serupa.

| Kriteria | Positivis | Interpretivis | Design Science |
|----------|-----------|---------------|----------------|
| Kesesuaian dengan topik (1–5) | 5 — topik ini sepenuhnya kuantitatif, ngukur performa dua metode secara objektif pakai eksperimen terkontrol | 1 — topik ini tidak studi makna atau konteks sosial, semua ngukur angka | 4 — kita membangun setup eksperimen sebagai artefak untuk menguji klaim performa |
| Jenis data yang dikumpulkan | Metrik numerik: detection rate, false positive rate, waktu deteksi dari confusion matrix | Wawancara, observasi kualitatif tentang pengalaman tim SOC | Hasil uji perbandingan dua sistem, komparasi kinerja terukur |
| Limitasi paradigma | Hasil mungkin tidak bisa digeneralisasi ke semua konteks enterprise karena dataset sintetis | Tidak relevan untuk topik ini karena tujuannya mengukur performa bukan memahami pengalaman | Artefak (setup eksperimen) bukan tujuan akhir — tetap harus menghasilkan temuan yang bisa difalsifikasi |

**Paradigma yang dipilih:** Positivis dengan elemen Design Science
**Alasan:** Penelitian ini pada dasarnya menguji hipotesis secara kuantitatif — apakah ada perbedaan signifikan antara dua metode — yang merupakan ciri khas positivis. Tapi kita juga membangun setup eksperimen sebagai artefak untuk membuktikan klaim tersebut, yang masuk ke ranah Design Science. Keduanya saling melengkapi dan cocok untuk topik perbandingan sistem keamanan seperti ini.

---

## Refleksi

> Sebelum membaca materi ini, apakah pernah mempertanyakan klaim "95% akurat"? Setelah memahami rantai distorsi, pertanyaan apa yang sekarang akan diajukan saat membaca paper?

**Jawaban:**
> Sebelum baca materi ini, kalau ada paper yang bilang "akurasi 95%" mungkin langsung diterima aja tanpa banyak pertanyaan. Tapi setelah paham rantai distorsi dari reality sampai knowledge, sekarang pertanyaan yang muncul jadi lebih kritis: 95% itu diukur di dataset seperti apa dan seberapa representatif dataset itu terhadap kondisi nyata? Dibandingin sama metode apa dan apakah baseline-nya dipilih secara jujur atau sengaja dipilih yang lemah biar hasilnya keliatan bagus? Siapa yang diuntungkan kalau hasil ini dipercaya? Dan yang paling penting — kalau eksperimennya diulang orang lain dengan dataset berbeda, hasilnya bakal sama tidak?