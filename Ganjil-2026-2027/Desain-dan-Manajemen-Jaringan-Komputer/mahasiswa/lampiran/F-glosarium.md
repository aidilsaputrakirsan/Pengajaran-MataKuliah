# Lampiran F — Glosarium

**DMJK | SI2514011 | 4 SKS | Ganjil 2026/2027**

---

## Mengapa modul ini memakai istilah Inggris

Modul memakai istilah yang muncul di **Cisco IOS, antarmuka Packet Tracer, dan dokumentasi resmi**, bukan padanan Indonesianya. Alasannya praktis:

> Perintah yang Anda ketik berbunyi `show ip interface brief`, bukan "tampilkan ringkasan antarmuka".

Pesan error, nama menu, keluaran perintah, dan seluruh rujukan yang akan Anda cari saat macet berbahasa Inggris. Istilah Indonesia yang tidak sama dengan yang tertulis di layar justru menambah satu lapis terjemahan yang harus Anda lakukan sendiri saat sedang bingung.

Padanan Indonesia tetap dicantumkan di sini karena sebagian dokumen resmi program studi menuntut istilah baku.

---

## Istilah dasar dan Layer 1–2

| Istilah dipakai modul | Padanan Indonesia | Artinya |
|---|---|---|
| **file** | berkas | Berkas kerja, misalnya `.pkt` |
| **interface** | antarmuka | Titik sambung pada perangkat, misalnya `Gi0/0` |
| **port** | — | Lubang fisik pada switch, atau nomor port pada Layer 4 |
| **frame** | bingkai | Satuan data Layer 2 |
| **packet** | paket | Satuan data Layer 3 |
| **MAC address** | alamat MAC | Alamat Layer 2; berlaku satu langkah, ditulis ulang tiap router |
| **flooding** / **di-flood** | dibanjirkan | Frame ke tujuan yang belum dikenal dikirim ke semua port kecuali port asal |
| **MAC address table** | tabel alamat MAC | Tabel pasangan MAC dan port yang dipelajari switch sendiri |
| **trunk** | — | Link yang membawa banyak VLAN sekaligus |
| **access port** | port akses | Port yang hanya membawa satu VLAN |
| **native VLAN** | VLAN bawaan | VLAN yang lewat trunk tanpa tag |

## Layer 3 dan routing

| Istilah dipakai modul | Padanan Indonesia | Artinya |
|---|---|---|
| **IP address** | alamat IP | Alamat Layer 3; berlaku ujung ke ujung |
| **IPv6 address** | alamat IPv6 | Alamat 128 bit, delapan kelompok heksadesimal |
| **addressing** | pengalamatan | Perancangan pembagian alamat pada jaringan |
| **subnet mask** / **prefix** | — | Penanda batas antara bagian jaringan dan bagian host |
| **VLSM** | — | Pemberian panjang prefix berbeda sesuai kebutuhan tiap segmen |
| **gateway** | gerbang | Pintu keluar segmen menuju jaringan lain |
| **static route** | rute statis | Rute yang Anda tulis sendiri, bukan hasil protokol routing |
| **default route** | rute bawaan | Rute yang dipakai bila tidak ada rute lain yang cocok |
| **administrative distance** | — | Angka kepercayaan sebuah sumber rute; makin kecil makin dipercaya |
| **metric** | — | Ukuran biaya sebuah rute dalam satu protokol |
| **backup route** | rute cadangan | Rute dengan administrative distance lebih besar; dipakai bila utama hilang |
| **inter-VLAN routing** | — | Merutekan lalu lintas antar-VLAN |

## Layanan jaringan

| Istilah dipakai modul | Padanan Indonesia | Artinya |
|---|---|---|
| **DHCP lease** | masa sewa DHCP | Jangka waktu sebuah IP address dipinjamkan ke satu host |
| **address pool** | kumpulan alamat | Rentang alamat yang boleh dibagikan DHCP atau dipakai NAT |
| **NAT** / **PAT** | — | Penerjemahan alamat privat ke publik; PAT memakai satu alamat dengan banyak port |
| **DNS** | — | Penerjemah nama ke IP address |
| **ACL** | daftar kontrol akses | Aturan yang menyaring lalu lintas berdasarkan kriteria tertentu |
| **filter** | penyaring | Mekanisme yang meloloskan atau menolak lalu lintas |
| **password** | kata sandi | Kredensial masuk perangkat |
| **wireless** | nirkabel | Jaringan tanpa kabel |
| **SSID** | — | Nama jaringan wireless yang terlihat klien |
| **backup** | cadangan | Salinan konfigurasi atau data untuk dipulihkan bila rusak |
| **recovery** | pemulihan | Prosedur mengembalikan jaringan ke keadaan berjalan |

## Diagnosis dan perkakas

| Istilah dipakai modul | Padanan Indonesia | Artinya |
|---|---|---|
| **output** | keluaran | Hasil yang ditampilkan sebuah perintah |
| **log** | catatan | Rekaman peristiwa pada perangkat |
| **Simulation Mode** | mode simulasi | Mode Packet Tracer yang memperlihatkan perjalanan paket per langkah |
| **PDU Details** | — | Tab yang memperlihatkan isi header tiap lapisan |
| **as-built** | terbangun | Dokumentasi yang mencerminkan keadaan jaringan yang sesungguhnya |
| **fault injection** | penyisipan kesalahan | Kesalahan yang sengaja dipasang untuk dilatih didiagnosis |
| **tool** | perkakas | Program bantu, misalnya skrip pemeriksa |
| **checklist** | daftar periksa | Daftar butir yang harus diverifikasi |
| **upload** / **download** | unggah / unduh | Mengirim ke, atau mengambil dari, sistem lain |

---

## Istilah yang **tidak** diterjemahkan, dan mengapa

| Istilah | Alasan |
|---|---|
| **keamanan jaringan** | Baku, lazim di industri Indonesia, dan tetap mudah dicari rujukannya |
| **otomasi jaringan** | Sama; istilah ini sudah mapan dalam bahasa Indonesia |
| **topologi**, **redundansi** | Serapan yang sudah baku dan tidak menghalangi pencarian |
| **luaran** | Istilah baku RPS dan dokumen akademik program studi |
| **READ · BREAK · FIX · BUILD** | Nama tahap khas mata kuliah ini |

---

## Kaidah penulisan pada modul

1. Perintah, nama menu, dan nama mode Packet Tracer ditulis **persis seperti di layar**: `show mac address-table`, tab **Config**, **Simulation Mode**. Jangan diterjemahkan pada laporan Anda.
2. Istilah teknis Inggris ditulis tanpa dicetak miring bila sudah lazim di kelas (frame, packet, trunk, lease, output).
3. Pada laporan dan lembar verifikasi, **pakai istilah yang sama dengan modul dan dengan layar.** Asisten mencocokkan laporan Anda dengan keluaran perintah; istilah yang berbeda memperlambat penilaian dan merugikan Anda sendiri.
4. Bila program studi menuntut istilah baku Indonesia pada dokumen resmi, pakai kolom "Padanan Indonesia" di atas.

---

## Bila sebuah istilah terasa asing

Jangan menghafal tabel ini. Baca bagian **Konsep** pada pekan tempat istilah itu diperkenalkan:

| Istilah | Diperkenalkan pada |
|---|---|
| frame, packet, MAC address, IP address, flooding, ARP | Pekan 1 |
| VLSM, prefix, IPv6 address, addressing | Pekan 2 |
| VLAN, trunk, access port, native VLAN, inter-VLAN routing | Pekan 3 |
| static route, default route, administrative distance, metric | Pekan 4 |
| DHCP lease, address pool, DNS, NAT, PAT | Pekan 5 |
| ACL, filter, password, port security | Pekan 6 |
| as-built, fault injection, diagnosis berlapis | Pekan 7 |
| wireless, SSID, IoT registration server | Pekan 9 |
| backup route, redundansi, gateway internet | Pekan 10 |
| otomasi jaringan, 5G | Pekan 13 |
