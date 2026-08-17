# LAMPIRAN C — Template Laporan Praktikum

**Maksimal 2 halaman per pekan.** Kecuali pekan 7, 14, dan 15 yang memakai format tersendiri.

> Laporan ini bukan esai. Setiap bagian punya batas panjang, dan melampauinya tidak menambah nilai. Yang dinilai adalah isi kolom, bukan jumlah kata.

---

```
LAPORAN PRAKTIKUM DMJK — PEKAN ___

Nama         : ______________________  NIM : ______________
Parameter X  : ____   Pasangan kerja  : ______________________
File       : nusantaranet-<NIM>-p<pekan>.pkt
```

---

## 1. Parameter yang Dipakai

Isi hanya yang relevan untuk pekan ini.

| Parameter | Nilai |
|---|---|
| Blok yang dipakai | |
| VLAN yang dibuat | |
| Perangkat yang disentuh | |

---

## 2. Tabel Rancangan

Tabel pekan ini sesuai instruksi BUILD. Sisipkan di sini, atau lampirkan sebagai halaman terpisah bila lebih dari sepuluh baris.

---

## 3. Hasil Tahap READ

Jawaban atas pertanyaan pada tahap READ. **Maksimal 150 kata.** Sebutkan output perintah yang menjadi dasar jawaban Anda.

---

## 4. Tabel Tahap BREAK

Kolom prediksi wajib terisi, dan diisi **sebelum** percobaan dilakukan.

| # | Yang diubah | Prediksi saya | Hasil sebenarnya | Perintah pembukti |
|---|---|---|---|---|
| 1 | | | | |
| 2 | | | | |
| 3 | | | | |
| 4 | | | | |
| 5 | | | | |
| 6 | | | | |

**Prediksi yang keliru dan tercatat lebih bernilai daripada kolom yang dikosongkan.** Prediksi yang selalu tepat sempurna untuk seluruh baris akan ditanyakan asisten.

Jawaban atas pertanyaan tambahan tahap BREAK, maksimal 100 kata:

---

## 5. Hasil Tahap FIX

| # | Gejala yang saya lihat lebih dulu | Akar masalah | Perbaikan |
|---|---|---|---|
| 1 | | | |
| 2 | | | |
| 3 | | | |
| 4 | | | |
| 5 | | | |

Jumlah fault yang ditemukan: ___ dari ___ Waktu yang dibutuhkan: ___ menit

Fault yang **tidak** berhasil ditemukan, dan tempat terakhir yang sudah Anda periksa:

---

## 6. Verifikasi Tahap BUILD

Bukti bahwa kebutuhan pekan ini terpenuhi. Sebutkan perintah dan hasilnya, bukan hanya klaim.

| Kebutuhan | Perintah verifikasi | Hasil |
|---|---|---|
| | | |
| | | |
| | | |

---

## 7. Tantangan Wajib

Jawaban tantangan wajib pekan ini. **Maksimal 250 kata.**

---

## 8. Hal yang Belum Selesai

Sebutkan apa yang belum berhasil dan sejauh mana sudah Anda telusuri. Bagian ini tidak mengurangi nilai; mengosongkannya padahal ada yang belum selesai akan mengurangi.

---

## 9. Pertukaran Peran

| | Paruh pertama | Paruh kedua |
|---|---|---|
| Saya mengerjakan | | |
| Pasangan mengerjakan | | |

Tanda tangan kedua pihak: ______________  ______________

---
---

# Format Khusus Pekan 7, 14, dan 15

## Pekan 7 — Dokumentasi As-Built

Bukan 2 halaman. Susunan file yang dikumpulkan:

1. Diagram topologi fisik (satu halaman)
2. Diagram topologi logis (satu halaman)
3. Tabel addressing as-built — dari output perangkat
4. **Tabel selisih rencana terhadap kenyataan**

   | Butir | Rencana pekan 2 | Kenyataan hari ini | Diperbaiki atau diterima | Alasan |
   |---|---|---|---|---|

5. Peta koneksi fisik dari `show cdp neighbors`
6. Matriks kontrol akses dengan rujukan baris ACL
7. Tiga prosedur recovery, masing-masing maksimal satu halaman
8. Fault Report delapan fault — Lampiran D
9. Prosedur recovery versi sebelum dan sesudah diuji pasangan, beserta titik tempat pasangan tersesat

## Pekan 14 — Dokumen Rancangan Tim

Maksimal 12 halaman. Susunan pada modul pekan 14 bagian 14.5.

## Pekan 15 — Dokumen As-Built Tim

Maksimal 20 halaman, ditambah laporan pengujian silang dan catatan kontribusi individual masing-masing anggota.
