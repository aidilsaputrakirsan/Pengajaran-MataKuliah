# Proweb — Pemrograman Web

**SI2514024 | Proyek: KampusLMS | Laravel 12 | Ganjil 2026/2027**

Modul berbasis proyek. Satu aplikasi dikerjakan berkelompok sepanjang enam belas minggu, dengan siklus **Read → Break → Fix → Build** tiap pekan.

---

## Pemisahan Berkas

| Folder | Pembaca | Boleh di-upload ke LMS |
|---|---|---|
| [mahasiswa/](mahasiswa/) | Mahasiswa | **Ya, seluruhnya** |
| pengajar/ (privat — repo dosen) | Dosen dan asdos | **Tidak** |

Aturannya sederhana: seluruh isi `mahasiswa/` aman di-upload tanpa perlu diperiksa lagi.

Perlu dicatat karena berbeda dari mata kuliah lain: **rubrik, format asesmen, dan pertanyaan interview sengaja dibuka kepada mahasiswa sejak awal semester**, dan karenanya berada di `mahasiswa/`. Yang ditahan di `pengajar/` hanya dua hal: cara pelaksanaan asesmen (pembagian sesi, pembagian peran, persiapan teknis) dan kunci repo cacat.

---

## Kesiapan Berkas

### Siap dibagikan ke mahasiswa

| Berkas | Dibagikan | Catatan |
|---|---|---|
| [mahasiswa/00-daftar-modul.md](mahasiswa/00-daftar-modul.md) | Minggu 1 | Pintu masuk: urutan baca, empat tahap, bobot nilai, milestone |
| [mahasiswa/01-spesifikasi-proyek-kampuslms.md](mahasiswa/01-spesifikasi-proyek-kampuslms.md) | Minggu 1 | Skema database, kontrak API, milestone, kebijakan AI. **Dibaca sebelum modul mana pun** |
| [mahasiswa/02-modul-minggu-01-03.md](mahasiswa/02-modul-minggu-01-03.md) | Minggu 1 | Fondasi Laravel, route/controller/view, database dan CRUD. Berakhir di M1 |
| [mahasiswa/03-modul-minggu-04-07.md](mahasiswa/03-modul-minggu-04-07.md) | Minggu 3 | State dan validasi, routing lanjutan dan IDOR, REST API, otorisasi. Berakhir di M2 |
| [mahasiswa/04-modul-minggu-08-uts.md](mahasiswa/04-modul-minggu-08-uts.md) | **Minggu 1** | UTS: format, rubrik, dan yang diuji. Dibagikan di awal, bukan menjelang ujian |
| [mahasiswa/05-modul-minggu-09-12.md](mahasiswa/05-modul-minggu-09-12.md) | Minggu 8 | Upload file, queue dan notifikasi, performa, deployment. Berakhir di M3 dan M4 |
| [mahasiswa/06-modul-minggu-13-16.md](mahasiswa/06-modul-minggu-13-16.md) | Minggu 12 | Kolaborasi dan peer review, testing, perapian, UAS |
| [mahasiswa/lampiran/A-glosarium.md](mahasiswa/lampiran/A-glosarium.md) | Minggu 1 | Rujukan istilah. Tidak dibaca berurutan |
| [mahasiswa/lampiran/B-repo-latihan-fix.md](mahasiswa/lampiran/B-repo-latihan-fix.md) | Minggu 1 | Daftar branch cacat dan cara mengumpulkan PR |

Nomor berkas mengikuti urutan baca. Dua di antaranya adalah dokumen asesmen — berkas 04 (UTS) dan bagian minggu 16 pada berkas 06 — dan keduanya memuat rubrik serta pertanyaan secara lengkap.

Berkas 04 dibagikan **sejak minggu 1**, bukan menjelang UTS. Keterbukaan itu disengaja: mahasiswa perlu tahu sejak awal bahwa yang diuji adalah kemampuan menjelaskan kodenya sendiri.

### Jangan dibagikan

| Berkas | Alasan |
|---|---|
| pengajar/01-panduan-pelaksanaan-asesmen.md (privat — repo dosen) | Pembagian sesi, pembagian peran dosen–asdos, persiapan teknis sebelum hari-H |
| pengajar/02-panduan-repo-cacat.md (privat — repo dosen) | Daftar induk seluruh cacat pada `kampuslms-broken` |
| pengajar/03-panduan-gaya-penulisan.md (privat — repo dosen) | Aturan istilah dan penyuntingan modul; untuk penulis, bukan pembaca |
| pengajar/04-prompt-pembangunan-repo.md (privat — repo dosen) | Prompt bertahap untuk membangun `kampuslms-sumber` dan menurunkan branch cacat |
| pengajar/penilaian/ (privat — repo dosen) | Lembar nilai berisi nilai mahasiswa sungguhan |
| pengajar/tools/ (privat — repo dosen) | Perekap nilai akhir dan penyamaan pembagian kelompok |
| pengajar/rps/ (privat — repo dosen) | Untuk prodi; boleh dibagikan bila memang dituntut, bukan bagian modul |
| Repo `kampuslms-broken` — catatan letak cacat | Kunci jawaban tahap FIX. Reponya dibagikan, catatan letaknya tidak |
| Skrip `uji-uts.sh` dan `uji-uas.sh` | Dipakai menguji keamanan aplikasi mahasiswa saat asesmen |

---

## Susunan Semester

| Mgg | Topik | Milestone | Penilaian |
|---|---|---|---|
| 1 | Fondasi: cara aplikasi web bekerja, struktur Laravel 12 | — | Kuis 1% + Praktikum 2% |
| 2 | Route, controller, view | — | Kuis 1% + Praktikum 2% |
| 3 | Database dan CRUD | **M1 — Fondasi Data** | Tugas 1 (3%) + interview |
| 4 | Session, validasi, pagination | — | Kuis 1% + Praktikum 2% |
| 5 | Routing lanjutan, model binding, middleware, IDOR | — | Kuis 1% + Praktikum 2% |
| 6 | REST API dan Sanctum | — | Kuis 1% + Praktikum 2% |
| 7 | Otorisasi: Gate, Policy, pengerasan | **M2 — Akses & Keamanan** | Tugas 2 (3%) + interview |
| 8 | **UTS** — demo + code walkthrough + uji keamanan | — | 20% |
| 9 | Upload file dan data dinamis | — | Kuis 1% + Praktikum 2% |
| 10 | Event, queue, notifikasi | **M3 — Alur Akademik Penuh** | Tugas 3 (7%) + interview |
| 11 | Optimisasi performa, N+1, index, cache | — | Kuis 1% + Praktikum 2% |
| 12 | Deployment, SSL, produksi | **M4 — Production Ready** | Tugas 4 (7%) + interview |
| 13 | Git, konflik, peer review | **Peer Review** | Review 10% + Kuis + Praktikum |
| 14 | Debugging dan testing | — | Kuis 1% + Praktikum 2% |
| 15 | Integrasi dan perapian akhir | — | Kuis 1% + Praktikum 2% |
| 16 | **UAS** — presentasi + interview individu | **Proyek Akhir** | 20% |

Total: Kuis 10% · Tugas 20% · Praktikum 20% · Review 10% · UTS 20% · Proyek/UAS 20%.

Pekan milestone (3, 7, 10, 12) tidak dinilai pada komponen Praktikum karena hasil kerjanya sudah dinilai lewat komponen Tugas — tidak ada penilaian ganda.

---

## Persiapan Sebelum Minggu 1

**1. Bangun repo `kampuslms-broken`.** Satu proyek sehat sampai minggu 14, lalu turunkan 13 branch cacat dari titik yang sesuai. Daftar induk dan aturan penyusunannya ada di pengajar/02-panduan-repo-cacat.md (privat — repo dosen). Yang pertama dibutuhkan minggu 1, jadi `w01` sampai `w03` perlu siap sebelum semester berjalan.

**2. Siapkan repo template kelompok** beserta branch protection, agar alur PR minggu 13 tidak dimulai dari nol.

**3. Upload folder `mahasiswa/` ke LMS,** seluruhnya, termasuk berkas minggu 8 dan 13–16.

**4. Siapkan bank soal kuis.** Sepuluh kuis auto-graded, lima soal per kuis, dijalankan di akhir sesi praktikum minggu 1, 2, 4, 5, 6, 9, 11, 13, 14, 15.

**5. Briefing asdos.** Pembagian peran, penilaian tahap FIX, dan persiapan asesmen ada di pengajar/01-panduan-pelaksanaan-asesmen.md (privat — repo dosen).

## Setiap Sesi Praktikum

Sesi 170 menit dengan urutan tetap: **Read** (telusuri yang sudah jalan) → **Break** (rusak sendiri, prediksi diisi lebih dulu) → **Fix** (perbaiki branch cacat, kirim PR) → **Build** (tambah satu lapisan ke KampusLMS). Kuis auto-graded di akhir sesi.

Interview milestone **tidak memakan jam praktikum**: tiap anggota merekam layar 3–4 menit sebelum tenggat, dan tatap muka hanya untuk yang rekamannya meragukan atau tidak cocok dengan `git log`. Rinciannya di Bagian 7.1 spesifikasi proyek.

---

## Yang Masih Perlu Dilengkapi

| Aset | Status | Dibutuhkan |
|---|---|---|
| Repo privat `kampuslms-sumber` — proyek referensi lengkap | Prompt pembangunannya siap (pengajar/04 (privat — repo dosen)), reponya belum dibangun | Minggu 1 (tahap 1–3 saja) |
| Repo publik `kampuslms-broken` beserta 13 branch | Spesifikasi cacat lengkap, reponya belum dibangun. Branch di-push bertahap per minggu | `w01`–`w03` sebelum minggu 1 |
| Catatan letak cacat per branch (`kunci/`) | Belum ada. Tidak pernah masuk repo publik | Bersamaan dengan tiap branch |
| Bank soal kuis (10 × 5 soal) | Belum ada | Minggu 1 |
| `uji-uts.sh` dan `uji-uas.sh` | Belum ada | Minggu 8 dan 16 |
| Lembar penilaian seluruh komponen | Siap — pengajar/penilaian/ (privat — repo dosen), tinggal isi `peserta.csv` | Minggu 1 |
| Repo template kelompok + 16 repo kelompok (A01-A09, B01-B07) | Belum ada | Minggu 1 |

Modul mingguan, spesifikasi proyek, seluruh rubrik, dan panduan pengajar sudah lengkap. Yang tersisa di tabel ini adalah aset operasional, bukan materi.
