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

Mata kuliah ini menggunakan pendekatan **Positivist** (fenomena TI bisa diukur objektif melalui eksperimen terkontrol) diperkuat **Design Science Research** (artefak dibuat sebagai instrumen pengujian hipotesis, bukan tujuan akhir).

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
Tanggal          : 12 April 2026

1. Ketika membaca klaim "metode X 95% akurat":
   - Pertanyaan pertama saya: Akurasi dihitung dari dataset apa?
   - Data yang dibutuhkan untuk verifikasi: Dataset uji, metode evaluasi, confusion matrix

2. Posisi paradigma:
   - Pendekatan: [v] Positivis  [ ] Interpretivis  [v] Design Science  [ ] Mixed
   - Alasan: Karena penelitian di bidang IT biasanya pakai data dan eksperimen untuk melihat hasilnya. Selain itu juga sering membuat model atau sistem untuk                      diuji, jadi cocok dengan Design Science.

3. Identifikasi distorsi:
   - Asumsi tersembunyi: Dataset yang dipakai dianggap sudah mewakili kondisi nyata.
   - Sumber bias potensial: Data kurang beragam, atau model terlalu menyesuaikan data training (overfitting).
   - Langkah mitigasi: Menggunakan data yang lebih beragam dan melakukan pengujian ulang seperti cross-validation.

4. Komitmen etika:
   - Data yang tidak akan dimanipulasi: Data hasil eksperimen, termasuk yang hasilnya jelek atau tidak sesuai harapan.
   - Batasan yang diakui sejak awal: Dataset terbatas dan hasil mungkin tidak bisa langsung diterapkan ke semua kondisi.
```

---

## Latihan 1 — Identifikasi Distorsi

Pilih satu paper riset di bidang TI yang mengklaim "metode X meningkatkan performa." Telusuri setiap tahap Research Trust Model.

**Paper yang dipilih:**
> Judul: Analisis Produktivitas Menggunakan Metode POSPAC dan Performance Prism
> Penulis (Tahun): Rony Prabowo & Rizal Aditia (2020

| Tahap | Apa yang Dilakukan | Potensi Distorsi |
|-------|-------------------|-----------------|
| Reality → Data | Mengumpulkan data keuangan internal PT. X (pendapatan bersih, HPP, biaya umum, gaji, modal) selama 12 bulan (Feb 2018 – Jan 2019) melalui wawancara dan observasi| Data hanya berasal dari satu perusahaan tunggal (PT. X Surabaya), sehingga tidak mewakili industri baja tulangan secara umum; identitas perusahaan disamarkan sehingga validasi eksternal tidak bisa dilakukan |
| Data → Processing | Menghitung rasio produktivitas parsial (produksi, organisasi, penjualan, produk, tenaga kerja, modal) menggunakan formula POSPAC, lalu menghitung Indeks Produktivitas (IP) tiap bulan | Periode baseline hanya menggunakan bulan Februari sebagai acuan (IP = 100%), sehingga semua perbandingan relatif terhadap satu titik yang bisa jadi bukan kondisi "normal" |
| Processing → Analysis | Membandingkan rasio dan indeks produktivitas antar bulan, lalu mengintegrasikan dengan KPI dari Performance Prism melalui kuesioner kepada stakeholder | Kuesioner tidak disebutkan jumlah respondennya secara eksplisit; FGD hanya melibatkan 6 orang yang semuanya internal/terkait perusahaan, rentan bias konfirmasi |
| Analysis → Inference | Menyimpulkan bahwa indikator tenaga kerja paling bermasalah karena nilai KPI hanya 29% (tingkat pelanggaran tinggi, kedisiplinan rendah) | Inferensi kausalitas langsung dibuat tanpa uji statistik — fluktuasi produktivitas diasumsikan disebabkan oleh faktor disiplin, padahal bisa ada variabel lain (musiman, permintaan pasar, kondisi mesin) |
| Inference → Knowledge | Merumuskan rekomendasi perbaikan berupa reward & punishment, sistem gaji berbasis KPI, dan jadwal maintenance rutin | Rekomendasi bersifat generik dan tidak diuji implementasinya; paper tidak menyajikan data follow-up apakah saran tersebut efektif setelah diterapkan |

**Distorsi paling besar di tahap:** Analysis → Inference

**Dua distorsi spesifik yang teridentifikasi:**
1. Single-case bias tanpa kontrol: Seluruh data berasal dari satu perusahaan tanpa kelompok pembanding. Produktivitas yang "fluktuatif" belum tentu buruk — bisa jadi pola      industri baja tulangan secara umum memang musiman, namun paper tidak membandingkan dengan benchmark industri.
2. Baseline bulan tunggal (Februari): Indeks Produktivitas dihitung dengan menjadikan Februari sebagai 100%, tanpa penjelasan mengapa Februari dipilih. Jika Februari adalah    bulan dengan produktivitas rendah, maka banyak bulan akan terlihat "baik"; sebaliknya jika Februari tidak representatif, seluruh analisis tren menjadi bias.

---

## Latihan 2 — Analisis Kasus Etika

Skenario: Seorang peneliti menemukan bahwa jika 3 data point outlier dihapus, hasil eksperimennya menjadi signifikan. Dengan outlier, hasilnya tidak signifikan.

| Perspektif | Analisis |
|------------|---------|
| Kejujuran ilmiah | Laporkan kedua versi hasil (dengan dan tanpa outlier) secara transparan. Menghapus outlier hanya demi signifikansi tanpa justifikasi metodologis yang kuat adalah bentuk p-hacking — praktik yang merusak integritas ilmu pengetahuan. Dalam konteks paper POSPAC ini pun, jika bulan Agustus dan September (yang memiliki IP rendah) dikeluarkan, kesimpulan tentang tren produktivitas akan berubah drastis. |
| Transparansi | Peneliti wajib mendokumentasikan alasan penghapusan outlier secara eksplisit: apakah ada kesalahan pengukuran? Kejadian luar biasa (force majeure)? Jika tidak ada alasan substantif, data harus dipertahankan. Transparansi juga berarti melaporkan metode seleksi data di bagian metodologi, bukan menyembunyikannya. |
| Peer review | Reviewer bertanggung jawab menanyakan secara kritis: "Bagaimana distribusi data asli sebelum pembersihan?" dan "Apa kriteria outlier yang digunakan?" Paper yang baik menyertakan analisis sensitivitas — menunjukkan bahwa kesimpulan tetap robust dengan dan tanpa outlier, atau menjelaskan secara meyakinkan mengapa outlier tidak valid. |

**Keputusan akhir dan justifikasi:**
> Outlier tidak boleh dihapus semata-mata karena mengganggu signifikansi. Keputusan yang etis adalah melaporkan hasil tanpa penghapusan sebagai temuan utama, lalu menyajikan analisis dengan penghapusan sebagai analisis sensitivitas disertai justifikasi metodologis yang jelas (misalnya: "bulan X dikecualikan karena terjadi kebakaran gudang yang terdokumentasi"). Jika tidak ada justifikasi kuat, kesimpulan yang benar adalah: "efek tidak terbukti signifikan dalam kondisi data lengkap."

---

## Latihan 3 — Posisi Paradigma

**Topik riset:** Pengukuran dan peningkatan produktivitas pada industri manufaktur baja tulangan

| Kriteria | Positivis | Interpretivis | Design Science |
|----------|-----------|---------------|----------------|
| Kesesuaian dengan topik (1–5) | 5 | 2 | 4 |
| Jenis data yang dikumpulkan | Data kuantitatif terukur: rasio produktivitas, indeks produktivitas, nilai KPI, data keuangan bulanan (pendapatan, HPP, gaji, modal) | Data kualitatif: persepsi, pengalaman, dan makna yang dirasakan karyawan, manajer, dan stakeholder terkait "produktivitas" | Artefak desain berupa framework pengukuran terintegrasi POSPAC + Performance Prism yang dapat diterapkan ulang di perusahaan lain |
| Limitasi paradigma | Mengabaikan konteks sosial dan budaya kerja yang memengaruhi produktivitas; tidak menjelaskan mengapa tenaga kerja tidak disiplin dari sudut pandang mereka sendiri | Sulit digeneralisasi; temuan sangat bergantung pada konteks spesifik PT. X dan tidak menghasilkan angka produktivitas yang terukur | Validasi terbatas pada satu kasus; belum diuji apakah framework ini efektif di industri berbeda (misalnya tekstil atau elektronik) |

**Paradigma yang dipilih:** Positivis
**Alasan:** Paper ini secara dominan menggunakan paradigma positivis — data dikumpulkan dalam bentuk angka keuangan yang objektif dan terukur, diolah dengan formula matematis yang replikatif, dan dianalisis untuk menemukan pola produktivitas yang dapat digeneralisasi. Asumsi dasarnya adalah bahwa produktivitas adalah realitas yang dapat diukur secara objektif melalui rasio input-output, bukan konstruksi sosial yang bergantung pada interpretasi subjektif. Penggunaan KPI sebagai tolak ukur performa juga mencerminkan keyakinan positivis bahwa kinerja organisasi dapat dikuantifikasi dan dibandingkan secara standar.

---

## Refleksi

> Sebelum membaca materi ini, apakah pernah mempertanyakan klaim "95% akurat"? Setelah memahami rantai distorsi, pertanyaan apa yang sekarang akan diajukan saat membaca paper?

**Jawaban:**
> Kalau lihat angka "produktivitas meningkat 123%", langsung percaya saja — kesannya metode POSPAC berhasil. Tidak terpikirkan untuk bertanya angka itu dihitung dari mana dan dibandingkan dengan apa.
> Kuesioner Performance Prism itu diisi siapa saja? Berapa orang? Dipilih secara acak atau asal tunjuk?
Tenaga kerja tidak disiplin dibilang jadi penyebab produktivitas turun — tapi bagaimana kalau ternyata harga baja lagi naik atau pesanan memang lagi sedikit di bulan itu?
Penelitian ini hanya di satu pabrik. Apakah kesimpulannya bisa berlaku juga untuk pabrik baja lain, atau hanya kebetulan cocok di PT. X saja?
