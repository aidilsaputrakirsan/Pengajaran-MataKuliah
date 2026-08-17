# LAMPIRAN B — Parameter Individual

**Semua parameter jaringan Anda diturunkan dari satu angka: `X`.**

---

## 1. Menentukan X

`X` adalah **nomor urut Anda pada daftar peserta resmi mata kuliah** (01–40), dibagikan dosen pada pekan 1 dan dicetak pada lembar parameter masing-masing mahasiswa.

> **Mengapa bukan dua digit terakhir NIM.** Dengan 40 mahasiswa dan 100 kemungkinan angka, peluang setidaknya dua orang memiliki dua digit terakhir NIM yang sama lebih dari 50% — dan dua mahasiswa dengan blok IP identik membuat deteksi penyalinan otomatis tidak berfungsi. Nomor urut peserta dijamin unik. NIM tetap dipakai untuk penamaan file.

Sepanjang dokumen ini contoh dihitung untuk **X = 27**. Angka Anda berbeda; jangan pernah menyalin tabel contoh ini sebagai jawaban.

---

## 2. Rumus

| Parameter | Rumus | Contoh (X = 27) |
|---|---|---|
| Blok HQ | `10.X.0.0/20` | `10.27.0.0/20` |
| Blok Cabang | `10.X.16.0/22` | `10.27.16.0/22` |
| Blok Gudang | `10.X.20.0/22` | `10.27.20.0/22` |
| Blok WAN | `172.16.X.0/24` dibagi /30 | `172.16.27.0/24` |
| Blok loopback | `172.31.X.0/24` dibagi /32 | `172.31.27.0/24` |
| Basis VLAN | `((X mod 8) + 1) × 100` | `((27 mod 8) + 1) × 100 = 400` |
| Blok publik ISP-1 | `203.0.(113 + ⌊(X−1)/32⌋).(8 × ((X−1) mod 32))/29` | `203.0.113.208/29` |
| Blok publik ISP-2 | `198.51.(100 + ⌊(X−1)/32⌋).(8 × ((X−1) mod 32))/29` | `198.51.100.208/29` |
| Prefix IPv6 | `2001:db8:X::/48` | `2001:db8:27::/48` |
| Domain internal | `nusantaraX.local` | `nusantara27.local` |
| `enable secret` | `DmjkX#2026` | `Dmjk27#2026` |

### Penamaan perangkat

| Perangkat | Pola | Contoh |
|---|---|---|
| Router HQ | `R-HQ-X` | `R-HQ-27` |
| Router Cabang | `R-BR-X` | `R-BR-27` |
| Router Gudang | `R-GD-X` | `R-GD-27` |
| Switch | `SW-<lokasi>-X-nn` | `SW-HQ-27-01` |
| Access point | `AP-<lokasi>-X-nn` | `AP-GD-27-01` |
| Server | `SRV-<peran>-X` | `SRV-WEB-27` |

---

## 3. Kebutuhan Host

| Segmen | Rumus | X = 27 |
|---|---|---:|
| HRD | `25 + X` | 52 |
| Keuangan | `12 + X` | 39 |
| Server | `8 + (X mod 5)` | 10 |
| WiFi-Karyawan | `60 + 2X` | 114 |
| WiFi-Tamu | `30 + X` | 57 |
| Manajemen | `6` (tetap) | 6 |
| Cabang-Umum | `30 + X` | 57 |
| WiFi-Cabang | `25 + X` | 52 |
| Produksi | `40 + X` | 67 |
| IoT | `15 + X` | 42 |
| WiFi-Gudang | `20 + X` | 47 |

Perhatikan bahwa panjang prefix hasil VLSM **berubah tergantung X**. Untuk X kecil, WiFi-Karyawan cukup `/25`; untuk X ≥ 34 kebutuhannya melewati 126 host sehingga butuh `/24`. Kalau Anda menyalin prefix dari teman, hampir pasti salah.

---

## 4. ID VLAN

Basis VLAN = `((X mod 8) + 1) × 100`. Sufiks tetap untuk semua mahasiswa:

| Segmen | Sufiks | VLAN untuk X = 27 |
|---|---:|---:|
| HRD | 10 | 410 |
| Keuangan | 20 | 420 |
| Server | 30 | 430 |
| WiFi-Karyawan | 40 | 440 |
| WiFi-Tamu | 50 | 450 |
| Produksi | 60 | 460 |
| IoT | 70 | 470 |
| WiFi-Gudang | 75 | 475 |
| Cabang-Umum | 80 | 480 |
| WiFi-Cabang | 85 | 485 |
| Manajemen | 99 | 499 |

Rentang hasilnya selalu 110–899, aman dari VLAN 1002–1005 yang dicadangkan Cisco. **VLAN 1 tidak pernah dipakai** untuk data maupun manajemen.

---

## 5. Contoh Terisi Penuh — X = 27

Bagian ini adalah *panduan bentuk jawaban*, bukan jawaban Anda. Perhatikan bahwa alokasi disusun **dari kebutuhan terbesar ke terkecil** — inilah inti VLSM.

### 5.1 HQ — dari `10.27.0.0/20`

| # | Segmen | Butuh | Prefix | Rentang | Gateway | Broadcast |
|--:|---|--:|---|---|---|---|
| 1 | WiFi-Karyawan | 114 | `10.27.0.0/25` | .0.0 – .0.127 | `10.27.0.1` | `10.27.0.127` |
| 2 | WiFi-Tamu | 57 | `10.27.0.128/26` | .0.128 – .0.191 | `10.27.0.129` | `10.27.0.191` |
| 3 | HRD | 52 | `10.27.0.192/26` | .0.192 – .0.255 | `10.27.0.193` | `10.27.0.255` |
| 4 | Keuangan | 39 | `10.27.1.0/26` | .1.0 – .1.63 | `10.27.1.1` | `10.27.1.63` |
| 5 | Server | 10 | `10.27.1.64/28` | .1.64 – .1.79 | `10.27.1.65` | `10.27.1.79` |
| 6 | Manajemen | 6 | `10.27.1.80/29` | .1.80 – .1.87 | `10.27.1.81` | `10.27.1.87` |

Sisa untuk pertumbuhan: `10.27.1.88` – `10.27.15.255`.

Catatan pada baris 6: `/29` memberi tepat 6 alamat host — pas, tanpa ruang tumbuh. Dalam desain sungguhan Anda mungkin memilih `/28`. Pertanyaan "kenapa Anda pilih yang mana" adalah bahan viva; **kedua jawaban bisa benar bila Anda punya alasannya.**

### 5.2 Cabang — dari `10.27.16.0/22`

| # | Segmen | Butuh | Prefix | Gateway |
|--:|---|--:|---|---|
| 1 | Cabang-Umum | 57 | `10.27.16.0/26` | `10.27.16.1` |
| 2 | WiFi-Cabang | 52 | `10.27.16.64/26` | `10.27.16.65` |

Sisa: `10.27.16.128` – `10.27.19.255`.

### 5.3 Gudang — dari `10.27.20.0/22`

| # | Segmen | Butuh | Prefix | Gateway |
|--:|---|--:|---|---|
| 1 | Produksi | 67 | `10.27.20.0/25` | `10.27.20.1` |
| 2 | WiFi-Gudang | 47 | `10.27.20.128/26` | `10.27.20.129` |
| 3 | IoT | 42 | `10.27.20.192/26` | `10.27.20.193` |

Sisa: `10.27.21.0` – `10.27.23.255`.

### 5.4 Tautan WAN — dari `172.16.27.0/24`

| Tautan | Prefix | Sisi A | Sisi B |
|---|---|---|---|
| HQ ↔ Cabang | `172.16.27.0/30` | `R-HQ-27` = .1 | `R-BR-27` = .2 |
| HQ ↔ Gudang | `172.16.27.4/30` | `R-HQ-27` = .5 | `R-GD-27` = .6 |
| HQ ↔ ISP-1 | `172.16.27.8/30` | `R-HQ-27` = .9 | `ISP-1` = .10 |
| HQ ↔ ISP-2 | `172.16.27.12/30` | `R-HQ-27` = .13 | `ISP-2` = .14 |

### 5.5 Loopback

| Perangkat | Alamat |
|---|---|
| `R-HQ-27` | `172.31.27.1/32` |
| `R-BR-27` | `172.31.27.2/32` |
| `R-GD-27` | `172.31.27.3/32` |

### 5.6 Alamat publik (mulai pekan 10)

Blok ISP-1: `203.0.113.208/29` → dapat dipakai `.209` – `.214`

| Peruntukan | Alamat |
|---|---|
| PAT (overload) untuk seluruh klien internal | `203.0.113.209` |
| Static NAT web server publik | `203.0.113.210` |
| Backup | `.211` – `.214` |

Blok ISP-2: `198.51.100.208/29`, dipakai hanya saat failover.

### 5.7 IPv6 (dua segmen saja, pekan 2)

| Segmen | Prefix |
|---|---|
| HRD | `2001:db8:27:410::/64` |
| Keuangan | `2001:db8:27:420::/64` |

Subnet ID IPv6 sengaja memakai **angka VLAN**. Ini konvensi yang dipakai di jaringan sungguhan supaya nomor VLAN dan nomor subnet tidak perlu dihafal dua kali.

---

## 6. Memeriksa Pekerjaan Anda Sendiri

Sebelum mengumpulkan, jalankan enam pemeriksaan ini. Semuanya bisa Anda lakukan tanpa asisten:

1. **Tidak ada tumpang tindih.** Alamat broadcast setiap segmen harus lebih kecil dari alamat jaringan segmen berikutnya.
2. **Semua di dalam blok yang diberikan.** Tidak ada alamat di luar `10.X.*`, `172.16.X.*`, `172.31.X.*`.
3. **Prefix cukup, tapi tidak berlebihan.** Untuk kebutuhan 52 host, `/26` benar; `/24` berarti membuang 202 alamat dan akan dikurangi nilainya.
4. **Urutan alokasi menurun.** Segmen terbesar mendapat blok pertama. Kalau tidak, biasanya ada celah yang tidak terpakai.
5. **VLAN sesuai rumus.** Cek ulang `((X mod 8) + 1) × 100`, bukan angka yang "kelihatan mirip".
6. **Blok IP Anda bukan blok teman Anda.** Cari `10.` di file konfigurasi Anda — angka oktet kedua harus **selalu** X.

Pemeriksaan nomor 6 juga dilakukan otomatis terhadap seluruh pengumpulan kelas sekaligus.
