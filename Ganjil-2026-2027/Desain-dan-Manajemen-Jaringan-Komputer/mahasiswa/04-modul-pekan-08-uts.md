# MODUL DMJK — PEKAN 8: UTS

**SI2514011 | Ujian Praktik Individual | Cisco Packet Tracer 8.2+**

> Dibaca sebelum pekan 8. Mengevaluasi Sub-CPMK 1 sampai 4, yaitu seluruh materi pekan 1 sampai 7.

---

# PEKAN 8 — UTS: Ujian Praktik Individual

**Yang dievaluasi:** Sub-CPMK 1, 2, 3, 4 (pekan 1–7)
**Bobot:** 25% dari nilai akhir
**Durasi:** 150 menit
**Sifat:** Individual, di laboratorium, file dikumpulkan lewat LMS

---

## 8.1 Aturan Parameter Ujian

Anda menerima **parameter Y** pada saat ujian dimulai, dan Y **berbeda** dari X yang Anda pakai sepanjang semester. Seluruh alamat, VLAN ID, dan hostname pada pekerjaan ujian diturunkan dari Y dengan rumus yang sama seperti Lampiran B.

Konsekuensinya: tidak ada bagian dari file NusantaraNet Anda yang dapat dipakai kembali. Anda boleh membuka dokumentasi dan modul, tetapi setiap angka harus dihitung ulang.

File ujian yang memuat blok alamat milik X Anda, atau milik mahasiswa lain, dinilai nol pada seluruh Bagian B.

## 8.2 Yang Boleh dan Tidak Boleh Dibuka

| Boleh | Tidak boleh |
|---|---|
| Modul mata kuliah dan seluruh lampiran | Layanan AI apa pun |
| Dokumentasi as-built NusantaraNet Anda | File `.pkt` milik sendiri atau orang lain |
| Catatan tulisan tangan | Komunikasi dengan siapa pun |
| Kalkulator biasa | Kalkulator subnet, daring maupun aplikasi |

Perhitungan VLSM dikerjakan sendiri. Ini salah satu hal yang diuji.

## 8.3 Skenario

**CV Borneo Sejahtera** adalah perusahaan jasa logistik dengan dua lokasi: kantor di Balikpapan dan depo di Penajam. Perusahaan baru pindah kantor dan seluruh jaringan dibangun dari nol.

**Kebutuhan kantor Balikpapan**, blok `10.Y.0.0/22`:

| Segmen | Kebutuhan host |
|---|---:|
| Administrasi | 45 + Y |
| Operasional | 20 + Y |
| Server | 6 |
| Manajemen perangkat | 6 |

**Kebutuhan depo Penajam**, blok `10.Y.8.0/23`:

| Segmen | Kebutuhan host |
|---|---:|
| Depo-Umum | 30 + Y |
| WiFi-Tamu | 15 + Y |

**Tautan dan alamat lain:**

- WAN kantor–depo: `172.16.Y.0/30`
- WAN kantor–ISP: `172.16.Y.4/30`, sisi ISP `.6`
- Alamat publik untuk PAT: `203.0.113.Y`
- Basis VLAN: `((Y mod 8) + 1) × 100`, sufiks 10 Administrasi, 20 Operasional, 30 Server, 40 Depo-Umum, 50 WiFi-Tamu, 99 Manajemen

**Kebijakan akses yang diminta manajemen:**

1. WiFi-Tamu boleh internet, boleh DHCP dan DNS, tidak boleh segmen internal mana pun.
2. Operasional boleh mengakses Server hanya untuk HTTP dan DNS.
3. Hanya segmen Manajemen boleh SSH ke perangkat jaringan; Telnet mati.

---

## 8.4 Susunan Ujian

| Bagian | Isi | Waktu | Poin |
|---|---|---:|---:|
| A | Perencanaan addressing | 25 menit | 25 |
| B | Implementasi | 80 menit | 45 |
| C | Diagnosis pada file yang disediakan | 30 menit | 20 |
| D | Dokumentasi | 15 menit | 10 |

Waktu adalah anjuran pembagian, bukan batas per bagian. Bagian C memakai file terpisah dan **dapat dikerjakan lebih dulu** jika Anda menghendaki.

### Bagian A — Perencanaan addressing (25 poin)

Isi lembar yang disediakan. Tidak dikerjakan di Packet Tracer.

1. Tabel VLSM keenam segmen: prefix, alamat jaringan, host pertama, host terakhir, broadcast, gateway, VLAN ID. **(15 poin)**
2. Alamat kedua ujung kedua tautan WAN. **(4 poin)**
3. Alamat sisa yang belum terpakai di masing-masing blok. **(3 poin)**
4. Satu kalimat untuk masing-masing: mengapa prefix segmen terbesar sebesar itu, dan mengapa Server dan Manajemen dipisah walaupun ukurannya sama. **(3 poin)**

Bagian A dinilai walaupun Bagian B tidak selesai. Kerjakan lebih dulu dan periksa ulang sebelum menyentuh simulator: kesalahan di sini akan merambat ke seluruh Bagian B.

### Bagian B — Implementasi (45 poin)

Bangun file `uts-<NIM>.pkt` dari nol.

| # | Yang dinilai | Poin |
|---|---|---:|
| 1 | Topologi fisik dua lokasi, penamaan perangkat sesuai Y | 4 |
| 2 | Keenam VLAN dengan ID sesuai Y; tidak ada port aktif di VLAN 1 | 6 |
| 3 | Trunk yang hanya mengizinkan VLAN yang dipakai | 4 |
| 4 | Inter-VLAN routing, gateway sesuai Bagian A | 6 |
| 5 | Static routing dua arah antara kedua lokasi, rute ringkas bila mungkin | 6 |
| 6 | Rute default ke ISP | 2 |
| 7 | DHCP terpusat di kantor melayani segmen depo lewat relay, dengan pengecualian alamat | 7 |
| 8 | PAT sehingga kedua lokasi dapat menjangkau internet | 5 |
| 9 | ACL yang menegakkan ketiga butir kebijakan | 5 |

Butir 7 dan 9 memuat bagian yang paling sering tidak selesai. Kalau waktu menipis, DHCP relay untuk depo bernilai lebih tinggi daripada menyempurnakan ACL.

### Bagian C — Diagnosis (20 poin)

Download `uts-diagnosis.pkt`. File ini **bukan** milik Anda dan tidak memakai parameter Y — ia jaringan yang sudah jadi dan berisi **4 fault**.

Untuk setiap fault, isi lembar Fault Report:

| Kolom | Poin per fault |
|---|---:|
| Gejala yang diamati | 1 |
| Cakupan: siapa terdampak, siapa tidak | 1 |
| Akar masalah: baris yang salah dan mengapa menimbulkan gejala itu | 2 |
| Perbaikan dan verifikasi | 1 |

File yang diperbaiki juga dikumpulkan, tetapi **poin ada pada laporannya**. Fault yang diperbaiki tanpa kolom Gejala dan Akar Masalah bernilai 1 dari 5.

Satu dari empat fault menyebabkan kegagalan hanya satu arah. Satu lagi tidak menimbulkan keluhan pengguna.

### Bagian D — Dokumentasi (10 poin)

1. Diagram logis: segmen, VLAN, subnet, letak gateway, titik kontrol akses. **(4 poin)**
2. Matriks kontrol akses ketiga kebijakan, dengan rujukan baris ACL yang menegakkannya. **(4 poin)**
3. Satu prosedur recovery singkat: apa yang diperiksa, dalam urutan apa, kalau depo kehilangan seluruh konektivitas. **(2 poin)**

---

## 8.5 Yang Dikumpulkan

1. `uts-<NIM>.pkt` — hasil Bagian B
2. `uts-diagnosis-<NIM>.pkt` — hasil Bagian C
3. `uts-konfigurasi-<NIM>.txt` — `show running-config` seluruh perangkat Bagian B
4. Lembar Bagian A, C, dan D — tulisan tangan atau file, sesuai instruksi pengawas

File nomor 3 diperiksa otomatis terhadap parameter Y Anda dan dibandingkan antar-peserta.

## 8.6 Saran Pengerjaan

Tiga hal yang paling sering menghabiskan waktu peserta, berdasarkan pelaksanaan sebelumnya:

**Menunda Bagian A.** Peserta yang langsung membuka Packet Tracer hampir selalu harus mengulang addressing di tengah ujian.

**Mencari kesalahan sendiri secara spekulatif di akhir.** Jalankan pengujian cakupan tujuh baris dari modul pekan 7 setelah setiap butir Bagian B selesai, bukan menumpuknya sampai akhir.

**Menyempurnakan satu butir sampai selesai betul.** Poin dihitung per butir. Delapan butir yang berjalan lebih tinggi nilainya daripada empat butir yang sempurna.
