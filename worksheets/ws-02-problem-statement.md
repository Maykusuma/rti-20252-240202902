# WS-02: Problem Statement

> **Bab 2 — Problem Formulation & System Context**

---

## Ringkasan Materi

### Problem Formation Model

Masalah riset melewati 5 tahap transformasi. Melompat langsung dari Reality ke Variable adalah kesalahan paling umum.

```
Reality → Observed Issue (Symptom) → Diagnosed Problem (Root Cause)
→ Researchable Problem (Scoped) → Measurable Variable (Operationalized)
```

### Topic ≠ Problem ≠ Research Problem

| Level | Contoh | Status |
|-------|--------|--------|
| **Topik** | Keamanan IoT | Terlalu luas, tidak bisa diuji |
| **Problem** | MQTT tidak terenkripsi | Spesifik tapi belum riset |
| **Research Problem** | Belum ada studi membandingkan overhead TLS 1.3 vs DTLS pada MQTT di IoT RAM < 64KB | Bisa dirancang eksperimennya |

### Symptom vs Root Cause

Apa yang diamati (gejala) ≠ mengapa terjadi (akar masalah). Gunakan **5 Whys** atau **Fishbone Diagram** untuk menggali.

Contoh: "User meninggalkan checkout" (symptom) → "Waktu loading > 8 detik karena API call sequential" (root cause).

### System Thinking

Setiap masalah riset TI harus terikat pada komponen sistem: **Input → Process → Output → Outcome → Constraints → Stakeholders**.

### Problem Quality Check

Masalah riset yang layak harus memenuhi 5 kriteria:
- **Clarity** — Satu orang membaca akan paham
- **Measurability** — Ada metrik kuantitatif
- **Relevance** — Penting untuk domain
- **Testability** — Bisa gagal (falsifiable)
- **Impact** — Ada kontribusi jika terjawab

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan | Menyelesaikan masalah (*solve*) | Memahami dan membuktikan (*understand & prove*) |
| Masalah | Bug, error, fitur belum ada | Gap dalam pengetahuan |
| Scope | Selesaikan semua yang perlu | Batasi agar bisa dibuktikan |
| Output | Working system | Evidence, paper, replicable findings |

### Istilah Penting

- **Problem Statement** — Formulasi tertulis: konteks sistem + gap + dampak + justifikasi
- **System Context** — Deskripsi lengkap: input, proses, output, outcome, constraints, stakeholders
- **Problem Drift** — Masalah "bermutasi" dari pendahuluan ke metodologi karena statement awal tidak presisi
- **Solution-First Thinking** — Memulai dari solusi tanpa masalah yang jelas — berbahaya dalam riset
- **Operational Definition** — Definisi variabel yang cukup jelas agar peneliti lain bisa mengukur hal yang sama

---

## Template A.2 — Problem Statement Builder

```
PROBLEM STATEMENT BUILDER

Domain & Konteks
  Domain   : Keamanan Siber (Cybersecurity) di Sistem Enterprise
  Konteks  : Perusahaan atau organisasi besar yang punya banyak karyawan dengan akses ke data dan sistem internal

System Context
  Input       : Log aktivitas pengguna — login/logout, buka file, kirim email, transfer data, dari mana aksesnya (IP address), semuanya
                dari Active Directory dan endpoint perusahaan
  Process     : Log-log itu dianalisis buat nyari pola yang mencurigakan, dibandingkan sama perilaku normal si pengguna, pakai rule-based
                SIEM atau Random Forest berbasis UEBA
  Output      : Log-log itu dianalisis buat nyari pola yang mencurigakan, dibandingkan sama perilaku normal si pengguna, pakai rule-based
                SIEM atau Random Forest berbasis UEBA
  Outcome     : Ancaman ketahuan lebih cepat, false alarm berkurang, tim keamanan bisa langsung respon sebelum terlanjur parah
  Constraints : Data log-nya gede banget, ada isu privasi karyawan, dataset insider threat yang berlabel susah dicari, dan harus bisa jalan real-time
  Stakeholders: Tim IT Security/SOC, manajemen perusahaan, karyawan (yang dimonitor), regulatorkepatuhan data

Fenomena → Problem
  Fenomena yang diamati             : Karyawan atau pengguna internal yang harusnya dipercaya ternyata bisa bocorkan atau salahgunakan data perusahaan
  Gejala (symptom) yang terukur     : Ancaman baru ketahuan setelah berjam-jam atau bahkan berhari-hari — detection lag-nya tinggi banget
  Masalah yang didiagnosis          : Sistem SIEM yang sekarang dipakai cuma andalkan aturan yang udah diset manual, jadi nggak bisa nangkep ancaman yang gerakannya
                                      pelan-pelan dan nggak obvious
  Masalah riset (researchable)      : Belum ada studi yang beneran bandingin rule-based SIEM sama ML-based UEBA secara langsung buat deteksi insider threat tipe data
                                      exfiltration di enterprise secara real-time
  Variabel yang terukur             : Detection rate (%), false positive rate (%), waktu deteksi (detik)

Problem Quality Check
   [✓] Clarity       — Orang yang baca bisa langsung ngerti masalahnya apa tanpa harus nanya lagi
  [✓] Measurability — Ada tiga angka yang bisa diukur: detection rate, false positive rate, sama waktu deteksi
  [✓] Relevance     — Insider threat itu nyata dan makin sering kejadian di perusahaan besar
  [✓] Testability   — Bisa diuji pakai CERT Insider Threat Dataset r5.2 yang udah tersedia publik
  [✓] Impact        — Kalau terjawab, hasilnya bisa jadi panduan buat tim IT Security milih sistem deteksi yang lebih efektif
Problem Statement (1 paragraf):
  Di perusahaan besar, karyawan punya akses ke data
  sensitif yang kalau disalahgunakan bisa merugikan
  organisasi. Sistem rule-based SIEM yang sekarang
  banyak dipakai sering kecolongan karena cuma bisa
  deteksi ancaman yang polanya udah jelas — kalau
  ancamannya bergerak pelan-pelan, sistem ini gampang
  kelewatan, dan akibatnya detection lag tinggi plus
  banyak false alarm. Sampai sekarang belum ada
  penelitian yang secara langsung bandingin rule-based
  SIEM sama ML-based UEBA pakai metrik yang sama untuk
  kasus insider threat tipe data exfiltration di
  enterprise. Penelitian ini mau isi gap itu dengan
  ngukur detection rate, false positive rate, dan waktu
  deteksi dari kedua metode pakai CERT Insider Threat
  Dataset r5.2.
```

---

## Latihan 1 — Dari Topik ke Masalah Riset

Pilih satu topik di bidang TI yang diminati. Transformasikan melalui 5 tahap Problem Formation Model.

**Topik awal:** Deteksi Insider Threat pada Sistem Enterprise

| Tahap | Hasil |
|-------|-------|
| Reality | Karyawan di perusahaan punya akses ke sistem dan data internal yang sensitif |
| Observed Issue (Symptom) | Kebocoran data atau akses mencurigakan baru ketahuan setelah berjam-jam atau berhari-hari |
| Diagnosed Problem (Root Cause) | Sistem rule-based SIEM yang dipakai nggak bisa nangkep anomali perilaku yang gerakannya pelan dan nggak melanggar aturan apapun |
| Researchable Problem |Belum ada studi yang bandingin langsung rule-based SIEM vs ML-based UEBA buat deteksi insider threat tipe data exfiltration di enterprise secara real-time|
| Measurable Variable |Detection rate (%), false positive rate (%), waktu deteksi rata-rata (detik)|

**Apakah terjebak solution-first thinking?** [ ] Ya / [V] Tidak

---

## Latihan 2 — System Context Decomposition

Gambarkan konteks sistem dari masalah riset di Latihan 1.

| Komponen | Deskripsi |
|----------|----------|
| Input |Log aktivitas pengguna: login/logout, akses file, transfer data, waktu akses, lokasi IP — dari Active Directory dan endpoint perusahaan|
| Process |Log dianalisis buat nyari pola yang menyimpang dari kebiasaan normal si pengguna, pakai rule-based SIEM atau Random Forest berbasis UEBA|
| Output |Alert yang isinya: siapa usernya, ancamannya apa, seberapa bahaya (severity score)|
| Outcome |Ancaman ketahuan lebih cepat, false alarm berkurang, tim SOC bisa respon lebih efisien|
| Constraints |Volume log gede banget, ada isu privasi karyawan, dataset berlabel susah dicari, harus bisa jalan real-time|
| Stakeholders |Tim IT Security/SOC, manajemen perusahaan, karyawan, regulator kepatuhan data|

**Komponen mana yang paling relevan dengan masalah riset?** 
Process dan Input — karena gap utamanya ada di metode analisisnya, dan kualitas log yang masuk nentuin seberapa akurat deteksinya

---

## Latihan 3 — Problem Quality Check

Evaluasi problem statement yang sudah dibuat menggunakan 5 kriteria.

| Kriteria | Skor (1-5) | Justifikasi |
|----------|-----------|-------------|
| Clarity | 5 | Masalahnya jelas: rule-based SIEM nggak cukup, dan perlu dibandingkan sama ML — bisa dipahami siapapun yang baca |
| Measurability | 5 | Ada tiga metrik yang konkret dan langsung bisa dihitung dari hasil eksperimen |
| Relevance | 5 | Insider threat makin sering terjadi dan jadi salah satu ancaman terbesar di perusahaan saat ini |
| Testability | 4 | Bisa diuji pakai CERT Insider Threat Dataset yang tersedia publik — skor 4 karena dataset enterprise nyata susah diakses karena isu privasi |
| Impact | 5 | Kalau terjawab, hasilnya langsung bisa dipakai tim SOC buat milih metode deteksi yang lebih efektif |

**Skor total:** 24 / 25

**Problem statement versi final (1 paragraf):**
> Di perusahaan besar, karyawan punya akses ke data sensitif yang kalau disalahgunakan bisa merugikan organisasi. Sistem rule-based SIEM yang sekarang banyak dipakai sering kecolongan karena cuma bisa deteksi ancaman yang polanya udah jelas — kalau ancamannya bergerak pelan-pelan, sistem ini gampang kelewatan, dan akibatnya detection lag tinggi plus banyak false alarm. Sampai sekarang belum ada penelitian yang secara langsung bandingin rule-based SIEM sama ML-based UEBA pakai metrik yang sama untuk kasus insider threat tipe data exfiltration di enterprise. Penelitian ini mau isi gap itu dengan ngukur detection rate, false positive rate, dan waktu deteksi dari kedua metode pakai CERT Insider Threat Dataset r5.2.

---

## Refleksi

> Bandingkan "masalah" yang biasa ditemui saat coding (bug, error) dengan masalah riset. Apa perbedaan fundamental dalam cara mendefinisikan dan mendekati keduanya?

**Jawaban:**
> Kalau lagi coding, masalah itu gampang dikenali — ada error, ada pesan kesalahannya, dan tahu persis kapan masalahnya selesai yaitu waktu programnya jalan. Tapi di riset, masalahnya nggak sejelas itu. Harus dulu nentuin: ini gejalanya apa, akar masalahnya apa, dan apa yang sebenernya belum pernah dijawab sebelumnya. Bahkan kalau hasil eksperimennya nunjukin "nggak ada bedanya antara dua metode", itu tetap jawaban yang valid dan harus dilaporin. Intinya, di coding debug sampai beres, di riset mendefinisikan masalah dengan presisi dulu baru bisa ngerancang eksperimennya — dan mendefinisikan masalah itu sendiri udah separuh dari pekerjaannya.
