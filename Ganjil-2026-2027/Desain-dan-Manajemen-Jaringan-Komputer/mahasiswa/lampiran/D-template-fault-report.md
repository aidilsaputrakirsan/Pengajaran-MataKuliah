# LAMPIRAN D — Template Fault Report

Dipakai pada pekan 7, 11, 12, 15, dan pada Bagian C UTS.

> **Yang dinilai bukan jumlah fault yang ditemukan, melainkan ketertelusuran cara menemukannya.** Enam fault dengan penalaran yang tercatat bernilai lebih tinggi daripada delapan fault yang ditemukan secara kebetulan. Kolom Hipotesis yang kosong membuat fault tersebut dihitung setengah.

---

## Bagian 1 — Pengujian Cakupan

Diisi **sebelum** membuka konfigurasi apa pun. Tujuh baris ini menyempitkan lokasi masalah lebih cepat daripada membaca `running-config`.

| Uji | Hasil | Catatan |
|---|---|---|
| Klien → gateway sendiri | | |
| Klien → klien lain di segmen sama | | |
| Klien → segmen lain | | |
| Klien → server, memakai IP address | | |
| Klien → server, memakai nama | | |
| Klien → internet | | |
| Klien **kedua** di segmen sama → tujuan yang sama | | |

Kesimpulan awal dari pola di atas — lapisan atau layanan mana yang paling mungkin bermasalah:

```


```

Baris terakhir tabel adalah yang paling sering dilewatkan. Ia memisahkan masalah satu perangkat dari masalah jaringan dalam satu langkah.

---

## Bagian 2 — Fault Report per Temuan

Salin blok ini sebanyak jumlah fault.

```
FAULT NOMOR ___          Ditemukan pada menit ke ___

GEJALA
Apa yang tidak bekerja, dari sudut pandang pengguna. Bukan penyebabnya.


CAKUPAN
Siapa yang terdampak:
Siapa yang TIDAK terdampak:
Apa yang disimpulkan dari perbedaan itu:


HIPOTESIS
Dugaan penyebab:
Akibat yang saya prediksi kalau dugaan ini benar:
Uji yang hasilnya akan berbeda tergantung dugaan ini benar atau salah:


UJI YANG DIJALANKAN
Perintah:
Output yang relevan:
Hipotesis terbukti / terbantah:


AKAR MASALAH
Baris konfigurasi yang salah (sebutkan perangkat dan barisnya):
Mengapa baris itu menimbulkan gejala di atas:


PERBAIKAN
Perubahan yang dilakukan (satu perubahan saja):


VERIFIKASI
Bukti gejala hilang:
Bukti tidak ada yang rusak baru:
```

---

## Bagian 3 — Ringkasan

| # | Gejala singkat | Lapisan | Waktu temu | Ditemukan dari uji atau kebetulan |
|---|---|---|---|---|
| 1 | | | | |
| 2 | | | | |
| 3 | | | | |
| 4 | | | | |
| 5 | | | | |
| 6 | | | | |
| 7 | | | | |
| 8 | | | | |

Jumlah fault ditemukan: ___ dari ___ Total waktu: ___ menit

**Fault yang tidak ditemukan.** Sebutkan bagian mana yang sudah Anda periksa dan bagian mana yang belum. Bagian ini tidak mengurangi nilai; mengosongkannya padahal ada fault yang terlewat akan mengurangi.

```


```

---

## Bagian 4 — Refleksi Metode

Tiga pertanyaan, masing-masing maksimal tiga kalimat.

**1. Fault mana yang paling lama ditemukan, dan apakah karena ia tersembunyi atau karena Anda mencari di tempat yang salah?**

```

```

**2. Adakah fault yang ditemukan secara kebetulan? Uji apa yang seharusnya menemukannya secara sistematis?**

```

```

**3. Informasi apa yang Anda butuhkan tetapi tidak ada di dokumentasi yang tersedia?**

```

```

Pertanyaan ketiga adalah yang paling berguna bagi Anda sendiri: jawabannya menunjukkan apa yang harus ditambahkan ke dokumentasi Anda sebelum pekan berikutnya.

---

## Catatan Penilaian

| Komponen | Bobot per fault |
|---|---:|
| Gejala | 1 |
| Cakupan: siapa terdampak, siapa tidak | 1 |
| Hipotesis yang dapat dibantah beserta uji pembedanya | 2 |
| Akar masalah dan alasan ia menimbulkan gejala tersebut | 2 |
| Perbaikan dan verifikasi | 1 |

Hipotesis berbentuk "sepertinya ada masalah di routing" tidak mendapat poin, karena tidak ada uji yang dapat membuktikannya salah. Hipotesis yang mendapat poin penuh berbentuk: **penyebab → akibat yang diprediksi → uji yang membedakan.**
