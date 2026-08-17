# MODUL DMJK — PEKAN 14-16: PROYEK AKHIR DAN UAS

**SI2514011 | Proyek Tim dan Ujian Akhir | Cisco Packet Tracer 8.2+**

> Dibaca sebelum pekan 14. Mengevaluasi Sub-CPMK 5 dan 6. Proyek dikerjakan berkelompok empat mahasiswa, tetapi UAS memuat komponen praktik individual yang tidak dapat diselesaikan oleh anggota lain.

---

# PEKAN 14 — Proyek Akhir: Perancangan

**Sub-CPMK 6:** Mahasiswa mampu mengintegrasikan seluruh konsep untuk merancang, mensimulasikan, dan mempresentasikan solusi jaringan enterprise pada studi kasus nyata. **(C5)**

**Tingkat:** Challenge

**Target akhir pekan:** Dokumen rancangan lengkap yang disetujui dosen, dan pembagian tanggung jawab yang jelas per anggota.

---

## 14.1 Bentuk Proyek

Tim berisi **4 mahasiswa** — delapan tim untuk satu kelas berisi 31 mahasiswa, sehingga satu tim berisi 3 orang. Setiap tim memilih satu skenario industri dan merancang jaringannya dari nol — bukan melanjutkan file NusantaraNet, tetapi memakai seluruh keterampilan yang dibangun untuk membangunnya.

Parameter tim: `Z` = nomor tim (01–08). Blok alamat yang diberikan `10.1Z.0.0/16`, sehingga tidak bertabrakan dengan blok X mahasiswa mana pun.

### Pembagian tanggung jawab

Setiap anggota memegang satu wilayah dan **wajib dapat menjelaskan seluruhnya**:

| Peran | Tanggung jawab utama |
|---|---|
| Perancang addressing | VLSM seluruh lokasi, rencana VLAN, dokumentasi as-built |
| Insinyur routing | WAN antar-lokasi, rute ringkas, redundansi |
| Insinyur layanan | DHCP, DNS, NAT, port forwarding |
| Insinyur keamanan | ACL, matriks akses, pengerasan perangkat, isolasi tamu dan IoT |

Wireless dan IoT dikerjakan bersama; tidak ada peran khusus untuk itu.

Pada UAS, **setiap anggota diuji secara individual atas seluruh jaringan**, bukan hanya wilayahnya. Pembagian peran mengatur siapa yang mengerjakan, bukan siapa yang perlu paham.

---

## 14.2 Skenario yang Tersedia

Setiap tim memilih satu; maksimal dua tim per skenario. Angka kebutuhan sudah termasuk pertumbuhan, jangan ditambah margin lagi.

### A. Rumah sakit tipe C

Tiga lantai, satu gedung. Segmen: Administrasi (60), Rekam Medis (25), Poliklinik (80), Laboratorium (20), Radiologi (15), Perangkat medis terhubung jaringan (40), WiFi-Pasien (150), Manajemen (8).

Batasan khusus: Rekam Medis tidak boleh dijangkau dari segmen mana pun kecuali Poliklinik dan Administrasi, dan hanya untuk satu layanan. WiFi-Pasien tidak boleh menjangkau apa pun di dalam. Perangkat medis tidak dapat diperbarui perangkat lunaknya dan tidak boleh mengakses internet.

### B. Kampus dua lokasi

Kampus utama dan kampus satelit terhubung tautan lease. Segmen per kampus: Dosen (80), Staf (40), Laboratorium komputer (120), Perpustakaan (30), WiFi-Mahasiswa (400), Server (12), Manajemen (8).

Batasan khusus: WiFi-Mahasiswa segmen terbesar dan paling padat — desain kapasitas wireless wajib dihitung. Laboratorium komputer harus dapat diisolasi total saat ujian daring berlangsung.

### C. Pabrik dan kantor

Kantor administrasi dan area produksi dalam satu kompleks. Segmen: Administrasi (35), Produksi-Terminal (60), Sensor dan kendali mesin (80), Gudang (25), WiFi-Karyawan (70), WiFi-Tamu (20), Server (10), Manajemen (8).

Batasan khusus: segmen kendali mesin tidak boleh terganggu lalu lintas lain dan tidak boleh mengakses internet. Kegagalan jaringan di area produksi menghentikan produksi, jadi redundansi jalur wajib dirancang.

### D. Jaringan ritel multi-cabang

Kantor pusat dan empat gerai. Segmen pusat: Administrasi (30), Server (12), Manajemen (8). Setiap gerai: Kasir (8), Gudang gerai (6), WiFi-Pelanggan (40).

Batasan khusus: segmen Kasir menangani transaksi pembayaran dan harus terisolasi dari WiFi-Pelanggan sepenuhnya. Empat gerai berarti empat tautan WAN — peringkasan rute menjadi penentu.

### E. Perusahaan jasa dengan kerja hibrida

Satu kantor, banyak karyawan bekerja dari luar. Segmen: Kantor-Umum (90), Server (15), DMZ (6), WiFi-Karyawan (100), WiFi-Tamu (30), Manajemen (8).

Batasan khusus: satu-satunya skenario dengan DMZ wajib. Layanan yang diakses dari luar harus berada di DMZ, dan DMZ tidak boleh menjangkau segmen internal. Dua tautan ISP wajib.

### F. Kantor pemerintahan daerah

Kantor dinas utama dan tiga kantor unit pelaksana. Segmen pusat: Pelayanan publik (50), Administrasi internal (40), Server (12), Arsip digital (10), WiFi-Pemohon (60), Manajemen (8). Setiap unit: Umum (15), WiFi (20).

Batasan khusus: Arsip digital hanya boleh dijangkau dari Administrasi internal. WiFi-Pemohon berada di area terbuka dan harus dianggap tidak dipercaya sepenuhnya.

---

## 14.3 Yang Harus Ada di Rancangan

Semua skenario menuntut hal yang sama, hanya angkanya berbeda:

**Addressing**
- VLSM seluruh segmen dari blok `10.1Z.0.0/16`, tersusun agar setiap lokasi dapat diringkas menjadi satu rute
- Alamat tautan WAN, loopback, dan alamat publik
- IPv6 pada minimal dua segmen
- Ruang pertumbuhan disebutkan eksplisit per lokasi

**Lapisan 2 dan 3**
- Rencana VLAN dengan ID yang konsisten
- Inter-VLAN routing di setiap lokasi
- Static routing dengan rute ringkas; cabang memakai rute default
- Redundansi sesuai tuntutan skenario

**Layanan**
- DHCP terpusat dengan relay; pengecualian alamat lengkap
- DNS internal
- PAT, dan port forwarding bila skenario menuntutnya

**Keamanan**
- Matriks kontrol akses seluruh segmen, dengan rujukan baris ACL
- Isolasi segmen yang tidak dipercaya (tamu, pasien, pemohon, perangkat yang tidak dapat diperbarui)
- Pengerasan perangkat dan pembatasan akses administratif
- Port security di segmen yang menuntutnya

**Wireless dan IoT**
- SSID dengan pemetaan VLAN, keamanan WPA2
- Rencana kanal dan penempatan AP, dengan perhitungan cakupan atau kapasitas
- Perangkat IoT bila skenario memuatnya

**Dokumentasi**
- Diagram fisik dan logis
- Tabel addressing
- Tiga prosedur recovery
- Daftar keputusan desain: apa yang dipilih, apa alternatifnya, mengapa yang ini

---

## 14.4 Kerja Pekan 14

### Tahap 1 — Analisis kebutuhan (60 menit)

Sebelum menghitung apa pun, tim menyusun daftar pertanyaan yang **tidak terjawab** oleh deskripsi skenario. Setiap skenario di atas sengaja dibuat tidak lengkap.

Contoh pertanyaan yang seharusnya muncul untuk skenario A: apakah perangkat medis butuh akses ke server rekam medis atau hanya ke server pemantauan sendiri? Apakah WiFi-Pasien perlu dibatasi bandwidth?

Ajukan pertanyaan kepada dosen sebagai pemilik proyek. Jawaban yang tidak diberikan menjadi **asumsi yang wajib ditulis** di dokumen rancangan. Asumsi tertulis adalah bagian dari nilai; asumsi tak tertulis yang ternyata salah menjadi temuan saat UAS.

### Tahap 2 — Rancangan addressing (60 menit)

Perancang addressing memimpin, tiga anggota lain memeriksa. Aturan pemeriksaan: setiap baris diperiksa oleh anggota yang **bukan** pembuatnya, dan pemeriksa mencantumkan namanya di tabel.

### Tahap 3 — Rancangan lapisan atas (50 menit)

Ketiga insinyur menyusun rancangan wilayahnya di atas addressing yang sudah disepakati. Output: daftar rute yang dibutuhkan per router, daftar pool DHCP, dan matriks kontrol akses — semuanya masih di atas kertas, belum di simulator.

Matriks kontrol akses diselesaikan **sebelum** implementasi. Tim yang menulis ACL lebih dulu lalu menyusun matriks dari hasil implementasinya akan menghasilkan kebijakan yang tidak seorang pun putuskan.

---

## 14.5 Yang Dikumpulkan Akhir Pekan 14

Dokumen rancangan, maksimal 12 halaman, memuat seluruh butir 14.3 dalam bentuk rencana, ditambah:

1. Daftar pertanyaan yang diajukan dan jawaban yang diterima
2. Daftar asumsi untuk pertanyaan yang tidak dijawab
3. Tabel pembagian peran, dengan nama pemeriksa untuk setiap bagian addressing
4. Daftar keputusan desain beserta alternatif yang ditolak

Dokumen ini menjadi **Tugas 3** (2,5% dari nilai akhir) dan harus disetujui sebelum implementasi dimulai. Rancangan yang ditolak diperbaiki dalam dua hari; implementasi pekan 15 tidak dapat dimulai tanpa persetujuan.

---
---

# PEKAN 15 — Proyek Akhir: Implementasi dan Pengujian Silang

**Sub-CPMK 6**

**Target akhir pekan:** Jaringan berjalan sesuai rancangan, teruji oleh tim lain, dan dokumentasi as-built selesai.

---

## 15.1 Aturan pekan ini: berhenti menambah

Tidak ada penambahan lingkup di pekan 15. Yang dikerjakan hanya tiga hal: mewujudkan rancangan pekan 14, mengujinya, dan memperbaiki apa yang ditemukan.

Tim yang pada pekan 15 masih memutuskan hal-hal mendasar — blok alamat, jumlah lokasi, kebijakan akses — hampir pasti tidak selesai. Kalau ada bagian rancangan yang ternyata tidak dapat diwujudkan, **catat sebagai temuan** dan sesuaikan, jangan diam-diam ganti rancangan.

## 15.2 Implementasi (sesi pertama)

Bagi pekerjaan menurut peran, tetapi terapkan urutan berikut — mengabaikannya menghasilkan pekerjaan yang saling menunggu:

1. Topologi fisik dan penamaan seluruh perangkat
2. VLAN dan port akses di setiap lokasi
3. Trunk dan inter-VLAN routing
4. Tautan WAN dan static routing; **verifikasi konektivitas penuh sebelum lanjut**
5. DHCP, DNS, NAT
6. Wireless dan IoT
7. ACL dan pengerasan — **paling akhir**

Butir 7 diletakkan terakhir dengan alasan yang sama seperti pekan 6: memasang ACL sebelum jaringan terbukti berfungsi berarti setiap gangguan punya dua kemungkinan sebab sekaligus.

Setelah butir 4 selesai, jalankan pengujian cakupan dan simpan hasilnya. Ini titik pulih Anda kalau ada yang rusak nanti.

## 15.3 Pengujian silang antar-tim (sesi kedua)

Setiap tim dipasangkan dengan tim lain.

**Babak 1 — Audit kebijakan (30 menit).** Tukar file dan matriks kontrol akses. Uji matriks tim lain: cari sel yang tertulis "ditolak" tetapi kenyataannya berhasil. Laporkan temuan dalam bentuk uji yang dapat diulang: dari perangkat mana, ke alamat mana, protokol apa, hasilnya apa.

**Babak 2 — Penanaman fault (20 menit).** Pada file tim lain, tanamkan **5 fault**:

- satu pada lapisan 1–2
- satu pada routing
- satu pada layanan
- satu pada kontrol akses, yang **tidak** menimbulkan keluhan pengguna
- satu bebas

Aturan: satu fault adalah satu baris, satu angka, atau satu kata yang salah. Tidak boleh menghapus blok konfigurasi. Semua fault harus menimbulkan akibat yang dapat dibuktikan. Catat kelimanya, jangan diberikan.

**Babak 3 — Perbaikan (40 menit).** Terima kembali file Anda, perbaiki kelima fault dengan dokumentasi as-built Anda sendiri sebagai bekal. Ukur waktunya.

**Babak 4 — Pembahasan (20 menit).** Kedua tim bertemu dan membahas: fault mana yang paling lama ditemukan, informasi apa yang kurang di dokumentasi, dan temuan audit mana yang nyata dan mana yang salah paham. Perbaiki dokumentasi sekarang.

Hasil pengujian silang dinilai **dua arah**: kualitas temuan Anda terhadap tim lain, dan kecepatan serta ketertelusuran perbaikan Anda sendiri.

## 15.4 Yang Dikumpulkan Akhir Pekan 15

1. `proyek-tim-<Z>.pkt` — jaringan lengkap dan berfungsi
2. `konfigurasi-tim-<Z>.txt` — `show running-config` seluruh perangkat
3. **Dokumen as-built** maksimal 20 halaman: seluruh butir 14.3 sebagai kenyataan, bukan rencana, ditambah tabel selisih rancangan terhadap hasil
4. **Laporan pengujian silang**: temuan Anda terhadap tim lain, dan Fault Report kelima fault yang ditanam pada file Anda
5. **Catatan kontribusi individual**: setiap anggota menulis setengah halaman berisi apa yang ia kerjakan, satu masalah yang ia selesaikan sendiri, dan satu hal yang ia pelajari dari anggota lain

Butir 5 dibaca sebelum UAS dan dipakai untuk menyusun pertanyaan individual. Catatan yang isinya umum tanpa masalah konkret menghasilkan pertanyaan yang lebih dasar, bukan lebih mudah.

---
---

# PEKAN 16 — UAS: Presentasi, Praktik Individual, dan Teori

**Yang dievaluasi:** Sub-CPMK 5 dan 6
**Bobot:** 40% dari nilai akhir

| Komponen | Bobot dari UAS | Sifat |
|---|---:|---|
| Presentasi dan demo tim | 35% | Kelompok |
| Praktik individual | 45% | Individual |
| Teori | 20% | Individual |

Praktik individual diberi bobot terbesar dengan alasan yang eksplisit: ia satu-satunya komponen yang tidak dapat diselesaikan oleh anggota lain. Nilai kelompok yang tinggi dengan praktik individual yang rendah menghasilkan nilai akhir rendah, dan itu memang maksudnya.

---

## 16.1 Presentasi dan Demo Tim (35%)

**Durasi 15 menit per tim**, seluruh anggota berbicara. Delapan tim, total 2 jam, satu sesi.

| Bagian | Waktu | Isi |
|---|---:|---|
| Konteks dan keputusan desain | 4 menit | Skenario, tiga keputusan penting beserta alternatif yang ditolak |
| Demo langsung | 7 menit | Bukan tangkapan layar |
| Pengujian silang | 2 menit | Temuan terhadap tim lain, dan apa yang diperbaiki dari temuan tim lain |
| Tanya jawab | 2 menit | Penanya memilih anggota mana yang menjawab |

Demo wajib memuat empat hal, dan urutannya bebas:

1. Konektivitas antar-lokasi berfungsi
2. Satu kebijakan keamanan terbukti menolak akses yang seharusnya ditolak
3. Klien mendapat alamat lewat DHCP relay dari lokasi jauh
4. Satu skenario kegagalan disimulasikan langsung, beserta apa yang tetap berjalan

Butir 4 sering dihindari tim karena berisiko gagal di depan kelas. Ia justru berbobot paling tinggi di antara keempatnya.

### Penilaian

| Kriteria | Bobot |
|---|---:|
| Rancangan menjawab batasan khusus skenario, bukan rancangan generik | 30% |
| Demo berjalan dan membuktikan yang diklaim | 30% |
| Keputusan desain dijelaskan beserta alternatif yang ditolak | 20% |
| Semua anggota dapat menjawab pertanyaan di luar wilayahnya | 20% |

## 16.2 Praktik Individual (45%)

**Durasi 60 menit, seluruh peserta serentak di laboratorium.** Setiap peserta mengerjakan file sendiri; tidak ada wawancara berurutan.

Anda menerima **parameter W** yang berbeda dari X dan Y sebelumnya, dan sebuah file awal yang sudah memuat topologi fisik dan addressing dasar.

| Tugas | Waktu | Poin |
|---|---:|---:|
| 1. Perbaiki 4 fault pada file yang disediakan | 25 menit | 40 |
| 2. Terapkan satu kebijakan akses baru yang diminta di lembar soal | 15 menit | 25 |
| 3. Tambahkan satu segmen baru dari blok sisa: subnet, VLAN, DHCP | 15 menit | 25 |
| 4. Isi lembar verifikasi: perintah yang Anda jalankan dan output-nya | 5 menit | 10 |

Tugas 1 memuat satu fault yang tidak menimbulkan keluhan pengguna. Tugas 3 menguji apakah Anda dapat menghitung subnet dari ruang sisa tanpa merusak alokasi yang ada.

File dan lembar verifikasi diperiksa dengan pemeriksa otomatis terhadap parameter W. Asisten melakukan viva 60 detik kepada peserta yang hasilnya menyimpang dari pola — nilai sangat tinggi maupun konfigurasi yang mirip dengan peserta lain.

## 16.3 Teori (20%)

**Durasi 60 menit, tertulis.** Tidak ada soal hafalan definisi.

| Bagian | Isi | Poin |
|---|---|---:|
| A | Empat soal diagnosis dari pola gejala: diberikan hasil pengujian, sebutkan penyebab dan uji pembedanya | 40 |
| B | Dua soal perhitungan: VLSM dari kebutuhan host, dan analisis satu alamat terhadap maskanya | 25 |
| C | Dua soal perancangan: menuliskan kebijakan akses sebagai daftar aturan berurutan, dan menjelaskan akibat urutan yang salah | 20 |
| D | Satu soal penilaian teknologi: diberikan sebuah klaim vendor, sebutkan apa yang benar, apa yang menyesatkan, dan pertanyaan apa yang harus diajukan | 15 |

Bagian A memakai format yang sama dengan latihan pekan 12: daftar hasil pengujian, bukan deskripsi cerita.

## 16.4 Ketentuan Gugur

Hal-hal berikut menghasilkan nilai nol pada komponen yang bersangkutan, bukan pengurangan:

| Keadaan | Akibat |
|---|---|
| File memakai blok alamat mahasiswa atau tim lain | Nol pada komponen tersebut |
| File praktik individual memakai parameter selain W milik Anda | Nol pada praktik individual |
| Tidak hadir presentasi tanpa surat yang sah | Nol pada komponen presentasi |
| Catatan kontribusi individual tidak dikumpulkan | Nol pada praktik individual |
| Anggota tim tidak dapat menjelaskan satu pun bagian di luar wilayahnya | Nilai presentasi individu tersebut dipotong setengah |

Untuk kerja kelompok, anggota yang terbukti tidak berkontribusi mendapat maksimal setengah dari nilai kelompok. Pembuktiannya dari tiga sumber: catatan kontribusi, jawaban saat tanya jawab, dan hasil praktik individual.

## 16.5 Ringkasan Komponen Nilai Semester

| Komponen | Bobot | Cara dinilai |
|---|---:|---|
| Praktikum | 20% | Checkpoint 10 pekan (2, 3, 4, 5, 6, 7, 9, 10, 11, 12), masing-masing 2% |
| Tugas | 10% | Tugas 1 (pekan 2), 2 (pekan 4), 3 (pekan 14), 4 (pekan 13), masing-masing 2,5% |
| Keaktifan dan diskusi | 5% | Partisipasi kelas dan forum |
| UTS | 25% | Ujian praktik individual pekan 8 |
| UAS | 40% | Presentasi 35%, praktik individual 45%, teori 20% dari komponen ini |

Praktikum dinilai di sepuluh pekan; pekan 13 masuk Tugas 4 dan pekan 14–15 masuk UAS, sehingga tidak dihitung dua kali.
