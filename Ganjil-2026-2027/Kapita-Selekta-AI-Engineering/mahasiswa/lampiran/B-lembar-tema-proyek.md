# Lampiran B — Lembar Tema Proyek dan Penurunan Lingkup dari Kode Peserta

**Kapita Selekta: AI Engineering | Ganjil 2026/2027**

Lampiran ini dipakai dua kali: **Minggu 1** untuk menghitung angka lingkup Anda, dan **Minggu 4** untuk menetapkan tema. Setelah Minggu 6 tema terkunci dan tidak dapat diganti.

---

## 1. Kode peserta K

**K** adalah nomor urut Anda pada daftar peserta kelas, diterima Minggu 1. Ia menentukan besaran lingkup wajib produk Anda, sehingga tidak ada dua mahasiswa yang memiliki beban yang persis sama, dan menyalin lingkup rekan menghasilkan angka yang salah.

### Aturan penurunan

| Unsur | Rumus | Berlaku sejak |
|---|---|---|
| Jumlah dokumen rujukan minimum | `5 + (K mod 4)` | Minggu 7 |
| Jumlah kasus uji minimum | `12 + (K mod 6)` | Minggu 14 |
| Jumlah tool minimum | `2 + (K mod 2)` | Minggu 11 |
| Mode keluaran tambahan | Bila `K` genap: wajib ada satu mode keluaran ringkas selain mode penuh | Minggu 5 |
| Jumlah rekan yang Anda review | 2 pada UTS, 2 pada Minggu 12 | Minggu 8 |

`mod` berarti sisa pembagian. Contoh untuk **K = 7**:

```
dokumen  = 5 + (7 mod 4) = 5 + 3 = 8
kasus uji= 12 + (7 mod 6) = 12 + 1 = 13
tool = 2 + (7 mod 2) = 2 + 1 = 3
K ganjil → tidak wajib mode keluaran tambahan
```

Contoh untuk **K = 12**:

```
dokumen  = 5 + (12 mod 4) = 5 + 0 = 5
kasus uji= 12 + (12 mod 6) = 12 + 0 = 12
tool = 2 + (12 mod 2) = 2 + 0 = 2
K genap  → wajib satu mode keluaran ringkas
```

Hitung angka Anda pada Minggu 1 dan tuliskan di catatan proses. Seluruh penilaian mulai Minggu 7 memakai angka ini, bukan angka contoh.

Angka-angka itu **minimum**. Melampauinya tidak otomatis menaikkan nilai; yang menaikkan nilai adalah mutu rancangan dan evaluasi.

---

## 2. Kelayakan dokumen rujukan

Dokumen rujukan Anda harus memenuhi **seluruh** syarat berikut. Dokumen yang gagal salah satu syarat tidak dihitung.

| Syarat | Penjelasan |
|---|---|
| **Sah dipakai** | Dokumen publik, dokumen milik Anda sendiri, atau dokumen yang pemiliknya mengizinkan. Dokumen internal organisasi tanpa izin tertulis tidak boleh |
| **Tidak memuat data pribadi** | Nama, nomor identitas, alamat, data kesehatan orang sungguhan. Bila ada, sensor dulu atau ganti dengan data sintetis |
| **Cukup berisi** | Sedikitnya 3 halaman setara. Selebaran satu halaman tidak dihitung |
| **Berbeda satu sama lain** | Delapan salinan dokumen yang sama dihitung satu |
| **Ada jawabannya** | Sedikitnya enam pertanyaan uji Anda harus terjawab oleh kumpulan dokumen ini |
| **Asalnya tercatat** | Judul, penerbit, tahun, tautan atau cara memperolehnya |

Bila Anda kesulitan memenuhi jumlah minimum, itu tanda tema Anda terlalu sempit atau bidangnya tidak memiliki basis dokumen yang memadai. Bicarakan pada Minggu 5, bukan Minggu 8.

---

## 3. Lembar Tema Proyek

Salin ke `lembar-tema.md`, isi, dan kumpulkan **Minggu 4**. Lembar ini menjadi acuan penilaian seluruh minggu berikutnya.

```markdown
# LEMBAR TEMA PROYEK

Nama            :
NIM             :
Program studi   :
Kode peserta K  :

## Angka lingkup saya
Dokumen rujukan minimum : 5 + (K mod 4) = ___
Kasus uji minimum       : 12 + (K mod 6) = ___
Tool minimum        : 2 + (K mod 2) = ___
Mode keluaran tambahan  : wajib / tidak wajib   (K genap / ganjil)

## 1. Persoalan
Judul kerja produk :

Siapa yang mengalami persoalan ini (sesebut mungkin, bukan "masyarakat") :

Bagaimana persoalan ini diselesaikan sekarang :

Mengapa cara sekarang tidak memadai :

## 2. Mengapa AI generatif
Mengapa persoalan ini TIDAK cukup diselesaikan dengan basis data,
formulir, atau seperangkat aturan biasa :

Apa yang membuat persoalan ini menuntut penafsiran bahasa :

## 3. Ukuran keberhasilan
Apa yang menjadi bukti bahwa produk ini berhasil :

Siapa yang dapat memeriksa keluarannya benar atau salah :

Apa yang terjadi bila keluarannya salah, dan siapa yang menanggungnya :

## 4. Dokumen rujukan
| # | Judul | Penerbit / asal | Tahun | Halaman | Status kelayakan |
|---|---|---|---|---|---|
| 1 |  |  |  |  |  |
| 2 |  |  |  |  |  |
(tambahkan sampai memenuhi jumlah minimum Anda)

Konfirmasi:
[ ] Seluruh dokumen sah saya pakai
[ ] Tidak ada data pribadi orang sungguhan di dalamnya
[ ] Sedikitnya enam pertanyaan uji saya terjawab oleh kumpulan ini

## 5. Uji lima kriteria penolakan
Untuk masing-masing, tulis LOLOS beserta alasan satu kalimat.

1. AI tidak dipaksakan pada persoalan deterministik :
2. Keluaran dapat dibuktikan benar atau salah      :
3. Sumber pengetahuan tersedia dan sah             :
4. Tidak bergantung pada data pribadi nyata        :
5. Lingkupnya muat dikerjakan seorang diri 12 minggu :

## 6. Batas yang saya tetapkan sendiri
Produk ini TIDAK dimaksudkan untuk :

Keputusan apa yang TIDAK boleh diserahkan kepada produk ini :

## 7. Rencana lapisan
| Blok | Yang akan saya tambahkan | Risiko terbesar |
|---|---|---|
| B — Kendali (Mgg 4–6) |  |  |
| C — Grounding (Mgg 7–9) |  |  |
| D — Agentic (Mgg 10–13) |  |  |
| E — Kematangan (Mgg 14–16) |  |  |

---
Tanda tangan mahasiswa :            Tanggal :
Catatan dosen          :
Vonis                  : DITERIMA / DITERIMA DENGAN PERBAIKAN / DITOLAK
```

---

## 4. Ketentuan penggantian tema

| Waktu | Ketentuan |
|---|---|
| Sampai akhir Minggu 6 | Boleh diganti, dengan lembar tema baru dan alasan tertulis |
| Minggu 7 ke atas | **Tidak dapat diganti.** Tema yang bermasalah diselesaikan dengan penyempitan lingkup, bukan penggantian |

Alasannya sederhana: produk dibangun berlapis. Mengganti tema pada Minggu 9 berarti membuang seluruh lapisan yang sudah dibangun, dan tidak ada cukup minggu tersisa untuk membangunnya kembali.

Bila Anda ragu terhadap tema Anda, bawalah keraguan itu pada Minggu 5, ketika masih ada waktu.

---

## 5. Pertanyaan yang sering muncul

**Bolehkah tema saya sama dengan rekan sekelas?**
Boleh bila bidang dan persoalannya memang mirip, tetapi dokumen rujukan, set uji, dan rancangannya harus milik Anda sendiri. Angka lingkup yang berbeda akan membuat produknya berbeda dengan sendirinya.

**Bolehkah memakai tema dari tugas mata kuliah lain?**
Boleh, dan bahkan dianjurkan bila persoalannya nyata. Yang tidak boleh adalah mengumpulkan karya yang sama untuk dua mata kuliah tanpa memberi tahu keduanya.

**Bolehkah tema berkaitan dengan skripsi saya?**
Boleh, dan ini sering menjadi pilihan terbaik. Catat bahwa yang dinilai di sini adalah rekayasa sistemnya, bukan kontribusi ilmiahnya.

**Bagaimana bila bidang saya tidak punya dokumen yang cukup?**
Perluas ke dokumen publik yang relevan: standar nasional, peraturan, panduan resmi, publikasi terbuka. Bila tetap tidak cukup, temanya perlu digeser — dan itu lebih baik disadari Minggu 5 daripada Minggu 9.
