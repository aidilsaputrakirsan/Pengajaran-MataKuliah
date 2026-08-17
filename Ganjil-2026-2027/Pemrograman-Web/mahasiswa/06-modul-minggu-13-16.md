# MODUL PEMROGRAMAN WEB — MINGGU 13–16

**SI2514024 | Proyek: KampusLMS | Laravel 12**

> Dokumen penutup. Berisi dua jenis isi:
> - **Minggu 13–15** — modul materi dengan format biasa: Konsep → Prompt Pack → Read-Break-Fix-Build → Checkpoint.
> - **Minggu 16** — panduan asesmen akhir (UAS). Ditujukan untuk dosen, asdos, **dan** mahasiswa; seluruh rubrik dan pertanyaan dibuka sejak awal semester.
>
> Panduan asesmen tengah semester ada pada berkas terpisah: `04-modul-minggu-08-uts.md`.

---
---

# MINGGU 13 — Kolaborasi, Version Control, dan Peer Review

**Sub-CPMK:** Mahasiswa mampu menerapkan version control dan kolaborasi dalam pengembangan aplikasi web (C3), menunjukkan sikap kooperatif dalam kerja tim (A4), serta membangun alur kerja kolaboratif menggunakan Git (P3).

**Target akhir minggu:** Kelompok Anda memakai Git dengan benar, dan Anda telah mereview kode kelompok lain secara individual.

> ⚠️ **Minggu ini menghasilkan komponen Review (10%) — dinilai per individu, bukan per kelompok.**

---

## 13.1 Konsep

### Git yang sebenarnya Anda pakai

Selama dua belas minggu Anda sudah memakai Git. Minggu ini kita rapikan pemahamannya.

```
Working Directory  →  Staging Area  →  Local Repo  →  Remote (GitHub)
      (edit)          (git add)      (git commit)     (git push)
```

Perintah yang benar-benar sering dipakai:

```bash
git status                    # selalu jalankan sebelum apa pun
git switch -c feat/upload     # branch baru
git add -p                    # tambahkan per potongan, bukan semua sekaligus
git commit -m "feat: tambah unggah materi"
git push -u origin feat/upload
git switch dev && git pull    # sinkron sebelum mulai kerja baru
```

`git add -p` layak dibiasakan: ia memaksa Anda **membaca kembali** perubahan sendiri sebelum commit. Banyak `.env` bocor dan `dd()` tertinggal karena orang terbiasa `git add .`.

### Conventional Commits

Format yang sudah diwajibkan sejak minggu 1:

```
feat: tambah endpoint pengumpulan tugas
fix: perbaiki IDOR pada unduhan submission
refactor: pindahkan logika notifikasi ke listener
docs: lengkapi dokumentasi deployment
test: tambah feature test untuk penilaian
```

Kenapa penting? Karena `git log --oneline` menjadi riwayat yang bisa dibaca, dan karena **inilah cara penguji tahu siapa mengerjakan apa**. Commit bernama "update", "fix bug", "revisi 3" membuat kontribusi Anda tidak terlihat.

### Alur branch KampusLMS

```
main    ←  hanya kode yang sudah direview dan CI-nya hijau
 ↑
dev     ←  integrasi antar fitur
 ↑
feat/*  ←  satu branch per fitur, satu orang
```

Aturannya:
- Satu fitur = satu branch = satu PR. Branch yang berisi lima fitur tidak bisa direview.
- Branch berumur pendek. Branch yang hidup dua minggu akan menghasilkan konflik yang menyakitkan.
- Sebelum membuka PR: `git switch dev && git pull && git switch feat/xxx && git merge dev` — selesaikan konflik di branch Anda, bukan di PR.

### Konflik: bukan bencana

Konflik terjadi ketika dua orang mengubah baris yang sama.

```
<<<<<<< HEAD
$courses = Course::with('lecturer')->paginate(15);
=======
$courses = Course::with('lecturer', 'students')->paginate(20);
>>>>>>> feat/filter
```

Yang harus dilakukan: **baca kedua sisi, pahami maksud keduanya, tulis versi yang benar.** Bukan memilih salah satu secara buta.

Pencegahan terbaik bukan alat, melainkan pembagian kerja: kalau dua orang selalu menyentuh berkas yang sama, pembagian tugasnya yang salah.

### Pull Request yang layak direview

PR bukan tombol merge. Ia adalah tempat menjelaskan.

Template `.github/pull_request_template.md`:

```markdown
## Apa yang diubah


## Kenapa


## Cara menguji
1.
2.

## Daftar periksa
- [ ] Validasi ada di server-side
- [ ] Otorisasi diperiksa (`Gate::authorize()` dipanggil di controller)
- [ ] Tidak ada N+1 (diperiksa dengan Debugbar)
- [ ] Ada feature test untuk jalur sukses dan jalur ditolak
- [ ] Tidak ada `dd()`, `dump()`, atau `console.log` tertinggal
- [ ] `.env` tidak tersentuh

## Bagian yang dibantu AI
(sebutkan bagian mana dan apa yang Anda ubah dari keluaran aslinya)
```

Bagian terakhir bukan untuk menghukum. Ia memberi tahu reviewer di mana harus lebih teliti — dan itu memang tempat bug paling sering bersembunyi, karena kode hasil AI biasanya benar secara sintaks tetapi tidak menyesuaikan konteks proyek.

### Cara mereview kode orang lain

Review yang buruk: "sudah bagus", "oke lanjut", "mantap".

Review yang berguna punya tiga unsur: **lokasi, risiko, saran.**

> `app/Http/Controllers/SubmissionController.php:34` — method `download` langsung mengembalikan berkas tanpa memanggil `Gate::authorize()`. Mahasiswa mana pun yang login bisa mengunduh submission orang lain dengan mengubah ID di URL. Saran: tambahkan `Gate::authorize('view', $submission)` sebelum baris `Storage::download`.

Urutan memeriksa PR yang efisien:
1. **Keamanan** — otorisasi, validasi, kebocoran data. Ini yang paling mahal kalau lolos.
2. **Kebenaran** — apakah fiturnya benar-benar bekerja sesuai deskripsi?
3. **Performa** — N+1, query di dalam loop.
4. **Keterbacaan** — penamaan, panjang method, duplikasi.

Nomor 4 terakhir dengan sengaja. Memperdebatkan nama variabel sementara ada IDOR terbuka adalah review yang gagal.

### Nada review

Anda mereview **kode**, bukan orang. Bandingkan:

- ❌ "Kamu ceroboh, ini bahaya banget."
- ✔ "Baris 34 belum memanggil `Gate::authorize()`. Kalau dibiarkan, submission bisa diunduh siapa saja yang mengubah ID di URL."

Review yang ditulis dengan nada menyerang membuat orang defensif dan tidak memperbaiki apa pun. Ini keterampilan kerja, bukan sekadar sopan santun.

---

## 13.2 Prompt Pack — Minggu 13

### A. Prompt Belajar Git dari Riwayat Sendiri

```
Berikut keluaran `git log --oneline --graph --all` dari repo kelompok
saya:

[tempel keluaran]

Analisis alur kerja kami:
1. Apakah kami benar-benar memakai branch per fitur, atau sebagian besar
   commit langsung ke satu branch?
2. Adakah branch yang hidup terlalu lama?
3. Apakah pesan commit mengikuti Conventional Commits?
4. Dari riwayat ini, apakah kontribusi anggota terlihat merata?

Beri saran perbaikan yang konkret untuk sisa semester.
Jangan menilai orang — nilai polanya.
```

### B. Prompt Persiapan Review — memahami kode asing

```
Saya harus mereview repo kelompok lain. Proyeknya sama dengan proyek
saya (LMS Laravel 12 dengan skema yang identik), tetapi implementasinya
berbeda.

Berikut Pull Request yang harus saya review:

[tempel diff]

JANGAN menuliskan review untuk saya.
Sebagai gantinya:
1. Jelaskan apa yang dilakukan perubahan ini, dalam bahasa sederhana.
2. Sebutkan area mana yang PALING LAYAK saya periksa dengan teliti,
   dan kenapa.
3. Berikan 5 pertanyaan yang sebaiknya saya tanyakan pada diri sendiri
   saat membaca kode ini.

Saya yang akan menulis reviewnya sendiri.
```

Batasan ini penting. Review yang ditulis AI lalu ditempel adalah kecurangan terhadap kelompok yang direview — mereka menerima masukan yang tidak dipahami pengirimnya dan tidak bisa didiskusikan.

### C. Prompt Menajamkan Review Sendiri

```
Saya sudah menulis temuan review berikut untuk kelompok lain:

[tempel temuan Anda sendiri]

Bantu saya menajamkannya, TANPA menambah temuan baru:
1. Apakah setiap temuan sudah menyebut lokasi, risiko, dan saran?
2. Adakah temuan yang nadanya menyerang orang, bukan kode? Sarankan
   penulisan ulang.
3. Adakah temuan yang sebenarnya soal selera, bukan masalah nyata?
   Tandai.
4. Apakah urutannya sudah dari yang paling penting?
```

### D. Prompt Menyelesaikan Konflik

```
Saya mengalami konflik merge berikut:

[tempel blok konflik lengkap]

Konteks: branch saya menambahkan [jelaskan], branch dev menambahkan
[jelaskan].

JANGAN langsung memberi hasil gabungannya.
1. Jelaskan apa maksud perubahan di masing-masing sisi.
2. Sebutkan apakah keduanya bisa digabung, atau memang saling
   bertentangan sehingga harus ada yang dipilih.
3. Kalau bisa digabung, tanyakan dulu perilaku mana yang saya inginkan
   sebelum menuliskan hasilnya.
```

---

## 13.3 Read → Break → Fix → Build

### READ — Baca riwayat kelompok Anda (30 menit)

```bash
git log --oneline --graph --all
git shortlog -sn                    # commit per orang
git log --pretty='%an' | sort | uniq -c
```

Tanpa AI, jawab di `docs/minggu-13-<nama>.md`:
1. Berapa commit atas nama Anda? Bandingkan dengan anggota lain.
2. Adakah minggu di mana Anda tidak punya commit sama sekali?
3. Berapa persen pesan commit yang mengikuti Conventional Commits?
4. Temukan satu commit Anda sendiri yang pesannya buruk. Tulis ulang seharusnya bagaimana.
5. `git log --stat` — temukan commit terbesar Anda. Apakah seharusnya dipecah?

Bagian ini terkadang tidak nyaman. Itu maksudnya — lebih baik Anda menemukan sendiri sekarang daripada penguji menemukannya di minggu 16.

### BREAK — Enam percobaan (40 menit)

Kerjakan di branch percobaan, jangan di `dev` atau `main`.

> ⛔ **Percobaan 1 memakai berkas `.env` PALSU.** Repo kelompok Anda bersifat publik, dan begitu sesuatu ter-push, ia harus dianggap sudah dibaca orang lain — persis pelajaran nomor 2 di bawah. Sebelum mulai:
>
> ```bash
> cp .env .env.asli.jangan-disentuh     # simpan yang asli, di luar Git
> printf 'APP_KEY=base64:PALSU\nDB_PASSWORD=palsu-untuk-latihan\n' > .env
> ```
>
> Setelah percobaan selesai, kembalikan dengan `mv .env.asli.jangan-disentuh .env`. Jangan pernah menjalankan latihan ini dengan `.env` yang memuat kredensial sungguhan.

| # | Yang dicoba | Yang harus Anda amati |
|---|-------------|------------------------|
| 1 | Commit `.env` **palsu** (lihat kotak di atas), lalu `git push` | CI harus **menolak**. Kalau lolos, CI Anda bermasalah |
| 2 | Hapus `.env` dari riwayat dengan `git rm --cached`, commit lagi | Berkas hilang dari HEAD tapi **masih ada di riwayat** |
| 3 | Dua anggota mengubah baris yang sama di `routes/web.php`, merge | Konflik sungguhan, selesaikan bersama |
| 4 | Push langsung ke `main` | Branch protection harus memblokir |
| 5 | Buka PR yang gagal CI, coba merge | Tombol merge harus terkunci |
| 6 | Buat commit dengan `git commit --author="Orang Lain <x@x.com>"` | **Kepenulisan Git bisa dipalsukan** |

Nomor 2 adalah pelajaran serius: sekali rahasia masuk ke riwayat Git dan ter-push, menghapusnya dari commit terbaru **tidak cukup**. Rahasia itu harus dianggap bocor dan diganti — kata sandi database diubah, `APP_KEY` dibuat ulang.

Nomor 6 menjelaskan kenapa penilaian kontribusi tidak semata-mata dari `git shortlog`, melainkan dari interview.

### FIX — Repo cacat (30 menit)

Branch `w13` pada repo `kampuslms-broken` berisi **6 masalah alur kerja**: `.env` ter-commit di riwayat, tidak ada branch protection, PR raksasa berisi lima fitur sekaligus, pesan commit tanpa format, `dd()` tertinggal di controller, dan `node_modules` ter-commit.

Perbaiki apa yang bisa diperbaiki, dan untuk yang **tidak bisa** dibatalkan (rahasia yang sudah masuk riwayat), tulis di PR apa langkah mitigasi yang seharusnya diambil.

### BUILD — Peer Review (komponen Review, 10%)

**Penugasan:** rotasi melingkar **di dalam kelas masing-masing**. A01 mereview A02, A02 mereview A03, dan seterusnya; A09 mereview A01. Begitu pula B01–B07. Ditetapkan dosen di awal sesi.

**Yang dikerjakan setiap mahasiswa secara individu:**

1. Kloning repo kelompok yang ditugaskan, jalankan `migrate:fresh --seed`, jalankan aplikasinya.
2. Pilih **satu Pull Request** dari repo itu (boleh berbeda antar anggota, dikoordinasikan agar tidak menumpuk di PR yang sama).
3. Tulis review sebagai komentar di PR tersebut **memakai akun GitHub sendiri**.
4. Wajib memuat **minimal 3 temuan dengan kategori berbeda**:
   - satu soal **keamanan** (otorisasi, validasi, kebocoran data)
   - satu soal **performa** (N+1, query dalam loop, data tidak dipaginasi)
   - satu soal **struktur atau keterbacaan** (logika di view, controller gemuk, duplikasi)
5. Setiap temuan menyebut **lokasi (berkas:baris), risiko, dan saran**.
6. Nada menyerang kode, bukan orang.

**Yang dikerjakan kelompok yang direview:**

7. **Wajib menanggapi** setiap temuan: diterima dan diperbaiki, atau ditolak dengan alasan. Menolak dengan alasan yang benar adalah tanggapan yang sah dan bernilai penuh.
8. Minimal satu temuan benar-benar diperbaiki lewat commit baru di PR itu, dengan pesan `fix:` yang merujuk temuan.

**Selain itu, untuk kelompok sendiri:**

9. Pasang `.github/pull_request_template.md`.
10. Pastikan branch protection aktif: PR wajib, CI wajib hijau, minimal satu approval.
11. Rapikan `README.md`: tabel pembagian peran yang jujur, mencerminkan `git shortlog`.

---

## 13.4 Rubrik Komponen Review (10%) — Per Individu

| Kriteria | Bobot | Yang dilihat |
|----------|-------|--------------|
| Kelengkapan temuan | 30% | Ada ≥3 temuan dari 3 kategori berbeda |
| Ketepatan | 30% | Temuan benar-benar masalah nyata, bukan selera; risiko dijelaskan tepat |
| Kegunaan saran | 25% | Saran konkret dan bisa langsung dikerjakan, menyebut lokasi |
| Nada dan tanggapan | 15% | Menyerang kode bukan orang; menanggapi review yang masuk ke kelompok sendiri dengan argumen |

Temuan yang dihasilkan AI lalu ditempel tanpa dipahami akan ketahuan saat dosen menanyakannya, dan dinilai 0. Anda boleh memakai AI untuk **memahami** kode asing (Prompt B) dan **menajamkan** tulisan sendiri (Prompt C), bukan untuk menghasilkan temuannya.

## 13.5 Checkpoint Minggu 13

- [ ] Berapa commit atas nama Anda? Tunjukkan `git shortlog -sn`.
- [ ] Kenapa `git rm --cached .env` tidak cukup kalau `.env` sudah pernah ter-push?
- [ ] Tunjukkan satu temuan review yang Anda tulis. Jelaskan risikonya tanpa membaca catatan.
- [ ] Tunjukkan satu temuan yang masuk ke kelompok Anda. Anda terima atau tolak? Kenapa?
- [ ] Kenapa PR sebaiknya kecil dan satu fitur?
- [ ] Kenapa penilaian kontribusi tidak bisa hanya berdasarkan jumlah commit?

**Kuis 13:** alur branch, Conventional Commits, konflik, isi review yang baik.

---
---

# MINGGU 14 — Debugging dan Testing

**Sub-CPMK:** Mahasiswa mampu menganalisis kesalahan dan melakukan pengujian pada aplikasi web (C4), menunjukkan sikap teliti dalam menemukan dan memperbaiki kesalahan (A4), serta membangun pengujian untuk menjamin kualitas aplikasi (P4).

**Target akhir minggu:** Alur kritis KampusLMS punya feature test yang berjalan di CI, dan Anda bisa mendiagnosis kegagalan secara sistematis.

---

## 14.1 Konsep

### Debugging adalah metode, bukan bakat

Pola yang berulang sepanjang semester ini: **kegagalan yang tidak menghasilkan pesan error.** Queue worker mati di minggu 10, `env()` setelah `config:cache` di minggu 11, deployment di minggu 12.

Metodenya selalu sama:

1. **Reproduksi.** Bug yang tidak bisa diulang tidak bisa diperbaiki. Catat langkah persisnya.
2. **Persempit.** Di mana ia masih benar, di mana mulai salah? Bagi dua terus-menerus.
3. **Rumuskan hipotesis tunggal.** "Saya menduga X, karena Y."
4. **Uji satu hipotesis.** Bukan mengubah lima hal sekaligus.
5. **Perbaiki, lalu tulis test** agar bug itu tidak kembali.

Langkah 5 yang paling sering dilewati, dan justru yang membedakan perbaikan permanen dari perbaikan sementara.

### Alat

```php
dd($variabel);            // berhenti dan tampilkan
dump($variabel);          // tampilkan, lanjut
Log::info('cek', ['user' => $user->id, 'course' => $course->id]);
DB::listen(fn ($q) => Log::debug($q->sql, $q->bindings));
ray($variabel);           // kalau memakai Ray
```

Aturan: `dd()` dan `dump()` **tidak boleh** masuk PR. Itu ada di daftar periksa minggu 13. `Log::` boleh, karena di produksi ia menulis ke berkas, bukan ke layar pengguna.

Membaca log:

```bash
tail -f storage/logs/laravel.log
grep -n "SQLSTATE" storage/logs/laravel.log
php artisan pail                      # butuh composer require laravel/pail --dev
```

Yang dibaca dari stack trace: **baris pertama yang menyebut berkas di `app/`**, bukan yang menyebut `vendor/`. Kesalahan hampir selalu ada di kode Anda, sementara jejak teratas biasanya menunjuk framework.

### Kenapa menulis test

Tanpa test, setiap perubahan berarti menguji ulang seluruh aplikasi secara manual — dan tidak ada yang benar-benar melakukannya. Akibatnya: fitur yang sudah jalan diam-diam rusak saat fitur lain ditambahkan.

Dengan test, Anda menjalankan satu perintah.

Test juga adalah **satu-satunya cara memverifikasi otorisasi secara menyeluruh.** Anda tidak akan mencoba manual seluruh kombinasi peran × endpoint setiap kali mengubah kode. Test bisa.

### Jenis test yang dipakai di KampusLMS

| Jenis | Menguji | Porsi di proyek ini |
|-------|---------|---------------------|
| **Feature test** | Satu alur lewat HTTP: request → response | **Utama.** Fokus di sini. |
| **Unit test** | Satu method terisolasi | Secukupnya, untuk logika perhitungan |

Untuk aplikasi web berbasis CRUD seperti ini, feature test memberi manfaat terbesar per usaha.

### Menulis feature test

```bash
php artisan make:test SubmissionTest
```

```php
use App\Models\{User, Course, Assignment, Submission};
use Illuminate\Foundation\Testing\RefreshDatabase;

uses(RefreshDatabase::class);

it('mengizinkan mahasiswa terdaftar mengumpulkan tugas', function () {
    $course = Course::factory()->create();
    $student = User::factory()->create(['role' => 'mahasiswa']);
    $course->students()->attach($student);
    $assignment = Assignment::factory()->for($course)->create([
        'due_at' => now()->addDay(),
        'status' => 'published',
    ]);

    $response = $this->actingAs($student)->post(
        route('assignments.submissions.store', $assignment),
        ['file' => UploadedFile::fake()->create('tugas.pdf', 200, 'application/pdf')]
    );

    $response->assertRedirect();
    expect(Submission::where('user_id', $student->id)->exists())->toBeTrue();
});
```

**`RefreshDatabase`** membuat database bersih untuk tiap test. Ini alasan migrasi Anda harus reversible sejak minggu 3 — test bergantung padanya.

### Test jalur gagal lebih penting daripada test jalur sukses

Ini poin terpenting minggu ini.

Setiap orang menulis test "fitur ini bekerja". Yang jarang ditulis, padahal jauh lebih bernilai, adalah test **"orang yang tidak berhak ditolak"**:

```php
it('menolak mahasiswa mengunduh submission milik orang lain', function () {
    $budi = User::factory()->create(['role' => 'mahasiswa']);
    $ani  = User::factory()->create(['role' => 'mahasiswa']);
    $submissionAni = Submission::factory()->for($ani)->create();

    $this->actingAs($budi)
        ->get(route('submissions.download', $submissionAni))
        ->assertForbidden();
});

it('menolak dosen mengedit mata kuliah dosen lain', function () {
    $dosenA = User::factory()->create(['role' => 'dosen']);
    $dosenB = User::factory()->create(['role' => 'dosen']);
    $course = Course::factory()->create(['lecturer_id' => $dosenB->id]);

    $this->actingAs($dosenA)
        ->put(route('courses.update', $course), ['name' => 'Diretas'])
        ->assertForbidden();
});
```

Inilah tabel Titik Rawan IDOR dari minggu 5 dan 7, diubah menjadi test yang berjalan otomatis. Setiap baris di `docs/keamanan.md` seharusnya punya satu test yang membuktikannya tertutup.

### Test yang menipu

Test bisa hijau padahal fiturnya rusak:

```php
// ❌ hijau meski otorisasi tidak ada sama sekali
it('menguji halaman', function () {
    $this->get('/courses')->assertStatus(200);
});
```

Tanda-tanda test yang menipu:
- Hanya memeriksa status 200 tanpa memeriksa isi
- Tidak ada assertion sama sekali
- Memeriksa hal yang tidak mungkin gagal
- Bergantung pada data yang sudah ada, bukan yang dibuat sendiri

Cara memverifikasi test Anda bermakna: **rusak kodenya dengan sengaja, dan pastikan test-nya merah.** Test yang tetap hijau setelah fitur dihapus adalah test yang tidak menguji apa pun. Ini persis latihan BREAK yang Anda lakukan sepanjang semester — sekarang diotomatiskan.

### Menguji notifikasi dan berkas tanpa efek samping

```php
Notification::fake();
Queue::fake();
Storage::fake('local');

// ... jalankan aksi ...

Notification::assertSentTo($student, NewAssignmentNotification::class);
Storage::disk('local')->assertExists($submission->file_path);
```

### Test di CI

```yaml
- name: Jalankan test
  run: php artisan test
```

Sudah ada di CI Anda sejak minggu 3. Sekarang isinya bermakna.

---

## 14.2 Prompt Pack — Minggu 14

### A. Prompt Perancangan Test

```
Konteks: LMS Laravel 12. Berikut tabel titik rawan keamanan saya:

[tempel docs/keamanan.md]

Dan berikut alur utama aplikasi:
[sebutkan: enrollment, unggah materi, kumpul tugas, penilaian,
notifikasi]

Rancangkan daftar feature test yang perlu saya tulis.

Ketentuan:
- Kelompokkan menjadi: jalur sukses, jalur ditolak (otorisasi), dan
  jalur validasi gagal.
- Untuk tiap test sebutkan: nama, kondisi awal, aksi, dan hasil yang
  diharapkan.
- Prioritaskan: tandai 10 test yang PALING penting kalau saya hanya
  sempat menulis sepuluh.
- Belum perlu kode.
```

### B. Prompt Implementasi Test

```
Implementasikan 10 test prioritas tadi untuk Laravel 12.

Ketentuan:
- Gunakan RefreshDatabase.
- Buat data sendiri dengan factory, JANGAN bergantung pada seeder.
- Untuk test otorisasi, pastikan yang diperiksa adalah 403, bukan
  sekadar "tidak 200".
- Gunakan Storage::fake dan Notification::fake di test yang relevan.
- Setiap test harus punya assertion yang benar-benar bermakna —
  jangan hanya assertStatus(200).
```

### C. Prompt Verifikasi Kualitas Test

```
Berikut test saya:

[tempel kode test]

Periksa apakah test ini benar-benar bermakna:
1. Untuk tiap test, sebutkan: kalau saya HAPUS fitur yang diujinya,
   apakah test ini akan merah? Kalau tidak, test itu tidak menguji
   apa pun.
2. Adakah test yang hanya memeriksa status tanpa memeriksa isi atau
   efek di database?
3. Adakah test yang bergantung pada urutan eksekusi atau pada data
   dari test lain?
4. Adakah jalur gagal penting yang belum saya uji?

Untuk tiap masalah, tunjukkan perbaikannya.
```

Poin 1 adalah inti minggu ini dan cara paling tajam menilai kualitas test.

### D. Prompt Debugging Sistematis

```
Saya punya bug berikut:

Gejala: [jelaskan]
Langkah reproduksi: [jelaskan]
Yang sudah saya periksa: [jelaskan]

JANGAN memberi solusi.
1. Berdasarkan gejala ini, susun daftar hipotesis penyebab, urut dari
   yang paling mungkin.
2. Untuk tiap hipotesis, sebutkan SATU pemeriksaan yang bisa
   mengonfirmasi atau menggugurkannya.
3. Urutkan pemeriksaan itu dari yang paling murah dilakukan.
4. Tanyakan hasil pemeriksaan pertama kepada saya sebelum melanjutkan.
```

---

## 14.3 Read → Break → Fix → Build

### READ — Baca kegagalan (30 menit)

1. Jalankan `php artisan test`. Catat berapa test yang ada dan berapa yang lolos.
2. Buat satu test sengaja gagal. Baca keluarannya: baris mana yang menyebut berkas Anda?
3. Buka `storage/logs/laravel.log`, cari satu exception lama. Telusuri stack trace-nya sampai menemukan baris pertama di `app/`.
4. Pasang `DB::listen()` di `AppServiceProvider`, buka halaman rekap nilai, baca log query-nya.

### BREAK — Tujuh percobaan (45 menit)

| # | Yang dicoba | Yang harus Anda amati |
|---|-------------|------------------------|
| 1 | Tulis test yang hanya `assertStatus(200)`, lalu hapus seluruh isi controller-nya | **Test tetap hijau** |
| 2 | Hapus `Gate::authorize()` dari satu controller, jalankan test | Apakah ada test yang merah? Kalau tidak, itu celah |
| 3 | Hapus `RefreshDatabase`, jalankan seluruh test dua kali | Test saling mengotori |
| 4 | Tulis test yang bergantung pada data seeder, jalankan di CI | Gagal karena database CI berbeda |
| 5 | Hapus `Storage::fake()`, jalankan test unggah | Berkas sungguhan menumpuk di `storage/` |
| 6 | Rusak satu migrasi `down()`, jalankan test | `RefreshDatabase` gagal |
| 7 | Tinggalkan `dd()` di controller, jalankan test | Seluruh test setelahnya berantakan |

Nomor 1 dan 2 adalah inti. Nomor 2 khususnya: kalau menghapus otorisasi tidak membuat satu pun test merah, seluruh keamanan aplikasi Anda tidak terjaga oleh apa pun.

### FIX — Repo cacat (30 menit)

Branch `w14` pada repo `kampuslms-broken` berisi **8 test yang menipu**: tiga hanya memeriksa status, dua tanpa assertion bermakna, satu bergantung pada seeder, satu bergantung pada test sebelumnya, dan satu memeriksa "tidak 200" padahal seharusnya memeriksa 403.

Perbaiki agar seluruhnya bermakna. **Buktikan** di deskripsi PR: untuk tiap test, tunjukkan bahwa ia merah ketika fitur yang diujinya dirusak.

### BUILD — Test suite KampusLMS

1. **Minimal 20 feature test**, mencakup:
   - Autentikasi: login berhasil, login gagal, akses tanpa login
   - CRUD mata kuliah: sukses oleh admin, ditolak untuk dosen/mahasiswa
   - Enrollment: mahasiswa hanya melihat mata kuliahnya
   - Unggah materi: sukses oleh dosen pengampu, ditolak untuk dosen lain
   - Pengumpulan tugas: sukses, ditolak setelah deadline bila `allow_late` false, ditolak untuk yang tidak terdaftar
   - Unduhan: ditolak untuk bukan pemilik
   - Penilaian: sukses oleh dosen pengampu, ditolak untuk dosen lain
   - Notifikasi: terkirim saat tugas dibuat dan saat nilai diberikan
2. **Setiap baris di `docs/keamanan.md` punya minimal satu test** yang membuktikannya tertutup. Tambahkan kolom "Test" berisi nama test-nya.
3. **`Storage::fake()` dan `Notification::fake()`** dipakai di test yang relevan.
4. **CI menjalankan `php artisan test`** dan hijau.
5. **`docs/debugging.md`** — catat tiga bug nyata yang kelompok Anda temui semester ini: gejalanya, cara Anda menemukan penyebabnya, dan test apa yang sekarang mencegahnya kembali.

Poin 5 sering diremehkan tetapi paling bernilai saat interview: ia membuktikan Anda pernah benar-benar mendiagnosis, bukan sekadar menyalin solusi.

---

## 14.4 Checkpoint Minggu 14

- [ ] Peragakan: hapus satu fitur, tunjukkan test yang menjadi merah karenanya.
- [ ] Kenapa test yang hanya `assertStatus(200)` sering tidak bermakna?
- [ ] Kenapa test jalur ditolak lebih penting daripada test jalur sukses di aplikasi ini?
- [ ] Apa fungsi `RefreshDatabase`? Kenapa migrasi harus reversible agar ia bekerja?
- [ ] Buka `docs/debugging.md`. Ceritakan satu bug: bagaimana Anda mempersempit penyebabnya?
- [ ] Kenapa `dd()` tidak boleh masuk PR, sementara `Log::info()` boleh?

**Kuis 14:** feature vs unit test, `RefreshDatabase`, fake, test yang menipu, metode debugging.

---
---

# MINGGU 15 — Integrasi Proyek dan Perapian Akhir

**Sub-CPMK:** Mahasiswa mampu mengevaluasi dan mengintegrasikan seluruh komponen aplikasi web menjadi sistem yang utuh (C4), menunjukkan sikap bertanggung jawab atas kualitas produk akhir (A5), serta membangun aplikasi web terintegrasi yang siap dipresentasikan (P4).

**Target akhir minggu:** KampusLMS utuh, rapi, terdokumentasi, dan siap dipresentasikan. Tidak ada fitur baru.

---

## 15.1 Konsep

### Aturan minggu ini: berhenti menambah

Godaan terbesar minggu 15 adalah menambah "satu fitur lagi". Jangan.

Fitur yang ditulis di minggu terakhir tidak sempat direview, tidak punya test, dan hampir pasti merusak sesuatu yang sudah jalan. Nilai UAS jauh lebih terpengaruh oleh aplikasi yang **utuh dan bisa dijelaskan** daripada aplikasi dengan satu fitur ekstra yang setengah jadi.

Yang dikerjakan minggu ini: menutup celah, merapikan, mendokumentasikan.

### Integrasi: apakah semuanya benar-benar menyatu?

Selama ini setiap fitur diuji sendiri-sendiri. Sekarang jalankan **alur penuh dari nol**, sebagai satu cerita:

1. Admin membuat mata kuliah, menugaskan dosen, mendaftarkan 20 mahasiswa
2. Dosen mengunggah materi, membuat tugas dengan deadline besok
3. Seluruh mahasiswa menerima notifikasi
4. 15 mahasiswa mengumpulkan tepat waktu, 3 terlambat, 2 tidak mengumpulkan
5. Dosen melihat daftar pengumpulan, menilai seluruhnya
6. Mahasiswa menerima notifikasi nilai, melihat rekapnya
7. Admin membuka statistik mata kuliah

Jalankan di **server produksi**, bukan lokal. Catat setiap yang janggal di `docs/integrasi.md`.

Yang biasanya ketahuan di tahap ini: notifikasi ganda, hitungan yang tidak cocok antara dua halaman, status "terlambat" yang tidak konsisten, dan halaman yang kosong ketika datanya nol (*empty state* terlupakan).

### Empty state dan error state

Aplikasi mahasiswa hampir selalu terlihat bagus dengan data seeder dan berantakan ketika kosong. Periksa setiap halaman daftar dalam tiga kondisi:

| Kondisi | Yang harus terlihat |
|---------|---------------------|
| Kosong | Pesan yang menjelaskan dan mengarahkan ("Belum ada tugas. Dosen akan menambahkannya.") |
| Normal | Data, terpaginasi |
| Sangat banyak | Tetap terpaginasi, tidak melebar keluar layar |

Dan kondisi gagal: halaman 403, 404, 419, 500 harus punya tampilan yang layak, tidak membocorkan informasi, dan punya tautan kembali.

### Konsistensi

Kumpulkan dan seragamkan:
- **Bahasa.** Pilih satu — Indonesia atau Inggris — untuk antarmuka. Campur aduk terlihat ceroboh.
- **Istilah.** "Tugas" atau "Assignment"? "Pengumpulan" atau "Submission"? Satu istilah untuk satu hal, di seluruh aplikasi.
- **Format tanggal.** Satu format, satu zona waktu (`APP_TIMEZONE=Asia/Jakarta`).
- **Pesan sukses/gagal.** Nada dan penempatan yang sama.
- **Tombol.** Aksi merusak (hapus) selalu punya konfirmasi dan warna berbeda.

### Membersihkan kode

```bash
composer require laravel/pint --dev
./vendor/bin/pint
```

Yang dicari secara manual:
- `dd()`, `dump()`, `console.log` yang tertinggal
- Route yang tidak dipakai (`php artisan route:list` vs yang benar-benar ditautkan)
- Controller yang menganggur
- View yang tidak pernah dipanggil
- Komentar `// TODO` yang sudah tidak relevan
- Kode yang dikomentari — hapus, Git yang menyimpan riwayat
- Duplikasi antar controller yang seharusnya jadi satu method di model

### Membaca kode anggota lain

Latihan wajib minggu ini: **setiap anggota membaca kode anggota lain.**

Selama 14 minggu Anda mungkin hanya menguasai bagian sendiri. Di UAS, penguji bisa bertanya tentang bagian mana pun, dan komponen Kontribusi Individu menilai pemahaman Anda atas keseluruhan.

Kerjakan berpasangan: A menjelaskan kodenya ke B, B bertanya sampai paham, lalu bertukar. Catat pertanyaan yang tidak terjawab — itu daftar pekerjaan Anda.

### Dokumentasi akhir

`README.md` yang lengkap:

```markdown
# KampusLMS — Kelompok XX

[Tautan aplikasi live]

## Anggota
| Nama | NIM | Bagian yang dikerjakan |

## Fitur
### Wajib
### Diferensiasi: [nama fitur]

## Teknologi
Laravel 12, MySQL, [jalur frontend], queue database

## Instalasi lokal
## Akun demo
## Menjalankan test
## Deployment
## Struktur dokumentasi
- docs/api.md — kontrak API
- docs/keamanan.md — titik rawan dan penanganannya
- docs/performa.md — pengukuran sebelum/sesudah
- docs/deployment.md — langkah dan kendala
- docs/debugging.md — bug yang ditemui
- docs/integrasi.md — hasil uji alur penuh
```

**Tabel pembagian bagian harus jujur** dan cocok dengan `git shortlog`. Ketidakcocokan antara klaim di README dan riwayat Git akan ditanyakan di UAS.

### Laporan proyek

Sesuai rubrik RPS, laporan bernilai 10% dari nilai proyek. Isi minimal:

1. Pendahuluan — latar belakang, tujuan, ruang lingkup
2. Analisis kebutuhan — peran, fitur, batasan
3. Perancangan — ERD, struktur route, kontrak API
4. Implementasi — teknologi, arsitektur, keputusan penting **beserta alasannya**
5. Pengujian — daftar test, hasil, cakupan
6. Keamanan — titik rawan dan penanganannya
7. Deployment — arsitektur server, kendala
8. **Refleksi** — apa yang berhasil, apa yang tidak, apa yang akan dilakukan berbeda
9. Pembagian kerja
10. Lampiran — tangkapan layar, keluaran test

Bagian 8 dinilai paling tinggi bobotnya. Kelompok yang jujur menuliskan kegagalan dan pelajarannya mendapat nilai lebih baik daripada yang menulis seolah semuanya lancar.

---

## 15.2 Prompt Pack — Minggu 15

### A. Prompt Audit Menyeluruh

```
Berikut struktur proyek dan daftar route saya:

[tempel php artisan route:list --except-vendor]
[tempel struktur folder app/]

Lakukan audit kelengkapan dan konsistensi:
1. Adakah route yang terdaftar tetapi tidak pernah ditautkan dari mana
   pun (kemungkinan sisa percobaan)?
2. Adakah pola penamaan yang tidak konsisten antar modul?
3. Adakah modul yang punya fitur lengkap sementara modul serupa tidak
   (misalnya satu punya pencarian, yang lain tidak)?
4. Adakah controller yang jauh lebih gemuk dari yang lain, menandakan
   logika yang seharusnya dipindah?

Jangan perbaiki. Buat daftar temuan berurut prioritas.
```

### B. Prompt Uji Empty State

```
Berikut view daftar saya:

[tempel kode]

Periksa perilakunya dalam tiga kondisi: nol data, data normal, dan
data sangat banyak.

Untuk tiap kondisi, sebutkan:
- apa yang akan dilihat pengguna
- apakah ada yang rusak atau membingungkan
- apa yang seharusnya ditampilkan

Fokus khusus pada kondisi nol data — sebutkan pesan yang sebaiknya
muncul dan aksi apa yang sebaiknya ditawarkan.
```

### C. Prompt Persiapan Presentasi

```
Berikut ringkasan proyek saya:

[tempel README.md]

Bantu saya menyiapkan presentasi 15 menit.

1. Susun alur demo yang paling meyakinkan: urutan apa yang menunjukkan
   sistem ini utuh, bukan sekadar kumpulan fitur?
2. Bagian mana yang paling layak ditonjolkan, dan bagian mana yang
   sebaiknya disebut singkat saja?
3. Buat daftar 10 pertanyaan tersulit yang mungkin diajukan penguji
   tentang proyek ini.
4. Untuk tiap pertanyaan, jangan beri jawabannya — sebutkan bagian kode
   atau dokumen mana yang harus saya kuasai untuk menjawabnya.
```

Poin 4 sengaja: mahasiswa perlu menyiapkan pemahaman, bukan menghafal jawaban.

### D. Prompt Refleksi Laporan

```
Berikut riwayat proyek kelompok saya:

[tempel git log --oneline]
[tempel docs/debugging.md]
[tempel docs/performa.md]

Bantu saya menyusun kerangka bagian REFLEKSI laporan.

1. Dari riwayat ini, apa saja keputusan yang tampaknya menghabiskan
   waktu paling banyak?
2. Adakah pola pengerjaan yang menandakan masalah (misalnya banyak
   perbaikan berulang di area yang sama)?
3. Ajukan 5 pertanyaan reflektif yang harus saya jawab sendiri untuk
   bagian ini.

Jangan menuliskan refleksinya untuk saya — itu harus dari pengalaman
kami sendiri.
```

---

## 15.3 Read → Break → Fix → Build

### READ — Baca kode anggota lain (45 menit)

Berpasangan, bergantian:
1. A membuka satu fitur yang ia tulis, menjelaskan alurnya ke B.
2. B bertanya sampai bisa menjelaskan ulang fitur itu tanpa bantuan A.
3. Bertukar peran.
4. Catat di `docs/minggu-15-<nama>.md`: fitur apa yang Anda pelajari, dan pertanyaan apa yang belum terjawab.

Setiap anggota harus bisa menjelaskan **minimal satu fitur yang bukan tulisannya** di UAS.

### BREAK — Uji ketahanan (40 menit)

Kali ini bukan merusak kode, melainkan menguji aplikasi dalam kondisi yang tidak nyaman:

| # | Yang dicoba | Yang harus Anda amati |
|---|-------------|------------------------|
| 1 | `migrate:fresh` tanpa seeder, buka seluruh halaman | Empty state di mana-mana |
| 2 | Buat mata kuliah tanpa mahasiswa, buka rekap nilai | Pembagian dengan nol? |
| 3 | Buat tugas dengan deadline kemarin, coba kumpulkan | Perilaku terlambat konsisten? |
| 4 | Nama mata kuliah 200 karakter, nama berkas panjang sekali | Tata letak jebol? |
| 5 | Buka aplikasi di layar HP | Tabel melebar keluar layar? |
| 6 | Akses `/dashboard` tanpa login | Redirect ke login, bukan error |
| 7 | Ubah `APP_TIMEZONE`, periksa seluruh tampilan tanggal | Konsisten? |
| 8 | Matikan queue worker, kumpulkan tugas | Aplikasi tetap jalan, hanya notifikasi tertunda |

Nomor 5 sering terlupakan padahal penguji kemungkinan besar membuka aplikasi Anda dari HP saat UAS.

### FIX — Perbaiki temuan sendiri (sisa praktikum)

Tidak ada repo cacat minggu ini. Yang diperbaiki adalah temuan Anda sendiri dari bagian READ dan BREAK, dan temuan dari audit Prompt A.

Prioritas: keamanan → kebenaran → empty state → konsistensi → kerapian.

### BUILD — Perapian akhir

1. **Uji alur penuh di server produksi**, terdokumentasi di `docs/integrasi.md` beserta temuan dan perbaikannya.
2. **Empty state** di seluruh halaman daftar.
3. **Halaman error kustom**: 403, 404, 419, 500.
4. **Konsistensi bahasa, istilah, format tanggal, dan pesan** di seluruh aplikasi.
5. **Pint dijalankan**, tidak ada `dd()`/`dump()`/`console.log` tertinggal, kode mati dihapus.
6. **README lengkap** dengan tabel pembagian yang jujur.
7. **Seluruh dokumen di `docs/` final**: api, keamanan, performa, deployment, debugging, integrasi.
8. **Laporan proyek** selesai, termasuk bagian refleksi.
9. **Seluruh test hijau di CI**, dan aplikasi produksi memakai versi terbaru.
10. **Setiap anggota menguasai minimal satu fitur yang bukan tulisannya.**

---

## 15.4 Checkpoint Minggu 15

- [ ] Jalankan alur penuh dari nol di server produksi tanpa terhenti.
- [ ] Tunjukkan satu halaman dalam kondisi nol data. Apakah jelas bagi pengguna?
- [ ] Buka aplikasi dari HP. Ada yang rusak?
- [ ] Jelaskan satu fitur yang **bukan** Anda yang menulisnya.
- [ ] Tunjukkan bahwa tabel pembagian di README cocok dengan `git shortlog`.
- [ ] Seluruh test hijau? Tunjukkan keluaran CI terakhir.

**Kuis 15:** integrasi, empty state, konsistensi, kerapian kode.

---
---

# MINGGU 16 — UAS: Presentasi Proyek dan Interview Individu

**Bobot:** Proyek 20% nilai akhir
**Bentuk:** Presentasi kelompok + demo + interview individu

---

## 16.1 Format

**30 menit per kelompok**, dibagi:

| Waktu | Bagian | Siapa |
|-------|--------|-------|
| 5 menit | **Presentasi** — latar belakang, arsitektur, keputusan penting | Kelompok |
| 8 menit | **Demo** alur penuh + fitur diferensiasi | Kelompok, bergantian |
| 12 menit | **Interview individu** — 3 menit per anggota | Perorangan |
| 5 menit | **Uji langsung** — keamanan, performa, ketahanan | Penguji |

### Presentasi (5 menit)

Bukan menceritakan ulang seluruh fitur. Fokus pada:
- Masalah apa yang diselesaikan dan untuk siapa
- Arsitektur: jalur frontend yang dipilih dan **alasannya**
- Dua atau tiga keputusan teknis paling penting beserta alasannya
- Satu kegagalan yang dialami dan pelajarannya

Bagian terakhir wajib. Kelompok yang mengaku tidak mengalami kegagalan apa pun kehilangan poin, bukan mendapat poin.

### Demo (8 menit)

Dari server produksi, dibuka lewat internet, memakai akun demo. Alur penuh sesuai `docs/integrasi.md`, plus fitur diferensiasi kelompok.

Setiap anggota mendemonstrasikan bagian yang berbeda.

### Interview Individu (12 menit — 3 menit per orang)

Ini yang paling menentukan. Penguji bertanya secara perorangan, dengan kode terbuka.

Setiap anggota mendapat **tiga pertanyaan**:

1. **Satu tentang kode yang ia tulis** (diverifikasi lewat `git log`)
2. **Satu tentang kode yang bukan ia tulis** — menguji apakah ia memahami sistemnya secara utuh
3. **Satu tentang penggunaan AI** — bagian mana yang dibantu AI, apa yang diubah, dan mengapa

Bank pertanyaan (dibuka sejak awal semester, gabungan dari seluruh checkpoint):

**Struktur data**
- Kenapa `course_user` punya unique composite? Peragakan tanpa itu.
- Kenapa `lecturer_id` `restrictOnDelete` sementara `materials.course_id` `cascadeOnDelete`?
- Kenapa `grades.submission_id` unique, bukan index biasa?

**Keamanan**
- Tunjukkan apa yang mencegah mahasiswa A membuka submission mahasiswa B.
- Apa itu mass assignment? Peragakan serangannya dengan `curl`.
- Kenapa `@can` di Blade tidak cukup? Peragakan.
- Kenapa submission tidak disimpan di `storage/app/public`?
- Kenapa pesan gagal login tidak membedakan email salah dan password salah?
- Apa risiko `APP_DEBUG=true` di produksi?

**Performa**
- Di halaman rekap nilai, berapa query yang dijalankan? Bagaimana Anda menguranginya?
- Apa beda `withCount()` dan `count($relasi)`?
- Data apa yang sengaja **tidak** Anda cache, dan kenapa?

**Asinkron dan deployment**
- Apa yang terjadi kalau queue worker mati? Bagaimana Anda mengetahuinya?
- Kenapa `queue:restart` wajib setelah deployment?
- Apa yang terjadi pada `env()` setelah `config:cache`?

**Testing**
- Tunjukkan satu test, lalu rusak fitur yang diujinya. Apakah merah?
- Kenapa test jalur ditolak lebih penting daripada jalur sukses?

**AI**
- Tunjukkan bagian yang dibantu AI. Apa yang Anda ubah, dan kenapa?
- Pernahkah AI memberi Anda jawaban yang salah? Bagaimana Anda tahu?

Pertanyaan terakhir sangat informatif. Mahasiswa yang benar-benar bekerja dengan AI selama satu semester pasti punya cerita — biasanya tentang kode Laravel 10 yang menyebut `Kernel.php`.

### Uji Langsung (5 menit)

Penguji, di depan kelompok:
1. Menjalankan satu skenario IDOR dari `docs/keamanan.md`
2. Membuka satu halaman sambil memeriksa jumlah query
3. Mematikan koneksi ke satu bagian atau mengosongkan data, melihat apakah aplikasi tetap layak
4. `curl -I https://domain/.env`

---

## 16.2 Rubrik UAS / Proyek Akhir (20%)

Mengacu rubrik RPS.

| Kriteria | Bobot | 4 (Sangat Baik) | 3 (Baik) | 2 (Cukup) | 1 (Kurang) |
|----------|-------|-----------------|----------|-----------|------------|
| **Fungsionalitas** | 20% | Seluruh fitur wajib + diferensiasi berjalan di produksi tanpa kendala | Berjalan, kendala kecil | Beberapa fitur wajib belum jalan | Alur inti tidak dapat didemonstrasikan |
| **Implementasi konsep** | 20% | Skema, otorisasi, asinkron, optimisasi, deployment seluruhnya sesuai spesifikasi | Sesuai dengan sedikit penyimpangan | Ada konsep inti yang belum diterapkan | Banyak menyimpang |
| **Pemahaman individu** | 25% | Menjawab ketiga pertanyaan lancar, termasuk kode yang bukan tulisannya | Menjawab dengan sedikit bantuan | Hanya menguasai bagiannya sendiri | Tidak dapat menjelaskan kode atas namanya |
| **Kualitas kode** | 10% | Bersih, konsisten, tanpa duplikasi berarti, test bermakna | Rapi dengan sedikit kekurangan | Ada duplikasi dan kode mati | Berantakan |
| **Laporan** | 10% | Lengkap, dan refleksinya jujur serta mendalam | Lengkap, refleksi dangkal | Kurang lengkap | Tidak memadai |
| **Presentasi** | 5% | Terstruktur, tepat waktu, meyakinkan | Baik | Kurang terstruktur | Tidak siap |
| **Kontribusi individu** | 10% | Merata, terbukti di `git log` dan cocok dengan README | Cukup merata | Timpang | Nyaris tidak berkontribusi |

**Pemahaman individu dan kontribusi individu bersifat perorangan** (35% total). Sisanya nilai kelompok.

### Ketentuan gugur

Berlaku pada komponen Implementasi Konsep, sesuai standar minggu 12:

- `APP_DEBUG=true` di produksi
- `.env` dapat diakses publik
- Berkas submission dapat diunduh tanpa autentikasi
- Terdapat IDOR yang belum tertutup pada data nilai atau submission

Keempatnya membocorkan data sungguhan kalau aplikasi ini benar-benar dipakai. Kelompok diberi kesempatan memperbaiki sebelum penilaian akhir ditutup.

### Ketentuan integritas

Sesuai kontrak kuliah RPS: plagiat bernilai 0. Untuk mata kuliah ini secara khusus:

- **Menyalin kode kelompok lain** tanpa atribusi — nilai proyek 0 untuk kelompok penyalin.
- **Memakai AI diperbolehkan penuh.** Yang bernilai 0 adalah kode yang tidak dapat dijelaskan penulisnya, bukan kode yang dibantu AI.
- **Commit yang seluruhnya atas nama satu orang** — anggota lain mendapat 0 pada komponen kontribusi individu.
- **Klaim di README yang tidak cocok dengan `git log`** ditanyakan langsung saat interview.

---

## 16.3 Panduan Pelaksanaan

Pembagian sesi, pembagian peran dosen dan asdos, serta persiapan teknis sebelum hari-H ada pada dokumen pengajar (`pengajar/01-panduan-pelaksanaan-asesmen.md`). Yang perlu Anda ketahui sebagai peserta: **uji langsung dijalankan dengan skrip yang seragam untuk seluruh kelompok**, dan `git shortlog` Anda dicocokkan dengan tabel kontribusi pada README repo.

---

## 16.4 Ringkasan Komponen Nilai Semester

| Komponen | Bobot | Minggu | Bentuk |
|----------|------:|--------|--------|
| Kuis | 10% | 1, 2, 4, 5, 6, 9, 11, 13, 14, 15 | Auto-graded LMS, 5 soal, akhir praktikum — 1% per kuis |
| Tugas 1 | 3% | 3 | Milestone M1 + interview |
| Tugas 2 | 3% | 7 | Milestone M2 + interview |
| Tugas 3 | 7% | 10 | Milestone M3 + interview |
| Tugas 4 | 7% | 12 | Milestone M4 + interview |
| UTS | 20% | 8 | Demo + code walkthrough |
| Praktikum | 20% | 1, 2, 4, 5, 6, 9, 11, 13, 14, 15 | Checkpoint mingguan — 2% per pekan |
| Review | 10% | 13 | Peer review individual di GitHub |
| Proyek / UAS | 20% | 16 | Presentasi + interview individu |
| **Total** | **100%** | | |

---

*Selesai. Modul lengkap Minggu 1–16.*
