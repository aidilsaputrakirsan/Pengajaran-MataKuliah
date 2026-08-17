# MODUL PEMROGRAMAN WEB — MINGGU 1–3

**SI2514024 | Proyek: KampusLMS | Laravel 12**

> **Cara memakai modul ini.** Setiap minggu punya empat bagian: **Konsep** (dibaca sebelum kelas), **Prompt Pack** (alat bantu AI), **Read–Break–Fix–Build** (dikerjakan saat praktikum), dan **Checkpoint** (gerbang sebelum lanjut).
>
> Anda **boleh dan dianjurkan** memakai AI untuk menulis kode. Yang dinilai adalah apakah Anda bisa membaca, memverifikasi, dan mempertanggungjawabkan kode itu.
>
> **Rujukan:** repo latihan FIX dan daftar branch ada di [Lampiran B](lampiran/B-repo-latihan-fix.md). Istilah asing ada di [Lampiran A](lampiran/A-glosarium.md). Bobot nilai dan aturan interview ada di [00-daftar-modul.md](00-daftar-modul.md).

---
---

# MINGGU 1 — Fondasi: Bagaimana Sebuah Aplikasi Web Bekerja

**Sub-CPMK:** Mahasiswa mampu menjelaskan konsep dasar pengembangan aplikasi web serta arsitektur frontend dan backend (C2), menunjukkan sikap aktif dan bertanggung jawab dalam mengikuti kontrak perkuliahan (A2), serta mengidentifikasi komponen penyusun aplikasi web (P1).

**Target akhir minggu:** Laravel 12 berjalan di komputer Anda, repo kelompok aktif, dan Anda bisa menunjukkan di mana letak setiap bagian aplikasi.

---

## 1.1 Konsep

### Yang sebenarnya terjadi saat Anda membuka sebuah halaman

Ketika Anda mengetik `kampuslms.test/courses` lalu menekan Enter, ada rantai peristiwa yang selalu sama di aplikasi web mana pun:

```
Browser  →  HTTP Request  →  Web Server  →  Aplikasi (Laravel)  →  Database
                                                    ↓
Browser  ←  HTML/JSON      ←  Web Server  ←  Response
```

Dua istilah yang sering dipakai serampangan:

- **Frontend** — bagian yang dijalankan di browser pengguna. HTML, CSS, JavaScript. Ia tidak bisa dipercaya, karena pengguna bisa mengubah apa pun di sana.
- **Backend** — bagian yang dijalankan di server Anda. PHP/Laravel, database. Hanya di sinilah keputusan penting boleh diambil.

Kalimat terakhir itu adalah fondasi seluruh mata kuliah ini. Tombol "Hapus" yang disembunyikan di frontend **bukan** keamanan — pengguna tetap bisa memanggil URL-nya langsung. Kita akan kembali ke ini berkali-kali.

### Kenapa framework, bukan PHP polos

PHP polos bisa menjalankan aplikasi web. Tapi setiap tim akhirnya membangun ulang hal yang sama: routing, koneksi database, validasi, autentikasi, pencegahan SQL injection. Framework menyediakan itu semua dalam bentuk yang sudah teruji jutaan aplikasi.

Laravel memakai pola **MVC**:

| Bagian | Tugasnya | Analogi restoran |
|--------|----------|------------------|
| **Route** | Mencocokkan URL dengan kode yang harus jalan | Daftar menu |
| **Controller** | Menerima request, mengatur alur, mengembalikan response | Pelayan |
| **Model** | Mewakili data dan aturannya, berbicara ke database | Gudang bahan |
| **View (Blade)** | Menyusun tampilan HTML | Penyajian di piring |

Aturan praktisnya: **controller tipis, model gemuk, view bodoh.** Controller tidak boleh berisi logika bisnis panjang. View tidak boleh melakukan query.

### Struktur proyek Laravel 12 yang wajib Anda kenali

Laravel 12 berbeda dari tutorial lama yang mungkin Anda temukan. Ini penting — banyak jawaban AI dan artikel blog masih memakai struktur Laravel 10 ke bawah.

```
kampuslms/
├── app/
│   ├── Http/Controllers/     ← pelayan
│   ├── Models/               ← gudang
│   ├── Policies/             ← aturan siapa boleh apa (minggu 7)
│   └── Providers/
├── bootstrap/
│   └── app.php               ← ⚠️ PUSAT KONFIGURASI di Laravel 11+
├── config/                   ← pengaturan, dibaca dari .env
├── database/
│   ├── migrations/           ← riwayat perubahan struktur tabel
│   ├── factories/            ← pabrik data palsu
│   └── seeders/              ← pengisi data awal
├── public/                   ← ⚠️ SATU-SATUNYA folder yang boleh diakses publik
│   └── index.php             ← pintu masuk semua request
├── resources/views/          ← Blade
├── routes/web.php            ← daftar URL
├── storage/                  ← berkas unggahan, log, cache (TIDAK publik)
├── .env                      ← ⚠️ rahasia. JANGAN PERNAH di-commit
└── artisan                   ← perkakas baris perintah
```

**Perubahan besar di Laravel 11/12** yang harus Anda tahu:
- Tidak ada lagi `app/Http/Kernel.php`. Middleware didaftarkan di `bootstrap/app.php`.
- Tidak ada lagi `app/Console/Kernel.php`. Scheduler ditulis di `routes/console.php`.
- Folder `app/Http/Middleware/` **tidak ada sama sekali** pada proyek baru — middleware bawaan berada di dalam framework. Folder itu baru muncul saat Anda menjalankan `php artisan make:middleware`.
- Berkas konfigurasi di `config/` lebih sedikit; sisanya bisa di-*publish* saat dibutuhkan.

Kalau AI memberi Anda kode yang menyuruh mengedit `app/Http/Kernel.php`, itu tanda kuat jawabannya berasal dari Laravel 10 ke bawah. **Ini akan sering terjadi.** Kemampuan mengenalinya adalah salah satu hal yang membedakan Anda dari orang yang sekadar menempel kode.

### `.env` dan kenapa ia berbahaya

`.env` berisi kredensial database, kunci enkripsi aplikasi, dan nanti kunci API layanan berbayar. Berkas ini:
- **Tidak** ikut ke Git (sudah ada di `.gitignore` bawaan — jangan diutak-atik)
- **Tidak** boleh ditempel ke ChatGPT/Claude saat minta bantuan
- Punya pendamping `.env.example` yang berisi *nama* variabel tanpa nilainya — inilah yang di-commit

`APP_KEY` dihasilkan sekali per instalasi dengan `php artisan key:generate`. Kalau hilang, semua data terenkripsi dan session menjadi tidak terbaca.

---

## 1.2 Prompt Pack — Minggu 1

Prompt bukan mantra. Yang membedakan hasil bagus dan buruk adalah **konteks** yang Anda berikan dan **peran** yang Anda tetapkan.

### A. Prompt Eksplorasi — untuk memahami, bukan menyalin

```
Saya mahasiswa semester 5 yang baru pertama kali memakai Laravel.
Saya sudah menginstal Laravel 12 dan melihat folder-foldernya.

Jelaskan alur sebuah HTTP request di Laravel 12, mulai dari browser
sampai HTML kembali ke browser. Sebutkan berkas mana yang tersentuh
di setiap langkah.

Syarat:
- Pakai Laravel 12, BUKAN versi 10 ke bawah. Ingat bahwa Kernel.php
  sudah tidak ada.
- Jangan beri kode dulu. Saya ingin memahami alurnya.
- Setelah menjelaskan, ajukan 3 pertanyaan kepada saya untuk menguji
  apakah saya benar-benar paham.
```

Bagian terakhir adalah yang paling berharga dan paling jarang dipakai orang. **Menyuruh AI menguji Anda** jauh lebih berguna daripada menyuruhnya menjawab.

### B. Prompt Verifikasi Versi — pakai setiap kali AI memberi kode Laravel

```
Kode yang kamu berikan, apakah valid untuk Laravel 12?

Periksa khusus:
1. Apakah ada referensi ke app/Http/Kernel.php? (berkas ini sudah dihapus
   di Laravel 11+)
2. Apakah middleware didaftarkan di bootstrap/app.php?
3. Apakah ada pemakaian API yang sudah deprecated di Laravel 12?

Kalau ada yang tidak sesuai, tunjukkan versi yang benar dan jelaskan
apa yang berubah.
```

### C. Prompt Debugging — larang AI langsung menjawab

```
Saya mendapat error berikut saat menjalankan Laravel 12:

[tempel pesan error lengkap di sini]

JANGAN langsung memberi solusi.
Sebagai gantinya:
1. Jelaskan apa arti pesan error ini dalam bahasa sederhana.
2. Sebutkan 3 kemungkinan penyebab, urut dari yang paling sering terjadi.
3. Tanyakan kepada saya apa yang perlu saya periksa untuk mempersempit
   kemungkinan tersebut.
```

Kenapa begini? Karena error yang sama akan Anda temui puluhan kali semester ini. Kalau setiap kali AI langsung memperbaikinya, Anda tidak akan pernah bisa memperbaikinya sendiri saat sedang demo di depan kelas.

### D. Prompt Terlarang

Jangan pernah menempelkan isi `.env`, kredensial database asli, atau data pribadi nyata (NIM, email mahasiswa sungguhan) ke layanan AI mana pun. Kalau perlu contoh, ganti nilainya dengan data palsu.

---

## 1.3 Read → Break → Fix → Build

### READ — Bedah instalasi Anda sendiri (45 menit)

Setelah instalasi selesai dan halaman selamat datang Laravel muncul, kerjakan **tanpa AI**:

1. Buka `public/index.php`. Baca dari atas ke bawah. Tulis dalam 3 kalimat apa yang dilakukan berkas ini.
2. Buka `bootstrap/app.php`. Identifikasi bagian mana yang mengurus route, mana yang mengurus middleware, mana yang mengurus exception.
3. Buka `routes/web.php`. Temukan route yang menghasilkan halaman selamat datang. Ubah teksnya, muat ulang browser, pastikan berubah.
4. Jalankan `php artisan route:list`. Cocokkan keluarannya dengan isi `routes/web.php`.

Tulis jawabannya di `docs/minggu-01-catatan.md` di repo kelompok. Setiap anggota menulis catatan sendiri di berkas terpisah (`docs/minggu-01-<nama>.md`).

### BREAK — Rusak dengan sengaja (30 menit)

Lakukan satu per satu, catat pesan errornya, lalu kembalikan:

| # | Yang dirusak | Prediksi Anda sebelum mencoba | Pesan error sebenarnya |
|---|--------------|-------------------------------|------------------------|
| 1 | Ganti nama `.env` menjadi `.env.bak` | | |
| 2 | Kosongkan nilai `APP_KEY` di `.env` | | |
| 3 | Ubah `DB_DATABASE` menjadi nama yang tidak ada | | |
| 4 | Ubah `APP_DEBUG=false`, lalu ulangi nomor 3 | | |

Nomor 4 adalah yang terpenting. Perhatikan bedanya: dengan `APP_DEBUG=true` Anda melihat seluruh isi konfigurasi dan jejak kode; dengan `false` Anda hanya melihat halaman 500 kosong. **Di server produksi nanti, `APP_DEBUG=true` berarti membocorkan kredensial database Anda kepada siapa pun yang memicu error.** Ini akan diuji di minggu 12.

### FIX — Perbaiki proyek yang cacat (30 menit)

Dosen menyediakan repo `kampuslms-broken`. Pindah ke branch `w01` — isinya proyek Laravel 12 yang tidak mau jalan. Ada **4 masalah**. Temukan dan perbaiki semuanya, lalu kirim Pull Request berisi penjelasan tiap perbaikan.

Petunjuk: masalahnya tersebar di berkas konfigurasi, dependensi, dan satu berkas yang seharusnya tidak ada di dalam repo.

### BUILD — Fondasi proyek kelompok (sisa waktu + tugas terstruktur)

1. Buat repo kelompok di dalam Organization mata kuliah. Nama: `kampuslms-kelompok-XX`.
2. Instal Laravel 12. Pastikan `php artisan serve` atau Herd berjalan.
3. Buat `README.md` berisi: nama proyek, daftar anggota + NIM, cara instalasi, dan tabel pembagian peran.
4. Pastikan `.env.example` lengkap dan `.env` **tidak** ter-commit. Verifikasi dengan `git status`.
5. Setiap anggota membuat minimal satu commit atas nama dan email masing-masing.
6. Aktifkan branch protection pada `main`.
7. Buat satu route baru `/tentang` yang menampilkan view berisi nama kelompok dan anggotanya.

---

## 1.4 Checkpoint Minggu 1

Jawab tanpa membuka catatan. Kalau ada yang tidak bisa dijawab, ulangi bagian READ.

- [ ] Sebutkan urutan berkas yang dilewati sebuah request dari browser sampai HTML kembali.
- [ ] Kenapa hanya folder `public/` yang boleh diakses dari internet? Apa yang terjadi kalau seluruh folder proyek diekspos?
- [ ] Apa beda `.env` dan `.env.example`, dan kenapa hanya satu yang di-commit?
- [ ] Di Laravel 12, di berkas mana middleware didaftarkan? Kenapa jawabannya berbeda dari kebanyakan tutorial di internet?
- [ ] Apa risiko konkret `APP_DEBUG=true` di server produksi?
- [ ] Tunjukkan di `git log` bahwa Anda punya commit atas nama Anda sendiri.

**Kuis 1** (auto-graded di LMS, 5 soal, akhir sesi): struktur proyek, alur request, peran `.env`, perbedaan frontend–backend.

---
---

# MINGGU 2 — Route, Controller, View: Menghubungkan Frontend dan Backend

**Sub-CPMK:** Mahasiswa mampu menjelaskan hubungan antara frontend, backend, client, server, dan alur komunikasi data dalam aplikasi web (C2), berpartisipasi aktif dalam diskusi mengenai arsitektur aplikasi (A2), serta menggunakan lingkungan pengembangan aplikasi web sebagai persiapan praktikum (P2).

**Target akhir minggu:** Kerangka halaman KampusLMS berdiri — layout, navigasi, dan halaman daftar mata kuliah dengan data statis.

---

## 2.1 Konsep

### Route: dari URL ke kode

Route adalah tabel pencocokan. Laravel membaca `routes/web.php` dari atas ke bawah dan memakai yang **pertama cocok**.

```php
Route::get('/courses', [CourseController::class, 'index'])->name('courses.index');
Route::get('/courses/{course}', [CourseController::class, 'show'])->name('courses.show');
```

Tiga hal yang harus Anda pahami di dua baris itu:

**Method HTTP itu bermakna.** `GET` untuk mengambil data (aman diulang, boleh di-bookmark), `POST` untuk membuat, `PUT/PATCH` untuk mengubah, `DELETE` untuk menghapus. Menghapus data lewat `GET` adalah kesalahan klasik — cukup satu crawler mesin pencari mengunjungi `/courses/5/delete` dan data Anda lenyap.

**Nama route lebih penting daripada kelihatannya.** Selalu pakai `route('courses.show', $course)` di view, jangan tulis `/courses/5`. Ketika URL berubah, Anda cukup mengedit satu baris di `web.php`, bukan mencari ke seluruh proyek.

**Urutan menentukan.** Kalau `/courses/{course}` ditulis sebelum `/courses/create`, maka `create` akan ditangkap sebagai `{course}` dan Laravel akan mencari mata kuliah bernama "create".

### Controller: penerima tamu, bukan koki

Controller yang sehat itu pendek. Tugasnya: menerima request, meminta data, menyerahkan ke view.

```php
class CourseController extends Controller
{
    public function index()
    {
        $courses = Course::all();          // minta data
        return view('courses.index', compact('courses'));   // serahkan ke view
    }
}
```

Kalau controller Anda mencapai 50 baris untuk satu method, ada logika yang seharusnya pindah ke model atau ke kelas tersendiri.

### Blade: template yang aman secara default

Blade adalah mesin template Laravel. Dua sintaks yang paling sering dipakai:

```blade
{{ $course->name }}      {{-- aman: HTML di-escape otomatis --}}
{!! $course->name !!}    {{-- BAHAYA: HTML dieksekusi mentah --}}
```

Perbedaan ini adalah pertahanan Anda terhadap **XSS (Cross-Site Scripting)**. Bayangkan seorang mahasiswa mendaftarkan namanya sebagai `<script>alert(document.cookie)</script>`. Dengan `{{ }}`, yang tampil adalah teks itu apa adanya. Dengan `{!! !!}`, skripnya benar-benar berjalan di browser semua orang yang membuka halaman tersebut.

Aturan: **jangan pernah pakai `{!! !!}`** kecuali Anda benar-benar tahu isinya aman dan bisa menjelaskan alasannya saat interview.

### Layout dan komponen

Jangan menyalin-tempel header dan navbar ke setiap halaman. Pakai **komponen Blade**. Sintaks lama `@extends`/`@section` masih berfungsi dan tidak salah, tetapi di mata kuliah ini kita mewajibkan komponen supaya satu gaya dipakai di seluruh proyek dan lebih mudah direview:

```
resources/views/
├── components/
│   └── layout.blade.php      ← kerangka bersama
├── courses/
│   ├── index.blade.php
│   └── show.blade.php
└── dashboard.blade.php
```

```blade
{{-- components/layout.blade.php --}}
<!DOCTYPE html>
<html lang="id">
<head>
    <title>{{ $title ?? 'KampusLMS' }}</title>
    @vite(['resources/css/app.css', 'resources/js/app.js'])
</head>
<body>
    <nav>...</nav>
    <main>{{ $slot }}</main>
</body>
</html>
```

```blade
{{-- courses/index.blade.php --}}
<x-layout title="Daftar Mata Kuliah">
    <h1>Daftar Mata Kuliah</h1>
    @foreach ($courses as $course)
        <a href="{{ route('courses.show', $course) }}">{{ $course->name }}</a>
    @endforeach
</x-layout>
```

`@vite` adalah penghubung ke Vite, pengelola aset di Laravel 12. Saat pengembangan jalankan `npm run dev`; saat deploy jalankan `npm run build`. Kalau CSS Anda tiba-tiba hilang, periksa dua perintah ini lebih dulu.

### Request dan Response

Data dari pengguna masuk lewat objek `Request`:

```php
public function index(Request $request)
{
    $keyword = $request->query('q');     // dari ?q=basis+data
    // ...
}
```

Ingat prinsip minggu 1: **semua yang datang dari `Request` adalah masukan yang tidak dipercaya.** Minggu 4 kita akan memvalidasinya secara serius.

---

## 2.2 Prompt Pack — Minggu 2

### A. Prompt Perancangan — sebelum menulis kode

```
Konteks: saya membangun LMS kampus dengan Laravel 12. Peran pengguna:
admin, dosen, mahasiswa. Modul yang akan ada: mata kuliah, materi,
tugas, pengumpulan, nilai, notifikasi.

Tolong rancangkan struktur route web (bukan API) untuk modul mata kuliah
saja, mengikuti konvensi RESTful Laravel.

Untuk setiap route sebutkan: method HTTP, URI, nama route, controller
dan method-nya, serta siapa yang boleh mengaksesnya.

Sajikan dalam bentuk tabel. Jangan tulis kode implementasinya dulu —
saya ingin menyetujui rancangannya lebih dahulu.
```

Menyuruh AI merancang sebelum mengimplementasi adalah kebiasaan yang membedakan pengembang berpengalaman. Kalau rancangannya salah, kode sebanyak apa pun tidak akan menyelamatkan.

### B. Prompt Implementasi — dengan batasan eksplisit

```
Berdasarkan tabel route yang tadi kita sepakati, buatkan:
1. routes/web.php untuk modul mata kuliah
2. CourseController dengan method index dan show (data masih statis
   berupa array, belum pakai database)
3. Layout Blade sebagai komponen (x-layout) + view index dan show

Batasan:
- Laravel 12. Komponen Blade, bukan @extends/@section.
- Semua tautan memakai helper route(), tidak boleh URL hardcode.
- Semua keluaran variabel memakai {{ }}, tidak boleh {!! !!}.
- Beri komentar singkat di setiap bagian yang menjelaskan APA fungsinya
  dan KENAPA ditulis begitu.
```

### C. Prompt Review — jalankan sebelum membuat Pull Request

```
Berikut kode route, controller, dan view saya:

[tempel kode]

Berperanlah sebagai reviewer senior yang teliti. Periksa:
1. Apakah ada URL yang di-hardcode padahal seharusnya pakai route()?
2. Apakah urutan route berpotensi saling menutupi?
3. Apakah ada method HTTP yang dipakai tidak semestinya?
4. Apakah ada potensi XSS?
5. Apakah ada logika di view yang seharusnya di controller?

Untuk setiap temuan: sebutkan barisnya, jelaskan risikonya, beri
perbaikannya. Kalau tidak ada temuan pada suatu poin, katakan saja.
Jangan mengarang masalah agar terlihat teliti.
```

Kalimat terakhir penting. AI cenderung "membantu" dengan menemukan masalah meski tidak ada.

---

## 2.3 Read → Break → Fix → Build

### READ — Telusuri satu request penuh (30 menit)

Ambil route `/tentang` yang Anda buat minggu lalu. Tanpa AI, tulis di catatan Anda:

1. Baris mana di `routes/web.php` yang menangkapnya?
2. Kalau ditangani controller, berkas dan method mana?
3. View mana yang dikembalikan? Di path apa persisnya?
4. Layout apa yang membungkusnya?
5. Jalankan `php artisan route:list --path=tentang`. Cocok dengan analisis Anda?

### BREAK — Delapan kerusakan (40 menit)

Lakukan berurutan. Untuk setiap nomor, **tulis prediksi Anda dulu** sebelum menjalankan.

| # | Yang dirusak | Yang Anda pelajari |
|---|--------------|--------------------|
| 1 | Ubah `Route::get` menjadi `Route::post` pada route daftar mata kuliah | Method HTTP tidak cocok → 405 |
| 2 | Ubah nama view di `return view(...)` menjadi yang tidak ada | Exception view not found |
| 3 | Hapus `->name('courses.show')`, lalu muat halaman yang memakai `route('courses.show')` | Kenapa nama route wajib |
| 4 | Pindahkan `/courses/{course}` ke ATAS `/courses/create`, lalu buka `/courses/create` | Urutan route menentukan |
| 5 | Ganti `{{ $nama }}` menjadi `{!! $nama !!}`, isi `$nama` dengan `<script>alert('XSS')</script>` | **XSS nyata di layar Anda sendiri** |
| 6 | Hapus `@vite(...)` dari layout | Aset tidak termuat |
| 7 | Hentikan `npm run dev` lalu muat ulang halaman | Beda dev server vs build |
| 8 | Panggil `route('courses.show')` tanpa mengirim parameter | Missing required parameter |

Nomor 5 wajib benar-benar dicoba, bukan dibayangkan. Melihat `alert` muncul dari data yang Anda "masukkan sebagai pengguna" mengubah cara Anda memandang setiap keluaran di layar selamanya.

### FIX — Repo cacat (30 menit)

Branch `w02` pada repo `kampuslms-broken` berisi modul mata kuliah dengan **6 masalah**: satu route saling menutupi, satu method HTTP salah, dua URL hardcode, satu XSS, dan satu logika query yang seharusnya tidak berada di view.

Temukan semuanya, perbaiki, kirim PR. **Deskripsi PR wajib menjelaskan risiko dari tiap masalah**, bukan sekadar "sudah diperbaiki".

### BUILD — Kerangka KampusLMS (sisa waktu)

Masih memakai data statis (array di controller) — database baru masuk minggu depan.

1. Layout `x-layout` dengan navbar berisi: Dashboard, Mata Kuliah, Tentang.
2. `CourseController` dengan `index` dan `show`.
3. `courses/index.blade.php` — tabel daftar mata kuliah (kode, nama, SKS, dosen).
4. `courses/show.blade.php` — detail satu mata kuliah.
5. Semua tautan memakai `route()`.
6. Halaman 404 kustom (`resources/views/errors/404.blade.php`).
7. Minimal 2 anggota berbeda membuat commit; PR di-review anggota lain sebelum merge.

---

## 2.4 Checkpoint Minggu 2

- [ ] Kenapa menghapus data lewat `GET` berbahaya? Beri satu skenario konkret.
- [ ] Apa yang terjadi kalau `/courses/{course}` ditulis sebelum `/courses/create`? Kenapa?
- [ ] Tunjukkan di kode Anda satu tempat yang memakai `route()`. Apa untungnya dibanding URL hardcode?
- [ ] Apa beda `{{ }}` dan `{!! !!}`? Peragakan XSS yang Anda buat di bagian BREAK.
- [ ] Apa fungsi `@vite`? Apa beda `npm run dev` dan `npm run build`?
- [ ] Jelaskan mengapa data dari `Request` tidak boleh dipercaya.

**Kuis 2:** method HTTP, penamaan route, urutan route, XSS, komponen Blade.

---
---

# MINGGU 3 — Database dan CRUD

**Sub-CPMK:** Mahasiswa mampu menerapkan operasi CRUD pada basis data (C3), menunjukkan ketelitian dalam pengelolaan data (A3), serta membangun fitur CRUD pada aplikasi web sesuai kebutuhan sistem (P3).

**Target akhir minggu:** Seluruh skema KampusLMS berdiri lewat migrasi, terisi seeder, dan modul mata kuliah + pengguna sudah CRUD penuh.

> ⚠️ **Minggu ini adalah Milestone M1 — Tugas 1 (3%) + interview kelompok.**

---

## 3.1 Konsep

### Migrasi: riwayat versi untuk struktur database

Migrasi adalah kode yang mendeskripsikan perubahan struktur database. Kenapa tidak langsung pakai phpMyAdmin?

Karena Anda bekerja **berempat**. Kalau setiap orang mengubah tabel lewat GUI di komputernya sendiri, dalam dua minggu tidak akan ada dua database yang identik, dan tidak ada yang tahu perubahan mana yang benar. Dengan migrasi, struktur database ikut masuk Git bersama kode — siapa pun bisa menjalankan `php artisan migrate:fresh --seed` dan mendapatkan database yang persis sama.

```php
public function up(): void
{
    Schema::create('courses', function (Blueprint $table) {
        $table->id();
        $table->string('code')->unique();
        $table->string('name');
        $table->text('description')->nullable();
        $table->unsignedTinyInteger('sks');
        $table->foreignId('lecturer_id')->constrained('users')->restrictOnDelete();
        $table->enum('status', ['draft', 'active', 'archived'])->default('draft');
        $table->timestamps();
    });
}

public function down(): void
{
    Schema::dropIfExists('courses');
}
```

**`down()` wajib benar-benar mengembalikan keadaan.** Migrasi yang `up()`-nya menambah kolom tapi `down()`-nya kosong adalah migrasi rusak, dan itu akan ketahuan saat CI menjalankan `migrate:refresh`.

### Foreign key dan perilaku penghapusan

Ini keputusan desain, bukan formalitas:

| Perilaku | Artinya | Kapan dipakai di KampusLMS |
|----------|---------|----------------------------|
| `cascadeOnDelete()` | Hapus induk → anak ikut terhapus | Mata kuliah dihapus → materi & tugasnya ikut. Memang tidak berguna lagi. |
| `restrictOnDelete()` | Tolak penghapusan selama masih ada anak | Dosen tidak boleh dihapus selama masih mengampu mata kuliah. |
| `nullOnDelete()` | Hapus induk → kolom anak jadi NULL | Jarang dipakai di proyek ini. |

Pertanyaan interview: *"Kenapa `lecturer_id` memakai `restrictOnDelete` sementara `course_id` pada `materials` memakai `cascadeOnDelete`?"*

### Unique composite: aturan bisnis yang ditegakkan database

Di spesifikasi proyek ada dua constraint yang mudah diabaikan:

```php
$table->unique(['course_id', 'user_id']);        // di course_user
$table->unique(['assignment_id', 'user_id']);    // di submissions
```

Yang pertama mencegah satu mahasiswa terdaftar dua kali di mata kuliah yang sama. Yang kedua mencegah satu mahasiswa punya dua submission untuk tugas yang sama.

Anda mungkin berpikir "kan sudah dicek di controller". Tapi pengecekan di controller bisa gagal saat dua request datang bersamaan (*race condition*) — mahasiswa yang klik tombol dua kali dengan cepat bisa lolos. Database tidak bisa ditipu seperti itu. **Aturan bisnis yang kritis ditegakkan di dua lapis: aplikasi untuk pesan error yang ramah, database untuk jaminan.**

### Eloquent dan relasi

```php
class Course extends Model
{
    protected $fillable = ['code', 'name', 'description', 'sks', 'lecturer_id', 'status'];

    public function lecturer(): BelongsTo
    {
        return $this->belongsTo(User::class, 'lecturer_id');
    }

    public function students(): BelongsToMany
    {
        return $this->belongsToMany(User::class)->withTimestamps();
    }

    public function assignments(): HasMany
    {
        return $this->hasMany(Assignment::class);
    }
}
```

### `$fillable` dan mass assignment — kerentanan yang akan Anda uji sendiri

```php
Course::create($request->all());
```

Baris di atas terlihat ringkas dan sering muncul di tutorial. Ia juga berbahaya.

`$request->all()` berisi **semua** yang dikirim pengguna — termasuk field yang tidak ada di formulir Anda. Kalau model `User` punya kolom `role` dan `$fillable`-nya longgar, seorang mahasiswa cukup menambahkan `role=admin` ke request pendaftaran, dan ia menjadi admin.

Pertahanannya:
- `$fillable` berisi **hanya** kolom yang memang boleh diisi pengguna. Kolom seperti `role`, `is_verified`, `lecturer_id` sebaiknya tidak masuk, atau diisi eksplisit di controller.
- Jangan pakai `$request->all()`. Pakai `$request->validated()` (mulai minggu 4) atau `$request->only([...])`.
- **Jangan pernah** memakai `protected $guarded = [];` — itu mematikan perlindungan sepenuhnya.

Ini akan Anda praktikkan langsung di bagian BREAK, dan akan ditanyakan saat interview.

### Casting di Laravel 12

Di Laravel 11+, casting ditulis sebagai method, bukan properti:

```php
protected function casts(): array
{
    return [
        'due_at' => 'datetime',
        'password' => 'hashed',
    ];
}
```

Kalau AI memberi Anda `protected $casts = [...]` sebagai properti, itu gaya Laravel 10. Masih jalan, tapi bukan konvensi Laravel 12.

### Factory dan Seeder

Factory adalah pabrik data palsu; seeder yang menjalankannya.

```php
// database/factories/CourseFactory.php
public function definition(): array
{
    return [
        'code' => 'SI' . fake()->unique()->numerify('#######'),
        'name' => fake()->randomElement(['Basis Data', 'Pemrograman Web', 'Jaringan Komputer']),
        'sks' => fake()->numberBetween(2, 4),
        'status' => 'active',
    ];
}
```

Ingat ketentuan seeder di spesifikasi proyek: **1 admin, 3 dosen, 30 mahasiswa, 5 mata kuliah, ≥100 submission.** Volume ini disengaja — tanpa data sebanyak itu, masalah performa di minggu 11 tidak akan terlihat.

---

## 3.2 Prompt Pack — Minggu 3

### A. Prompt Migrasi — dengan spesifikasi sebagai konteks

```
Saya membangun LMS dengan Laravel 12. Berikut skema database yang sudah
ditetapkan dosen dan TIDAK BOLEH diubah:

[tempel Bagian 4 dari spesifikasi proyek]

Buatkan berkas migrasi untuk seluruh tabel tersebut.

Ketentuan:
- Laravel 12.
- Urutan berkas migrasi harus benar: tabel yang dirujuk foreign key dibuat
  lebih dulu.
- Terapkan persis perilaku onDelete yang tertulis di spesifikasi.
- Sertakan seluruh unique composite yang disebutkan.
- down() harus benar-benar membalik up().
- Setelah selesai, buat daftar periksa agar saya bisa memverifikasi
  sendiri bahwa hasilnya sesuai spesifikasi.
```

### B. Prompt Model & Relasi

```
Berdasarkan migrasi tersebut, buatkan model Eloquent untuk Laravel 12.

Ketentuan:
- Definisikan seluruh relasi sesuai Bagian 4.3 spesifikasi, lengkap dengan
  return type (BelongsTo, HasMany, BelongsToMany, HasOne).
- $fillable HANYA berisi kolom yang aman diisi langsung dari input
  pengguna. Untuk setiap kolom yang kamu KELUARKAN dari $fillable,
  jelaskan alasannya.
- Casting ditulis sebagai method casts(), bukan properti $casts.
- Jangan gunakan $guarded.
```

### C. Prompt Audit Keamanan — jalankan setelah CRUD jadi

```
Berikut controller CRUD saya:

[tempel kode]

Audit khusus mass assignment:
1. Apakah ada pemakaian $request->all() atau $request->input() tanpa
   penyaringan?
2. Untuk setiap model yang disentuh, adakah kolom di $fillable yang
   seharusnya tidak boleh diisi pengguna?
3. Tunjukkan secara konkret: sebagai penyerang, field apa yang bisa saya
   selipkan ke request untuk mendapat hak akses yang tidak seharusnya?
   Beri contoh payload-nya.

Untuk setiap temuan, jelaskan dampaknya dan beri perbaikannya.
```

Poin 3 sengaja meminta AI berpikir sebagai penyerang. Ini cara tercepat menemukan lubang di kode Anda sendiri.

### D. Prompt Seeder Realistis

```
Buatkan factory dan seeder untuk Laravel 12 yang menghasilkan data
sesuai ketentuan berikut:

[tempel Bagian 4.4 spesifikasi]

Ketentuan tambahan:
- Data harus konsisten secara logis: submission hanya boleh dari
  mahasiswa yang terdaftar di mata kuliah tugas tersebut.
- Sebagian submission ditandai terlambat (submitted_at > due_at).
- Gunakan bahasa Indonesia untuk nama orang dan nama mata kuliah.
- Seeder harus idempoten: aman dijalankan ulang setelah migrate:fresh.
```

---

## 3.3 Read → Break → Fix → Build

### READ — Baca skema sebelum menulisnya (30 menit)

Sebelum menyentuh kode, kerjakan bersama kelompok:

1. Gambar ulang ERD dari spesifikasi di papan/kertas, tanpa melihat dokumen.
2. Untuk setiap foreign key, tentukan perilaku `onDelete`-nya dan **tuliskan alasannya**.
3. Jawab: kalau seorang dosen dihapus, apa yang terjadi pada mata kuliahnya? Kenapa dirancang begitu?
4. Jawab: kenapa `grades.submission_id` bersifat unique, bukan sekadar index biasa?

### BREAK — Lima kerusakan (45 menit)

| # | Yang dicoba | Yang harus Anda amati |
|---|-------------|------------------------|
| 1 | Hapus `unique(['course_id','user_id'])` dari `course_user`, lalu daftarkan mahasiswa yang sama dua kali | Data ganda lolos tanpa keluhan |
| 2 | Tambahkan `role` ke `$fillable` model `User`, lalu kirim request pembuatan user dengan `role=admin` lewat form yang **tidak punya field role** | **Mass assignment nyata** — Anda baru saja jadi admin |
| 3 | Ganti seluruh `$fillable` dengan `protected $guarded = [];` lalu ulangi nomor 2 | Kenapa `$guarded` kosong dilarang |
| 4 | Kosongkan isi `down()` di satu migrasi, lalu jalankan `php artisan migrate:refresh` | Migrasi tidak reversible = CI merah |
| 5 | Ubah `restrictOnDelete` pada `lecturer_id` menjadi `cascadeOnDelete`, lalu hapus satu dosen | Kehilangan data berantai |

Nomor 2 wajib benar-benar dilakukan, bukan sekadar dibayangkan. Anda perlu melihat sendiri bahwa **formulir di frontend bukan pembatas apa pun**.

Cara paling mudah minggu ini adalah lewat Tinker, karena ia menirukan persis apa yang dikerjakan controller Anda:

```bash
php artisan tinker
```
```php
// persis seperti Course::create($request->all()) di controller
App\Models\User::create([
    'name' => 'Penyusup',
    'email' => 'x@x.test',
    'password' => 'rahasia123',
    'role' => 'admin',          // ← field ini TIDAK ADA di formulir Anda
]);

App\Models\User::where('email','x@x.test')->value('role');   // 'admin' → Anda baru saja jadi admin
```

Kalau ingin membuktikannya lewat HTTP sungguhan, ingat satu hal: **request POST tanpa token CSRF ditolak dengan 419 sebelum sampai ke controller**, jadi `curl` polos tidak akan memperagakan apa pun. Anda perlu membawa cookie session dan token:

```bash
# 1. ambil halaman form, simpan cookie, dan cungkil token CSRF-nya
TOKEN=$(curl -s -c cookies.txt http://kampuslms.test/users/create \
  | grep -o 'name="_token" value="[^"]*"' | cut -d'"' -f4)

# 2. kirim request dengan field liar yang tidak ada di formulir
curl -X POST http://kampuslms.test/users -b cookies.txt \
  -d "_token=$TOKEN" \
  -d "name=Penyusup" -d "email=x@x.test" -d "password=rahasia123" \
  -d "role=admin"
```

Perhatikan bahwa token CSRF **tidak menghentikan serangan ini sama sekali** — ia hanya memastikan request berasal dari halaman Anda sendiri. Yang menahan mass assignment adalah `$fillable`, bukan CSRF. Dua mekanisme berbeda untuk dua masalah berbeda; jangan tertukar.

Kembalikan semuanya setelah selesai, lalu catat temuan Anda di `docs/minggu-03-<nama>.md`.

### FIX — Repo cacat (30 menit)

Branch `w03` pada repo `kampuslms-broken` berisi migrasi dan model dengan **7 masalah**: urutan migrasi salah, dua unique composite hilang, satu `onDelete` keliru, satu `$guarded = []`, satu `down()` kosong, dan satu controller yang memakai `$request->all()`.

Perbaiki, kirim PR, dan **jelaskan dampak nyata tiap masalah** di deskripsi PR — bukan sekadar menyebut apa yang diubah.

### BUILD — Milestone M1 (tugas terstruktur + belajar mandiri)

Deliverable yang dinilai sebagai **Tugas 1**:

1. **Seluruh migrasi** sesuai Bagian 4 spesifikasi, termasuk semua constraint dan index. Reversible.
2. **Seluruh model** dengan relasi lengkap sesuai Bagian 4.3, `$fillable` yang ketat, `casts()` sebagai method.
3. **Factory + seeder** yang memenuhi Bagian 4.4, termasuk 3 akun demo.
4. **CRUD Mata Kuliah** berfungsi penuh (index, create, store, show, edit, update, destroy).
5. **CRUD Pengguna** berfungsi penuh, dengan `role` **tidak** di `$fillable` melainkan diisi eksplisit di controller.
6. **CI GitHub Actions aktif dan hijau**: `migrate:fresh --seed` sukses, `migrate:refresh` sukses, `.env` tidak ter-commit.
7. Setiap anggota punya commit atas namanya sendiri, dan setiap fitur masuk lewat PR yang direview anggota lain.

**Belum diperlukan minggu ini:** login, validasi mendalam, pembatasan hak akses. Itu minggu 4–7. Fokus sekarang di struktur data yang benar.

---

## 3.4 Checkpoint Minggu 3 — Gerbang Interview Tugas 1

Setiap anggota akan ditanya secara individu, dengan kode kelompok terbuka di layar.

- [ ] Tunjukkan migrasi yang **Anda** tulis. Jelaskan setiap constraint di dalamnya.
- [ ] Kenapa `course_user` punya unique composite? Peragakan apa yang terjadi kalau dihapus.
- [ ] Apa itu mass assignment? Tunjukkan di kode Anda apa yang mencegahnya, lalu peragakan serangannya dengan `curl`.
- [ ] Kenapa `role` tidak boleh ada di `$fillable`? Di mana ia diisi sebagai gantinya?
- [ ] Kenapa `lecturer_id` memakai `restrictOnDelete` sementara `materials.course_id` memakai `cascadeOnDelete`?
- [ ] Jalankan `php artisan migrate:refresh` di depan penguji. Harus berhasil tanpa error.
- [ ] Tunjukkan satu bagian kode yang Anda tulis dengan bantuan AI. Apa yang Anda ubah dari keluaran aslinya, dan kenapa?

Pertanyaan terakhir bukan jebakan. Jawaban "saya pakai AI lalu saya ubah bagian X karena Y" adalah jawaban yang **baik** dan menunjukkan Anda memverifikasi. Jawaban yang bermasalah adalah "saya tidak tahu, itu dari AI".

**Kuis 3:** migrasi, relasi Eloquent, foreign key constraint, mass assignment.

---

## Rubrik Ringkas Tugas 1

| Kriteria | Bobot | Yang dilihat |
|----------|-------|--------------|
| Fungsionalitas | 35% | CRUD mata kuliah & pengguna berjalan; seeder menghasilkan data sesuai ketentuan |
| Implementasi Konsep | 30% | Skema persis sesuai spesifikasi; constraint lengkap; relasi benar; `$fillable` ketat; migrasi reversible |
| Interview/Pemahaman | 25% | Mampu menjelaskan tiap keputusan desain dan memperagakan mass assignment |
| Kualitas Kode | 10% | Controller tipis; penamaan konsisten; commit message rapi; PR direview |

Mengacu pada rubrik RPS. Kode yang berjalan sempurna tetapi tidak bisa dijelaskan penulisnya dinilai **0 pada komponen Interview**, dan itu 25% dari nilai tugas.

---

*Modul Minggu 4–16 menyusul dengan format yang sama.*
