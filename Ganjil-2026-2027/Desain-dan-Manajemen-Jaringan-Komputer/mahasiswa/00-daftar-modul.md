# DMJK — Daftar Modul dan Cara Memakainya

**Desain dan Manajemen Jaringan Komputer | SI2514011 | 4 SKS | Ganjil 2026/2027**
**Perangkat: Cisco Packet Tracer 8.2 atau lebih baru**

---

## Bacalah berurutan

| File | Dibaca sebelum | Isi |
|---|---|---|
| [01-spesifikasi-proyek.md](01-spesifikasi-proyek.md) | Pekan 1 | Proyek NusantaraNet yang Anda bangun sepanjang semester |
| [02-modul-pekan-01-04.md](02-modul-pekan-01-04.md) | Pekan 1 | Fundamental, VLSM dan IPv6, VLAN, static routing |
| [03-modul-pekan-05-07.md](03-modul-pekan-05-07.md) | Pekan 5 | DHCP/DNS/NAT, keamanan dan ACL, dokumentasi dan diagnosis |
| [04-modul-pekan-08-uts.md](04-modul-pekan-08-uts.md) | Pekan 7 | Aturan, skenario, dan susunan UTS |
| [05-modul-pekan-09-10.md](05-modul-pekan-09-10.md) | Pekan 9 | Wireless, IoT, gateway internet dan redundansi |
| [06-modul-pekan-11-13.md](06-modul-pekan-11-13.md) | Pekan 11 | Konsolidasi, latihan soal, 5G dan otomasi jaringan |
| [07-modul-pekan-14-16-proyek-akhir.md](07-modul-pekan-14-16-proyek-akhir.md) | Pekan 13 | Proyek akhir tim dan UAS |

Baca modul **sebelum** sesi praktikum, bukan saat sesi berjalan. Bagian Konsep dirancang untuk dibaca lebih dulu; tiga tahap sisanya dikerjakan di laboratorium.

## Lampiran

| File | Dipakai untuk |
|---|---|
| [lampiran/A-command-reference.md](lampiran/A-command-reference.md) | Referensi perintah IOS. Mulai pekan 5, ini satu-satunya tempat perintah tersedia |
| [lampiran/B-parameter-individual.md](lampiran/B-parameter-individual.md) | Aturan penurunan seluruh alamat dan VLAN Anda dari angka X |
| [lampiran/C-template-laporan.md](lampiran/C-template-laporan.md) | Template laporan mingguan, maksimal 2 halaman |
| [lampiran/D-template-fault-report.md](lampiran/D-template-fault-report.md) | Template laporan diagnosis, dipakai pekan 7, 11, 12, 15, dan UTS |
| [lampiran/E-bank-pertanyaan-viva.md](lampiran/E-bank-pertanyaan-viva.md) | Seluruh pertanyaan viva, dibagikan terbuka |
| [lampiran/F-glosarium.md](lampiran/F-glosarium.md) | Padanan istilah Inggris dan Indonesia, serta di pekan mana tiap istilah diperkenalkan |

Lampiran E memuat semua pertanyaan yang mungkin ditanyakan asisten. Ia tidak dirahasiakan karena tidak satu pun dapat dijawab dengan menghafal — semuanya menunjuk ke layar Anda dan menanyakan jaringan Anda sendiri.

---

## Empat hal yang perlu Anda ketahui sebelum pekan 1

**1. Semua angka Anda berbeda dari teman Anda.** Pada pekan 1 Anda menerima lembar parameter berisi angka **X**, yaitu nomor urut Anda pada daftar peserta. Seluruh IP address, ID VLAN, dan nama perangkat diturunkan dari X. File yang memuat blok alamat mahasiswa lain dinilai nol untuk pekan tersebut, dan pemeriksaannya otomatis. Kebutuhan jumlah host juga bergantung pada X, sehingga menyalin tabel subnet teman bukan hanya terdeteksi — hasilnya salah.

**2. Modul akan berhenti memberi perintah.** Pekan 1 sampai 4 memberi konfigurasi lengkap dan berurutan. Mulai pekan 5 yang Anda terima hanya kebutuhan dan tabel kosong, dengan perintah dipindah ke Lampiran A tanpa urutan pengerjaan. Mulai pekan 7 bahkan kebutuhan pun diganti skenario dan kriteria sukses. Empat pekan pertama adalah persiapan untuk itu.

**3. Setiap pekan punya empat tahap.**

| Tahap | Yang dikerjakan |
|---|---|
| READ | Membaca konfigurasi yang jalan dan menjelaskan mengapa ia bekerja. Tanpa AI |
| BREAK | Merusak sendiri dari tabel percobaan. **Kolom prediksi diisi sebelum mencoba** |
| FIX | Memperbaiki file yang sudah dirusak dosen; jumlah kesalahannya selalu diberitahukan |
| BUILD | Menambah satu lapisan ke proyek NusantaraNet Anda |

Prediksi yang keliru dan tercatat lebih bernilai daripada kolom yang dikosongkan. Prediksi yang tepat sempurna untuk seluruh baris akan ditanyakan asisten.

**4. Anda boleh memakai AI.** Yang dinilai adalah apakah Anda bisa membaca, memverifikasi, dan mempertanggungjawabkan konfigurasi tersebut. Setiap pekan menyediakan Prompt Pack, termasuk prompt untuk memeriksa apakah perintah yang diberikan AI memang didukung Packet Tracer — dan ia sering tidak, karena Packet Tracer hanya mengimplementasikan sebagian perintah Cisco IOS.

Yang dilarang: menempelkan konfigurasi perangkat jaringan kampus yang sungguhan, password asli, atau data pribadi mahasiswa lain ke layanan AI mana pun.

---

## Penilaian

| Komponen | Bobot |
|---|---:|
| Praktikum — Checkpoint pada pekan 2, 3, 4, 5, 6, 7, 9, 10, 11, 12 | 20% |
| Tugas — pekan 2, 4, 13, 14 | 10% |
| Keaktifan dan diskusi | 5% |
| UTS — praktik individual pekan 8 | 25% |
| UAS — presentasi, praktik individual, teori | 40% |

Bobot di dalam setiap Checkpoint: konfigurasi berfungsi 40%, verifikasi dan analisis hasil 25%, dokumentasi 20%, tantangan wajib 15%.

Praktikum dikerjakan **berpasangan tetapi dinilai individual**: Anda bekerja berdua di satu meja, masing-masing membangun jaringan dengan parameter sendiri. Peran wajib bertukar di pertengahan sesi.

## Yang dikumpulkan setiap pekan

1. File `nusantaranet-<NIM>-p<pekan>.pkt` — kumulatif, bukan hanya bagian pekan itu
2. `konfigurasi-p<pekan>.txt` — hasil `show running-config` seluruh perangkat
3. Laporan memakai template Lampiran C, maksimal 2 halaman
