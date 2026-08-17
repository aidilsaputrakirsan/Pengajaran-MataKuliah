# DMJK — Desain dan Manajemen Jaringan Komputer

**SI2514011 | 4 SKS | Semester 3 | Ganjil 2026/2027 | Cisco Packet Tracer 8.2+**

Revisi menyeluruh atas modul Ganjil 2025/2026. Diagnosis dan alasan setiap keputusan ada di pengajar/01-rancangan-revisi.md (privat — repo dosen).

---

## Pemisahan File

| Folder | Pembaca | Boleh di-upload ke LMS |
|---|---|---|
| [mahasiswa/](mahasiswa/) | Mahasiswa | **Ya, seluruhnya** |
| pengajar/ (privat — repo dosen) | Dosen dan asisten | **Tidak** |

Aturannya sederhana: seluruh isi `mahasiswa/` aman di-upload tanpa perlu diperiksa lagi. Tidak ada satu pun kunci jawaban, resep fault, atau catatan internal di dalamnya.

---

## Kesiapan File

### Siap dibagikan ke mahasiswa

| File | Dibagikan | Catatan |
|---|---|---|
| [mahasiswa/00-daftar-modul.md](mahasiswa/00-daftar-modul.md) | Pekan 1 | Indeks, aturan main, penilaian |
| [mahasiswa/01-spesifikasi-proyek.md](mahasiswa/01-spesifikasi-proyek.md) | Pekan 1 | Proyek NusantaraNet |
| [mahasiswa/02-modul-pekan-01-04.md](mahasiswa/02-modul-pekan-01-04.md) | Pekan 1 | Contoh konfigurasi memakai X=27 |
| [mahasiswa/03-modul-pekan-05-07.md](mahasiswa/03-modul-pekan-05-07.md) | Pekan 4 | Scaffolding mulai dikurangi |
| [mahasiswa/04-modul-pekan-08-uts.md](mahasiswa/04-modul-pekan-08-uts.md) | Pekan 7 | Skenario UTS terbuka, parameter Y dirahasiakan sampai hari ujian |
| [mahasiswa/05-modul-pekan-09-10.md](mahasiswa/05-modul-pekan-09-10.md) | Pekan 8 | |
| [mahasiswa/06-modul-pekan-11-13.md](mahasiswa/06-modul-pekan-11-13.md) | Pekan 10 | Memuat 14 soal latihan berparameter X |
| [mahasiswa/07-modul-pekan-14-16-proyek-akhir.md](mahasiswa/07-modul-pekan-14-16-proyek-akhir.md) | Pekan 13 | Enam skenario industri, rubrik UAS |
| [mahasiswa/lampiran/](mahasiswa/lampiran/) | Pekan 1 | A sampai F, keenamnya untuk mahasiswa |
| `lembar-parameter/parameter-<NIM>.md` | Pekan 1 | Dibangkitkan skrip; **satu file ke satu mahasiswa**, bukan disebar semua |

File terakhir adalah satu-satunya yang tidak boleh dibagikan serentak: setiap mahasiswa hanya menerima lembarnya sendiri.

### Jangan dibagikan

| File | Alasan |
|---|---|
| pengajar/01-rancangan-revisi.md (privat — repo dosen) | Dokumen keputusan internal |
| pengajar/02-panduan-fault-injection.md (privat — repo dosen) | Berisi resep setiap fault beserta gejalanya |
| pengajar/rps/ (privat — repo dosen) | Untuk prodi; boleh dibagikan bila memang dituntut, bukan bagian modul |
| pengajar/tools/ (privat — repo dosen) | Tool penilaian |
| `lembar-parameter/master-parameter.csv` | Parameter seluruh kelas |
| `lembar-parameter/KUNCI-vlsm.md` | Kunci alokasi VLSM |
| File `.pkt` sehat dan lembar kunci fault | Kunci jawaban tahap FIX |

---

## Susunan Semester

| Pekan | Topik | Tingkat | Sub-CPMK | Penilaian |
|---|---|---|:-:|---|
| 1 | Fundamental dan analisis paket | Guided | 1 | — |
| 2 | VLSM dan IPv6 | Guided | 2 | Checkpoint 2% + Tugas 1 |
| 3 | VLAN dan inter-VLAN routing | Guided | 3 | Checkpoint 2% |
| 4 | Static routing multi-lokasi | Guided | 3 | Checkpoint 2% + Tugas 2 |
| 5 | DHCP, DNS, NAT | Partial | 3 | Checkpoint 2% |
| 6 | Keamanan dan kontrol akses | Partial | 3 | Checkpoint 2% |
| 7 | Dokumentasi as-built dan diagnosis | Challenge | 4 | Checkpoint 2% |
| 8 | **UTS** — praktik individual 150 menit | — | 1–4 | 25% |
| 9 | Wireless dan IoT | Partial | 5 | Checkpoint 2% |
| 10 | Gateway internet dan redundansi | Partial | 5 | Checkpoint 2% |
| 11 | Konsolidasi addressing dan switching | Challenge | 4 | Checkpoint 2% |
| 12 | Konsolidasi routing, layanan, akses | Challenge | 4 | Checkpoint 2% |
| 13 | 5G, teknologi lanjut, otomasi | Kuliah | 5 | Tugas 4 |
| 14 | Proyek akhir: perancangan | Challenge | 6 | Tugas 3 |
| 15 | Proyek akhir: implementasi dan uji silang | Challenge | 6 | Komponen UAS |
| 16 | **UAS** — presentasi, praktik individual, teori | — | 5, 6 | 40% |

Tingkat scaffolding: **Guided** perintah diberikan lengkap dan berurutan, **Partial** kebutuhan dan tabel kosong dengan perintah di Lampiran A tanpa urutan, **Challenge** skenario dan kriteria sukses saja.

---

## Persiapan Sebelum Pekan 1

**1. Bangkitkan parameter peserta.**

```bash
cd pengajar/tools
python generate-parameter.py peserta.csv --output ../../lembar-parameter
```

Bagikan `parameter-<NIM>.md` kepada masing-masing mahasiswa. Simpan `master-parameter.csv` dan `KUNCI-vlsm.md` untuk Anda sendiri.

**2. Siapkan file simulasi.** Sembilan file sehat dan sembilan file rusak, ditambah file UTS dan UAS. Resep lengkapnya ada di pengajar/02-panduan-fault-injection.md (privat — repo dosen), termasuk perangkat, baris yang diubah, gejala yang diharapkan, dan uji yang seharusnya menemukannya. Yang pertama dibutuhkan pekan 3.

**3. Upload folder `mahasiswa/` ke LMS.**

**4. Briefing asisten.** Siklus READ/BREAK/FIX/BUILD, cara memakai bank pertanyaan viva, dan tiga hal yang harus konsisten antar-asisten pada penilaian tahap FIX — ada di bagian 6 panduan fault-injection.

## Setiap Sesi Praktikum

Pengerjaan berpasangan, penilaian individual. Lima belas pasang dan satu kelompok bertiga, sekitar delapan per asisten, peran wajib bertukar di tengah sesi. Tiga checkpoint biner diverifikasi langsung di layar, lalu satu pertanyaan viva 30 detik. Target 90 detik per pasang per checkpoint.

Setelah pengumpulan:

```bash
cd pengajar/tools
python cek-konfigurasi.py ../../kumpulan-p05 --master ../../lembar-parameter/master-parameter.csv \
    --pekan 5 --laporan ../../laporan-p05.md
```

Nilai Checkpoint diisi di pengajar/penilaian/checkpoint-praktikum.csv (privat — repo dosen) pada blok pekan yang bersangkutan. Cara pakainya di pengajar/penilaian/README.md (privat — repo dosen).

---

## Perubahan Utama dari 2025/2026

| Aspek | 2025/2026 | 2026/2027 |
|---|---|---|
| Parameter praktikum | Sama untuk seluruh mahasiswa | Diturunkan dari X per mahasiswa |
| Scaffolding | Konfigurasi verbatim sampai pekan 15 | Memudar bertahap dalam tiga tingkat |
| Troubleshooting | Disebut di Sub-CPMK, tidak diuji | Sub-CPMK tersendiri dengan sembilan lab fault |
| Pekan 11–13 | Materi baru yang berat, bertentangan dengan RPS | Konsolidasi sesuai RPS |
| Materi tak dapat dieksekusi | Skrip bash, SNMP, load balancer, payment gateway | Dibuang; 5G dan otomasi jadi kuliah dan tugas analisis |
| Bagian yang menuntut berpikir | "Latihan Mandiri" opsional | Tantangan wajib, 15% nilai per pekan |
| Bobot praktikum | ~70% pada konfigurasi benar | 40% konfigurasi, 25% verifikasi, 20% dokumentasi, 15% tantangan |
| Fitur Packet Tracer yang dipakai | Topologi dan CLI | Ditambah Simulation Mode, perangkat IoT, registration server |
| Kebijakan AI | Tidak disebutkan | Eksplisit, dengan Prompt Pack per pekan |
| Volume modul | 8.977 baris | 3.900 baris, materi tak-tereksekusi dibuang |
