# Lampiran F — Panduan Tool, Lingkungan Kerja, dan Biaya

**Kapita Selekta: AI Engineering | Ganjil 2026/2027**

Lampiran ini ditulis dengan asumsi Anda **tidak memiliki latar belakang pemrograman**. Ia tidak menyebut merek atau versi tertentu karena lanskap tool berubah cepat; nama layanan yang dipakai kelas disampaikan pada pertemuan pertama dan diperbarui di forum kelas.

---

## 1. Yang Anda butuhkan

| Kebutuhan | Ketentuan |
|---|---|
| Perangkat | Komputer jinjing, atau akses laboratorium komputer kampus |
| Akses model | Melalui **satu model gateway terpadu** yang ditetapkan kelas — satu kredensial, banyak model |
| Tool pengembangan berbantuan AI | Yang tersedia gratis atau berlisensi pendidikan |
| Tempat menyimpan pekerjaan | Repositori pribadi, **tidak publik** |
| Browser dan pengolah teks | Apa pun yang Anda kuasai |

Anda **tidak** membutuhkan: komputer berspesifikasi tinggi, kartu grafis, server, atau langganan berbayar pribadi ke penyedia model mana pun.

---

## 2. Urutan penyiapan Minggu 3

Kerjakan berurutan. Kalau satu langkah gagal, jangan lanjut — pakai pola **F1 — Pembaca error** pada Lampiran A.

```
1. Buat repositori pribadi untuk produk Anda (tidak publik).
2. Buat file .gitignore, dan pastikan file rahasia ada di dalamnya
   SEBELUM Anda membuat file rahasia itu.
3. Terima kredensial model gateway dari dosen.
4. Simpan kredensial di file rahasia — di luar file kode.
5. Lakukan pemanggilan model pertama.
6. Bedah pemanggilan itu memakai tabel Minggu 3 bagian AMATI.
7. Buat catatan-pemakaian.md dan isi baris pertama.
```

Langkah 2 dikerjakan **sebelum** langkah 4, bukan sesudah. File rahasia yang sudah terlanjur terunggah tetap terekam pada riwayat repositori meskipun kemudian dihapus.

### Kalau Anda benar-benar belum pernah memrogram

Itu diasumsikan, bukan kekurangan. Tiga saran:

1. **Minta penjelasan, bukan hanya perintah.** Pola A dan F pada Lampiran A dirancang untuk memaksa AI menjelaskan sambil memandu. Perintah yang Anda salin tanpa paham akan menagih pada UTS.
2. **Kerjakan satu langkah, uji, baru lanjut.** Kesalahan yang muncul setelah dua puluh langkah sulit dilacak; kesalahan yang muncul setelah satu langkah mudah.
3. **Simpan yang berhasil sebelum mengubahnya.** Salin file yang jalan sebelum mencoba perubahan besar, sehingga Anda selalu bisa kembali.

---

## 3. Aturan kredensial

Tiga aturan yang berlaku sepanjang semester:

1. API key **tidak pernah** ditulis di dalam file kode.
2. API key **tidak pernah** ikut terunggah ke repositori.
3. API key **tidak pernah** ditempel ke percakapan AI — termasuk saat meminta bantuan memperbaiki error.

Aturan ketiga paling sering dilanggar. Sebelum menempel pesan error, tutupi kuncinya.

**Kalau kunci Anda terlanjur terunggah atau tertempel:** laporkan kepada dosen hari itu juga agar kunci dicabut dan diganti. Melaporkan tidak mengurangi nilai. Menyembunyikan, lalu ketahuan pada pemeriksaan Minggu 15, ditangani sebagai pelanggaran etika profesi.

---

## 4. Pengendalian biaya

### Kebijakan kelas

Anggaran ditetapkan di awal semester dan dikelola melalui model gateway dengan batas terpasang. **Tidak ada mahasiswa yang dirugikan karena keterbatasan biaya**; alternatif tanpa biaya selalu disediakan. Kalau anggaran Anda menipis sebelum semester berakhir, sampaikan sebelum habis — bukan setelah.

### Kebiasaan yang menghemat besar

| Kebiasaan | Mengapa berpengaruh besar |
|---|---|
| Tugas rutin ke model kecil | Klasifikasi dan ekstraksi hampir tidak berbeda hasilnya, biayanya berbeda berkali lipat |
| Uji dengan 3 kasus dulu, baru 15 | Kesalahan rancangan yang ketahuan pada kasus ketiga menghemat dua belas pemanggilan |
| Batasi panjang keluaran | Tarif keluaran beberapa kali lipat tarif masukan |
| Jangan kirim seluruh dokumen | Inilah alasan Blok C ada |
| Pasang batas langkah pada agen sejak awal | Agen yang berputar menghabiskan anggaran dalam hitungan menit |
| Catat pemakaian tiap minggu | Anda menyadari pembengkakan pada minggu ia terjadi, bukan pada Minggu 14 |

### Berjalan tanpa biaya sama sekali

Kalau anggaran habis atau Anda memilih tidak memakai model berbayar, hal berikut tetap dapat dikerjakan penuh: seluruh tahap AMATI, seluruh perancangan instruksi dan skema, seluruh rancangan RAG, seluruh analisis workflow-vs-agen, seluruh kajian risiko dan etika, dan penyusunan set uji.

Yang membutuhkan pemanggilan model — tahap PATAHKAN, PERBAIKI, dan pengujian — dijalankan pada model bertarif rendah atau tanpa biaya yang tersedia lewat model gateway. Sampaikan kepada dosen agar dialokasikan. **Tidak ada penurunan nilai** karena memakai model murah; laporan evaluasi yang jujur tentang keterbatasan model murah justru bernilai penuh.

---

## 5. Format `catatan-pemakaian.md`

Dibuat Minggu 3, diisi setiap minggu, menjadi bahan mentah laporan biaya Minggu 14.

```markdown
# CATATAN PEMAKAIAN — <NIM>

| Tanggal | Minggu | Kegiatan | Model | Token masuk | Token keluar | Perkiraan biaya |
|---|:--:|---|---|---:|---:|---:|
|  |  |  |  |  |  |  |

Total kumulatif : Rp ___
Anggaran tersisa: Rp ___
```

Mengisinya tiap minggu memakan lima menit. Merekonstruksinya pada Minggu 14 dari ingatan tidak mungkin, dan laporan biaya tanpa data nyata tidak dapat dinilai.

---

## 6. Bantuan di luar jam kuliah

Kelas ini tidak memiliki asisten. Saluran bantuan utama adalah **forum kelas**, dan kualitasnya bergantung pada kualitas pertanyaan yang masuk.

### Aturan tiga puluh menit

Kalau macet, coba mandiri **30 menit** sebelum bertanya. Dalam tiga puluh menit itu, kerjakan berurutan:

1. Baca ulang pesan error sampai kalimat terakhir — jawabannya sering di situ
2. Pakai pola **F1 — Pembaca error**
3. Kembalikan ke keadaan terakhir yang berjalan, lalu ubah satu hal saja
4. Cari di forum, apakah sudah pernah ditanyakan

### Format bertanya

Pakai format ini supaya pertanyaan Anda bisa dijawab cepat:

```
JUDUL   : <gejala dalam satu kalimat>

Konteks : minggu ke-<N>, tahap <AMATI/PATAHKAN/PERBAIKI/RAKIT>
Yang saya harapkan terjadi :
Yang sebenarnya terjadi    :
Yang sudah saya coba (min. 3) :
Dugaan saya tentang penyebabnya :
Yang saya butuhkan : <arah, bukan jawaban jadi>
```

Alasannya bukan formalitas: menyusun pertanyaan dalam format ini menyelesaikan sebagian pertanyaan sebelum dikirim. Menuliskan "yang saya harapkan" dan "yang sebenarnya terjadi" berdampingan sering langsung memperlihatkan letak salahnya.

### Menjawab pertanyaan rekan

Menjawab pertanyaan rekan di forum dihitung sebagai komponen **Sikap dan Profesionalisme**. Jawaban yang mengarahkan ("sudah cek X?") dinilai lebih tinggi daripada jawaban yang menyerahkan solusi jadi — karena yang kedua merampas kesempatan belajar rekan Anda dan tidak membuktikan Anda paham.

---

## 7. Yang harus dihindari

| Kebiasaan | Akibatnya |
|---|---|
| Menempel seluruh kode ke AI lalu menempel hasilnya kembali tanpa membaca | Anda tidak dapat menjelaskan karya Anda pada UTS; nilai aspek itu nol |
| Menumpuk perubahan tanpa menguji | Kesalahan tidak dapat dilacak ke sebabnya |
| Menyalin rancangan rekan | Angka lingkup K berbeda; hasilnya salah dan terdeteksi |
| Menunda catatan proses sampai akhir pekan | Detail yang bernilai sudah lupa; catatan menjadi karangan |
| Menunggu sampai Minggu 14 untuk mengevaluasi | Tidak ada waktu memperbaiki apa pun yang ditemukan |
| Menyimpan hanya versi terakhir instruksi | Tidak dapat menjawab "versi mana yang lebih baik, dan mengapa" |
| Memakai data pribadi orang sungguhan | Pelanggaran etika, ditangani terpisah dari nilai |

---

## 8. Daftar periksa kesiapan sebelum Minggu 4

- [ ] Repositori pribadi ada dan tidak publik
- [ ] File rahasia masuk .gitignore, diperiksa dengan melihat isi repositori
- [ ] API key tersimpan di luar file kode
- [ ] Pemanggilan model pertama berhasil, buktinya tersimpan
- [ ] Tiga model berbeda pernah dijalankan pada prompt yang sama
- [ ] `catatan-pemakaian.md` ada dan terisi
- [ ] Kode peserta K dicatat dan angka lingkup dihitung
- [ ] Tiga calon persoalan tertulis lengkap
- [ ] Anda tahu ke mana bertanya kalau macet, dan formatnya
