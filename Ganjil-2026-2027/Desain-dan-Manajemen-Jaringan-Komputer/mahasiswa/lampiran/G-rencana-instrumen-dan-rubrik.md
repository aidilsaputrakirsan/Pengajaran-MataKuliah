# Lampiran G — Rencana Instrumen dan Rubrik Penilaian

**DMJK | SI2514011 | Ganjil 2026/2027**

Seluruh yang dinilai sepanjang semester, beserta rubriknya, dikumpulkan di satu tempat. Tidak ada kriteria rahasia: apa yang dinilai dan bagaimana angkanya keluar tertulis lengkap di sini sejak pekan 1.

Rinciannya tetap ada di modul pekan masing-masing; lampiran ini merangkumnya supaya Anda dapat memeriksa posisi nilai kapan saja tanpa membuka tujuh berkas.

---

## Ringkasan bobot

| Komponen | Bobot | Instrumen |
|---|---:|---|
| Tugas | 15% | Keaktifan (5%) + Tugas 1–4 (2,5% masing-masing) |
| Praktikum | 20% | Checkpoint pada sepuluh pekan |
| UTS | 25% | Ujian praktik individual pekan 8 |
| UAS | 40% | Presentasi 35% · praktik individual 45% · teori 20% dari komponen ini |

### Skala rubrik 1–4

Dipakai pada seluruh Checkpoint dan pada penilaian presentasi UAS.

| Skor | Sebutan | Artinya |
|:--:|---|---|
| 4 | Sangat Baik | Tercapai penuh, dan Anda dapat menjelaskan mengapa |
| 3 | Baik | Tercapai, penjelasan sebagian |
| 2 | Cukup | Tercapai sebagian |
| 1 | Kurang | Hadir dan mencoba, hasil belum tercapai |
| 0 | Gugur | Ketentuan gugur terpenuhi — bukan sekadar nilai rendah |

Skor diubah ke skala 0–100 dengan `(skor − 1) ÷ 3 × 100`, sehingga 1 menjadi 0 dan 4 menjadi 100. Nol tetap nol.

**Ketentuan gugur** berlaku pada seluruh instrumen: berkas yang memuat blok alamat milik mahasiswa lain dinilai nol pada pekan tersebut, dan pemeriksaannya otomatis.

---

## 1. Keaktifan dan Diskusi

**Bobot 5% · Kategori Tugas · Dinilai sepanjang semester**

### Yang dinilai

Partisipasi di kelas dan di forum LMS. Bukan jumlah komentar, melainkan apakah pertanyaan dan jawaban Anda menunjukkan bahwa Anda sudah membaca modul dan sudah mencoba sendiri lebih dulu.

Yang dihitung: pertanyaan yang menyertakan apa yang sudah Anda coba dan hasilnya · jawaban atas pertanyaan rekan yang menunjuk penyebab, bukan sekadar memberi perintah untuk disalin · tanggapan pada diskusi kelas saat asisten membuka kasus.

### Rubrik

| Nilai | Ciri |
|---|---|
| 86–100 | Bertanya berformat (gejala, yang sudah dicoba, dugaan) dan membantu rekan dengan menjelaskan penyebab. Muncul konsisten, bukan hanya menjelang tenggat |
| 76–85 | Bertanya dan menjawab secara berkala, sebagian menunjukkan usaha mandiri lebih dulu |
| 66–75 | Berpartisipasi seperlunya, umumnya menanyakan perintah tanpa menyebut apa yang sudah dicoba |
| 56–65 | Jarang, dan hampir selalu meminta jawaban jadi |
| di bawah 56 | Hampir tidak ada jejak partisipasi |

Menempelkan konfigurasi perangkat jaringan kampus yang sungguhan, password asli, atau data pribadi rekan ke forum maupun ke layanan AI mana pun dinilai nol pada komponen ini, terlepas dari keaktifan lainnya.

---

## 2. Tugas 1: Desain dan Perencanaan IP Address

**Bobot 2,5% · Kategori Tugas · Dikumpulkan pekan 2 · Sub-CPMK 2**

### Yang dinilai

Tabel perencanaan VLSM dan IPv6 untuk parameter **X Anda sendiri**, mengikuti kebutuhan host pada modul pekan 2. Kebutuhan host bergantung pada X, sehingga tabel milik rekan menghasilkan angka yang salah.

Yang dikumpulkan: tabel subnet lengkap (alamat jaringan, rentang host, broadcast, mask) beserta perhitungan yang menunjukkan urutan alokasi dari kebutuhan terbesar.

### Rubrik

| Nilai | Ciri |
|---|---|
| 86–100 | Alokasi optimal, sisa ruang alamat tersusun rapi untuk pertumbuhan, dan setiap keputusan terdokumentasi |
| 76–85 | Desain baik dan efisien, dokumentasi sebagian |
| 66–75 | Desain logis dan cukup efisien |
| 56–65 | Desain dasar, ruang alamat terbuang banyak |
| 51–56 | Desain tidak logis atau perhitungan tidak konsisten |

---

## 3. Tugas 2: Konfigurasi Switching dan Routing

**Bobot 2,5% · Kategori Tugas · Dikumpulkan pekan 4 · Sub-CPMK 3**

### Yang dinilai

Berkas NusantaraNet Anda setelah VLAN, trunk, inter-VLAN routing, dan static routing antar-lokasi terpasang, beserta bukti pengujian konektivitas.

Yang dikumpulkan: `nusantaranet-<NIM>-p4.pkt` · `konfigurasi-p4.txt` hasil `show running-config` seluruh perangkat · laporan Lampiran C maksimal dua halaman.

### Rubrik

| Nilai | Ciri |
|---|---|
| 86–100 | Seluruh fungsi berjalan dan sudah disertai kontrol akses dasar yang beralasan |
| 76–85 | Seluruh fungsi berjalan dengan baik dan terverifikasi |
| 66–75 | Fungsi utama berjalan |
| 56–65 | Sebagian berfungsi |
| 51–56 | Konfigurasi error atau tidak dapat diuji |

Klaim "sudah berfungsi" tanpa keluaran perintah pemeriksaan tidak dinilai sebagai berfungsi.

---

## 4. Checkpoint Praktikum Pekan 2-4

**Bobot 6% · Kategori Praktikum · Dinilai akhir tiap sesi · Sub-CPMK 3**

### Yang dinilai

Checkpoint diperiksa asisten langsung di layar Anda pada akhir sesi pekan 2, 3, dan 4, ditambah satu pertanyaan lisan singkat dari Lampiran E. Nilai akhir instrumen ini adalah rata-rata ketiga pekan.

### Rubrik

Empat dimensi, dinilai 1–4, dengan bobot berikut:

| Dimensi | Bobot | Yang diperiksa |
|---|---:|---|
| Konfigurasi berfungsi | 40% | Hasil yang diminta pekan itu benar-benar berjalan |
| Verifikasi dan analisis hasil | 25% | Anda menjalankan sendiri perintah pemeriksaan dan dapat membaca keluarannya |
| Dokumentasi | 20% | Laporan Lampiran C terisi, termasuk kolom prediksi tahap BREAK |
| Tantangan wajib | 15% | Varian yang diminta modul dikerjakan |

Pertanyaan lisan menanyakan alasan di balik konfigurasi, bukan sintaksnya. Ketidakmampuan menjelaskan bagian yang Anda tulis sendiri menurunkan dimensi "konfigurasi berfungsi", karena yang dinilai adalah penguasaan, bukan keberadaan teks.

---

## 5. Checkpoint Praktikum Pekan 5-7

**Bobot 6% · Kategori Praktikum · Sub-CPMK 3 dan 4**

### Yang dinilai

Checkpoint pekan 5 (DHCP, DNS, NAT), pekan 6 (keamanan dan ACL), serta pekan 7 (dokumentasi as-built dan diagnosis sistematis). Rata-rata ketiganya.

Mulai pekan 5 modul tidak lagi memberi perintah berurutan — perintah pindah ke Lampiran A tanpa urutan pengerjaan. Mulai pekan 7 kebutuhan diganti skenario beserta kriteria sukses.

### Rubrik

Empat dimensi yang sama dengan instrumen 4 (40/25/20/15). Tambahan khusus pekan 7:

| Yang diperiksa | Masuk dimensi |
|---|---|
| Dokumen as-built sesuai keadaan perangkat yang sebenarnya | Dokumentasi |
| Fault Report lengkap kelima bagiannya: gejala, cakupan, hipotesis, akar masalah, verifikasi | Verifikasi dan analisis |

Fault Report yang menyebut akar masalah tanpa langkah verifikasi dinilai maksimal 2 pada dimensi verifikasi, karena menebak dengan benar tidak sama dengan mendiagnosis.

---

## 6. UTS: Ujian Praktik Individual

**Bobot 25% · Kategori Tes / Ujian (UTS) · Pekan 8 · Sub-CPMK 1–4**

### Yang dinilai

Ujian praktik individual 150 menit di laboratorium. Anda menerima **parameter Y** saat ujian dimulai, berbeda dari X yang dipakai sepanjang semester, sehingga tidak ada bagian berkas NusantaraNet Anda yang dapat dipakai ulang.

Boleh dibuka: modul dan seluruh lampiran, dokumentasi as-built Anda, catatan tulisan tangan, kalkulator biasa. Tidak boleh: layanan AI, berkas `.pkt` mana pun, kalkulator subnet, komunikasi dengan siapa pun.

### Rubrik

Poin langsung, total 100:

| Bagian | Isi | Waktu | Poin |
|:--:|---|---:|---:|
| A | Perencanaan addressing | 25 menit | 25 |
| B | Implementasi | 80 menit | 45 |
| C | Diagnosis pada berkas yang disediakan | 30 menit | 20 |
| D | Dokumentasi | 15 menit | 10 |

Berkas ujian yang memuat blok alamat milik X Anda, atau milik mahasiswa lain, dinilai nol pada seluruh Bagian B.

Skenario lengkap, kebutuhan host, dan kebijakan akses yang diminta ada di `04-modul-pekan-08-uts.md` dan dibagikan sebelum pekan 8.

---

## 7. Checkpoint Praktikum Pekan 9-10

**Bobot 4% · Kategori Praktikum · Sub-CPMK 5**

### Yang dinilai

Checkpoint pekan 9 (jaringan nirkabel dan perangkat IoT) dan pekan 10 (gateway internet, redundansi, konektivitas cloud). Rata-rata keduanya.

### Rubrik

Empat dimensi yang sama (40/25/20/15). Penekanan pekan ini:

| Yang diperiksa | Masuk dimensi |
|---|---|
| Desain WLAN menjawab kebutuhan cakupan dan keamanan, bukan sekadar menyala | Konfigurasi berfungsi |
| Analisis jalur redundan menyebutkan apa yang tetap berjalan saat satu jalur putus | Verifikasi dan analisis |

---

## 8. Checkpoint Praktikum Pekan 11-12

**Bobot 4% · Kategori Praktikum · Sub-CPMK 4**

### Yang dinilai

Dua pekan konsolidasi. Lembar jawaban dikumpulkan pada akhir tiap sesi; sebagian soal diperiksa terhadap kunci parameter X Anda, sebagian dibahas lisan.

Pekan 11 menekankan perhitungan VLSM tanpa alat bantu dan penyempitan kemungkinan penyebab dari pola gejala. Pekan 12 menekankan identifikasi akar masalah beserta cakupan dampaknya.

### Rubrik

Empat dimensi yang sama (40/25/20/15). Pada dua pekan ini, "konfigurasi berfungsi" dibaca sebagai ketepatan jawaban terhadap kunci, dan "verifikasi dan analisis" sebagai mutu penalaran yang menuntun ke jawaban itu.

Soal diagnosis berbobot dua kali soal lainnya. Jawaban benar tanpa uji pembeda yang menjelaskan mengapa kemungkinan lain gugur dinilai separuh.

---

## 9. Tugas 4: Analisis Relevansi 5G dan Otomasi Jaringan

**Bobot 2,5% · Kategori Tugas · Dikumpulkan pekan 13 · Sub-CPMK 5**

### Yang dinilai

Analisis relevansi lima teknologi — 5G, Wi-Fi generasi lanjut, IPv6 menyeluruh, otomasi konfigurasi, dan zero trust — bagi **jaringan NusantaraNet yang Anda rancang sendiri**, bukan bagi organisasi pada umumnya.

Materi ini tidak dipraktikkan di Packet Tracer karena tidak didukung. Yang dinilai adalah putusan dan alasannya, bukan kemampuan menyalin skrip yang tidak pernah dijalankan.

### Rubrik

| Nilai | Ciri |
|---|---|
| 86–100 | Beralasan berbasis keadaan jaringan sendiri, dan memuat audit tajam terhadap klaim vendor |
| 76–85 | Beralasan dan berpijak pada jaringan sendiri |
| 66–75 | Putusan beralasan, sebagian masih generik |
| 56–65 | Putusan tanpa alasan konkret |
| 51–56 | Mengulang uraian modul |

**Putusan "tidak relevan" yang beralasan bernilai sama tinggi dengan putusan "terapkan sekarang".** Tugas yang menyatakan kelima teknologi perlu diterapkan segera hampir pasti tidak dikerjakan berdasarkan keadaan jaringan mana pun, dan akan dinilai rendah.

---

## 10. Tugas 3: Dokumen Rancangan Proyek Akhir

**Bobot 2,5% · Kategori Tugas · Dikumpulkan pekan 14 · Sub-CPMK 6**

### Yang dinilai

Dokumen rancangan jaringan enterprise maksimal 12 halaman, disusun tim, beserta daftar asumsi dan keputusan desain. Dokumen ini **harus disetujui sebelum implementasi dimulai** — rancangan yang ditolak diperbaiki dalam dua hari, dan implementasi pekan 15 tidak dapat dimulai tanpa persetujuan.

### Rubrik

| Nilai | Ciri |
|---|---|
| 86–100 | Alternatif yang ditolak dijelaskan beserta alasan penolakannya |
| 76–85 | Keputusan desain dijustifikasi |
| 66–75 | Menjawab batasan skenario, asumsi ditulis lengkap |
| 56–65 | Rancangan generik, asumsi tidak ditulis |
| 51–56 | Tidak menjawab batasan khusus skenario |

---

## 11. UAS: Presentasi, Praktik Individual, dan Teori

**Bobot 40% · Kategori Tes / Ujian (UAS) · Pekan 16 · Sub-CPMK 5 dan 6**

Tiga komponen dengan bobot berbeda di dalam UAS:

| Komponen | Bobot dari UAS | Sifat |
|---|---:|---|
| Presentasi dan demo tim | 35% | Kelompok |
| Praktik individual | 45% | Individual |
| Teori | 20% | Individual |

Praktik individual diberi bobot terbesar karena ia satu-satunya komponen yang tidak dapat diselesaikan oleh anggota lain. Nilai kelompok tinggi dengan praktik individual rendah menghasilkan nilai akhir rendah, dan itu memang maksudnya.

### 11a. Presentasi dan demo tim — rubrik

Durasi 15 menit per tim, seluruh anggota berbicara. Dinilai dengan skala 1–4 pada empat kriteria:

| Kriteria | Bobot |
|---|---:|
| Rancangan menjawab batasan khusus skenario, bukan rancangan generik | 30% |
| Demo berjalan dan membuktikan yang diklaim | 30% |
| Keputusan desain dijelaskan beserta alternatif yang ditolak | 20% |
| Semua anggota dapat menjawab pertanyaan di luar wilayahnya | 20% |

Demo wajib memuat empat hal: konektivitas antar-lokasi, satu kebijakan keamanan yang terbukti menolak akses, klien mendapat alamat lewat DHCP relay dari lokasi jauh, dan satu skenario kegagalan yang disimulasikan langsung beserta apa yang tetap berjalan. Butir terakhir berbobot paling tinggi di antara keempatnya.

### 11b. Praktik individual — rubrik

Durasi 60 menit, serentak di laboratorium, dengan **parameter W** yang berbeda dari X dan Y. Poin langsung, total 100:

| Tugas | Waktu | Poin |
|---|---:|---:|
| 1. Perbaiki 4 fault pada berkas yang disediakan | 25 menit | 40 |
| 2. Terapkan satu kebijakan akses baru dari lembar soal | 15 menit | 25 |
| 3. Tambahkan satu segmen baru dari blok sisa: subnet, VLAN, DHCP | 15 menit | 25 |
| 4. Isi lembar verifikasi: perintah yang dijalankan dan keluarannya | 5 menit | 10 |

Tugas 1 memuat satu fault yang tidak menimbulkan keluhan pengguna. Berkas diperiksa otomatis terhadap parameter W.

### 11c. Teori — rubrik

Durasi 60 menit, tertulis. Tidak ada soal hafalan definisi. Poin langsung, total 100:

| Bagian | Isi | Poin |
|:--:|---|---:|
| A | Empat soal diagnosis dari pola gejala | 40 |
| B | Dua soal perhitungan VLSM dan analisis alamat terhadap masknya | 25 |
| C | Dua soal perancangan kebijakan akses sebagai daftar aturan berurutan | 20 |
| D | Satu soal penilaian klaim vendor | 15 |

### Ketentuan gugur pada UAS

| Keadaan | Akibat |
|---|---|
| Berkas memakai blok alamat mahasiswa atau tim lain | Nol pada komponen tersebut |
| Berkas praktik individual memakai parameter selain W milik Anda | Nol pada praktik individual |
| Tidak hadir presentasi tanpa surat yang sah | Nol pada komponen presentasi |
| Catatan kontribusi individual tidak dikumpulkan | Nol pada praktik individual |
| Anggota tim tidak dapat menjelaskan satu pun bagian di luar wilayahnya | Nilai presentasi individu tersebut dipotong setengah |

Anggota yang terbukti tidak berkontribusi mendapat maksimal setengah dari nilai kelompok. Pembuktiannya dari tiga sumber: catatan kontribusi, jawaban saat tanya jawab, dan hasil praktik individual.

---

## Skala huruf

Mengikuti Panduan Akademik ITK:

| Nilai angka | Huruf |
|---|---|
| 86 ≤ N ≤ 100 | A |
| 76 ≤ N < 86 | AB |
| 66 ≤ N < 76 | B |
| 56 ≤ N < 66 | BC |
| 51 ≤ N < 56 | C |
| 41 ≤ N < 51 | D |
| N < 41 | E |

Kehadiran minimum **80%**. Tidak hadir pada salah satu evaluasi blok — UTS atau UAS — menghasilkan nilai akhir maksimal D pada seluruh komponen penilaian.
