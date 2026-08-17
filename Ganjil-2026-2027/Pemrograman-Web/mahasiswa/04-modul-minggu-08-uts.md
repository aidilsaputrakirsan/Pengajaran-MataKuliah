# MODUL PEMROGRAMAN WEB — MINGGU 8

**SI2514024 | Proyek: KampusLMS | Laravel 12**

> Dokumen asesmen tengah semester. Ditujukan untuk dosen, asdos, **dan** mahasiswa: seluruh rubrik dan pertanyaan dibuka sejak awal semester, bukan menjelang ujian.
>
> Dibaca sejak Minggu 1. Format, rubrik, dan daftar yang diuji ada di sini sehingga Anda tahu sejak awal bahwa yang dinilai adalah kemampuan menjelaskan kode Anda sendiri.

---
---


# MINGGU 8 — UTS: Demo dan Code Walkthrough

**Bobot:** 20% nilai akhir
**Bentuk:** Demonstrasi aplikasi + penelusuran kode + uji keamanan langsung
**Bukan** ujian tulis.

---

## 8.1 Kenapa Bentuknya Begini

Mata kuliah ini berbasis proyek. Ujian tulis akan menguji hal yang berbeda dari yang dikerjakan sepanjang setengah semester, dan lebih mudah dipalsukan daripada menjelaskan kode sendiri di depan penguji.

Yang diuji: apakah aplikasi Anda benar-benar berjalan, dan apakah Anda benar-benar memahami kode yang ada di repo atas nama Anda.

---

## 8.2 Format

**20 menit per kelompok**, dibagi:

| Waktu | Bagian | Siapa |
|-------|--------|-------|
| 7 menit | **Demo alur** dari tiga peran (admin, dosen, mahasiswa) | Seluruh anggota bergantian |
| 8 menit | **Code walkthrough** — penguji menunjuk fitur, anggota yang menulisnya menjelaskan | Individu |
| 5 menit | **Uji keamanan langsung** — penguji mencoba menembus | Seluruh anggota |

### Bagian 1 — Demo (7 menit)

Kelompok mendemonstrasikan alur lengkap yang sudah berjalan sampai minggu 7:

1. Admin membuat mata kuliah, menugaskan dosen, mendaftarkan mahasiswa
2. Dosen login, membuat tugas dan materi di mata kuliahnya
3. Mahasiswa login, melihat mata kuliah yang diikutinya

**Aturan:** jalankan dari `migrate:fresh --seed` yang bersih, memakai akun demo. Tidak boleh ada langkah yang "dilewati saja karena masih bermasalah". Fitur yang tidak jalan lebih baik disebutkan terus terang di awal daripada dihindari diam-diam — kejujuran tidak mengurangi nilai, ketahuan menyembunyikan mengurangi.

### Bagian 2 — Code Walkthrough (8 menit)

Penguji memilih **2–3 fitur secara acak** dan meminta anggota yang menulisnya menjelaskan. Penguji memverifikasi kepenulisan lewat `git log`.

Yang ditanyakan bukan hafalan sintaks, melainkan **keputusan**: kenapa ditulis begini, apa alternatifnya, apa yang terjadi kalau bagian ini dihapus.

Bank pertanyaan (dibuka sejak awal):

- Tunjukkan migrasi yang Anda tulis. Jelaskan setiap constraint di dalamnya.
- Kenapa `course_user` punya unique composite? Peragakan tanpa itu.
- Apa itu mass assignment? Tunjukkan apa yang mencegahnya di kode Anda.
- Kenapa `role` tidak ada di `$fillable`? Di mana ia diisi?
- Tunjukkan satu Policy yang Anda tulis. Jelaskan tiap barisnya.
- Kenapa `@can` di Blade tidak cukup? Peragakan.
- Apa beda 401 dan 403? Tunjukkan masing-masing satu di API Anda.
- Kenapa `$request->validated()` lebih aman daripada `$request->all()`?
- Kenapa filter pencarian ada di query string, bukan session?
- Kenapa `session()->regenerate()` dipanggil setelah login?
- Di mana middleware didaftarkan pada Laravel 12? Kenapa beda dari tutorial?
- Tunjukkan bagian yang Anda tulis dengan bantuan AI. Apa yang Anda ubah, dan kenapa?

Pertanyaan terakhir **selalu** ditanyakan. Jawaban "saya pakai AI lalu saya ubah X karena Y" adalah jawaban yang baik. Jawaban "saya tidak tahu, itu dari AI" adalah nol untuk fitur tersebut.

### Bagian 3 — Uji Keamanan Langsung (5 menit)

Penguji membuka `docs/keamanan.md` milik kelompok, memilih **satu baris** dari tabel Titik Rawan IDOR, lalu mencoba menembusnya lewat `curl` atau browser di depan kelompok.

Kelompok harus bisa menjelaskan apa yang menahannya. Kalau ternyata **tembus**, kelompok diberi kesempatan menjelaskan mengapa dan bagaimana memperbaikinya — pemahaman atas kegagalan sendiri tetap bernilai, meski lebih kecil daripada kode yang benar.

---

## 8.3 Rubrik UTS (20%)

| Kriteria | Bobot | 4 (Sangat Baik) | 3 (Baik) | 2 (Cukup) | 1 (Kurang) |
|----------|-------|-----------------|----------|-----------|------------|
| **Fungsionalitas demo** | 25% | Seluruh alur tiga peran berjalan mulus dari seed bersih | Berjalan, 1–2 kendala kecil | Beberapa fitur belum jalan | Alur inti tidak dapat didemonstrasikan |
| **Penerapan konsep** | 25% | Skema, validasi, otorisasi seluruhnya sesuai spesifikasi | Sesuai dengan sedikit penyimpangan | Ada konsep inti yang belum diterapkan | Banyak menyimpang dari spesifikasi |
| **Pemahaman individu** | 30% | Setiap anggota menjelaskan kodenya lancar, termasuk alternatif dan konsekuensi | Menjelaskan dengan sedikit bantuan | Menjelaskan sebagian, ragu pada keputusan desain | Tidak dapat menjelaskan kode atas namanya |
| **Keamanan** | 20% | Uji tembus gagal; kelompok menjelaskan pertahanannya dengan tepat | Uji tembus gagal, penjelasan kurang lengkap | Tembus, tapi kelompok paham sebabnya | Tembus dan tidak paham sebabnya |

**Nilai pemahaman individu bersifat perorangan**, tidak dirata-rata kelompok. Tiga kriteria lainnya nilai kelompok.

---

## 8.4 Panduan Pelaksanaan

Pembagian sesi dan persiapan penguji ada pada dokumen pengajar (`pengajar/01-panduan-pelaksanaan-asesmen.md`). Dua hal yang perlu Anda ketahui sebagai peserta: **`migrate:fresh --seed` dijalankan pada repo Anda sebelum sesi dimulai** — kelompok yang gagal di langkah itu sudah kehilangan poin fungsionalitas sebelum demo, dan kelompok yang belum giliran mengerjakan **peer observation** yang catatannya dikumpulkan.
