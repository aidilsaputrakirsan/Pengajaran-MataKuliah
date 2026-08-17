# Proweb — Daftar Modul dan Cara Memakainya

**Pemrograman Web | SI2514024 | 3 SKS | Ganjil 2026/2027**
**Proyek: KampusLMS | Laravel 12, MySQL/MariaDB**
**Kelas A — 9 kelompok (A01–A09) · Kelas B — 7 kelompok (B01–B07)**

---

## Bacalah berurutan

| File | Dibaca sebelum | Isi |
|---|---|---|
| [01-spesifikasi-proyek-kampuslms.md](01-spesifikasi-proyek-kampuslms.md) | Minggu 1 | Kontrak proyek: fitur wajib, skema database, kontrak API, milestone, kebijakan AI |
| [02-modul-minggu-01-03.md](02-modul-minggu-01-03.md) | Minggu 1 | Fondasi Laravel, route/controller/view, database dan CRUD. Berakhir di M1 |
| [03-modul-minggu-04-07.md](03-modul-minggu-04-07.md) | Minggu 3 | State dan validasi, routing lanjutan dan IDOR, REST API, otorisasi. Berakhir di M2 |
| [04-modul-minggu-08-uts.md](04-modul-minggu-08-uts.md) | **Minggu 1** | Format dan rubrik UTS. Dibagikan sejak awal, bukan menjelang ujian |
| [05-modul-minggu-09-12.md](05-modul-minggu-09-12.md) | Minggu 8 | Unggah berkas, queue dan notifikasi, performa, deployment. Berakhir di M3 dan M4 |
| [06-modul-minggu-13-16.md](06-modul-minggu-13-16.md) | Minggu 12 | Kolaborasi dan peer review, testing, perapian, UAS |

Baca bagian **Konsep** sebelum sesi praktikum, bukan saat sesi berjalan. Tiga tahap sisanya dikerjakan di laboratorium.

## Lampiran

| File | Dipakai untuk |
|---|---|
| [lampiran/A-glosarium.md](lampiran/A-glosarium.md) | Arti istilah dan padanan Indonesianya, beserta minggu istilah itu muncul |
| [lampiran/B-repo-latihan-fix.md](lampiran/B-repo-latihan-fix.md) | Repo `kampuslms-broken`, daftar branch per minggu, dan cara mengumpulkan PR |

Rubrik dan pertanyaan interview tidak dirahasiakan — semuanya ada di modul minggu bersangkutan. Tidak satu pun dapat dijawab dengan menghafal, karena semuanya menunjuk ke kode Anda sendiri.

---

## Lima hal yang perlu Anda ketahui sebelum minggu 1

**1. Setiap minggu punya empat tahap.**

| Tahap | Yang dikerjakan |
|---|---|
| READ | Menelusuri kode yang sudah jalan dan menjelaskan mengapa ia bekerja |
| BREAK | Merusak sendiri dari tabel percobaan. **Kolom prediksi diisi sebelum mencoba** |
| FIX | Memperbaiki branch cacat; jumlah masalahnya selalu diberitahukan. Dikirim sebagai PR |
| BUILD | Menambah satu lapisan ke KampusLMS kelompok Anda |

Sesi 170 menit, urutannya tetap, ditutup kuis auto-graded.

**2. Praktikum dinilai dari Checkpoint, bukan dari jumlah commit.** Checkpoint di akhir tiap minggu adalah instrumen penilaian, bukan latihan mandiri. Commit atas nama sendiri tetap wajib sebagai bukti kepenulisan — tetapi bukan skor.

**3. Kelompok dan peer review tidak melintasi kelas.** Kelompok Anda berisi 4 orang (dua kelompok di Kelas B berisi 5) dan seluruh anggotanya sekelas. Fitur diferensiasi unik di dalam kelas Anda — kelompok di kelas sebelah boleh memilih fitur yang sama. Peer review minggu 13 juga berputar di dalam kelas.

**4. Interview milestone lewat rekaman.** Di minggu 3, 7, 10, dan 12 tiap anggota merekam layar 3–4 menit dan mengunggahnya ke LMS sebelum tenggat. Tatap muka hanya untuk yang perlu klarifikasi. Rinciannya di Bagian 7.1 spesifikasi proyek.

**5. Anda boleh memakai AI.** Yang dinilai adalah apakah Anda bisa membaca, memverifikasi, dan mempertanggungjawabkan kode itu. Setiap minggu menyediakan Prompt Pack, termasuk prompt untuk memeriksa apakah jawaban AI masih memakai struktur Laravel 10 ke bawah — dan itu sering terjadi.

Yang dilarang: menempelkan `.env`, kredensial, atau data pribadi nyata ke layanan AI mana pun. Kode yang tidak bisa Anda jelaskan saat interview dinilai sebagai tidak dikerjakan.

---

## Penilaian

| Komponen | Bobot |
|---|---:|
| Kuis — akhir sesi minggu 1, 2, 4, 5, 6, 9, 11, 13, 14, 15 | 10% |
| Praktikum — Checkpoint pada sepuluh minggu yang sama | 20% |
| Tugas — milestone M1 (3%), M2 (3%), M3 (7%), M4 (7%) | 20% |
| Review — peer review antar kelompok, minggu 13 | 10% |
| UTS — demo dan code walkthrough, minggu 8 | 20% |
| Proyek Akhir / UAS — presentasi dan interview individu, minggu 16 | 20% |

Minggu milestone (3, 7, 10, 12) tidak dinilai pada komponen Praktikum karena hasil kerjanya sudah dinilai lewat Tugas. Rubrik lengkap ada di Lampiran B RPS.

## Milestone

| Minggu | Milestone | Yang harus jalan |
|---|---|---|
| 3 | **M1 — Fondasi Data** | Migrasi dan seeder sesuai Bagian 4, CRUD mata kuliah dan pengguna |
| 7 | **M2 — Akses & Keamanan** | Login, 3 role, middleware, policy, validasi. Materi dan tugas ter-CRUD |
| 10 | **M3 — Alur Akademik Penuh** | Unggah materi, pengumpulan tugas, penilaian, notifikasi via queue |
| 12 | **M4 — Production Ready** | Ter-deploy di domain publik dengan SSL, optimisasi query terdokumentasi |
| 16 | **Proyek Akhir** | Presentasi, interview individu, laporan |
