# Lampiran C — Rencana Instrumen dan Rubrik Penilaian

**Proweb | SI2514024 | Proyek KampusLMS | Ganjil 2026/2027**

Seluruh yang dinilai sepanjang semester, beserta rubriknya, dikumpulkan di satu tempat. Tidak ada kriteria rahasia — rubrik dan pertanyaan interview memang dibuka sejak minggu 1, karena tidak satu pun dapat dijawab dengan menghafal: semuanya menunjuk ke kode Anda sendiri.

Rinciannya tetap ada di modul minggu masing-masing; lampiran ini merangkumnya supaya Anda dapat memeriksa posisi nilai kapan saja tanpa membuka enam berkas.

---

## Ringkasan bobot

| Komponen | Bobot | Kapan |
|---|---:|---|
| Kuis | 10% | Akhir sesi minggu 1, 2, 4, 5, 6, 9, 11, 13, 14, 15 |
| Praktikum | 20% | Checkpoint pada sepuluh minggu yang sama |
| Tugas | 20% | Milestone M1 (3%), M2 (3%), M3 (7%), M4 (7%) |
| Review | 10% | Peer review antar kelompok, minggu 13 |
| UTS | 20% | Demo dan code walkthrough, minggu 8 |
| Proyek Akhir / UAS | 20% | Presentasi dan interview individu, minggu 16 |

Minggu milestone — 3, 7, 10, dan 12 — tidak dinilai pada komponen Praktikum, karena hasil kerja minggu itu sudah dinilai lewat Tugas.

### Skala rubrik 1–4

Dipakai pada Tugas, Review, UTS, dan UAS.

| Skor | Sebutan | Artinya |
|:--:|---|---|
| 4 | Sangat Baik | Tercapai penuh, dan Anda dapat menjelaskan mengapa |
| 3 | Baik | Tercapai, penjelasan sebagian |
| 2 | Cukup | Tercapai sebagian |
| 1 | Kurang | Dikerjakan, hasil belum tercapai |
| 0 | Gugur | Ketentuan gugur terpenuhi — bukan sekadar nilai rendah |

Skor diubah ke skala 0–100 dengan `(skor − 1) ÷ 3 × 100`, sehingga 1 menjadi 0 dan 4 menjadi 100. Nol tetap nol.

**Aturan yang menembus seluruh rubrik:** kode yang tidak dapat dijelaskan oleh mahasiswa yang namanya tercantum sebagai penulis dinilai **0** pada bagian tersebut, baik dihasilkan dengan bantuan AI maupun tidak.

---

## Kuis akhir sesi

**Bobot 10% total · Kategori Tes / Ujian (Quiz) · Sepuluh kali sepanjang semester**

### Yang dinilai

Kuis pendek auto-graded di LMS pada akhir tiap sesi praktikum minggu 1, 2, 4, 5, 6, 9, 11, 13, 14, dan 15. Soalnya menguji pemahaman konsep minggu itu, bukan hafalan sintaks — misalnya akibat urutan route, beda 401 dan 403, atau risiko `APP_DEBUG=true` di produksi.

Kuis dikerjakan setelah tahap BUILD selesai, sehingga Anda menjawab dengan kode yang baru saja Anda tulis masih segar di ingatan.

### Rubrik

Auto-graded, skala 0–100 langsung dari LMS. Tidak ada penilaian manual dan tidak ada rubrik bertingkat. Nilai komponen adalah rata-rata kesepuluh kuis.

Kuis yang terlewat karena tidak hadir bernilai 0 kecuali ada surat sakit atau izin yang diunggah sesuai kontrak kuliah.

---

## Checkpoint praktikum

**Bobot 20% total · Kategori Praktikum · Sepuluh minggu**

### Yang dinilai

Checkpoint di akhir tiap sesi adalah **instrumen penilaian, bukan latihan mandiri**. Isinya enam butir pertanyaan atau peragaan yang diperiksa langsung — misalnya "sebutkan urutan berkas yang dilewati sebuah request", atau "peragakan serangan mass assignment beserta pencegahannya".

Yang dinilai adalah penguasaan, bukan jumlah commit. Commit atas nama sendiri tetap wajib sebagai bukti kepenulisan, tetapi bukan skor.

### Rubrik

Biner per butir: **lolos atau tidak**, enam butir per minggu.

```
nilai minggu itu = (butir yang lolos ÷ 6) × 100
```

Nilai komponen adalah rata-rata kesepuluh minggu. Tidak ada nilai parsial per butir — sebuah butir dianggap lolos hanya bila Anda dapat menunjukkan atau menjelaskannya tanpa dibantu.

Daftar enam butir tiap minggu tertulis lengkap di bagian Checkpoint pada modul minggu bersangkutan, dibagikan sebelum sesi dimulai.

---

## Tugas 1: Milestone M1 Fondasi Data

**Bobot 3% · Kategori Tugas · Minggu 3 · Sub-CPMK 3**

### Yang dinilai

Migrasi dan seeder sesuai Bagian 4 spesifikasi proyek, beserta CRUD mata kuliah dan pengguna yang berjalan. Dinilai lewat rekaman layar 3–4 menit per anggota yang diunggah ke LMS sebelum tenggat.

### Rubrik

| Kriteria | Bobot | Yang dilihat |
|---|---:|---|
| Fungsionalitas | 35% | CRUD mata kuliah dan pengguna berjalan; seeder menghasilkan data sesuai ketentuan |
| Implementasi Konsep | 30% | Skema persis sesuai spesifikasi; constraint lengkap; relasi benar; `$fillable` ketat; migrasi reversible |
| Interview/Pemahaman | 25% | Mampu menjelaskan tiap keputusan desain dan memperagakan mass assignment |
| Kualitas Kode | 10% | Controller tipis; penamaan konsisten; commit message rapi; PR direview |

Kode yang berjalan sempurna tetapi tidak bisa dijelaskan penulisnya dinilai **0 pada Interview** — dan itu 25% dari nilai tugas ini.

---

## Tugas 2: Milestone M2 Akses dan Keamanan

**Bobot 3% · Kategori Tugas · Minggu 7 · Sub-CPMK 7**

### Yang dinilai

Login, tiga peran, middleware, policy, dan validasi berjalan. Materi dan tugas sudah ter-CRUD.

### Rubrik

| Kriteria | Bobot | Yang dilihat |
|---|---:|---|
| Fungsionalitas | 30% | Login, tiga peran, dashboard per peran, CRUD materi dan tugas berjalan |
| Implementasi Konsep | 35% | Policy lengkap dan benar; penyaringan di level query; validasi server-side; tidak ada IDOR tersisa |
| Interview/Pemahaman | 25% | Mampu menjelaskan dan **memperagakan** perbedaan tombol tersembunyi versus akses tertutup |
| Kualitas Kode | 10% | Policy tidak duplikatif; controller tipis; dokumentasi keamanan rapi; PR direview |

Bobot Implementasi Konsep dinaikkan minggu ini karena keamanan adalah inti Sub-CPMK-7. Aplikasi yang seluruh fiturnya jalan tetapi masih menyisakan satu IDOR terbuka **tidak dapat lulus** komponen ini.

---

## Tugas 3: Milestone M3 Alur Akademik Penuh

**Bobot 7% · Kategori Tugas · Minggu 10 · Sub-CPMK 9**

### Yang dinilai

Alur unggah materi, pengumpulan tugas, penilaian, dan notifikasi lewat queue berjalan utuh dari ketiga peran.

### Rubrik

| Kriteria | Bobot | Yang dilihat |
|---|---:|---|
| Fungsionalitas | 35% | Alur unggah → kumpul → nilai → notifikasi berjalan utuh dari tiga peran |
| Implementasi Konsep | 30% | Queue benar-benar asinkron; Event/Listener terpisah rapi; penanganan job gagal; payload aman |
| Interview/Pemahaman | 25% | Mampu menjelaskan kegagalan senyap dan cara mendiagnosisnya |
| Kualitas Kode | 10% | Controller tipis; tidak ada N+1 pada pengiriman notifikasi; PR direview |

---

## Tugas 4: Milestone M4 Production Ready

**Bobot 7% · Kategori Tugas · Minggu 12 · Sub-CPMK 11**

### Yang dinilai

Aplikasi ter-deploy di domain publik dengan SSL aktif, dan optimisasi query terdokumentasi.

### Rubrik

| Kriteria | Bobot | Yang dilihat |
|---|---:|---|
| Fungsionalitas | 30% | Aplikasi live, HTTPS aktif, seluruh fitur berjalan sama seperti di lokal, akun demo berfungsi |
| Implementasi Konsep | 30% | Daftar periksa keamanan terlewati; Supervisor berjalan; optimisasi produksi diterapkan; `deploy.sh` benar |
| Interview/Pemahaman | 25% | Mampu menjelaskan risiko tiap konfigurasi dan mendiagnosis masalah produksi tanpa menyalakan debug |
| Kualitas Kode | 10% | Dokumentasi deployment jelas dan bisa diikuti orang lain; PR direview |

**Diskualifikasi otomatis** pada Implementasi Konsep: `APP_DEBUG=true` di produksi, `.env` dapat diakses publik, atau berkas submission dapat diunduh tanpa login. Ketiganya bukan kekurangan kecil — ketiganya membocorkan data sungguhan kalau aplikasi ini dipakai.

---

## Review: Peer Review Antar Kelompok

**Bobot 10% · Kategori Sikap dan Profesionalisme · Minggu 13 · Sub-CPMK 12**

### Yang dinilai

Menelaah repositori kelompok lain di dalam kelas yang sama, menuliskan temuan, lalu menanggapi review yang masuk ke kelompok sendiri. Peer review berputar di dalam kelas dan tidak melintasi kelas.

### Rubrik

| Kriteria | Bobot | Yang dilihat |
|---|---:|---|
| Kelengkapan temuan | 30% | Ada ≥3 temuan dari 3 kategori berbeda |
| Ketepatan | 30% | Temuan benar-benar masalah nyata, bukan selera; risiko dijelaskan tepat |
| Kegunaan saran | 25% | Saran konkret dan bisa langsung dikerjakan, menyebut lokasi |
| Nada dan tanggapan | 15% | Menyerang kode bukan orang; menanggapi review yang masuk dengan argumen |

Temuan yang dihasilkan AI lalu ditempel tanpa dipahami akan ketahuan saat dosen menanyakannya, dan dinilai **0**. Anda boleh memakai AI untuk **memahami** kode asing dan **menajamkan** tulisan sendiri, bukan untuk menghasilkan temuannya.

---

## UTS: Demo dan Code Walkthrough

**Bobot 20% · Kategori Tes / Ujian (UTS) · Minggu 8 · Sub-CPMK 1–7**

### Yang dinilai

Demo aplikasi dari seed bersih, code walkthrough, dan uji keamanan langsung — 20 menit per kelompok. Aplikasi harus lolos `migrate:fresh --seed` di awal; kelompok yang gagal di langkah ini sudah kehilangan poin fungsionalitas sebelum demo dimulai.

Kelompok yang belum giliran mengerjakan peer observation: menonton kelompok lain dan mencatat satu hal yang lebih baik serta satu hal yang lebih buruk dari punya mereka. Catatan ini menjadi pemanasan untuk peer review minggu 13.

### Rubrik

| Kriteria | Bobot | 4 (Sangat Baik) | 3 (Baik) | 2 (Cukup) | 1 (Kurang) |
|---|---:|---|---|---|---|
| Fungsionalitas demo | 25% | Seluruh alur tiga peran berjalan mulus dari seed bersih | Berjalan, 1–2 kendala kecil | Beberapa fitur belum jalan | Alur inti tidak dapat didemonstrasikan |
| Penerapan konsep | 25% | Skema, validasi, otorisasi seluruhnya sesuai spesifikasi | Sesuai dengan sedikit penyimpangan | Ada konsep inti yang belum diterapkan | Banyak menyimpang dari spesifikasi |
| Pemahaman individu | 30% | Setiap anggota menjelaskan kodenya lancar, termasuk alternatif dan konsekuensi | Menjelaskan dengan sedikit bantuan | Menjelaskan sebagian, ragu pada keputusan desain | Tidak dapat menjelaskan kode atas namanya |
| Keamanan | 20% | Uji tembus gagal; kelompok menjelaskan pertahanannya dengan tepat | Uji tembus gagal, penjelasan kurang lengkap | Tembus, tapi kelompok paham sebabnya | Tembus dan tidak paham sebabnya |

**Nilai pemahaman individu bersifat perorangan**, tidak dirata-rata kelompok. Tiga kriteria lainnya nilai kelompok.

---

## UAS: Presentasi dan Interview Individu

**Bobot 20% · Kategori Tes / Ujian (UAS) · Minggu 16 · Sub-CPMK 14**

### Yang dinilai

Presentasi, demo dari server produksi, dan interview individu — 30 menit per kelompok.

### Rubrik

| Kriteria | Bobot | 4 (Sangat Baik) | 3 (Baik) | 2 (Cukup) | 1 (Kurang) |
|---|---:|---|---|---|---|
| Fungsionalitas | 20% | Seluruh fitur wajib dan diferensiasi berjalan di produksi tanpa kendala | Berjalan, kendala kecil | Beberapa fitur wajib belum jalan | Alur inti tidak dapat didemonstrasikan |
| Implementasi konsep | 20% | Skema, otorisasi, asinkron, optimisasi, deployment seluruhnya sesuai spesifikasi | Sesuai dengan sedikit penyimpangan | Ada konsep inti yang belum diterapkan | Banyak menyimpang |
| Pemahaman individu | 25% | Menjawab ketiga pertanyaan lancar, termasuk kode yang bukan tulisannya | Menjawab dengan sedikit bantuan | Hanya menguasai bagiannya sendiri | Tidak dapat menjelaskan kode atas namanya |
| Kualitas kode | 10% | Bersih, konsisten, tanpa duplikasi berarti, test bermakna | Rapi dengan sedikit kekurangan | Ada duplikasi dan kode mati | Berantakan |
| Laporan | 10% | Lengkap, refleksinya jujur serta mendalam | Lengkap, refleksi dangkal | Kurang lengkap | Tidak memadai |
| Presentasi | 5% | Terstruktur, tepat waktu, meyakinkan | Baik | Kurang terstruktur | Tidak siap |
| Kontribusi individu | 10% | Merata, terbukti di `git log` dan cocok dengan README | Cukup merata | Timpang | Nyaris tidak berkontribusi |

**Pemahaman individu dan kontribusi individu bersifat perorangan** — 35% total. Sisanya nilai kelompok.

---

## Ketentuan gugur dan sanksi

| Keadaan | Akibat |
|---|---|
| Kode tidak dapat dijelaskan penulisnya | 0 pada bagian tersebut, terlepas dari kualitas kodenya |
| Kecurangan: plagiat, menyontek | Nilai 0 pada evaluasi bersangkutan |
| Kecurangan pengisian daftar hadir | Tidak lulus |
| Tidak hadir saat presentasi kelompok | 0 bagi mahasiswa bersangkutan |
| Laporan ketidakaktifan dari ketua kelompok | Pengurangan maksimal 50% nilai tugas kelompok bagi yang bersangkutan |
| `APP_DEBUG=true` di produksi, `.env` publik, atau berkas submission terbuka | 0 pada Implementasi Konsep Tugas 4 |
| Temuan peer review hasil tempelan AI tanpa pemahaman | 0 pada komponen Review |
| Menempelkan `.env`, kredensial, atau data pribadi nyata ke layanan AI | Pelanggaran kontrak kuliah, ditangani terpisah dari nilai |

---

## Skala huruf

| Nilai angka | Huruf |
|---|---|
| 86 ≤ N ≤ 100 | A |
| 76 ≤ N < 86 | AB |
| 66 ≤ N < 76 | B |
| 56 ≤ N < 66 | BC |
| 51 ≤ N < 56 | C |
| 41 ≤ N < 51 | D |
| N < 41 | E |

Presensi kurang dari ketentuan minimum tanpa keterangan berakibat tidak diperkenankan mengikuti evaluasi akhir. Angka minimumnya tercantum pada kontrak kuliah RPS.
