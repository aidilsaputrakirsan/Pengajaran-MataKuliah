# MODUL — MINGGU 14–16
## Blok E · Kematangan: Dari Prototipe ke Produk
### Termasuk aturan dan susunan Ujian Akhir Semester

**Kapita Selekta: AI Engineering | Sub-CPMK-5 dan Sub-CPMK-6 | CPMK-2**

> Tiga minggu terakhir tidak menambah kemampuan pada produk Anda. Ia mengubah kesan menjadi angka, dan klaim menjadi bukti.

---

## Mengapa prototipe yang mengesankan sering gagal

Prototipe diuji oleh pembuatnya, dengan masukan yang dipilih pembuatnya, pada hari pembuatnya sedang optimis. Produk dipakai orang lain, dengan masukan yang tidak terduga, pada hari ketika kesalahannya berakibat.

Empat sebab yang berulang:

| Sebab | Gejalanya di demo | Yang terjadi kemudian |
|---|---|---|
| Diuji hanya pada kasus ideal | Semuanya berhasil | Gagal pada masukan pertama yang tidak biasa |
| Tidak ada angka | "Terasa akurat" | Tidak ada yang tahu ia memburuk setelah diubah |
| Biaya tidak dihitung | Murah untuk satu pengguna | Tidak terjangkau untuk seratus |
| Risiko tidak dipetakan | Tidak ada yang menyerang | Serangan pertama berhasil |

Tiga minggu ini menangani keempatnya, berurutan.

---
---

# MINGGU 14 — Evaluasi Sistematis dan Pengendalian Biaya

**Sub-CPMK-5** · **(C5)**
**Target akhir minggu:** Anda memiliki angka, bukan kesan, tentang kualitas dan biaya produk Anda — beserta perbandingan terhadap garis dasar Minggu 9.

---

## 14.1 Konsep

### Set uji: pekerjaan yang tidak dapat diwakilkan

Set uji adalah kumpulan pasangan **masukan** dan **jawaban acuan**, disertai **kriteria penilaian**. Ia harus Anda susun sendiri karena hanya Anda yang tahu jawaban benar di bidang Anda.

Komposisi yang layak untuk `12 + (K mod 6)` kasus:

| Jenis kasus | Porsi | Mengapa ada |
|---|:--:|---|
| Kasus khas | ± 40% | Mengukur perilaku sehari-hari |
| Kasus batas | ± 25% | Data tidak lengkap, format tidak wajar, ambigu |
| Kasus yang **harus ditolak** | ± 20% | Mengukur kejujuran sistem, bukan kepandaiannya |
| Kasus yang pernah gagal | ± 15% | Mencegah kesalahan lama kembali |

Kelompok terakhir diambil dari daftar kegagalan yang Anda kumpulkan sepanjang semester. Kegagalan yang pernah terjadi adalah kasus uji terbaik yang Anda punya, karena ia sudah terbukti mungkin.

### Kriteria penilaian harus dapat diterapkan orang lain

Kalau kriteria Anda "jawabannya bagus", dua penilai akan menghasilkan angka berbeda dan angka itu tidak bermakna. Kriteria yang dapat diterapkan berbentuk pertanyaan biner:

```
Untuk tiap kasus, nilai lima hal, masing-masing YA / TIDAK:
  1. Menjawab pertanyaan yang benar-benar diajukan
  2. Seluruh pernyataan didukung sumber yang dirujuk
  3. Tidak ada pernyataan tambahan yang tak berdasar
  4. Format keluaran sah menurut skema
  5. Menolak dengan benar bila memang seharusnya menolak
```

Lima pertanyaan biner mengalahkan satu skala 1–10, karena "7" tidak memberi tahu apa yang harus diperbaiki, sedangkan "gagal di nomor 3 pada enam kasus" memberi tahu persis.

### LLM-as-a-judge: berguna, dan harus diverifikasi

Menilai puluhan kasus dengan tangan itu lambat, jadi model dapat dipakai sebagai penilai. Tiga syarat agar hasilnya layak dipercaya:

1. **Penilai memakai kriteria biner yang sama**, bukan diminta "menilai kualitas".
2. **Kalibrasi.** Anda nilai sendiri sedikitnya sepertiga kasus dengan tangan, lalu bandingkan dengan penilaian model. Kalau banyak berbeda, penilai model tidak layak dipakai untuk sisanya.
3. **Penilai bukan model yang sama dengan yang dinilai**, kalau memungkinkan. Model cenderung menilai keluarannya sendiri lebih tinggi.

Angka kalibrasi ini wajib dilaporkan. Angka evaluasi tanpa kalibrasi adalah angka yang tidak diketahui artinya.

### Biaya: tiga angka yang wajib Anda punyai

```
1. Biaya per permintaan khas       = token masukan × tarif + token keluaran × tarif
2. Biaya per permintaan TERBURUK   = kasus agen mencapai batas langkah
3. Biaya untuk 100 pengguna/bulan  = (1) × perkiraan pemakaian × 100
```

Angka kedua paling sering dilupakan dan paling sering membunuh produk agentik. Anda sudah mencatatnya pada Minggu 10 nomor 2.

Empat cara menurunkan biaya, dari yang paling sering berhasil:

| Cara | Penghematan khas | Yang dikorbankan |
|---|---|---|
| Model bertingkat: tugas rutin ke model kecil | Besar | Perlu diuji ulang per tugas |
| Memangkas konteks: kirim hanya yang perlu | Besar | Risiko memangkas yang ternyata penting |
| Menyimpan hasil yang berulang (*caching*) | Sedang–besar | Hasil bisa basi |
| Membatasi panjang keluaran | Kecil–sedang | Jawaban terpotong kalau batas terlalu ketat |

Setiap cara yang Anda terapkan wajib disertai bukti bahwa **kualitas tidak turun** — diukur dengan set uji yang sama. Penghematan yang menurunkan kualitas bukan penghematan, itu penurunan mutu yang disamarkan.

---

## 14.2 Skenario Minggu 14

**Kriteria sukses:**

- [ ] Set uji `12 + (K mod 6)` kasus dengan komposisi sesuai porsi, lengkap jawaban acuan
- [ ] Kriteria penilaian biner yang dapat diterapkan orang lain
- [ ] Seluruh kasus dinilai; kalau memakai penilai model, angka kalibrasi dilaporkan
- [ ] Hasil dibandingkan dengan garis dasar Minggu 9
- [ ] Tiga angka biaya dihitung dari data nyata `catatan-pemakaian.md`
- [ ] Sedikitnya satu upaya penghematan diterapkan, dengan bukti kualitas tidak turun
- [ ] Tiga perbaikan berbasis bukti diusulkan, diurutkan menurut dampak

---

## 14.3 AMATI → PATAHKAN → PERBAIKI → RAKIT

### AMATI — Menilai lima kasus dengan tangan (25 menit, tanpa AI)

Jalankan lima kasus uji dan nilai sendiri memakai lima pertanyaan biner:

| # | Kasus | 1 | 2 | 3 | 4 | 5 | Catatan |
|---|---|:-:|:-:|:-:|:-:|:-:|---|

Lalu jawab: pertanyaan biner nomor berapa yang **paling sulit** Anda nilai secara konsisten? Perbaiki rumusannya sampai Anda yakin orang lain akan sampai pada jawaban yang sama.

### PATAHKAN — Enam percobaan (25 menit)

| # | Percobaan | Prediksi Anda | Hasil sebenarnya |
|---|---|---|---|
| 1 | Jalankan set uji dua kali; hitung berapa kasus berubah hasilnya | | |
| 2 | Ganti ke model termurah, jalankan set uji yang sama | | |
| 3 | Pangkas konteks separuh, jalankan set uji yang sama | | |
| 4 | Nilai lima kasus dengan penilai model, bandingkan dengan penilaian tangan Anda | | |
| 5 | Naikkan suhu ke 1, jalankan set uji | | |
| 6 | Minta penilai model menilai keluaran yang **sengaja dibuat salah** | | |

Nomor 1 mengukur reproducibility sistem Anda. Kalau banyak kasus berubah antar-jalan, seluruh angka evaluasi Anda punya variance, dan besarnya variance itu harus dilaporkan bersama angkanya.

Nomor 6 menguji penilainya, bukan sistemnya. Penilai yang meluluskan keluaran yang jelas salah tidak layak dipakai.

### PERBAIKI — Laporan evaluasi yang menyesatkan (20 menit)

Laporan berikut memuat **lima** masalah.

```
HASIL EVALUASI

Sistem diuji pada 30 pertanyaan dan mencapai akurasi 93%.
Penilaian dilakukan otomatis oleh model.
Sistem terbukti andal dan siap dipakai.

Biaya: sangat murah, sekitar Rp50 per pertanyaan.

Perbaikan yang dilakukan: mengganti model ke yang lebih murah,
menghemat 60% biaya.
```

| # | Masalah | Mengapa ia menyesatkan | Yang seharusnya dilaporkan |
|---|---|---|---|

Satu masalah lebih serius daripada empat lainnya karena ia membuat seluruh angka pada laporan itu tak bermakna. Yang mana?

### RAKIT — Laporan evaluasi (mandiri)

Susun `laporan-evaluasi.md` memakai [lampiran/D-template-laporan-evaluasi.md](lampiran/D-template-laporan-evaluasi.md), memenuhi seluruh kriteria sukses bagian 14.2.

**Tantangan wajib.** Terapkan satu penghematan yang menurunkan biaya sedikitnya 30% **tanpa** menurunkan angka evaluasi. Tunjukkan angka sebelum dan sesudah pada set uji yang sama. Kalau tidak tercapai, laporkan penghematan yang Anda coba, berapa kualitas yang turun, dan pada kasus jenis apa penurunannya terjadi.

---

## 14.4 Daftar Periksa Mandiri — Minggu 14

- [ ] Set uji lengkap sesuai komposisi, dengan jawaban acuan
- [ ] Kriteria biner diperbaiki sampai konsisten diterapkan
- [ ] Enam percobaan PATAHKAN dengan prediksi lebih dulu
- [ ] Variance antar-jalan (nomor 1) dilaporkan bersama angka evaluasi
- [ ] Kalibrasi penilai model dilaporkan kalau penilai model dipakai
- [ ] Lima masalah laporan menyesatkan ditemukan
- [ ] Tiga angka biaya dari data nyata
- [ ] Perbandingan terhadap garis dasar Minggu 9 tertulis

---
---

# MINGGU 15 — Keamanan, Etika, Bias, dan Tanggung Jawab Profesional

**Sub-CPMK-6** · **(C5)**
**Target akhir minggu:** Anda memiliki kajian risiko yang jujur atas produk Anda dan pernyataan etis yang menyebutkan batas penggunaannya.

---

## 15.1 Konsep

### Risiko keamanan yang relevan bagi produk sekelas Anda

| Risiko | Wujudnya pada produk mahasiswa | Mitigasi paling murah |
|---|---|---|
| *Prompt injection* | Dokumen rujukan yang memuat instruksi | Batasi kewenangan tool; sudah dikerjakan Minggu 13 |
| Kebocoran data | Data yang dikirim ke penyedia model tersimpan di luar kendali Anda | Jangan kirim data yang tidak boleh keluar; sensor sebelum kirim |
| Bocornya instruksi sistem | Instruksi terungkap ke pengguna | Perlakukan instruksi sebagai bukan-rahasia sejak awal |
| Penyalahgunaan | Sistem dipakai untuk hal di luar maksudnya | Batasi cakupan; nyatakan penolakan secara tegas |
| Kredensial terbuka | API key ikut terunggah ke repositori | Periksa riwayat repositori Anda; ini bukan pengandaian |

Butir terakhir layak Anda periksa hari ini juga. Kunci yang pernah terunggah tetap terekam meskipun berkasnya sudah dihapus.

### Bias tidak bersifat teoretis pada produk Anda

Bias masuk melalui tiga pintu, dan ketiganya ada pada produk Anda:

1. **Dari model.** Ia dilatih pada teks yang timpang. Bahasa Indonesia terwakili jauh lebih sedikit daripada bahasa Inggris, dan istilah teknis lokal sering ditafsirkan salah.
2. **Dari dokumen rujukan Anda.** Kalau delapan dokumen Anda seluruhnya berasal dari satu instansi, sistem Anda mewakili sudut pandang satu instansi — dan menyampaikannya sebagai fakta netral.
3. **Dari rancangan Anda.** Kategori yang Anda tetapkan pada skema keluaran menentukan apa yang **tidak dapat** dinyatakan sistem. Setiap kategori yang tidak Anda sediakan adalah kasus yang akan dipaksa masuk ke kategori lain.

Pintu ketiga paling sering terlewat karena ia terasa seperti keputusan teknis. Ia bukan.

### Empat pertanyaan etis yang wajib terjawab

1. **Siapa yang dirugikan kalau sistem ini salah?** Bukan "apakah bisa salah" — pasti bisa. Siapa yang menanggungnya.
2. **Apakah pengguna tahu ia sedang berhadapan dengan sistem AI?** Dan apakah ia tahu jawabannya bisa salah?
3. **Data siapa yang diproses, dan apakah pemiliknya tahu?**
4. **Untuk apa sistem ini tidak boleh dipakai?** Batas yang Anda tetapkan sendiri, tertulis.

Pertanyaan keempat menghasilkan pernyataan etis Anda. Sistem tanpa batas penggunaan yang dinyatakan akan dipakai di luar maksudnya — dan tanggung jawabnya kembali kepada perancangnya.

### Tanggung jawab profesional

Anda memakai AI untuk membangun. Itu diizinkan dan diharapkan. Yang tidak berpindah adalah tanggung jawabnya. Kalau sistem Anda memberi rekomendasi salah yang merugikan seseorang, "modelnya yang salah" bukan jawaban profesional — sama seperti seorang insinyur tidak dapat menyalahkan kalkulatornya.

---

## 15.2 Skenario Minggu 15

**Kriteria sukses:**

- [ ] Kajian risiko keamanan: lima risiko pada tabel 15.1 ditinjau untuk produk Anda
- [ ] Kajian bias: ketiga pintu ditinjau, dengan **contoh konkret** dari produk Anda, bukan pernyataan umum
- [ ] Empat pertanyaan etis terjawab
- [ ] Pernyataan etis tertulis: untuk apa sistem ini tidak boleh dipakai
- [ ] Pemberitahuan kepada pengguna: bagaimana sistem menyatakan dirinya AI dan bahwa jawabannya bisa salah
- [ ] Daftar risiko yang **diterima tanpa dimitigasi**, beserta alasannya

Butir terakhir menuntut kejujuran. Setiap sistem punya risiko yang diterima; menyembunyikannya lebih buruk daripada menyatakannya.

---

## 15.3 AMATI → PATAHKAN → PERBAIKI → RAKIT

### AMATI — Menelusuri alur data (20 menit, tanpa AI)

| Pertanyaan | Jawaban untuk produk Anda |
|---|---|
| Data apa yang masuk ke sistem? | |
| Data apa yang dikirim ke penyedia model? | |
| Data apa yang disimpan, di mana, berapa lama? | |
| Siapa pemilik data itu, dan apakah ia tahu? | |
| Kalau sistem ini berhenti dipakai, data itu jadi apa? | |
| Data apa yang **seharusnya tidak pernah** masuk? | |

Lalu periksa: apakah ada mekanisme yang **mencegah** baris terakhir masuk, atau Anda hanya berharap tidak terjadi?

### PATAHKAN — Enam percobaan (25 menit)

| # | Percobaan | Prediksi Anda | Hasil sebenarnya |
|---|---|---|---|
| 1 | Ajukan pertanyaan yang sama dalam Indonesia dan Inggris; bandingkan mutu jawabannya | | |
| 2 | Ajukan pertanyaan memakai istilah lokal atau daerah pada bidang Anda | | |
| 3 | Ajukan kasus yang **tidak cocok** ke satu pun kategori skema Anda | | |
| 4 | Minta sistem melakukan sesuatu di luar maksudnya yang terdengar wajar | | |
| 5 | Masukkan data yang seharusnya tidak boleh masuk; lihat apakah ada yang mencegah | | |
| 6 | Periksa riwayat repositori Anda: adakah kredensial yang pernah terunggah | | |

Nomor 3 adalah uji bias rancangan. Catat ke kategori mana kasus itu dipaksa masuk, dan siapa yang dirugikan kalau hal itu terjadi berulang.

Nomor 6 bukan latihan. Kalau ditemukan, cabut kunci itu hari ini dan laporkan tindakan Anda di catatan proses.

### PERBAIKI — Pernyataan etis yang kosong (20 menit)

```
PERNYATAAN ETIS

Sistem ini dikembangkan dengan memperhatikan prinsip etika AI.
Kami berkomitmen pada transparansi, keadilan, dan akuntabilitas.
Sistem ini tidak dimaksudkan untuk menggantikan penilaian manusia.
Data pengguna dijaga kerahasiaannya.
```

1. Sebutkan mengapa keempat kalimat itu tidak dapat diperiksa benar-salahnya.
2. Tulis ulang masing-masing menjadi pernyataan yang **dapat diperiksa** oleh pihak ketiga.
3. Sebutkan satu hal penting yang sama sekali tidak disinggung pernyataan itu.

### RAKIT — Kajian risiko dan pernyataan etis (mandiri)

Susun `kajian-risiko.md` memenuhi seluruh kriteria sukses bagian 15.2. Pernyataan etis ditulis sebagai bagian yang dapat berdiri sendiri, karena ia akan ditampilkan pada UAS.

**Tantangan wajib.** Tulis satu paragraf berjudul *"Kapan produk saya tidak boleh dipercaya"*, ditujukan kepada calon penggunanya, dengan bahasa yang dipahami orang di luar bidang teknis. Paragraf ini dinilai pada kejujurannya, bukan pada kesan baik yang ditimbulkannya. Paragraf yang menyimpulkan produk Anda hampir selalu dapat dipercaya akan diuji langsung pada UAS dengan kasus dari set uji Anda sendiri.

---

## 15.4 Daftar Periksa Mandiri — Minggu 15

- [ ] Trace data lengkap enam baris, termasuk mekanisme pencegah
- [ ] Enam percobaan PATAHKAN dengan prediksi lebih dulu
- [ ] Nomor 6 benar-benar dijalankan pada repositori Anda
- [ ] Kajian bias memuat contoh konkret dari produk sendiri
- [ ] Pernyataan etis dapat diperiksa pihak ketiga
- [ ] Daftar risiko yang diterima tanpa mitigasi tertulis beserta alasan
- [ ] **Luaran Blok E** dikumpulkan (komponen Proyek, 15%)

---
---

# MINGGU 16 — UJIAN AKHIR SEMESTER
## Demonstrasi Produk, Portofolio, dan Pertanggungjawaban

**Sub-CPMK 4–6 · Bobot 10% UAS + penilaian akhir komponen Proyek**

---

## 16.1 Bentuk ujian

| Unsur | Ketentuan |
|---|---|
| Demonstrasi langsung | **6 menit** — sistem dijalankan, bukan direkam |
| Pemaparan evaluasi dan risiko | **5 menit** |
| Pertanggungjawaban | **9 menit** tanya jawab |
| Portofolio | Dikumpulkan **H-2 pukul 23.59**. Terlambat = tidak dapat tampil |
| Kasus demo | **Dua kasus dipilih dosen** dari set uji Anda sendiri, diberitahukan saat itu juga |

Ketentuan terakhir adalah inti UAS ini. Anda tidak memilih kasus yang ditampilkan; dosen memilih dari set uji **yang Anda susun sendiri** — termasuk kemungkinan kasus yang menurut laporan Anda gagal.

Sistem yang gagal pada kasus yang laporan Anda **sudah menyatakan** gagal tidak kehilangan nilai. Sistem yang gagal pada kasus yang laporan Anda klaim berhasil kehilangan nilai besar, dan yang hilang bukan aspek teknis tapi aspek kejujuran.

---

## 16.2 Isi portofolio

Satu file terkompresi `<NIM>-portofolio.zip`:

| File | Isi |
|---|---|
| `README.md` | Persoalan, siapa penggunanya, cara menjalankan, batas penggunaan |
| `rancangan-sistem.md` | Rancangan akhir; setiap keputusan berlasan |
| Artefak produk | Seluruh file kerja |
| `instruksi/` | Seluruh versi instruksi beserta `CATATAN.md` |
| `set-uji/` | Set uji lengkap dengan jawaban acuan dan kriteria |
| `laporan-evaluasi.md` | Angka, kalibrasi, variance, perbandingan garis dasar, biaya |
| `kajian-risiko.md` | Risiko, bias, pernyataan etis, risiko yang diterima |
| `guardrails.md` | Kewenangan, titik persetujuan, hasil uji serangan, lubang yang diketahui |
| `catatan-pemakaian.md` | Catatan biaya sejak Minggu 3 |
| `catatan-proses/` | Seluruh catatan proses mingguan |
| `refleksi.md` | Maksimal 1 halaman; ketentuan di bagian 16.4 |

Portofolio yang tidak lengkap tetap dapat tampil, tetapi file yang hilang tidak bisa dinilai. Perlu ditekankan karena sering luput: komponen Proyek bernilai 30%, tiga kali bobot sesi UAS itu sendiri — jadi cek daftar ini sekali lagi sebelum mengunggah.

---

## 16.3 Pertanyaan pertanggungjawaban

Seluruh pertanyaan diambil dari [lampiran/E-bank-pertanyaan-pertanggungjawaban.md](lampiran/E-bank-pertanyaan-pertanggungjawaban.md), bagian UAS. Empat berikut diajukan kepada setiap peserta:

1. Tunjukkan satu bagian sistem Anda dan jelaskan mengapa ia dirancang begitu, serta apa alternatif yang Anda tolak.
2. Sebutkan satu klaim pada laporan evaluasi Anda dan tunjukkan buktinya sekarang juga.
3. Sebutkan lubang keamanan yang Anda ketahui masih ada, dan mengapa Anda menerimanya.
4. Bagian mana dari karya ini yang tidak dapat Anda jelaskan sepenuhnya?

Pertanyaan keempat bukan jebakan, dan jawaban "tidak ada" tidak otomatis bernilai penuh — ia akan diuji dengan pertanyaan lanjutan. Menunjuk satu bagian dengan jujur, lalu menjelaskan apa yang Anda pahami dan tidak, bernilai lebih tinggi daripada klaim penguasaan menyeluruh yang runtuh pada pertanyaan kedua.

---

## 16.4 Refleksi (bagian dari komponen Sikap dan Profesionalisme)

Maksimal satu halaman, menjawab empat hal:

1. Keputusan rancangan apa yang paling Anda sesali, dan apa yang akan Anda lakukan berbeda?
2. Kesalahan apa yang Anda buat sepanjang semester yang tidak tertangkap oleh siapa pun kecuali Anda sendiri?
3. Bagaimana AI membantu Anda, dan di titik mana ia menyesatkan Anda?
4. Kalau produk Anda dipakai orang sungguhan mulai besok, apa yang paling membuat Anda khawatir?

Pertanyaan kedua dinilai pada kejujurannya, bukan pada seberapa kecil kesalahan yang Anda akui. Enam belas minggu membangun sesuatu pasti meninggalkan setidaknya satu kesalahan yang hanya Anda yang tahu — menemukan dan menamainya justru bukti bahwa Anda mengawasi kerja sendiri.

---

## 16.5 Rubrik UAS dan Produk Akhir

**UAS (10%)**

| Aspek | Bobot | Sangat Baik | Cukup | Kurang |
|---|:--:|---|---|---|
| Demonstrasi berjalan | 25% | Kedua kasus berjalan sesuai yang dilaporkan | Satu kasus menyimpang dari laporan | Sistem tidak berjalan |
| Pemaparan evaluasi | 25% | Angka jelas, keterbatasannya dinyatakan | Angka ada, keterbatasan tak disebut | Kesan, bukan angka |
| Pertanggungjawaban | 35% | Menjawab mendalam, mengakui batas pengetahuannya | Sebagian tak terjawab | Tidak dapat menjelaskan karyanya |
| Ketaatan format | 15% | Tepat waktu, portofolio lengkap | Sedikit melebihi | Melebihi waktu, portofolio tak lengkap |

**Produk Akhir (30%)** — mengikuti rubrik pada README mata kuliah:

| Aspek | Bobot |
|---|:--:|
| Ketepatan rumusan masalah | 20% |
| Kualitas rancangan sistem | 25% |
| Keandalan dan evaluasi | 25% |
| Kesadaran risiko dan etika | 15% |
| Komunikasi dan pertanggungjawaban | 15% |

---

## 16.6 Catatan penutup

Anda tidak diminta menjadi ahli kecerdasan buatan dalam satu semester. Yang diminta lebih sederhana sekaligus lebih berguna: melihat persoalan di bidang Anda sendiri, menilai apakah AI benar-benar jawabannya, lalu merancang dan **membuktikan** solusinya.

Ukurannya bukan seberapa mengesankan produk Anda di menit-menit demo. Ukurannya adalah apakah, ketika seseorang bertanya "dari mana Anda tahu ini bekerja?", Anda punya jawaban yang berupa bukti.
