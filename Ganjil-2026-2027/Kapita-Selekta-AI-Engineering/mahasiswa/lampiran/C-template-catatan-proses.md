# Lampiran C — Template Catatan Proses Mingguan

**Kapita Selekta: AI Engineering | Ganjil 2026/2027**

Catatan proses dikumpulkan **setiap minggu**, maksimal 2 halaman. Ia bernilai 25% dari bobot luaran mingguan — sama besar dengan bukti bahwa sistem Anda bekerja.

---

## Mengapa catatan proses bernilai sebesar itu

Kelas ini tidak memiliki asisten. Tidak ada yang mengamati Anda bekerja, tidak ada yang mencatat bahwa Anda mencoba tiga pendekatan sebelum yang keempat berhasil. **Catatan ini adalah satu-satunya tempat kerja itu terlihat** — dan justru tiga percobaan yang gagal itulah bagian yang paling bernilai di sini.

Catatan proses juga satu-satunya bukti bahwa penggunaan AI Anda transparan. Karya bagus tanpa catatan proses tidak dapat dibedakan dari karya yang seluruhnya dihasilkan orang atau alat lain, dan diperlakukan sesuai ketidakjelasan itu.

Yang dinilai bukan kerapian bahasanya, tapi **kejujuran dan ketelusurannya**.

---

## Template

Salin ke `catatan-proses.md` di dalam file pengumpulan Anda setiap minggu.

```markdown
# CATATAN PROSES — MINGGU <N>

Nama / NIM / K :
Tanggal        :
Waktu yang saya habiskan minggu ini : ___ jam

---

## 1. Yang saya kerjakan minggu ini
(3–5 kalimat. Apa yang berubah pada produk saya, bukan apa yang saya baca.)


## 2. Tahap AMATI
Yang saya amati :
Yang mengejutkan saya :
(Wajib diisi. Bila tidak ada yang mengejutkan, tulis apa yang sudah
Anda duga sebelumnya dan mengapa dugaan itu tepat.)


## 3. Tahap PATAHKAN
| # | Percobaan | Prediksi saya | Hasil sebenarnya | Prediksi tepat? |
|---|---|---|---|---|
|   |   |   |   | ya / tidak |

Prediksi saya yang paling MELESET : nomor ___
Apa yang salah dari cara saya berpikir sebelumnya :


## 4. Tahap PERBAIKI
Kesalahan yang saya temukan :
Bagaimana saya membuktikan diagnosis itu, bukan sekadar menduganya :
Kesalahan yang TIDAK saya temukan sendiri (bila ada) dan bagaimana
akhirnya ketemu :


## 5. Tahap RAKIT
Yang berhasil :
Yang tidak berhasil :
Keputusan rancangan yang saya ambil minggu ini, beserta alasannya :
| Keputusan | Alternatif yang saya tolak | Alasan memilih |
|---|---|---|
|   |   |   |


## 6. Tantangan wajib
Hasil :
Bila tidak tercapai — apa yang sudah dicoba dan mengapa gagal :


## 7. Penggunaan AI minggu ini
| # | Untuk apa | Prompt (ringkas / pola Lampiran A) | Yang saya UBAH dari hasilnya | Bagaimana saya verifikasi |
|---|---|---|---|---|
| 1 |   |   |   |   |
| 2 |   |   |   |   |

Satu hal yang AI berikan dan ternyata KELIRU minggu ini :
(Bila memang tidak ada, tulis "tidak ada" — tetapi periksa lagi dulu.)

Bagian karya minggu ini yang paling sulit saya jelaskan bila ditanya :


## 8. Pemakaian dan biaya
| Kegiatan | Model | Perkiraan token | Perkiraan biaya |
|---|---|---|---|
|   |   |   |   |
Total minggu ini : Rp ___
Total kumulatif  : Rp ___
(Salin juga ke `catatan-pemakaian.md`.)


## 9. Daftar periksa mandiri
(Salin daftar periksa dari modul minggu ini dan centang yang benar-benar
selesai. Butir yang belum selesai TETAP dicantumkan tidak tercentang —
jangan dihapus.)


## 10. Yang macet dan rencana minggu depan
Yang masih macet :
Apa yang sudah saya coba untuk mengatasinya :
Rencana konkret minggu depan :
```

---

## Ketentuan pengisian

**Panjang.** Maksimal 2 halaman. Catatan yang lebih panjang tidak dibaca melebihi halaman kedua. Menulis padat adalah bagian dari keterampilan yang dinilai.

**Bagian 3.** Kolom prediksi diisi **sebelum** percobaan dijalankan. Isinya tidak dinilai benar atau salah — yang dinilai adalah bahwa ia ditulis lebih dulu. Prediksi yang meleset dan dicatat apa adanya adalah bahan paling berguna di catatan proses Anda, karena di situ terlihat pemahaman Anda berubah.

**Bagian 7.** Ini bagian yang paling sering diisi seadanya dan paling banyak berpengaruh pada komponen Sikap dan Profesionalisme. Kolom "yang saya ubah" yang selalu berisi "tidak ada" berarti Anda menempelkan hasil AI tanpa menilai — dan itu justru yang dilarang mata kuliah ini.

**Bagian 9.** Butir yang belum selesai tidak boleh dihapus. Daftar periksa yang selalu tercentang penuh pada setiap minggu kurang dipercaya daripada daftar yang menunjukkan apa yang tertinggal dan kemudian dikejar.

---

## Contoh pengisian yang lemah dan yang kuat

| Bagian | Lemah | Kuat |
|---|---|---|
| 2 · Yang mengejutkan | "Ternyata modelnya pintar" | "Saya kira menaikkan jumlah chunk dari 3 ke 10 pasti memperbaiki jawaban. Ternyata tiga kasus justru memburuk karena chunk tak relevan mengaburkan yang benar" |
| 5 · Keputusan | "Saya pakai chunk 500 kata" | "Saya pakai chunking per pasal, bukan per jumlah huruf, karena dokumen saya berupa peraturan yang satu pasalnya adalah satu gagasan utuh. Saya tolak chunking tetap karena mencoba ini pada dua pasal panjang dan konteksnya terpotong di tengah ketentuan" |
| 7 · Yang saya ubah | "tidak ada" | "AI memberi skema dengan medan `confidence` berupa angka 0–1. Saya ubah jadi tiga tingkat karena angka itu tidak terkalibrasi dan saya tidak punya cara memverifikasinya" |
| 7 · Yang salah | "tidak ada" | "AI menyatakan pustaka X memiliki fungsi Y. Fungsi itu tidak ada. Saya temukan setelah error, lalu memeriksa dokumentasi resminya" |
| 10 · Yang macet | "masih bingung" | "Retrieval gagal pada pertanyaan yang memakai istilah lokal. Sudah saya coba: menambah jumlah chunk, mengganti kata kunci, memperbesar tumpang tindih. Dugaan saya masalahnya pada embedding untuk istilah non-baku. Minggu depan saya coba menambahkan daftar padanan istilah" |
