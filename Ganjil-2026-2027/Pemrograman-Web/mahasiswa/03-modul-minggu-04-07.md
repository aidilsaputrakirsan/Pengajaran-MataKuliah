# MODUL PEMROGRAMAN WEB — MINGGU 4–7

**SI2514024 | Proyek: KampusLMS | Laravel 12**

> Lanjutan dari Modul Minggu 1–3. Format tetap sama: **Konsep → Prompt Pack → Read-Break-Fix-Build → Checkpoint**.
>
> Empat minggu ini adalah bagian tersulit dari semester. Minggu 3 Anda membangun *tempat menyimpan data*. Minggu 4–7 Anda membangun *aturan siapa boleh menyentuh apa*. Sebagian besar kerentanan aplikasi web nyata berasal dari kesalahan di rentang minggu ini.

---
---

# MINGGU 4 — Pengelolaan State: Session, Validasi, dan Data yang Banyak

**Sub-CPMK:** Mahasiswa mampu menerapkan pengelolaan state pada aplikasi web (C3), menunjukkan ketelitian dalam menjaga konsistensi data antar permintaan (A3), serta membangun fitur yang mempertahankan state pengguna (P3).

**Target akhir minggu:** Form KampusLMS tervalidasi di server, daftar mata kuliah bisa dicari dan difilter dengan state yang bertahan, flash message berjalan.

---

## 4.1 Konsep

### Masalah mendasar: HTTP tidak punya ingatan

HTTP bersifat *stateless*. Setiap request adalah perkenalan ulang — server tidak tahu bahwa request yang barusan datang dari orang yang sama dengan request sebelumnya.

Padahal aplikasi butuh ingatan: siapa yang sedang login, apa isi keranjang, filter apa yang sedang aktif, pesan sukses apa yang harus ditampilkan setelah redirect. Semua itu adalah **state**.

Laravel menyediakan beberapa tempat menyimpan state, dan **memilih tempat yang salah adalah sumber bug yang sulit dilacak**:

| Tempat | Umur | Cocok untuk | Bahaya |
|--------|------|-------------|--------|
| **Query string** (`?q=basis`) | Selama URL itu | Filter, pencarian, halaman | Terlihat pengguna, bisa diubah |
| **Session** | Selama browser terbuka | Identitas login, data wizard multi-langkah | Boros kalau diisi data besar |
| **Flash session** | Satu request berikutnya | Pesan sukses/gagal setelah redirect | Hilang kalau tidak segera dibaca |
| **Cookie** | Sesuai kadaluarsa | Preferensi tampilan | Sepenuhnya di tangan pengguna |
| **Database** | Permanen | Apa pun yang harus bertahan | — |

Aturan praktisnya: **filter dan pencarian ke query string, jangan ke session.** Kalau filter disimpan di session, pengguna tidak bisa membagikan tautan hasil pencariannya, tombol *back* jadi kacau, dan membuka dua tab akan saling mengganggu.

### Pola PRG: Post → Redirect → Get

Setelah `POST` berhasil, **jangan** kembalikan view langsung. Selalu `redirect()`.

```php
public function store(StoreCourseRequest $request)
{
    $course = Course::create($request->validated());

    return redirect()
        ->route('courses.show', $course)
        ->with('success', 'Mata kuliah berhasil ditambahkan.');
}
```

Kenapa? Kalau Anda `return view()` setelah POST, lalu pengguna menekan F5, browser akan mengirim ulang POST-nya — dan data tersimpan dua kali. Anda pasti pernah melihat peringatan "Confirm Form Resubmission" di browser. Itu gejala aplikasi yang tidak memakai PRG.

`->with('success', ...)` menaruh pesan di **flash session**: tersedia tepat satu request berikutnya, lalu hilang sendiri.

```blade
@if (session('success'))
    <div class="alert alert-success">{{ session('success') }}</div>
@endif
```

### Validasi: pertahanan yang sesungguhnya

Ingat prinsip minggu 1: frontend tidak bisa dipercaya. `required` di HTML dan pengecekan JavaScript adalah **kenyamanan pengguna**, bukan keamanan. Keduanya bisa dilewati dalam sepuluh detik lewat DevTools atau `curl`.

Cara yang benar di Laravel 12 adalah **Form Request** — kelas tersendiri, bukan validasi berjejal di controller:

```bash
php artisan make:request StoreCourseRequest
```

```php
class StoreCourseRequest extends FormRequest
{
    public function authorize(): bool
    {
        return true;   // minggu 7 diganti dengan pengecekan hak akses sungguhan
    }

    public function rules(): array
    {
        return [
            'code'        => ['required', 'string', 'max:20', 'unique:courses,code'],
            'name'        => ['required', 'string', 'max:150'],
            'description' => ['nullable', 'string'],
            'sks'         => ['required', 'integer', 'between:1,6'],
            'lecturer_id' => ['required', 'exists:users,id'],
            'status'      => ['required', 'in:draft,active,archived'],
        ];
    }

    public function messages(): array
    {
        return [
            'code.unique' => 'Kode mata kuliah ini sudah dipakai.',
            'sks.between' => 'SKS harus antara 1 sampai 6.',
        ];
    }
}
```

Keuntungan Form Request bukan sekadar kerapian:
- `$request->validated()` mengembalikan **hanya** field yang lolos aturan — ini penawar mass assignment dari minggu 3.
- Kalau validasi gagal, Laravel otomatis me-redirect kembali dengan error dan input lama. Anda tidak menulis satu baris pun untuk itu.
- Aturan validasi jadi bisa diuji terpisah (minggu 14).

Dua aturan yang wajib Anda pahami betul:
- `exists:users,id` memastikan nilai yang dikirim benar-benar ada di database. Tanpa ini, pengguna bisa mengirim `lecturer_id=99999` dan membuat data yatim.
- `in:draft,active,archived` mengunci nilai enum. Tanpa ini, pengguna bisa mengirim status apa pun.

### Menampilkan error dan mempertahankan input

```blade
<input type="text" name="code" value="{{ old('code', $course->code ?? '') }}">
@error('code')
    <p class="text-red-600">{{ $message }}</p>
@enderror
```

`old()` mengambil input dari request sebelumnya yang gagal. Tanpa ini, pengguna yang salah mengisi satu field harus mengetik ulang seluruh formulir — keluhan nomor satu pada aplikasi kampus.

### CSRF: kenapa ada `@csrf` di setiap form

```blade
<form method="POST" action="{{ route('courses.store') }}">
    @csrf
    ...
</form>
```

Tanpa token CSRF, situs jahat bisa memasang formulir tersembunyi yang mengirim POST ke aplikasi Anda memakai session korban yang sedang login. Korban cukup membuka satu halaman, dan tanpa sadar menghapus mata kuliah. Laravel menolak POST tanpa token yang cocok — itu sebabnya lupa `@csrf` menghasilkan error 419.

### Pagination, pencarian, dan filter

```php
public function index(Request $request)
{
    $courses = Course::query()
        ->with('lecturer')
        ->when($request->filled('q'), fn ($query) =>
            $query->where('name', 'like', '%' . $request->q . '%')
                  ->orWhere('code', 'like', '%' . $request->q . '%'))
        ->when($request->filled('status'), fn ($query) =>
            $query->where('status', $request->status))
        ->latest()
        ->paginate(15)
        ->withQueryString();

    return view('courses.index', compact('courses'));
}
```

Tiga detail yang sering terlewat:

- **`->withQueryString()`** membuat tautan halaman 2 tetap membawa filter yang aktif. Tanpa ini, pengguna mencari "basis data", klik halaman 2, dan filternya hilang. Bug klasik.
- **`->with('lecturer')`** adalah *eager loading*. Tanpa ini, 15 baris berarti 16 query. Minggu 11 kita bahas tuntas, tapi kebiasaannya dimulai sekarang.
- **`Course::query()`** sebagai pembuka membuat rantai `when()` terbaca rapi dan menghindari kondisi bersarang.

---

## 4.2 Prompt Pack — Minggu 4

### A. Prompt Perancangan Validasi

```
Konteks: LMS Laravel 12. Saya akan membuat form tambah/edit Mata Kuliah.
Skema tabel courses: id, code (unique), name, description (nullable),
sks (1-6), lecturer_id (FK ke users), status (draft/active/archived).

Rancangkan aturan validasi untuk operasi STORE dan UPDATE.

Ketentuan:
- Sajikan sebagai tabel: field, aturan, alasan tiap aturan.
- Jelaskan perbedaan aturan unique antara store dan update, dan kenapa
  update butuh perlakuan berbeda.
- Belum perlu kode. Saya ingin menyetujui aturannya dulu.
```

Aturan `unique` pada update adalah jebakan paling umum: kalau ditulis polos, mengedit mata kuliah tanpa mengubah kodenya akan gagal karena kodenya "sudah dipakai" — oleh dirinya sendiri. Biarkan mahasiswa menemukan ini lewat prompt, bukan lewat frustrasi.

### B. Prompt Implementasi Form Request

```
Buatkan Form Request untuk Laravel 12 berdasarkan tabel aturan yang
tadi kita sepakati: StoreCourseRequest dan UpdateCourseRequest.

Ketentuan:
- Laravel 12.
- Sertakan messages() dalam bahasa Indonesia yang ramah dan spesifik.
- Pada UpdateCourseRequest, tangani aturan unique dengan benar
  menggunakan Rule::unique()->ignore().
- Method authorize() untuk sementara return true, beri komentar TODO
  bahwa ini akan diganti dengan Policy di minggu 7.
- Tunjukkan juga bagaimana controller memanggilnya dan kenapa harus
  memakai $request->validated(), bukan $request->all().
```

### C. Prompt Audit State

```
Berikut controller index saya yang menangani pencarian, filter,
dan pagination:

[tempel kode]

Periksa hal-hal berikut:
1. Apakah filter tetap bertahan saat pengguna berpindah halaman?
2. Apakah state disimpan di tempat yang tepat (query string vs session)?
   Kalau ada yang di session, jelaskan masalah apa yang timbul saat
   pengguna membuka dua tab.
3. Apakah ada N+1 query?
4. Apakah input pencarian berpotensi SQL injection?
5. Apakah pola PRG diterapkan pada method store dan update?

Untuk tiap temuan sebutkan barisnya, risikonya, dan perbaikannya.
Kalau suatu poin sudah benar, katakan sudah benar — jangan mengarang
masalah.
```

### D. Prompt Uji Tembus Validasi

```
Berikut Form Request saya:

[tempel kode]

Berperanlah sebagai penyerang. Susun 5 payload request yang mencoba
melewati validasi ini, misalnya:
- mengirim field yang tidak ada di formulir
- mengirim tipe data yang tidak diharapkan (array padahal string)
- mengirim nilai enum di luar daftar
- mengirim foreign key yang tidak ada

Untuk setiap payload, jelaskan apakah validasi saya menahannya atau
tidak, dan kalau tidak, aturan apa yang harus saya tambahkan.
Sertakan perintah curl agar saya bisa mencobanya sendiri.
```

---

## 4.3 Read → Break → Fix → Build

### READ — Telusuri satu siklus form gagal (30 menit)

Tanpa AI. Buat form tambah mata kuliah, isi dengan data yang pasti tidak valid (SKS = 99), kirim, lalu jawab:

1. Method apa yang menerima request? Di controller mana?
2. Di titik mana persisnya validasi terjadi — sebelum atau sesudah baris pertama method controller?
3. Ke mana Laravel me-redirect setelah gagal? Siapa yang menentukan tujuannya?
4. Dari mana `@error('sks')` mengambil pesannya?
5. Dari mana `old('sks')` mengambil nilainya? Berapa lama nilai itu bertahan?
6. Buka DevTools → Application → Cookies. Temukan cookie session Laravel. Catat namanya.

### BREAK — Tujuh kerusakan (45 menit)

Tulis prediksi lebih dulu, baru jalankan.

| # | Yang dirusak | Yang harus Anda amati |
|---|--------------|------------------------|
| 1 | Hapus `@csrf` dari form, lalu kirim | Error 419 — dan renungkan apa yang dicegahnya |
| 2 | Ganti `$request->validated()` menjadi `$request->all()`, lalu kirim field liar lewat `curl` | Mass assignment kembali terbuka |
| 3 | Hapus validasi `exists:users,id` pada `lecturer_id`, kirim `lecturer_id=99999` | Data yatim masuk database |
| 4 | Hapus validasi `in:...` pada `status`, kirim `status=superadmin` | Enum jebol |
| 5 | Hapus `->withQueryString()`, lakukan pencarian lalu klik halaman 2 | Filter hilang — bug klasik |
| 6 | Ganti `return redirect()` menjadi `return view()` pada `store`, lalu tekan F5 setelah simpan | Data ganda; ini alasan PRG ada |
| 7 | Hapus `old(...)` dari semua input, lalu kirim form dengan satu kesalahan | Rasakan sendiri sebagai pengguna |

Nomor 2 dan 3 wajib dicoba lewat `curl`, bukan lewat browser:

```bash
curl -X POST http://kampuslms.test/courses \
  -H "X-CSRF-TOKEN: <ambil dari halaman>" \
  -b cookies.txt \
  -d "code=XX01" -d "name=Uji" -d "sks=3" \
  -d "lecturer_id=99999" -d "status=superadmin"
```

Catat hasilnya di `docs/minggu-04-<nama>.md`.

### FIX — Repo cacat (30 menit)

Branch `w04` pada repo `kampuslms-broken` berisi modul mata kuliah dengan **6 masalah**: validasi hanya di frontend, `unique` pada update yang menolak dirinya sendiri, filter disimpan di session, pagination kehilangan query string, `store` tanpa redirect, dan satu form tanpa `@csrf`.

Perbaiki, kirim PR, jelaskan dampak tiap masalah dari sudut pandang pengguna **dan** dari sudut pandang penyerang.

### BUILD — Form dan daftar yang layak pakai

1. `StoreCourseRequest` dan `UpdateCourseRequest` lengkap dengan pesan bahasa Indonesia.
2. Form tambah/edit mata kuliah dengan `old()`, `@error`, dan `@csrf`.
3. Flash message sukses/gagal di layout, sehingga berlaku untuk seluruh halaman.
4. Daftar mata kuliah dengan pencarian (kode/nama), filter status, dan pagination 15 per halaman — **filter bertahan saat berpindah halaman**.
5. Hal yang sama diterapkan pada modul Pengguna.
6. Konfirmasi sebelum menghapus (modal atau halaman konfirmasi), dan penghapusan memakai method `DELETE`.

---

## 4.4 Checkpoint Minggu 4

- [ ] Kenapa validasi di JavaScript tidak dianggap keamanan? Peragakan cara melewatinya.
- [ ] Apa yang dikembalikan `$request->validated()` dan kenapa lebih aman daripada `$request->all()`?
- [ ] Jelaskan pola PRG. Apa yang terjadi kalau `store` mengembalikan view?
- [ ] Kenapa filter pencarian sebaiknya di query string, bukan session? Beri satu skenario yang rusak kalau dipindah ke session.
- [ ] Apa fungsi `@csrf`? Serangan apa yang dicegahnya, dan bagaimana serangan itu bekerja?
- [ ] Kenapa aturan `unique` pada update perlu `ignore()`?

**Kuis 4:** session vs query string, Form Request, PRG, CSRF, `old()`.

---
---

# MINGGU 5 — Routing Lanjutan, Model Binding, dan Middleware

**Sub-CPMK:** Mahasiswa mampu menerapkan routing lanjutan pada aplikasi web (C3), menunjukkan kedisiplinan dalam menyusun struktur navigasi aplikasi (A3), serta membangun sistem routing yang terstruktur (P3).

**Target akhir minggu:** Route KampusLMS tersusun rapi dalam grup, memakai route model binding, dan middleware pertama sudah terpasang.

> Minggu ini memperkenalkan **IDOR** — kerentanan yang paling sering muncul di aplikasi kampus buatan mahasiswa. Perhatikan baik-baik.

---

## 5.1 Konsep

### Route resource: tujuh route dalam satu baris

```php
Route::resource('courses', CourseController::class);
```

Menghasilkan:

| Method | URI | Nama | Aksi |
|--------|-----|------|------|
| GET | `/courses` | `courses.index` | daftar |
| GET | `/courses/create` | `courses.create` | form tambah |
| POST | `/courses` | `courses.store` | simpan |
| GET | `/courses/{course}` | `courses.show` | detail |
| GET | `/courses/{course}/edit` | `courses.edit` | form edit |
| PUT/PATCH | `/courses/{course}` | `courses.update` | perbarui |
| DELETE | `/courses/{course}` | `courses.destroy` | hapus |

Varian yang berguna:

```php
Route::resource('courses', CourseController::class)->only(['index', 'show']);
Route::resource('courses', CourseController::class)->except(['destroy']);
Route::resource('courses.assignments', AssignmentController::class)->shallow();
```

`->shallow()` menghasilkan `/courses/5/assignments` untuk daftar, tapi `/assignments/12` untuk detail — URL tidak menjadi terlalu dalam. Ini yang akan Anda pakai untuk materi dan tugas.

### Route model binding

Alih-alih menerima ID lalu mencarinya sendiri:

```php
public function show($id)
{
    $course = Course::findOrFail($id);   // cara lama
}
```

Laravel bisa melakukannya otomatis kalau nama parameter route cocok dengan nama variabel:

```php
Route::get('/courses/{course}', [CourseController::class, 'show']);

public function show(Course $course)   // sudah berupa objek Course
{
    return view('courses.show', compact('course'));
}
```

Kalau tidak ditemukan, Laravel otomatis mengembalikan 404. Rapi.

Bisa juga mengikat berdasarkan kolom lain:

```php
Route::get('/courses/{course:code}', ...);   // /courses/SI2514024
```

### ⚠️ IDOR: jebakan terbesar route model binding

Route model binding **hanya menjamin data itu ada**. Ia sama sekali tidak menjamin **orang ini berhak melihatnya**.

Perhatikan skenario nyata di KampusLMS:

```php
Route::get('/submissions/{submission}', [SubmissionController::class, 'show']);

public function show(Submission $submission)
{
    return view('submissions.show', compact('submission'));   // ❌ BOCOR
}
```

Mahasiswa Budi membuka pengumpulan tugasnya sendiri di `/submissions/41`. Ia lalu mengubah URL menjadi `/submissions/42`, `/submissions/43`, dan seterusnya — dan bisa membaca seluruh pengumpulan tugas satu angkatan, lengkap dengan nilainya.

Ini disebut **IDOR** (*Insecure Direct Object Reference*). Tidak ada yang "diretas". Tidak butuh alat khusus. Cukup mengubah angka di URL.

Tiga hal yang **bukan** perbaikan:
- Menyembunyikan tautan di halaman — URL tetap bisa ditebak.
- Mengganti ID berurutan dengan UUID — memperlambat penebakan, bukan mencegah akses.
- Mengecek di JavaScript — tidak berlaku sama sekali.

Perbaikan yang benar ada dua lapis, dan minggu ini kita pasang lapis pertama:

**Lapis 1 — Scoped binding.** Ambil data lewat relasi pengguna yang sedang login, bukan dari seluruh tabel:

```php
public function show(Submission $submission)
{
    abort_unless($submission->user_id === auth()->id(), 403);
    // atau lebih baik, ambil lewat relasi:
    // $submission = auth()->user()->submissions()->findOrFail($id);
}
```

⚠️ **Jangan salin baris itu apa adanya ke proyek Anda.** Sengaja disederhanakan supaya idenya terlihat jelas: ambil data lewat pemiliknya. Tapi menurut matriks hak akses di Bagian 3 spesifikasi, dosen pengampu dan admin **juga** berhak melihat submission — dan aturan di atas memblokir keduanya. Kalau Anda memakainya apa adanya, fitur penilaian dosen akan rusak di minggu 10.

Versi minggu ini yang masih boleh dipakai sementara:

```php
abort_unless(
    $submission->user_id === auth()->id()
        || auth()->user()->role === 'admin'
        || $submission->assignment->course->lecturer_id === auth()->id(),
    403
);
```

Lihat betapa cepat kondisi ini memanjang, dan bayangkan menyalinnya ke sepuluh controller. Itu persis alasan Policy ada di minggu 7.

**Lapis 2 — Policy** (minggu 7). Aturan otorisasi dipindah ke kelas tersendiri sehingga tidak tercecer di seluruh controller.

Untuk route bersarang, Laravel punya bantuan bawaan:

```php
Route::scopeBindings()->group(function () {
    Route::get('/courses/{course}/assignments/{assignment}', ...);
});
```

Dengan `scopeBindings()`, Laravel memastikan `{assignment}` benar-benar milik `{course}` tersebut. Tanpa itu, `/courses/1/assignments/99` akan berhasil meski tugas 99 milik mata kuliah lain.

### Route group: mengurangi pengulangan

```php
Route::middleware('auth')->group(function () {

    Route::get('/dashboard', DashboardController::class)->name('dashboard');

    Route::middleware('role:admin')->prefix('admin')->name('admin.')->group(function () {
        Route::resource('users', UserController::class);
        Route::resource('courses', CourseController::class);
    });

    Route::middleware('role:dosen')->prefix('dosen')->name('dosen.')->group(function () {
        Route::resource('courses.assignments', AssignmentController::class)->shallow();
    });

});
```

Perhatikan `->name('admin.')` — nama route menjadi `admin.users.index`, sehingga tidak bentrok dengan route dosen.

### Middleware: penjaga di depan pintu

Middleware adalah lapisan yang dilewati request **sebelum** sampai ke controller.

```bash
php artisan make:middleware EnsureUserHasRole
```

```php
class EnsureUserHasRole
{
    public function handle(Request $request, Closure $next, string ...$roles): Response
    {
        abort_unless($request->user() && in_array($request->user()->role, $roles), 403);

        return $next($request);
    }
}
```

**Pendaftaran di Laravel 12 ada di `bootstrap/app.php`**, bukan `Kernel.php`:

```php
->withMiddleware(function (Middleware $middleware) {
    $middleware->alias([
        'role' => \App\Http\Middleware\EnsureUserHasRole::class,
    ]);
})
```

Ini contoh konkret dari peringatan minggu 1: hampir semua tutorial di internet dan sebagian besar jawaban AI akan menyuruh Anda mengedit `app/Http/Kernel.php`. Berkas itu **tidak ada** di proyek Anda.

### Middleware vs Policy — kapan pakai yang mana

| | Middleware | Policy |
|---|-----------|--------|
| Menjawab | "Boleh masuk area ini?" | "Boleh menyentuh data spesifik ini?" |
| Contoh | Hanya dosen boleh membuka `/dosen/*` | Dosen ini boleh mengedit mata kuliah **miliknya** |
| Butuh data | Tidak | Ya |

Middleware saja tidak cukup. Middleware `role:dosen` meloloskan **semua** dosen — termasuk dosen yang mencoba mengedit mata kuliah dosen lain. Itu sebabnya minggu 7 ada Policy.

---

## 5.2 Prompt Pack — Minggu 5

### A. Prompt Perancangan Struktur Route

```
Konteks: LMS Laravel 12. Peran: admin, dosen, mahasiswa.
Modul: users, courses, enrollment, materials, assignments, submissions,
grades, notifications.

Rancangkan struktur lengkap routes/web.php memakai route group.

Ketentuan:
- Kelompokkan berdasarkan peran dengan prefix dan name prefix.
- Gunakan Route::resource dan shallow() di tempat yang tepat.
- Untuk setiap grup, sebutkan middleware yang dipakai.
- Sajikan sebagai TABEL dulu (method, URI, nama, controller, peran),
  baru setelah saya setujui, tulis kodenya.
- Tandai route mana saja yang menerima parameter model dan karenanya
  berisiko IDOR.
```

Instruksi terakhir sangat berguna: AI akan menandai sendiri titik-titik rawan, dan mahasiswa jadi punya daftar periksa keamanan sebelum menulis controller.

### B. Prompt Middleware Laravel 12

```
Buatkan middleware bernama EnsureUserHasRole untuk Laravel 12 yang
menerima parameter peran (bisa lebih dari satu, contoh: role:admin,dosen).

Ketentuan PENTING:
- Ini Laravel 12. app/Http/Kernel.php SUDAH TIDAK ADA.
  Tunjukkan pendaftaran alias middleware di bootstrap/app.php.
- Tangani kasus pengguna belum login secara berbeda dari pengguna
  yang login tapi perannya salah (401 vs 403).
- Jelaskan kenapa middleware ini saja TIDAK cukup untuk mengamankan
  data per-objek, dan apa yang masih dibutuhkan.
```

### C. Prompt Uji IDOR

```
Berikut route dan controller saya untuk modul submissions:

[tempel kode]

Berperanlah sebagai mahasiswa nakal yang sudah login dengan akun
biasa (bukan dosen, bukan admin).

1. Daftarkan semua URL yang bisa saya coba dengan mengganti-ganti angka
   ID untuk mengakses data milik orang lain.
2. Untuk tiap URL, tebak apa yang akan saya lihat kalau controller-nya
   tidak memeriksa kepemilikan.
3. Tunjukkan perbaikannya, dan jelaskan kenapa menyembunyikan tautan
   di halaman TIDAK memperbaiki apa pun.
4. Berikan perintah curl agar saya bisa membuktikan sendiri lubangnya
   sebelum dan sesudah perbaikan.
```

### D. Prompt Verifikasi Nested Route

```
Saya punya route bersarang:
/courses/{course}/assignments/{assignment}

Pertanyaan:
1. Kalau tugas dengan id 99 sebenarnya milik mata kuliah id 7, apakah
   URL /courses/1/assignments/99 akan berhasil? Kenapa?
2. Apa yang dilakukan Route::scopeBindings() dan bagaimana ia mengubah
   jawaban nomor 1?
3. Tunjukkan cara menerapkannya di Laravel 12, dan cara menguji bahwa
   perlindungannya benar-benar bekerja.
```

---

## 5.3 Read → Break → Fix → Build

### READ — Peta route Anda sendiri (30 menit)

1. Jalankan `php artisan route:list --except-vendor`. Salin keluarannya ke catatan.
2. Tandai setiap route yang menerima parameter model (`{course}`, `{assignment}`, dst).
3. Untuk setiap route bertanda, jawab: **siapa saja yang seharusnya boleh mengaksesnya, dan apa yang saat ini mencegah orang lain?** Kemungkinan besar jawabannya "belum ada apa-apa" — itu wajar, dan itulah pekerjaan minggu ini dan minggu 7.
4. Buat tabel di `docs/minggu-05-<nama>.md` berjudul "Daftar Titik Rawan IDOR". Tabel ini akan Anda pakai lagi di minggu 7 dan saat interview.

### BREAK — Enam kerusakan (45 menit)

| # | Yang dicoba | Yang harus Anda amati |
|---|-------------|------------------------|
| 1 | Login sebagai mahasiswa A. Buka submission milik mahasiswa B dengan mengubah angka di URL | **IDOR nyata di aplikasi Anda sendiri** |
| 2 | Buka `/courses/1/assignments/99` di mana tugas 99 milik mata kuliah lain | Nested route tanpa scoping |
| 3 | Aktifkan `Route::scopeBindings()`, ulangi nomor 2 | Bandingkan hasilnya |
| 4 | Daftarkan middleware di `app/Http/Kernel.php` seperti tutorial lama | Berkas itu tidak ada — kenali gejalanya |
| 5 | Pasang `role:admin` pada grup, lalu akses sebagai dosen | 403 dari middleware |
| 6 | Sebagai dosen A, edit mata kuliah milik dosen B (keduanya lolos `role:dosen`) | **Middleware saja tidak cukup** |

Nomor 1 dan 6 adalah inti minggu ini. Nomor 6 khususnya: banyak mahasiswa mengira sudah aman karena "kan sudah ada middleware role". Rasakan sendiri bahwa itu keliru.

### FIX — Repo cacat (30 menit)

Branch `w05` pada repo `kampuslms-broken` berisi **7 masalah**: satu route di luar grup `auth`, dua IDOR pada submission dan material, nested route tanpa `scopeBindings`, middleware didaftarkan di berkas yang salah, nama route bentrok antar peran, dan satu route destruktif yang memakai `GET`.

Perbaiki, kirim PR. Deskripsi PR wajib memuat **bukti** — sertakan perintah `curl` sebelum dan sesudah perbaikan untuk minimal satu IDOR.

### BUILD — Struktur route KampusLMS

1. Susun ulang `routes/web.php` sepenuhnya dengan route group berdasarkan peran (`admin`, `dosen`, `mahasiswa`), lengkap dengan prefix dan name prefix.
2. Middleware `EnsureUserHasRole`, terdaftar di `bootstrap/app.php` sebagai alias `role`.
3. Terapkan route model binding di seluruh controller — hilangkan `findOrFail($id)` manual.
4. Route bersarang untuk materi dan tugas memakai `->shallow()` dan `scopeBindings()`.
5. Tambahkan pemeriksaan kepemilikan sementara (`abort_unless`) pada setiap titik rawan yang Anda daftarkan di bagian READ. Ini akan dirapikan menjadi Policy di minggu 7.
6. Halaman 403 kustom yang informatif tapi tidak membocorkan informasi (jangan tulis "submission ini milik Budi").

---

## 5.4 Checkpoint Minggu 5

- [ ] Apa itu IDOR? Peragakan satu contoh di aplikasi Anda, lalu tunjukkan perbaikannya.
- [ ] Kenapa mengganti ID berurutan dengan UUID **bukan** perbaikan IDOR?
- [ ] Route model binding menjamin apa, dan **tidak** menjamin apa?
- [ ] Apa yang dilakukan `Route::scopeBindings()`? Beri contoh URL yang lolos tanpa itu.
- [ ] Di berkas mana middleware didaftarkan pada Laravel 12? Kenapa berbeda dari kebanyakan tutorial?
- [ ] Kenapa middleware `role:dosen` tidak cukup untuk mencegah dosen A mengedit mata kuliah dosen B?

**Kuis 5:** route resource, model binding, IDOR, route group, pendaftaran middleware.

---
---

# MINGGU 6 — REST API dan Integrasi

**Sub-CPMK:** Mahasiswa mampu menerapkan penggunaan API dalam pengembangan aplikasi web (C3), menunjukkan sikap kolaboratif dalam integrasi antar komponen sistem (A4), serta membangun integrasi API pada aplikasi web (P3).

**Target akhir minggu:** REST API KampusLMS berjalan sesuai kontrak di Bagian 5 spesifikasi, terautentikasi dengan Sanctum, dan terdokumentasi.

> **Minggu ini adalah gerbang jalur frontend.** Setelah API berdiri, kelompok Anda memilih jalur untuk sisa semester: tetap Blade+Livewire, Inertia+Vue/React/Svelte, atau SPA terpisah.

---

## 6.1 Konsep

### Kenapa API, padahal Blade sudah jalan

Karena satu backend akan melayani banyak jenis klien: aplikasi web, aplikasi mobile, sistem lain di kampus, atau frontend yang ditulis dengan framework berbeda. Kalau logika bisnis hanya hidup di controller Blade, semuanya harus ditulis ulang.

API memaksa Anda memisahkan **apa yang aplikasi lakukan** dari **bagaimana ia ditampilkan**. Pemisahan itu sendiri yang bernilai, bahkan kalau kliennya cuma satu.

### `routes/api.php` di Laravel 12

Berbeda dari versi lama, `routes/api.php` **tidak dibuat otomatis**. Aktifkan dengan:

```bash
php artisan install:api
```

Perintah ini membuat `routes/api.php`, memasang Sanctum, dan menambahkan migrasi tabel token. Setelah itu `bootstrap/app.php` akan memuat route API dengan prefix `/api`.

Ini kejutan bagi banyak orang yang mengikuti tutorial Laravel 10 — mereka mencari berkas yang belum ada.

### Perbedaan mendasar web vs API

| | `routes/web.php` | `routes/api.php` |
|---|---|---|
| Identitas | Session + cookie | Token (Bearer) |
| CSRF | Ya | Tidak (tidak pakai cookie) |
| Gagal autentikasi | Redirect ke login | 401 JSON |
| Keluaran | HTML | JSON |
| Stateful | Ya | Tidak |

Karena API tidak punya session, **setiap request harus membawa identitasnya sendiri** dalam bentuk token.

### API Resource: jangan pernah kembalikan model mentah

```php
return response()->json(Course::all());   // ❌
```

Baris itu mengirimkan **seluruh kolom** — termasuk yang tidak seharusnya dilihat klien. Pada model `User`, itu berarti hash password, token reset, alamat email semua orang. Kebocoran data paling umum di API buatan pemula bukan hasil peretasan, melainkan `return response()->json($user)`.

Cara yang benar:

```bash
php artisan make:resource CourseResource
```

```php
class CourseResource extends JsonResource
{
    public function toArray(Request $request): array
    {
        return [
            'id'       => $this->id,
            'code'     => $this->code,
            'name'     => $this->name,
            'sks'      => $this->sks,
            'status'   => $this->status,
            'lecturer' => new UserResource($this->whenLoaded('lecturer')),
            'counts'   => [
                'materials'   => $this->whenCounted('materials'),
                'assignments' => $this->whenCounted('assignments'),
            ],
            'created_at' => $this->created_at->toIso8601String(),
        ];
    }
}
```

Resource adalah **daftar putih**: hanya yang Anda sebutkan yang keluar. Kolom baru di database tidak otomatis bocor ke API.

`whenLoaded()` mencegah N+1: relasi hanya disertakan kalau memang sudah di-*eager load*, bukan di-query ulang untuk setiap baris.

```php
return CourseResource::collection(
    Course::with('lecturer')->withCount(['materials', 'assignments'])->paginate(15)
);
```

Karena memakai `paginate()`, Laravel otomatis menyertakan `meta` sesuai kontrak di spesifikasi.

### Sanctum: token untuk API

```php
public function login(LoginRequest $request)
{
    $user = User::where('email', $request->email)->first();

    if (! $user || ! Hash::check($request->password, $user->password)) {
        throw ValidationException::withMessages([
            'email' => ['Email atau kata sandi salah.'],
        ]);
    }

    return response()->json([
        'token' => $user->createToken($request->device_name ?? 'api')->plainTextToken,
        'user'  => new UserResource($user),
    ]);
}
```

Perhatikan pesan errornya: **satu pesan untuk email salah maupun password salah**. Kalau Anda membedakannya ("email tidak terdaftar" vs "password salah"), Anda memberi penyerang alat untuk mendaftar email mana saja yang ada di sistem Anda. Ini disebut *user enumeration*.

Route yang dilindungi:

```php
Route::middleware('auth:sanctum')->group(function () {
    Route::get('/me', [AuthController::class, 'me']);
    Route::apiResource('courses', Api\CourseController::class);
});
```

`apiResource` sama seperti `resource` tapi tanpa `create` dan `edit` — API tidak butuh halaman form.

### Status code yang benar

Ini bukan formalitas; klien Anda mengandalkannya.

| Kode | Kapan | Catatan |
|------|-------|---------|
| 200 | Berhasil | |
| 201 | Berhasil membuat | Kembalikan objek yang dibuat |
| 204 | Berhasil, tanpa isi | Untuk `destroy` |
| 401 | Belum login / token tidak valid | Bukan 403 |
| 403 | Sudah login tapi tidak berhak | Bukan 401 |
| 404 | Tidak ditemukan | |
| 422 | Validasi gagal | Laravel otomatis |
| 429 | Terlalu banyak permintaan | Dari rate limiting |

Membedakan 401 dan 403 adalah pertanyaan interview yang hampir pasti muncul.

### Rate limiting

```php
Route::middleware(['auth:sanctum', 'throttle:60,1'])->group(function () { ... });
Route::post('/auth/login', ...)->middleware('throttle:5,1');
```

Endpoint login **wajib** dibatasi. Tanpa itu, siapa pun bisa mencoba ribuan kombinasi kata sandi per menit.

### Menguji API tanpa frontend

Anda tidak perlu menunggu frontend selesai. Pakai `curl`, Postman, atau Insomnia:

```bash
# login
curl -X POST http://kampuslms.test/api/v1/auth/login \
  -H "Accept: application/json" \
  -d "email=dosen@kampuslms.test" -d "password=password"

# pakai token
curl http://kampuslms.test/api/v1/courses \
  -H "Accept: application/json" \
  -H "Authorization: Bearer 1|abcdef..."
```

Header `Accept: application/json` penting — tanpanya Laravel mungkin mengembalikan HTML error, bukan JSON.

---

## 6.2 Prompt Pack — Minggu 6

### A. Prompt Implementasi Sesuai Kontrak

```
Konteks: LMS Laravel 12 dengan Sanctum. Berikut kontrak API yang sudah
ditetapkan dosen dan TIDAK BOLEH diubah:

[tempel Bagian 5 spesifikasi proyek]

Implementasikan endpoint untuk modul courses dan assignments saja.

Ketentuan:
- Laravel 12. routes/api.php diaktifkan lewat php artisan install:api.
- Setiap response memakai API Resource, JANGAN kembalikan model mentah.
- Format response persis seperti kontrak, termasuk struktur meta pada
  koleksi.
- Status code sesuai standar: 201 untuk store, 204 untuk destroy,
  401 vs 403 dibedakan dengan benar.
- Sertakan eager loading agar tidak ada N+1.
```

### B. Prompt Audit Kebocoran Data

```
Berikut API Resource dan controller saya:

[tempel kode]

Audit khusus kebocoran data:
1. Adakah field sensitif yang ikut keluar di response? Periksa satu per
   satu terhadap kolom tabel yang bersangkutan.
2. Apakah relasi yang disertakan membocorkan data milik pengguna lain?
   Contoh: apakah endpoint detail mata kuliah menampilkan daftar
   mahasiswa lengkap dengan emailnya kepada sesama mahasiswa?
3. Apakah pesan error membocorkan informasi (misalnya membedakan
   "email tidak terdaftar" dan "password salah")?
4. Apakah endpoint yang seharusnya terbatas peran benar-benar dijaga
   di sisi API, bukan hanya di frontend?

Untuk tiap temuan, tunjukkan response JSON yang bocor dan perbaikannya.
```

### C. Prompt Uji Otorisasi API

```
Berikut route API saya:

[tempel routes/api.php]

Buatkan skrip pengujian berbasis curl yang membuktikan otorisasi
bekerja. Untuk setiap endpoint, uji empat kondisi:
1. Tanpa token sama sekali → harus 401
2. Token mahasiswa mengakses endpoint dosen → harus 403
3. Token dosen A mengakses data milik dosen B → harus 403
4. Token yang benar dengan hak yang benar → harus 200/201

Sajikan sebagai skrip bash yang bisa saya jalankan, dengan komentar
menjelaskan hasil yang diharapkan tiap perintah.
```

Skrip ini akan berguna sepanjang sisa semester — mahasiswa bisa menjalankannya ulang setiap kali menambah fitur.

### D. Prompt Dokumentasi

```
Berdasarkan routes/api.php dan API Resource saya, buatkan dokumentasi
API dalam format Markdown untuk berkas docs/api.md.

Untuk setiap endpoint sertakan: method, URI, peran yang berhak,
parameter, contoh request (curl), contoh response sukses, dan contoh
response gagal.

Gunakan data yang konsisten dengan seeder saya (akun demo
dosen@kampuslms.test dan seterusnya).
```

---

## 6.3 Read → Break → Fix → Build

### READ — Bandingkan dua jalur (30 menit)

1. Jalankan `php artisan install:api`. Baca perubahan yang terjadi di `bootstrap/app.php`.
2. Buat satu endpoint `GET /api/v1/courses` sederhana.
3. Bandingkan dengan `CourseController` versi web yang sudah ada. Tulis di catatan: apa yang **sama** dan apa yang **berbeda** di antara keduanya?
4. Panggil endpoint API tanpa header `Accept: application/json`. Lalu dengan header itu. Catat bedanya.
5. Jalankan `php artisan route:list --path=api`. Cocokkan dengan kontrak di spesifikasi.

### BREAK — Tujuh kerusakan (45 menit)

| # | Yang dicoba | Yang harus Anda amati |
|---|-------------|------------------------|
| 1 | Kembalikan `response()->json(User::all())` di satu endpoint uji | **Hash password tampil di layar** |
| 2 | Hapus `auth:sanctum` dari grup route, panggil tanpa token | Data terbuka untuk publik |
| 3 | Panggil endpoint terlindungi dengan token yang sudah dihapus | 401 |
| 4 | Login sebagai mahasiswa, panggil `POST /api/v1/assignments` | Harus 403, bukan 401 — periksa punya Anda |
| 5 | Hapus eager loading, panggil daftar mata kuliah, lihat Telescope/Debugbar | N+1 di API |
| 6 | Hapus `throttle` dari login, jalankan 50 percobaan berturut-turut | Brute force tanpa hambatan |
| 7 | Buat pesan login berbeda untuk email salah vs password salah | *User enumeration* — kenapa ini berbahaya |

Nomor 1 wajib benar-benar dilihat. Melihat hash password dan email seluruh pengguna tampil rapi dalam JSON adalah pengalaman yang membuat mahasiswa tidak pernah lagi mengembalikan model mentah.

### FIX — Repo cacat (30 menit)

Branch `w06` pada repo `kampuslms-broken` berisi **8 masalah**: model mentah dikembalikan pada dua endpoint, satu endpoint tanpa `auth:sanctum`, status code salah pada `store` dan `destroy`, 403 dikembalikan sebagai 401, login tanpa throttle, pesan login membocorkan keberadaan email, dan N+1 pada endpoint daftar.

Perbaiki, kirim PR, sertakan skrip `curl` sebagai bukti.

### BUILD — API KampusLMS

1. `php artisan install:api`, Sanctum terpasang.
2. Seluruh endpoint di Bagian 5 spesifikasi terimplementasi, **format response persis sesuai kontrak**.
3. API Resource untuk User, Course, Material, Assignment, Submission, Grade.
4. Eager loading di semua endpoint koleksi.
5. Rate limiting: 60/menit umum, 5/menit untuk login.
6. `docs/api.md` berisi dokumentasi lengkap dengan contoh `curl`.
7. `scripts/test-api.sh` berisi skrip pengujian otorisasi dari Prompt C.
8. **Kelompok mendaftarkan jalur frontend** untuk minggu 7–16 kepada dosen.

---

## 6.4 Checkpoint Minggu 6

- [ ] Kenapa mengembalikan model mentah berbahaya? Peragakan kebocorannya.
- [ ] Apa beda 401 dan 403? Tunjukkan di API Anda satu contoh masing-masing.
- [ ] Kenapa `routes/api.php` tidak ada secara default di Laravel 12? Bagaimana mengaktifkannya?
- [ ] Apa fungsi `whenLoaded()`? Apa yang terjadi tanpanya?
- [ ] Kenapa pesan gagal login tidak boleh membedakan email salah dan password salah?
- [ ] Kenapa endpoint login wajib di-*throttle*? Berapa nilai yang Anda pakai dan mengapa?

**Kuis 6:** REST, status code, API Resource, Sanctum, rate limiting.

---
---

# MINGGU 7 — Autentikasi, Otorisasi, dan Validasi Menyeluruh

**Sub-CPMK:** Mahasiswa mampu menerapkan autentikasi, middleware, dan validasi pada aplikasi web (C3), menunjukkan tanggung jawab dalam menjaga keamanan data pengguna (A4), serta membangun sistem autentikasi yang aman (P4).

**Target akhir minggu:** KampusLMS punya login penuh, tiga peran yang benar-benar terpisah, dan Policy yang menutup seluruh titik rawan IDOR dari minggu 5.

> ⚠️ **Minggu ini adalah Milestone M2 — Tugas 2 (3%) + interview kelompok.**

---

## 7.1 Konsep

### Autentikasi vs Otorisasi

Dua kata yang sering tertukar, dan tertukarnya menghasilkan lubang keamanan:

- **Autentikasi** — *siapa Anda*. Login, session, token.
- **Otorisasi** — *apa yang boleh Anda lakukan*. Peran, kepemilikan, Policy.

Aplikasi yang autentikasinya sempurna tetapi otorisasinya bolong justru lebih berbahaya daripada aplikasi tanpa login sama sekali, karena ia memberi rasa aman palsu.

### Memasang autentikasi

Untuk KampusLMS, gunakan starter kit resmi Laravel 12 atau Laravel Breeze. Yang penting Anda **memahami** apa yang dipasangnya, bukan sekadar menjalankannya.

Setelah terpasang, telusuri sendiri: di mana form login, controller mana yang memprosesnya, di mana session dibuat, dan di mana `Auth::attempt()` dipanggil.

### Hashing kata sandi

```php
protected function casts(): array
{
    return ['password' => 'hashed'];
}
```

Dengan cast `hashed`, kata sandi otomatis di-hash saat disimpan. Anda tidak perlu memanggil `Hash::make()` manual.

Yang wajib Anda pahami:
- Hash **bukan** enkripsi. Ia satu arah — tidak ada cara mengembalikannya ke teks asli. Karena itu fitur "lupa password" mengirim tautan reset, bukan kata sandi lama.
- Bcrypt sengaja lambat. Itu fiturnya, bukan bug — memperlambat serangan tebakan massal.
- **Jangan pernah** menyimpan kata sandi dalam bentuk apa pun yang bisa dibaca kembali.

### Session fixation dan regenerasi

```php
public function store(LoginRequest $request)
{
    $request->authenticate();
    $request->session()->regenerate();      // ← wajib

    return redirect()->intended(route('dashboard'));
}
```

`regenerate()` mengganti ID session setelah login berhasil. Tanpa itu, penyerang yang berhasil menanamkan ID session tertentu pada korban akan ikut terbawa masuk saat korban login. Starter kit sudah menyertakannya — jangan dihapus saat merapikan kode.

Sebaliknya, saat logout:

```php
Auth::guard('web')->logout();
$request->session()->invalidate();
$request->session()->regenerateToken();
```

### Policy: otorisasi per objek

Inilah yang menutup IDOR dari minggu 5 secara rapi.

```bash
php artisan make:policy CoursePolicy --model=Course
```

```php
class CoursePolicy
{
    public function viewAny(User $user): bool
    {
        return true;   // semua yang login boleh melihat daftar (sudah difilter di query)
    }

    public function view(User $user, Course $course): bool
    {
        return $user->role === 'admin'
            || $course->lecturer_id === $user->id
            || $course->students()->whereKey($user->id)->exists();
    }

    public function update(User $user, Course $course): bool
    {
        return $user->role === 'admin' || $course->lecturer_id === $user->id;
    }

    public function delete(User $user, Course $course): bool
    {
        return $user->role === 'admin';
    }
}
```

Di Laravel 12, Policy ditemukan otomatis selama penamaannya konvensional (`Course` → `CoursePolicy`).

### ⚠️ `$this->authorize()` tidak ada di Laravel 12

Hampir setiap tutorial menulis `$this->authorize('update', $course)` di controller. **Di Laravel 11 ke atas, baris itu melempar `Call to undefined method`.**

Sebabnya sama persis dengan cerita `Kernel.php` di minggu 1: kelas dasar `App\Http\Controllers\Controller` sekarang kosong, dan trait `AuthorizesRequests` **tidak lagi disertakan** di dalamnya.

Ada dua jalan keluar. Pakai yang pertama:

```php
use Illuminate\Support\Facades\Gate;

public function update(UpdateCourseRequest $request, Course $course)
{
    Gate::authorize('update', $course);          // ✔ selalu jalan, tanpa syarat apa pun

    $course->update($request->validated());

    return redirect()->route('courses.show', $course)->with('success', 'Tersimpan.');
}
```

Atau, kalau Anda memang ingin gaya `$this->authorize()`, tambahkan sendiri trait-nya ke kelas dasar:

```php
// app/Http/Controllers/Controller.php
namespace App\Http\Controllers;

use Illuminate\Foundation\Auth\Access\AuthorizesRequests;

abstract class Controller
{
    use AuthorizesRequests;      // ← baru setelah ini $this->authorize() bisa dipakai
}
```

Pilih **satu** dan konsisten di seluruh proyek. Modul ini memakai `Gate::authorize()` mulai titik ini, karena tidak bergantung pada apa pun dan tetap benar di dalam Livewire, Job, maupun *route closure* — tempat yang tidak punya `$this` sebuah controller.

Ini contoh kedua dari pola yang akan terus muncul: **kode yang diberikan AI benar untuk Laravel 10, dan gagal di proyek Anda.** Kalau AI menuliskan `$this->authorize()` tanpa menyinggung trait, itu tanda jawabannya berasal dari versi lama.

Atau langsung di route:

```php
Route::put('/courses/{course}', ...)->can('update', 'course');
```

Di Blade:

```blade
@can('update', $course)
    <a href="{{ route('courses.edit', $course) }}">Edit</a>
@endcan
```

**Peringatan penting:** `@can` di Blade hanya menyembunyikan tombol. Ia **bukan** pengaman. Kalau controller tidak memanggil `Gate::authorize()`, URL edit tetap bisa diakses langsung. Selalu keduanya: `@can` untuk kenyamanan, `Gate::authorize()` untuk keamanan.

### `authorize()` di dalam Form Request

Ingat `authorize()` yang di minggu 4 kita isi `return true`? Sekarang saatnya:

```php
public function authorize(): bool
{
    return $this->user()->can('update', $this->route('course'));
}
```

### Menutup kebocoran di level query

Policy melindungi akses ke satu objek. Tapi daftar juga bisa bocor:

```php
// ❌ mahasiswa melihat semua mata kuliah, termasuk yang tidak diikutinya
$courses = Course::paginate(15);

// ✔ setiap peran melihat miliknya sendiri
$courses = (match ($user->role) {
    'admin'     => Course::query(),
    'dosen'     => $user->taughtCourses(),
    'mahasiswa' => $user->courses(),
    default     => abort(403),
})->with('lecturer')->paginate(15);
```

Dua detail yang mudah terlewat di potongan itu:

- **Tanda kurung mengelilingi `match` itu wajib.** `match (...) { ... }->with(...)` adalah *parse error* — hasil sebuah ekspresi `match` tidak bisa langsung dirantai dengan `->`. Ini kesalahan yang sering muncul pada kode buatan AI.
- **`default` wajib ada.** Tanpa itu, peran yang tidak dikenal (mis. data lama atau kolom yang baru ditambah) melempar `UnhandledMatchError` — halaman 500, bukan 403 yang rapi.

Prinsipnya: **jangan mengambil data yang tidak berhak lalu menyaringnya di view.** Saring di query.

### Verifikasi menyeluruh titik rawan

Buka kembali tabel "Daftar Titik Rawan IDOR" yang Anda buat di minggu 5. Setiap baris harus sekarang punya jawaban konkret: Policy mana yang menutupnya, atau query mana yang membatasinya. Tabel yang sudah terisi penuh inilah deliverable keamanan Tugas 2.

---

## 7.2 Prompt Pack — Minggu 7

### A. Prompt Perancangan Otorisasi

```
Konteks: LMS Laravel 12. Peran: admin, dosen, mahasiswa.
Model: Course, Material, Assignment, Submission, Grade.

Berikut matriks hak akses yang ditetapkan dosen:

[tempel Bagian 3 spesifikasi proyek]

Rancangkan Policy untuk setiap model.

Ketentuan:
- Untuk setiap method policy (viewAny, view, create, update, delete),
  tuliskan aturannya dalam KALIMAT BAHASA INDONESIA dulu, bukan kode.
- Tandai method mana yang perlu memeriksa kepemilikan lewat relasi,
  dan relasi mana yang dipakai.
- Tunjukkan di mana matriks itu ambigu atau belum lengkap, dan tanyakan
  kepada saya untuk memperjelas. Jangan mengarang aturan sendiri.
```

Instruksi terakhir melatih hal penting: AI cenderung mengisi kekosongan spesifikasi dengan asumsi diam-diam. Menyuruhnya bertanya jauh lebih aman.

### B. Prompt Implementasi Policy

```
Berdasarkan rancangan yang tadi kita sepakati, implementasikan seluruh
Policy untuk Laravel 12.

Ketentuan:
- Laravel 12 (policy discovery otomatis, tidak perlu didaftarkan manual
  di AuthServiceProvider).
- Hindari N+1: kalau perlu memeriksa keanggotaan, gunakan exists()
  atau relasi yang sudah dimuat, bukan mengambil seluruh koleksi.
- Tunjukkan cara memanggilnya di tiga tempat: controller, Form Request,
  dan Blade.
- Pakai Gate::authorize(), BUKAN $this->authorize() — trait
  AuthorizesRequests tidak lagi ada di kelas dasar Controller pada
  Laravel 11+.
- Jelaskan kenapa @can di Blade TIDAK menggantikan pemeriksaan
  otorisasi di controller.
```

### C. Prompt Audit Kebocoran Daftar

```
Berikut method index dari seluruh controller saya:

[tempel kode]

Periksa kebocoran pada tingkat DAFTAR, bukan objek tunggal:
1. Untuk setiap index, apakah mahasiswa bisa melihat data yang bukan
   miliknya atau bukan dari mata kuliah yang diikutinya?
2. Apakah dosen bisa melihat data mata kuliah dosen lain?
3. Apakah ada penyaringan yang dilakukan di VIEW padahal seharusnya
   di QUERY? Kalau ada, jelaskan kenapa itu berbahaya.
4. Apakah ada endpoint pencarian yang bisa dipakai untuk menebak
   keberadaan data milik orang lain?

Untuk tiap temuan tunjukkan query perbaikannya.
```

### D. Prompt Uji Tembus Menyeluruh

```
Berikut seluruh routes/web.php, routes/api.php, dan Policy saya:

[tempel kode]

Susun rencana pengujian keamanan dengan tiga akun: admin, dosen,
mahasiswa (seluruhnya dari seeder saya).

Untuk setiap kombinasi (akun × endpoint), tentukan hasil yang
DIHARAPKAN (200/403/404), lalu buatkan skrip bash berisi curl untuk
mengujinya.

Fokus khusus pada:
- mahasiswa mengakses submission mahasiswa lain
- dosen mengakses mata kuliah dosen lain
- mahasiswa mengakses mata kuliah yang tidak diikutinya
- pengubahan role lewat request

Tandai dengan jelas kasus mana yang paling mungkin lolos di kode saya.
```

---

## 7.3 Read → Break → Fix → Build

### READ — Bedah starter kit (30 menit)

Setelah memasang autentikasi, telusuri **tanpa AI**:

1. Berkas mana yang menangani POST login? Method apa?
2. Di baris mana `Auth::attempt()` atau setaranya dipanggil?
3. Temukan `session()->regenerate()`. Kenapa ia ada di situ?
4. Di mana kata sandi di-hash? Cari cast `hashed` di model `User`.
5. Buka DevTools → Cookies sebelum dan sesudah login. Bandingkan nilai cookie session.
6. Logout, lalu tekan tombol *back*. Apa yang terjadi? Kenapa?

### BREAK — Delapan kerusakan (50 menit)

| # | Yang dicoba | Yang harus Anda amati |
|---|-------------|------------------------|
| 1 | Hapus `session()->regenerate()` dari login | Session fixation terbuka |
| 2 | Hapus `Gate::authorize()` dari `update`, tapi biarkan `@can` di Blade | **Tombol hilang, URL tetap jalan** |
| 3 | Sebagai dosen A, kirim PUT ke mata kuliah dosen B lewat `curl` | Uji Policy Anda sungguhan |
| 4 | Sebagai mahasiswa, buka submission mahasiswa lain (IDOR minggu 5) | Sekarang harus 403 |
| 5 | Ubah `index` menjadi `Course::paginate()` polos, login sebagai mahasiswa | Kebocoran tingkat daftar |
| 6 | Kirim `role=admin` pada form edit profil | Mass assignment kembali diuji |
| 7 | Ganti cast `hashed` menjadi tidak ada, buat user baru, lihat kolom password di database | **Kata sandi polos di database** |
| 8 | Login, salin cookie session, tempel di browser lain | Kenapa cookie harus HttpOnly & Secure |

Nomor 2 adalah pelajaran terpenting minggu ini. Banyak mahasiswa mengira `@can` sudah mengamankan. Buktikan sendiri bahwa tidak.

### FIX — Repo cacat (35 menit)

Branch `w07` pada repo `kampuslms-broken` berisi **9 masalah**: `@can` tanpa `Gate::authorize()` di dua controller, Policy yang selalu `return true`, kebocoran daftar untuk mahasiswa, `role` bisa diubah lewat update profil, `session()->regenerate()` hilang, pesan login membocorkan keberadaan email, satu route dosen tanpa middleware, dan Policy yang menyebabkan N+1.

Perbaiki, kirim PR dengan bukti `curl` untuk minimal tiga masalah.

### BUILD — Milestone M2

Deliverable yang dinilai sebagai **Tugas 2**:

1. **Autentikasi lengkap**: login, logout, register (khusus admin yang mendaftarkan), reset kata sandi.
2. **Tiga peran berfungsi** dengan dashboard berbeda per peran.
3. **Policy untuk seluruh model** (Course, Material, Assignment, Submission, Grade), dipanggil di controller **dan** dipakai di Blade.
4. **Seluruh index disaring di level query** sesuai peran.
5. **Tabel Titik Rawan IDOR terisi penuh** di `docs/keamanan.md`: tiap baris menyebut Policy atau query yang menutupnya.
6. **CRUD Materi dan Tugas** berfungsi dengan otorisasi yang benar (unggah berkas baru minggu 9 — cukup metadata dulu).
7. **`scripts/test-authz.sh`** — skrip pengujian otorisasi yang seluruhnya lolos.
8. CI hijau, setiap anggota punya commit, tiap fitur lewat PR yang direview.

---

## 7.4 Checkpoint Minggu 7 — Gerbang Interview Tugas 2

Ditanyakan per individu dengan kode terbuka di layar.

- [ ] Apa beda autentikasi dan otorisasi? Tunjukkan satu contoh masing-masing di kode Anda.
- [ ] Tunjukkan Policy yang **Anda** tulis. Jelaskan tiap barisnya.
- [ ] Kenapa `@can` di Blade tidak cukup? Peragakan dengan mengakses URL langsung.
- [ ] Buka `docs/keamanan.md`. Pilih satu baris, jelaskan bagaimana ia ditutup, lalu buktikan dengan `curl`.
- [ ] Kenapa kata sandi di-hash, bukan dienkripsi? Apa konsekuensinya untuk fitur lupa password?
- [ ] Apa fungsi `session()->regenerate()` saat login?
- [ ] Kenapa daftar mata kuliah tidak boleh diambil semua lalu disaring di view?
- [ ] Tunjukkan satu bagian yang Anda tulis dengan bantuan AI. Apa yang Anda ubah, dan kenapa?

**Kuis 7:** autentikasi vs otorisasi, Policy, hashing, session, kebocoran tingkat daftar.

---

## Rubrik Ringkas Tugas 2

| Kriteria | Bobot | Yang dilihat |
|----------|-------|--------------|
| Fungsionalitas | 30% | Login, tiga peran, dashboard per peran, CRUD materi & tugas berjalan |
| Implementasi Konsep | 35% | Policy lengkap dan benar; penyaringan di level query; validasi server-side; tidak ada IDOR tersisa |
| Interview/Pemahaman | 25% | Mampu menjelaskan dan **memperagakan** perbedaan tombol tersembunyi vs akses tertutup |
| Kualitas Kode | 10% | Policy tidak duplikatif; controller tipis; dokumentasi keamanan rapi; PR direview |

Bobot Implementasi Konsep dinaikkan minggu ini karena keamanan adalah inti Sub-CPMK. Aplikasi yang seluruh fiturnya jalan tetapi masih punya satu IDOR terbuka **tidak dapat lulus** komponen ini.

---

## Menuju UTS (Minggu 8)

UTS bukan ujian tulis. Bentuknya **demo aplikasi + code walkthrough**, 20 menit per kelompok:

- 7 menit: demo alur lengkap dari tiga peran berbeda
- 8 menit: penelusuran kode — penguji menunjuk fitur acak, anggota yang menulisnya menjelaskan
- 5 menit: uji keamanan langsung — penguji mencoba satu IDOR, kelompok menjelaskan apa yang menahannya

Persiapan yang paling berguna: pastikan `migrate:fresh --seed` berjalan bersih, akun demo berfungsi, dan setiap anggota tahu persis bagian mana yang ia tulis.

---

*Modul Minggu 9–12 menyusul: unggah berkas, realtime & notifikasi, optimisasi performa, deployment.*
