# MODUL PEMROGRAMAN WEB — MINGGU 9–12

**SI2514024 | Proyek: KampusLMS | Laravel 12**

> Lanjutan dari Modul Minggu 4–7. Format tetap: **Konsep → Prompt Pack → Read-Break-Fix-Build → Checkpoint**.
>
> Empat minggu ini mengubah KampusLMS dari "aplikasi yang jalan di laptop saya" menjadi "aplikasi yang jalan di internet dan tidak roboh saat dipakai bersama". Minggu 12 aplikasi Anda harus benar-benar bisa dibuka orang lain dari HP mereka.

---
---

# MINGGU 9 — Upload File dan Data Dinamis

**Sub-CPMK:** Mahasiswa mampu menerapkan pengelolaan unggahan berkas dan data dinamis pada aplikasi web (C3), menunjukkan ketelitian dalam menangani berkas pengguna (A3), serta membangun fitur unggah dan penyajian data dinamis (P3).

**Target akhir minggu:** Dosen bisa mengunggah materi, mahasiswa bisa mengumpulkan tugas, dan **tidak seorang pun bisa mengunduh berkas milik orang lain**.

---

## 9.1 Konsep

### Dua jenis berkas, dua perlakuan berbeda

Ini keputusan desain paling penting minggu ini, dan paling sering salah:

| Jenis | Contoh di KampusLMS | Disimpan di | Diakses lewat |
|-------|---------------------|-------------|---------------|
| **Publik** | Foto profil, logo kampus | `storage/app/public` + `storage:link` | URL langsung |
| **Privat** | Materi kuliah, pengumpulan tugas | `storage/app/private` | Controller + otorisasi |

Spesifikasi proyek melarang menyimpan materi dan submission di `storage/app/public`. Alasannya sederhana: apa pun yang ada di sana bisa diunduh **siapa saja** yang tahu URL-nya. Tidak ada login, tidak ada Policy, tidak ada apa pun yang menghalangi.

Dan URL-nya mudah ditebak. Kalau Anda menyimpan dengan nama asli, `/storage/submissions/tugas-budi.pdf` bisa ditebak dalam sekali coba. Kalau Anda menyimpan dengan nama acak, cukup satu mahasiswa membagikan tautannya di grup WhatsApp dan seluruh angkatan punya jawabannya.

### Menyimpan berkas privat

```php
public function store(StoreSubmissionRequest $request, Assignment $assignment)
{
    Gate::authorize('submit', $assignment);

    $file = $request->file('file');

    $path = $file->store("submissions/{$assignment->id}", 'local');
    // tersimpan di storage/app/private/submissions/12/xxxxx.pdf

    Submission::updateOrCreate(
        ['assignment_id' => $assignment->id, 'user_id' => $request->user()->id],
        [
            'file_path'     => $path,
            'original_name' => $file->getClientOriginalName(),
            'file_size'     => $file->getSize(),
            'submitted_at'  => now(),
            'is_late'       => now()->gt($assignment->due_at),
        ]
    );

    return back()->with('success', 'Tugas berhasil dikumpulkan.');
}
```

Perhatikan tiga hal:

**`$file->store()` menghasilkan nama acak.** Jangan pakai `storeAs()` dengan nama asli dari pengguna — nama berkas adalah masukan yang tidak dipercaya. Nama seperti `../../.env` atau `laporan.php` bisa menimbulkan masalah serius. Simpan nama aslinya di kolom `original_name` untuk ditampilkan, tapi berkas fisiknya bernama acak.

**`updateOrCreate` menegakkan unique composite** dari minggu 3. Mahasiswa mengumpulkan ulang → berkas lama diganti, bukan menumpuk baris baru.

**`is_late` dihitung di server.** Jangan percaya waktu dari browser.

### Mengunduh berkas privat dengan otorisasi

```php
public function download(Submission $submission)
{
    Gate::authorize('view', $submission);

    abort_unless(Storage::disk('local')->exists($submission->file_path), 404);

    return Storage::disk('local')->download(
        $submission->file_path,
        $submission->original_name
    );
}
```

Setiap unduhan melewati controller, dan setiap controller memanggil Policy. Inilah yang membedakan berkas privat dari publik.

### Validasi berkas: yang benar dan yang menipu

```php
'file' => ['required', 'file', 'mimes:pdf,doc,docx', 'max:5120'],   // 5 MB
```

Yang perlu Anda pahami:

- **`max` dalam kilobyte**, bukan megabyte. `max:5120` = 5 MB. Salah tafsir di sini menghasilkan batas 5 KB.
- **`mimes` memeriksa isi berkas**, bukan hanya ekstensinya. Laravel membaca *magic bytes*. Berkas PHP yang diganti namanya menjadi `.pdf` akan tertolak.
- **`extensions`** (Laravel 11+) memeriksa ekstensi saja. Untuk keamanan gunakan `mimes` atau keduanya.
- Untuk gambar, `dimensions:max_width=2000,max_height=2000` mencegah unggahan berukuran ekstrem yang menghabiskan memori saat diproses.

⚠️ **Batas PHP mengalahkan batas Laravel.** Validasi `max:5120` tidak ada gunanya kalau `upload_max_filesize` di `php.ini` bernilai 2M — berkas ditolak server sebelum Laravel melihatnya, dan pesan errornya membingungkan. Periksa `upload_max_filesize` dan `post_max_size`. Ini akan menggigit Anda lagi saat deployment di minggu 12.

### Menghapus berkas saat data dihapus

```php
protected static function booted(): void
{
    static::deleting(function (Submission $submission) {
        Storage::disk('local')->delete($submission->file_path);
    });
}
```

Tanpa ini, `storage/` Anda akan penuh berkas yatim yang tidak punya baris database — dan tetap bisa diakses kalau ada yang menemukan path-nya.

Hati-hati dengan `cascadeOnDelete` dari minggu 3: menghapus mata kuliah akan menghapus baris tugas dan submission di database, tetapi **event model tidak terpicu** karena penghapusan terjadi di level database. Berkasnya tertinggal. Untuk kasus itu, hapus berkas secara eksplisit di controller sebelum menghapus induknya.

### Data dinamis tanpa memuat ulang halaman

Untuk jalur Blade + Livewire:

```php
class SubmissionList extends Component
{
    public Assignment $assignment;

    public function render()
    {
        return view('livewire.submission-list', [
            'submissions' => $this->assignment->submissions()
                ->with('user', 'grade')
                ->latest('submitted_at')
                ->paginate(20),
        ]);
    }
}
```

Untuk jalur SPA/Inertia, endpoint API dari minggu 6 sudah menyediakan datanya — cukup panggil dan render.

**Berlaku untuk semua jalur:** otorisasi tetap di server. Komponen Livewire dan endpoint API sama-sama harus memanggil Policy. Menyembunyikan tombol di komponen bukan pengaman — pelajaran minggu 7 berlaku persis sama di sini.

---

## 9.2 Prompt Pack — Minggu 9

### A. Prompt Perancangan Penyimpanan

```
Konteks: LMS Laravel 12. Ada tiga jenis berkas:
1. Foto profil pengguna
2. Materi kuliah yang diunggah dosen (PDF/PPTX)
3. Pengumpulan tugas mahasiswa (PDF/DOCX)

Untuk masing-masing, tentukan:
- disk penyimpanan yang tepat (public vs private/local) dan alasannya
- struktur folder
- bagaimana berkas diakses pengguna
- siapa yang berhak mengaksesnya

Sajikan sebagai tabel. Untuk yang privat, jelaskan mengapa menyimpan
di storage/app/public akan membocorkan data meskipun aplikasi punya
login dan Policy.

Belum perlu kode.
```

### B. Prompt Implementasi Unggah Aman

```
Implementasikan fitur pengumpulan tugas untuk Laravel 12 berdasarkan
rancangan tadi.

Ketentuan:
- Berkas disimpan di disk privat, BUKAN storage/app/public.
- Nama berkas fisik harus acak; nama asli disimpan di kolom terpisah.
- Validasi: mimes pdf/doc/docx, maksimal 5 MB, wajib ada.
- Terapkan updateOrCreate agar mahasiswa yang mengumpulkan ulang
  menimpa submission lama, sesuai unique composite
  (assignment_id, user_id).
- Hitung is_late di server berdasarkan due_at.
- Buatkan juga endpoint download yang memanggil Policy lewat
  Gate::authorize() sebelum mengirim berkas. Ingat: $this->authorize()
  tidak tersedia di Laravel 12.
- Tambahkan penghapusan berkas fisik saat record dihapus, dan jelaskan
  kasus di mana event model TIDAK terpicu.
```

### C. Prompt Uji Tembus Berkas

```
Berikut controller unggah dan unduh saya:

[tempel kode]

Berperanlah sebagai mahasiswa yang ingin membaca pengumpulan tugas
teman sekelasnya.

1. Daftarkan semua cara saya bisa mencoba mengakses berkas orang lain:
   tebak URL, ubah ID di endpoint download, akses path storage langsung,
   dan lainnya.
2. Untuk tiap cara, sebutkan apakah kode saya menahannya.
3. Uji juga sisi unggah: apa yang terjadi kalau saya mengunggah berkas
   PHP yang saya ganti namanya menjadi .pdf? Bagaimana kalau nama
   berkasnya saya isi dengan ../../.env?
4. Beri perintah curl untuk membuktikan tiap skenario.
```

### D. Prompt Debugging Unggahan

```
Unggahan saya gagal dengan gejala berikut:

[jelaskan gejalanya — misalnya berkas 3 MB ditolak padahal validasi
max:5120, atau halaman langsung error tanpa pesan validasi]

JANGAN langsung memberi solusi.
1. Jelaskan lapisan apa saja yang dilewati sebuah unggahan, dari browser
   sampai tersimpan di disk, dan di lapisan mana saja ia bisa gagal.
2. Sebutkan pengaturan server yang bisa membatalkan unggahan SEBELUM
   Laravel melihatnya.
3. Tanyakan kepada saya apa yang perlu saya periksa untuk mempersempit
   penyebabnya.
```

---

## 9.3 Read → Break → Fix → Build

### READ — Telusuri satu berkas (30 menit)

Unggah satu berkas materi, lalu tanpa AI:

1. Temukan berkas fisiknya di sistem berkas. Path lengkapnya apa?
2. Buka baris database yang bersangkutan. Bandingkan `file_path` dengan lokasi fisiknya.
3. Coba akses berkas itu langsung lewat browser dengan menebak URL-nya. Berhasil atau tidak? Kenapa?
4. Buka `config/filesystems.php`. Identifikasi disk `local` dan `public` — apa persis bedanya?
5. Jalankan `php artisan storage:link`. Apa yang dibuatnya? Kenapa materi Anda **tidak boleh** ada di sana?

### BREAK — Delapan kerusakan (50 menit)

| # | Yang dicoba | Yang harus Anda amati |
|---|-------------|------------------------|
| 1 | Pindahkan berkas submission ke `storage/app/public`, akses URL-nya tanpa login | **Tugas seluruh kelas terbuka untuk publik** |
| 2 | Hapus `Gate::authorize()` dari endpoint download, akses submission orang lain | IDOR versi berkas |
| 3 | Simpan berkas dengan `storeAs()` memakai nama asli, unggah berkas bernama `../../test.txt` | Path traversal |
| 4 | Unggah berkas PHP yang diganti namanya jadi `.pdf` dengan validasi `extensions`, lalu dengan `mimes` | Beda memeriksa ekstensi vs isi |
| 5 | Ubah `max:5120` menjadi `max:5`, unggah berkas 1 MB | Satuannya kilobyte |
| 6 | Set `upload_max_filesize=1M` di php.ini, unggah berkas 3 MB dengan validasi `max:5120` | **Batas server mengalahkan validasi** |
| 7 | Hapus `updateOrCreate`, ganti `create`, kumpulkan tugas dua kali | Unique constraint minggu 3 menyelamatkan Anda |
| 8 | Hapus satu submission dari database lewat SQL langsung, cek folder storage | Berkas yatim tertinggal |

Nomor 1 wajib benar-benar dicoba. Membuka tugas teman sekelas dari jendela penyamaran, tanpa login, adalah pengalaman yang menjelaskan seluruh minggu ini dalam sepuluh detik.

### FIX — Repo cacat (30 menit)

Branch `w09` pada repo `kampuslms-broken` berisi **7 masalah**: submission disimpan di disk publik, endpoint download tanpa `Gate::authorize()`, `storeAs()` memakai nama asli, validasi hanya memeriksa ekstensi, `max` salah satuan, tidak ada penghapusan berkas fisik, dan komponen Livewire yang menyembunyikan tombol tanpa otorisasi di server.

Perbaiki, kirim PR. Sertakan bukti: URL yang tadinya bisa dibuka tanpa login, dan hasilnya setelah perbaikan.

### BUILD — Alur berkas KampusLMS

1. **Unggah materi** oleh dosen: PDF/PPTX/DOCX, maksimal 20 MB, disk privat, atau alternatif berupa tautan eksternal.
2. **Unduh materi** oleh mahasiswa yang terdaftar di mata kuliah tersebut — lewat controller + Policy.
3. **Pengumpulan tugas** oleh mahasiswa: `updateOrCreate`, `is_late` dihitung server, tolak jika `allow_late` bernilai false dan sudah lewat deadline.
4. **Unduh submission**: hanya pemiliknya, dosen pengampu, dan admin.
5. **Daftar pengumpulan** untuk dosen: siapa sudah/belum mengumpulkan, ditandai terlambat.
6. **Penghapusan berkas fisik** saat record dihapus, termasuk penanganan kasus cascade.
7. Tampilkan `original_name` dan ukuran berkas dalam format yang terbaca manusia.

---

## 9.4 Checkpoint Minggu 9

- [ ] Kenapa submission tidak boleh disimpan di `storage/app/public`? Peragakan kebocorannya.
- [ ] Kenapa nama berkas fisik dibuat acak? Serangan apa yang dicegah?
- [ ] Apa beda validasi `mimes` dan `extensions`? Mana yang lebih aman dan kenapa?
- [ ] `max:5120` berarti berapa MB? Apa yang bisa membatalkan batas ini di level server?
- [ ] Tunjukkan di kode Anda apa yang mencegah mahasiswa mengunduh submission mahasiswa lain.
- [ ] Kenapa `is_late` dihitung di server, bukan dikirim dari browser?

**Kuis 9:** disk publik vs privat, validasi berkas, penamaan berkas, otorisasi unduhan.

---
---

# MINGGU 10 — Realtime, Event, Queue, dan Notifikasi

**Sub-CPMK:** Mahasiswa mampu menerapkan fitur realtime dan notifikasi pada aplikasi web (C3), menunjukkan sikap responsif terhadap kebutuhan interaksi pengguna (A4), serta membangun fitur notifikasi yang berjalan asinkron (P4).

**Target akhir minggu:** Notifikasi KampusLMS berjalan lewat queue, dan mahasiswa mendapat pemberitahuan saat ada tugas baru atau nilai keluar.

> ⚠️ **Minggu ini adalah Milestone M3 — Tugas 3 (7%) + interview kelompok.**

---

## 10.1 Konsep

### Masalahnya: pekerjaan lambat menyandera pengguna

Ketika dosen membuat tugas baru untuk mata kuliah berisi 40 mahasiswa, aplikasi harus mengirim 40 notifikasi. Kalau itu dilakukan langsung dalam request:

```php
public function store(StoreAssignmentRequest $request, Course $course)
{
    $assignment = $course->assignments()->create($request->validated());

    foreach ($course->students as $student) {
        $student->notify(new NewAssignmentNotification($assignment));   // ❌ lambat
    }

    return redirect()->route('assignments.show', $assignment);
}
```

Dosen menekan "Simpan" dan menunggu. Kalau notifikasinya lewat email, 40 kali koneksi SMTP bisa memakan 30 detik atau lebih — dan kalau satu email gagal, seluruh request gagal dan tugasnya mungkin tidak tersimpan.

**Prinsipnya: request hanya mengerjakan yang harus selesai sekarang. Sisanya dititipkan.**

### Queue: menitipkan pekerjaan

```
Request  →  simpan tugas  →  masukkan job ke queue  →  response (cepat)
                                        ↓
                         Worker (proses terpisah) mengambil job
                                        ↓
                              kirim 40 notifikasi
```

Konfigurasi minimum untuk KampusLMS:

```bash
php artisan make:queue-table
php artisan migrate
```

```env
QUEUE_CONNECTION=database
```

Menjalankan worker:

```bash
php artisan queue:work
```

⚠️ **Ini yang paling sering membuat mahasiswa bingung:** kalau `QUEUE_CONNECTION=database` tetapi worker tidak dijalankan, job hanya menumpuk di tabel `jobs` dan **tidak ada notifikasi yang pernah terkirim**. Tidak ada pesan error. Aplikasi terlihat normal. Ini bukan bug — ini memang cara kerjanya.

Sebaliknya, `QUEUE_CONNECTION=sync` menjalankan job langsung tanpa queue. Berguna saat mengembangkan, tetapi menghilangkan seluruh manfaatnya.

### Event dan Listener: memisahkan sebab dan akibat

Controller yang sehat tidak perlu tahu apa saja yang harus terjadi setelah tugas dibuat. Ia cukup mengumumkan:

```php
// app/Events/AssignmentPublished.php
class AssignmentPublished
{
    use Dispatchable, SerializesModels;

    public function __construct(public Assignment $assignment) {}
}
```

```php
// controller
$assignment = $course->assignments()->create($request->validated());

AssignmentPublished::dispatch($assignment);

return redirect()->route('assignments.show', $assignment);
```

```php
// app/Listeners/SendAssignmentNotification.php
class SendAssignmentNotification implements ShouldQueue
{
    public function handle(AssignmentPublished $event): void
    {
        $students = $event->assignment->course->students;

        Notification::send($students, new NewAssignmentNotification($event->assignment));
    }
}
```

`implements ShouldQueue` adalah satu-satunya perbedaan antara listener yang menyandera pengguna dan listener yang berjalan di latar. Satu baris.

Satu hal yang akan membingungkan saat Anda membuka tabel `jobs`: kalau Listener **dan** Notification sama-sama `implements ShouldQueue`, yang terbentuk adalah satu job untuk listener-nya, lalu job listener itu melahirkan satu job lagi per penerima. Empat puluh mahasiswa berarti 41 job, bukan 1. Itu bukan bug — memang begitu rancangannya, dan justru bagus: satu notifikasi yang gagal tidak menjatuhkan 39 lainnya. Tapi Anda perlu tahu, supaya tidak menyangka ada yang berlipat ganda.

Di Laravel 12, listener ditemukan otomatis selama tipe parameter `handle()` sesuai dengan event-nya.

### Notification: satu pesan, banyak saluran

```bash
php artisan make:notification NewAssignmentNotification
```

```php
class NewAssignmentNotification extends Notification implements ShouldQueue
{
    use Queueable;

    public function __construct(public Assignment $assignment) {}

    public function via(object $notifiable): array
    {
        return ['database'];   // tambahkan 'mail' bila diperlukan
    }

    public function toDatabase(object $notifiable): array
    {
        return [
            'assignment_id' => $this->assignment->id,
            'course_name'   => $this->assignment->course->name,
            'title'         => $this->assignment->title,
            'due_at'        => $this->assignment->due_at->toIso8601String(),
            'url'           => route('assignments.show', $this->assignment),
        ];
    }
}
```

Tabel notifikasi sudah ada di skema Bagian 4 spesifikasi:

```bash
php artisan make:notifications-table
```

Membaca notifikasi:

```php
$user->unreadNotifications;              // koleksi
$user->unreadNotifications->count();     // untuk lonceng di navbar
$notification->markAsRead();
```

⚠️ **Jangan simpan data sensitif di payload notifikasi.** Kolom `data` tidak terenkripsi dan ikut terbaca siapa pun yang bisa membaca tabel itu. Simpan ID dan judul, jangan nilai atau isi feedback.

### Job yang gagal

```bash
php artisan make:queue-failed-table
php artisan migrate
```

⚠️ Namanya `make:queue-failed-table`, bukan `queue:failed-table`. Perintah lama itu sudah tidak ada sejak Laravel 11 — sama seperti `make:queue-table` dan `make:notifications-table` di atas. Kalau AI memberi Anda versi tanpa awalan `make:`, jawabannya berasal dari Laravel 10.

```php
class SendAssignmentNotification implements ShouldQueue
{
    public int $tries = 3;
    public int $backoff = 10;
}
```

Job yang gagal tiga kali masuk tabel `failed_jobs`. Periksa dengan `php artisan queue:failed`, jalankan ulang dengan `php artisan queue:retry all`.

Ini penting untuk minggu 12: di server produksi, **tidak ada yang memberi tahu Anda kalau worker mati.** Notifikasi berhenti diam-diam. Salah satu pertanyaan interview minggu ini adalah tentang ini persis.

### Realtime sungguhan (opsional)

Untuk lonceng notifikasi yang berubah tanpa memuat ulang halaman, ada dua tingkat:

**Tingkat cukup** — polling. Livewire `wire:poll.30s`, atau `setInterval` yang memanggil `GET /api/v1/notifications`. Sederhana, tidak butuh infrastruktur tambahan, dan **cukup memenuhi Sub-CPMK minggu ini**.

**Tingkat lanjut** — WebSocket via Laravel Reverb:

```bash
php artisan install:broadcasting
```

Event yang `implements ShouldBroadcast` akan dikirim ke channel. Gunakan `PrivateChannel` agar notifikasi tidak bisa disadap:

```php
public function broadcastOn(): array
{
    return [new PrivateChannel('users.' . $this->userId)];
}
```

Kalau memakai `Channel` biasa (publik), siapa pun bisa berlangganan channel itu dan membaca notifikasi orang lain. Ini IDOR versi WebSocket.

**Saran:** kerjakan polling dulu sampai berjalan sempurna. Reverb hanya kalau kelompok Anda sudah selesai dengan seluruh fitur wajib.

---

## 10.2 Prompt Pack — Minggu 10

### A. Prompt Perancangan Alur Asinkron

```
Konteks: LMS Laravel 12. Ada dua peristiwa yang memicu notifikasi:
1. Dosen mempublikasikan tugas baru → seluruh mahasiswa di mata kuliah
   itu diberi tahu
2. Dosen memberi nilai pada submission → mahasiswa pemilik submission
   diberi tahu

Rancangkan alurnya menggunakan Event, Listener, dan Notification.

Untuk tiap peristiwa sebutkan:
- nama Event, data yang dibawanya
- nama Listener, apakah perlu ShouldQueue dan mengapa
- nama Notification, saluran (via), isi payload
- siapa penerimanya dan bagaimana ditentukan

Sajikan sebagai diagram alur teks + tabel. Belum perlu kode.
Tandai bagian mana yang akan gagal diam-diam kalau queue worker mati.
```

### B. Prompt Implementasi

```
Implementasikan rancangan tadi untuk Laravel 12.

Ketentuan:
- QUEUE_CONNECTION=database. Sertakan perintah migrasi tabel yang
  dibutuhkan.
- Listener implements ShouldQueue.
- Notification via database, dengan payload yang TIDAK memuat data
  sensitif (jangan masukkan nilai atau isi feedback).
- Hindari N+1 saat mengambil daftar penerima.
- Sertakan penanganan kegagalan: tries, backoff, dan method failed().
- Tunjukkan cara menampilkan lonceng notifikasi dengan jumlah yang
  belum dibaca di navbar, dan cara menandai sudah dibaca.
```

### C. Prompt Debugging Queue

```
Notifikasi saya tidak sampai. Tidak ada pesan error, aplikasi berjalan
normal, tugas tersimpan dengan benar.

JANGAN langsung memberi solusi.
1. Jelaskan seluruh titik di mana sebuah notifikasi bisa hilang tanpa
   jejak dalam alur Event → Listener → Queue → Notification → database.
2. Untuk tiap titik, sebutkan perintah artisan atau tabel apa yang bisa
   saya periksa untuk memastikan.
3. Tanyakan kepada saya hasil pemeriksaan itu untuk mempersempit
   penyebabnya.
```

Prompt ini melatih hal yang paling berharga minggu ini: **mendiagnosis kegagalan yang tidak menghasilkan pesan error.**

### D. Prompt Audit Kebocoran Notifikasi

```
Berikut Notification dan komponen lonceng saya:

[tempel kode]

Periksa:
1. Adakah data sensitif di payload notifikasi yang seharusnya tidak
   disimpan permanen di tabel notifications?
2. Apakah endpoint pengambilan notifikasi hanya mengembalikan
   notifikasi milik pengguna yang sedang login? Uji dengan mengubah ID.
3. Apakah endpoint "tandai sudah dibaca" memeriksa kepemilikan, atau
   bisa dipakai menandai notifikasi orang lain?
4. Kalau saya memakai broadcasting, apakah channel-nya privat? Apa
   akibatnya kalau memakai channel publik?

Sertakan curl untuk membuktikan tiap temuan.
```

---

## 10.3 Read → Break → Fix → Build

### READ — Ikuti satu job (30 menit)

Setelah Event dan Listener pertama jadi, tanpa AI:

1. Matikan worker. Buat satu tugas baru. Buka tabel `jobs` — apa isinya?
2. Baca kolom `payload`. Bisakah Anda mengenali nama Listener dan ID tugasnya?
3. Jalankan `php artisan queue:work`, amati keluarannya. Periksa tabel `jobs` lagi.
4. Periksa tabel `notifications`. Berapa baris yang muncul? Cocok dengan jumlah mahasiswa?
5. Baca kolom `data` pada satu notifikasi. Adakah yang tidak seharusnya tersimpan di situ?

### BREAK — Delapan kerusakan (50 menit)

| # | Yang dicoba | Yang harus Anda amati |
|---|-------------|------------------------|
| 1 | Set `QUEUE_CONNECTION=database`, jangan jalankan worker, buat tugas baru | **Gagal total tanpa satu pun pesan error** |
| 2 | Hapus `implements ShouldQueue` dari Listener, buat tugas untuk 40 mahasiswa | Rasakan sendiri lamanya |
| 3 | Ganti `QUEUE_CONNECTION=sync`, ulangi nomor 2 | Bandingkan dengan nomor 1 dan 2 |
| 4 | Buat Notification melempar exception, jalankan worker | Amati `tries` dan tabel `failed_jobs` |
| 5 | Hentikan worker di tengah pemrosesan job | Job kembali ke queue atau hilang? |
| 6 | Masukkan nilai mahasiswa ke payload notifikasi, lalu buka tabel `notifications` sebagai admin | Kebocoran data permanen |
| 7 | Panggil endpoint "tandai dibaca" dengan ID notifikasi orang lain | IDOR versi notifikasi |
| 8 | Kalau memakai Reverb: ganti `PrivateChannel` menjadi `Channel`, langganan channel orang lain | Penyadapan realtime |

Nomor 1 adalah pelajaran inti. Aplikasi yang "tidak error tapi tidak berfungsi" jauh lebih sulit didiagnosis daripada aplikasi yang jelas-jelas rusak, dan ini akan terjadi lagi di minggu 12 saat deployment.

### FIX — Repo cacat (30 menit)

Branch `w10` pada repo `kampuslms-broken` berisi **7 masalah**: Listener tanpa `ShouldQueue`, notifikasi dikirim dalam loop satu per satu (N+1), payload memuat nilai mahasiswa, endpoint tandai-dibaca tanpa pemeriksaan kepemilikan, tidak ada `failed_jobs`, job tanpa `tries`, dan satu channel broadcast yang publik.

Perbaiki, kirim PR. Sertakan bukti: waktu respons sebelum dan sesudah untuk masalah pertama.

### BUILD — Milestone M3

Deliverable yang dinilai sebagai **Tugas 3**:

1. **Event + Listener + Notification** untuk dua peristiwa: tugas baru dipublikasikan, dan nilai diberikan.
2. **Queue database aktif**, worker terdokumentasi di README (cara menjalankannya).
3. **`failed_jobs` terpasang**, job punya `tries` dan `backoff`, dan method `failed()` yang mencatat log.
4. **Lonceng notifikasi** di navbar dengan jumlah belum dibaca, memakai polling (minimal) atau Reverb (opsional).
5. **Halaman daftar notifikasi**, bisa ditandai sudah dibaca, dengan pemeriksaan kepemilikan.
6. **Alur penilaian lengkap**: dosen melihat daftar pengumpulan, memberi nilai + feedback, mahasiswa menerima notifikasi dan melihat nilainya.
7. **Rekap nilai** per mata kuliah (dosen) dan per mahasiswa (mahasiswa) — ini akan jadi bahan optimisasi minggu 11, jadi jangan khawatirkan performanya dulu.
8. Payload notifikasi **tidak memuat nilai atau feedback**, hanya ID dan judul.

---

## 10.4 Checkpoint Minggu 10 — Gerbang Interview Tugas 3

- [ ] Apa yang terjadi kalau queue worker mati saat notifikasi dikirim? Peragakan.
- [ ] Kenapa kegagalan itu tidak menghasilkan pesan error? Bagaimana Anda akan mengetahuinya di server produksi?
- [ ] Apa beda `QUEUE_CONNECTION=sync` dan `database`? Kapan masing-masing dipakai?
- [ ] Tunjukkan Listener yang **Anda** tulis. Apa akibatnya kalau `ShouldQueue` dihapus?
- [ ] Kenapa nilai mahasiswa tidak boleh masuk payload notifikasi?
- [ ] Tunjukkan apa yang mencegah mahasiswa menandai notifikasi orang lain sebagai sudah dibaca.
- [ ] Tunjukkan satu bagian yang Anda tulis dengan bantuan AI. Apa yang Anda ubah, dan kenapa?

**Kuis 10:** queue, Event/Listener, Notification, job gagal, channel privat.

---

## Rubrik Ringkas Tugas 3

| Kriteria | Bobot | Yang dilihat |
|----------|-------|--------------|
| Fungsionalitas | 35% | Alur unggah → kumpul → nilai → notifikasi berjalan utuh dari tiga peran |
| Implementasi Konsep | 30% | Queue benar-benar asinkron; Event/Listener terpisah rapi; penanganan job gagal; payload aman |
| Interview/Pemahaman | 25% | Mampu menjelaskan kegagalan senyap dan cara mendiagnosisnya |
| Kualitas Kode | 10% | Controller tipis; tidak ada N+1 pada pengiriman notifikasi; PR direview |

---
---

# MINGGU 11 — Optimisasi Performa

**Sub-CPMK:** Mahasiswa mampu menganalisis dan meningkatkan performa aplikasi web (C4), menunjukkan sikap kritis terhadap efisiensi sistem (A4), serta membangun aplikasi web dengan performa yang optimal (P4).

**Target akhir minggu:** Anda mengukur performa KampusLMS Anda sendiri, menemukan masalahnya, memperbaikinya, dan **mendokumentasikan angka sebelum-sesudah**.

> Minggu ini tidak ada fitur baru. Yang dikerjakan adalah memperbaiki apa yang sudah Anda tulis sendiri sembilan minggu terakhir.

---

## 11.1 Konsep

### Ukur dulu, baru optimalkan

Aturan pertama optimisasi: **jangan menebak.** Perbaikan yang didasarkan pada firasat biasanya membuat kode lebih rumit tanpa mempercepat apa pun.

Pasang alat ukur:

```bash
composer require barryvdh/laravel-debugbar --dev
# atau
php artisan install:telescope
```

Yang Anda lihat: jumlah query per halaman, waktu tiap query, memori terpakai, dan query mana yang berulang.

**Inilah alasan seeder di Bagian 4.4 spesifikasi harus besar.** Dengan 5 baris data, halaman apa pun terasa cepat. Dengan 30 mahasiswa dan 100+ submission, masalahnya muncul sendiri.

### N+1: masalah performa nomor satu

```php
// controller
$courses = Course::paginate(15);
```

```blade
@foreach ($courses as $course)
    {{ $course->lecturer->name }}      {{-- 1 query per baris --}}
@endforeach
```

15 baris = 1 query untuk daftar + 15 query untuk dosen = **16 query**. Dengan 100 baris, 101 query.

Perbaikannya satu kata:

```php
$courses = Course::with('lecturer')->paginate(15);   // 2 query, apa pun jumlah barisnya
```

Untuk relasi bersarang dan penghitungan:

```php
$courses = Course::with(['lecturer', 'assignments.submissions.grade'])
    ->withCount(['students', 'assignments'])
    ->paginate(15);
```

`withCount()` jauh lebih murah daripada memuat seluruh relasi hanya untuk menghitungnya. `$course->students()->count()` adalah satu query; `count($course->students)` memuat seluruh barisnya ke memori.

### Mencegah N+1 secara otomatis

Laravel bisa melempar exception setiap kali relasi diakses tanpa dimuat lebih dulu. Tambahkan di `AppServiceProvider`:

```php
public function boot(): void
{
    Model::preventLazyLoading(! app()->isProduction());
}
```

Di lingkungan pengembangan, N+1 menjadi error yang tidak bisa diabaikan. Di produksi ia tetap berjalan agar pengguna tidak melihat halaman rusak.

Pasang ini dan jalankan aplikasi Anda. Kemungkinan besar Anda akan menemukan beberapa N+1 yang selama ini tidak terlihat.

### Index database

Index adalah daftar isi. Tanpanya, database membaca seluruh tabel untuk menemukan satu baris.

Yang wajib punya index:
- Seluruh kolom foreign key (`foreignId()->constrained()` membuatnya otomatis)
- Kolom yang sering muncul di `where` — misalnya `users.role`, `assignments.due_at`
- Kolom yang sering dipakai mengurutkan
- Kombinasi yang sering dipakai bersama: `index(['course_id', 'due_at'])`

Yang **tidak** boleh: memasang index di semua kolom. Setiap index memperlambat penulisan dan memakan ruang.

Memeriksa apakah index terpakai:

```sql
EXPLAIN SELECT * FROM assignments WHERE course_id = 5 ORDER BY due_at;
```

Kolom `type` bernilai `ALL` berarti seluruh tabel dipindai — index tidak terpakai.

⚠️ **`LIKE '%kata%'` tidak bisa memakai index.** Pencarian yang diawali wildcard selalu memindai seluruh tabel. Untuk KampusLMS ini masih dapat diterima, tapi Anda harus tahu alasannya dan bisa menjelaskannya.

### Ambil hanya yang dibutuhkan

```php
// ❌ mengambil seluruh kolom termasuk yang tidak dipakai
$students = $course->students;

// ✔
$students = $course->students()->select('users.id', 'users.name', 'users.nim_nip')->get();
```

Dan untuk data besar, jangan muat semuanya ke memori:

```php
Submission::where('assignment_id', $id)->chunk(200, function ($submissions) {
    // proses per 200 baris
});
```

### Cache

Cache adalah menyimpan hasil yang mahal agar tidak dihitung ulang.

```php
$stats = Cache::remember("course.{$course->id}.stats", now()->addMinutes(10), function () use ($course) {
    return [
        'students'    => $course->students()->count(),
        'assignments' => $course->assignments()->count(),
        'ungraded'    => $course->assignments()
            ->withCount(['submissions' => fn ($q) => $q->doesntHave('grade')])
            ->get()->sum('submissions_count'),
    ];
});
```

Yang harus Anda pikirkan sebelum memakai cache: **kapan data ini menjadi basi, dan apa akibatnya?**

Statistik dashboard yang basi 10 menit tidak masalah. Nilai mahasiswa yang basi 10 menit adalah keluhan. Rekap kehadiran yang basi saat presensi berlangsung adalah bencana.

Membersihkan cache saat data berubah:

```php
protected static function booted(): void
{
    static::saved(fn (Assignment $a) => Cache::forget("course.{$a->course_id}.stats"));
    static::deleted(fn (Assignment $a) => Cache::forget("course.{$a->course_id}.stats"));
}
```

### Optimisasi produksi

```bash
php artisan config:cache
php artisan route:cache
php artisan view:cache
php artisan event:cache
composer install --optimize-autoloader --no-dev
npm run build
```

⚠️ **Setelah `config:cache`, fungsi `env()` di luar berkas `config/` mengembalikan `null`.** Ini penyebab bug deployment yang sangat sering terjadi: aplikasi jalan sempurna di lokal, lalu rusak di server. Aturannya: `env()` **hanya** boleh dipanggil di dalam `config/*.php`. Di tempat lain gunakan `config('nama.kunci')`.

Ingat baik-baik — ini akan menggigit Anda minggu depan.

### Frontend

- `npm run build`, bukan `npm run dev`, di produksi
- Gambar diberi `loading="lazy"`
- Pagination, bukan menampilkan seluruh data
- Untuk jalur SPA: jangan panggil endpoint yang sama berulang kali; simpan hasilnya di state

---

## 11.2 Prompt Pack — Minggu 11

### A. Prompt Analisis, Bukan Perbaikan

```
Berikut controller dan view untuk halaman rekap nilai saya:

[tempel kode]

Debugbar melaporkan halaman ini menjalankan 87 query dalam 1,4 detik
dengan 20 baris data.

JANGAN langsung memperbaiki.
1. Berdasarkan kode ini, jelaskan dari mana kira-kira 87 query itu
   berasal. Kelompokkan berdasarkan penyebabnya.
2. Urutkan penyebabnya dari yang paling besar dampaknya.
3. Untuk tiap penyebab, perkirakan berapa query yang bisa dihemat.

Setelah itu berhenti. Saya ingin memverifikasi analisismu terhadap
laporan Debugbar sebelum kita memperbaiki apa pun.
```

Pola ini — **analisis dulu, verifikasi, baru perbaiki** — adalah inti minggu ini. Mahasiswa yang langsung menyuruh AI "perbaiki performanya" tidak akan pernah belajar mendiagnosis.

### B. Prompt Perbaikan Bertahap

```
Analisismu sudah cocok dengan laporan Debugbar.

Sekarang perbaiki HANYA penyebab nomor 1 (yang paling besar dampaknya).
Jangan perbaiki yang lain dulu.

Ketentuan:
- Tunjukkan kode sebelum dan sesudah, berdampingan.
- Jelaskan berapa query yang seharusnya berkurang dan mengapa.
- Jangan mengubah perilaku halaman sedikit pun.

Setelah saya ukur ulang, kita lanjut ke penyebab berikutnya.
```

Memperbaiki satu per satu memungkinkan mahasiswa mengukur dampak tiap perubahan. Memperbaiki semuanya sekaligus menghasilkan angka bagus yang tidak dipahami sebabnya.

### C. Prompt Perancangan Index

```
Berikut migrasi saya dan daftar query yang paling sering dijalankan
aplikasi:

[tempel migrasi]
[tempel 5 query tersering dari Telescope]

Rancangkan index yang perlu ditambahkan.

Ketentuan:
- Untuk tiap index, sebutkan query mana yang diuntungkan.
- Sebutkan juga BIAYA-nya: operasi tulis mana yang jadi lebih lambat.
- Tandai index yang menurutmu TIDAK perlu ditambahkan meski kelihatan
  masuk akal, dan jelaskan kenapa.
- Sertakan perintah EXPLAIN agar saya bisa membuktikan index itu
  benar-benar terpakai.
```

### D. Prompt Strategi Cache

```
Berikut halaman dashboard saya beserta data yang ditampilkannya:

[tempel kode]

Untuk setiap potong data di halaman ini, tentukan:
- apakah layak di-cache
- berapa lama masa berlakunya
- apa akibatnya bagi pengguna kalau data ini basi

Tandai secara khusus data yang TIDAK BOLEH di-cache dan jelaskan
alasannya.

Lalu tunjukkan cara membersihkan cache yang relevan ketika data
sumbernya berubah.
```

---

## 11.3 Read → Break → Fix → Build

### READ — Ukur aplikasi Anda sendiri (40 menit)

Pastikan seeder penuh sudah dijalankan (`migrate:fresh --seed`). Lalu tanpa AI, isi tabel ini di `docs/performa.md`:

| Halaman | Jumlah query | Waktu (ms) | Memori |
|---------|-------------:|-----------:|-------:|
| Daftar mata kuliah | | | |
| Detail mata kuliah | | | |
| Daftar tugas | | | |
| Daftar pengumpulan (dosen) | | | |
| **Rekap nilai** | | | |
| Dashboard mahasiswa | | | |
| Dashboard dosen | | | |

Kemudian:
1. Urutkan dari yang terburuk.
2. Untuk halaman terburuk, buka Debugbar tab Queries. Temukan query yang **berulang dengan pola sama**. Itu N+1 Anda.
3. Salin satu query terlambat, jalankan `EXPLAIN` di database. Catat kolom `type` dan `rows`.

Halaman rekap nilai hampir pasti yang terburuk. Itu memang disengaja sejak perancangan skema.

### BREAK — Tujuh percobaan (45 menit)

| # | Yang dicoba | Yang harus Anda amati |
|---|-------------|------------------------|
| 1 | Pasang `Model::preventLazyLoading()`, jalankan seluruh halaman | Berapa N+1 tersembunyi yang muncul? |
| 2 | Hapus semua `with()` dari satu controller, ukur ulang | Bandingkan angkanya |
| 3 | Ganti `withCount('students')` menjadi `count($course->students)`, ukur memori | Beda menghitung vs memuat |
| 4 | Hapus index dari `assignments.course_id`, jalankan `EXPLAIN` | `type` berubah menjadi `ALL` |
| 5 | Naikkan seeder menjadi 300 mahasiswa dan 1000 submission, buka rekap nilai | Masalah yang tadinya kecil jadi jelas |
| 6 | Jalankan `config:cache`, lalu panggil `env('APP_NAME')` dari sebuah controller | **Mengembalikan null** — ingat baik-baik |
| 7 | Pasang cache 10 menit pada nilai mahasiswa, ubah nilainya, muat ulang | Data basi yang merugikan pengguna |

Nomor 6 adalah bekal untuk minggu depan. Nomor 7 mengajarkan bahwa cache bukan selalu perbaikan.

### FIX — Repo cacat (30 menit)

Branch `w11` pada repo `kampuslms-broken` berisi halaman rekap nilai dengan **6 masalah**: N+1 pada tiga tingkat relasi, `count()` yang memuat seluruh koleksi, `select *` padahal hanya dua kolom dipakai, index hilang pada kolom yang sering difilter, seluruh data dimuat tanpa pagination, dan `env()` dipanggil di luar `config/`.

Perbaiki, kirim PR. **Deskripsi PR wajib memuat tabel sebelum-sesudah**: jumlah query, waktu, memori.

### BUILD — Optimisasi KampusLMS

1. **`docs/performa.md`** berisi tabel pengukuran sebelum dan sesudah untuk seluruh halaman utama.
2. **`Model::preventLazyLoading()`** aktif di lingkungan non-produksi, dan aplikasi berjalan **tanpa melanggarnya**.
3. Seluruh N+1 diperbaiki dengan `with()` / `withCount()` / `loadMissing()`.
4. **Migrasi tambahan** berisi index yang Anda rancang, dengan komentar menyebut query yang diuntungkan.
5. Pagination di seluruh halaman daftar, tidak ada yang memuat seluruh tabel.
6. Cache pada minimal satu bagian dashboard, lengkap dengan pembersihan otomatis saat data berubah — dan penjelasan di `docs/performa.md` tentang bagian mana yang sengaja **tidak** di-cache dan mengapa.
7. Seluruh pemanggilan `env()` di luar `config/` dihapus, diganti `config()`.
8. Target: halaman rekap nilai turun menjadi **di bawah 15 query**.

---

## 11.4 Checkpoint Minggu 11

- [ ] Buka `docs/performa.md`. Halaman mana yang terburuk sebelum optimisasi? Kenapa halaman itu?
- [ ] Peragakan N+1 di aplikasi Anda: matikan `with()` pada satu halaman dan tunjukkan lonjakan query di Debugbar.
- [ ] Apa beda `withCount('students')` dan `count($course->students)`? Kenapa yang kedua boros?
- [ ] Tunjukkan index yang Anda tambahkan. Jalankan `EXPLAIN` dan buktikan ia terpakai.
- [ ] Sebutkan satu data di aplikasi Anda yang **tidak boleh** di-cache. Kenapa?
- [ ] Apa yang terjadi pada `env()` setelah `config:cache`? Kenapa ini penting minggu depan?

**Kuis 11:** N+1, eager loading, index, cache, optimisasi produksi.

---
---

# MINGGU 12 — Deployment

**Sub-CPMK:** Mahasiswa mampu menerapkan proses deployment aplikasi web (C3), menunjukkan tanggung jawab dalam pengelolaan aplikasi di lingkungan produksi (A4), serta membangun dan menjalankan aplikasi web pada server produksi (P4).

**Target akhir minggu:** KampusLMS Anda bisa dibuka siapa pun dari HP mereka, lewat HTTPS, dengan domain yang layak.

> ⚠️ **Minggu ini adalah Milestone M4 — Tugas 4 (7%) + interview kelompok.**

---

## 12.1 Konsep

### Perbedaan lokal dan produksi

| | Lokal | Produksi |
|---|---|---|
| `APP_ENV` | `local` | `production` |
| `APP_DEBUG` | `true` | **`false`** |
| Aset | `npm run dev` | `npm run build` |
| Cache | mati | `config/route/view:cache` |
| Composer | dengan dev | `--no-dev --optimize-autoloader` |
| Queue worker | manual | Supervisor |
| HTTPS | tidak | wajib |
| Kesalahan | tampil di layar | masuk log |

### `APP_DEBUG=false` — aturan yang tidak bisa ditawar

Ingat percobaan minggu 1 nomor 4? Sekarang taruhannya nyata.

Dengan `APP_DEBUG=true` di server publik, siapa pun yang memicu error akan melihat halaman Whoops berisi: seluruh isi `.env` (termasuk kata sandi database), jejak kode lengkap dengan path server, dan versi seluruh paket yang Anda pakai.

Ini bukan risiko teoretis. Memindai internet untuk mencari halaman debug Laravel yang terbuka adalah kegiatan otomatis yang berlangsung terus-menerus.

Aturan minggu ini: **`APP_DEBUG=false` sejak menit pertama deployment**, bukan "nanti setelah selesai testing".

### Berkas dan folder yang tidak boleh diakses publik

Web server harus mengarah ke folder `public/`, **bukan** ke akar proyek. Kalau salah, seluruh isi proyek Anda bisa diunduh — termasuk `.env`.

Cara memeriksanya setelah deploy, wajib dilakukan:

```bash
curl -I https://domain-anda.com/.env
curl -I https://domain-anda.com/storage/logs/laravel.log
curl -I https://domain-anda.com/composer.json
```

Ketiganya harus mengembalikan **404**. Kalau ada yang 200, hentikan semuanya dan perbaiki dulu.

### Langkah deployment

**1. Siapkan server.** VPS dengan Ubuntu, PHP 8.3, Nginx, MySQL, Composer, Node.js. Alternatif: shared hosting dengan akses SSH, tetapi pastikan document root bisa diarahkan ke `public/`.

**2. Ambil kode.**

```bash
git clone https://github.com/org-anda/kampuslms-kelompok-XX.git
cd kampuslms-kelompok-XX
composer install --no-dev --optimize-autoloader
npm ci && npm run build
```

**3. Konfigurasi.**

```bash
cp .env.example .env
php artisan key:generate
```

Isi `.env` produksi:

```env
APP_ENV=production
APP_DEBUG=false
APP_URL=https://kampuslms-kelompok-xx.example.com

DB_DATABASE=...
DB_USERNAME=...
DB_PASSWORD=...

QUEUE_CONNECTION=database
SESSION_SECURE_COOKIE=true
```

`APP_KEY` produksi **berbeda** dari lokal, dan tidak boleh dibagikan.

**4. Izin folder.**

```bash
chown -R www-data:www-data storage bootstrap/cache
chmod -R 775 storage bootstrap/cache
```

Ini penyebab error 500 paling umum saat deployment pertama.

**5. Migrasi.**

```bash
php artisan migrate --force
php artisan db:seed --force    # hanya untuk demo; JANGAN di aplikasi sungguhan
```

`--force` diperlukan karena Laravel menolak migrasi di produksi tanpanya — pengaman agar Anda tidak tidak sengaja menjalankan `migrate:fresh` dan menghapus data sungguhan.

**6. Optimisasi.**

```bash
php artisan config:cache
php artisan route:cache
php artisan view:cache
php artisan event:cache
php artisan storage:link
```

Kalau setelah ini aplikasi rusak padahal lokal baik-baik saja, tersangka pertama adalah `env()` yang dipanggil di luar `config/` — persis yang Anda temukan di minggu 11.

**7. Queue worker dengan Supervisor.**

`php artisan queue:work` yang dijalankan lewat SSH akan mati begitu Anda logout, dan notifikasi berhenti diam-diam.

```ini
[program:kampuslms-worker]
process_name=%(program_name)s_%(process_num)02d
command=php /var/www/kampuslms/artisan queue:work --sleep=3 --tries=3 --max-time=3600
autostart=true
autorestart=true
user=www-data
numprocs=2
redirect_stderr=true
stdout_logfile=/var/www/kampuslms/storage/logs/worker.log
stopwaitsecs=3600
```

```bash
sudo supervisorctl reread && sudo supervisorctl update && sudo supervisorctl start kampuslms-worker:*
```

⚠️ **Setelah setiap deployment, worker harus di-restart**: `php artisan queue:restart`. Worker yang lama masih memegang kode versi lama di memori.

**8. HTTPS.**

```bash
sudo certbot --nginx -d kampuslms-kelompok-xx.example.com
```

Sertifikat gratis dari Let's Encrypt, berlaku 90 hari, diperbarui otomatis. Setelah HTTPS aktif, set `SESSION_SECURE_COOKIE=true` agar cookie session tidak pernah dikirim lewat koneksi tidak terenkripsi.

**9. Scheduler** (kalau dipakai) — di Laravel 12 ditulis di `routes/console.php`:

```cron
* * * * * cd /var/www/kampuslms && php artisan schedule:run >> /dev/null 2>&1
```

### Deployment ulang

Buat `deploy.sh` dan jalankan tiap kali ada perubahan:

```bash
#!/usr/bin/env bash
set -e

php artisan down

git pull origin main
composer install --no-dev --optimize-autoloader
npm ci && npm run build
php artisan migrate --force

php artisan config:cache
php artisan route:cache
php artisan view:cache
php artisan event:cache
php artisan queue:restart

php artisan up
```

`set -e` menghentikan skrip pada kesalahan pertama — lebih baik berhenti di tengah daripada melanjutkan dengan keadaan setengah jadi.

### Ketika ada yang salah

Karena `APP_DEBUG=false`, layar hanya menampilkan halaman 500 kosong. Itu benar. Informasinya ada di log:

```bash
tail -f storage/logs/laravel.log
tail -f /var/log/nginx/error.log
```

**Jangan pernah** menyalakan `APP_DEBUG=true` di produksi untuk mencari tahu penyebab error. Baca lognya.

### Daftar periksa keamanan produksi

Wajib dilewati sebelum menyerahkan Tugas 4:

- [ ] `APP_DEBUG=false`
- [ ] `APP_ENV=production`
- [ ] `curl -I https://domain/.env` → 404
- [ ] `curl -I https://domain/storage/logs/laravel.log` → 404
- [ ] HTTPS aktif, HTTP dialihkan ke HTTPS
- [ ] `SESSION_SECURE_COOKIE=true`
- [ ] Kata sandi database bukan kata sandi lokal, dan bukan `password`
- [ ] Akun demo memakai kata sandi yang berbeda dari contoh di dokumentasi
- [ ] Skrip pengujian otorisasi dari minggu 7 dijalankan **terhadap server produksi** dan seluruhnya lolos
- [ ] Berkas submission tidak bisa diunduh tanpa login — diuji dari jendela penyamaran
- [ ] Queue worker berjalan lewat Supervisor, dibuktikan dengan `supervisorctl status`
- [ ] `upload_max_filesize` dan `post_max_size` di server sesuai validasi aplikasi

Poin terakhir adalah oleh-oleh dari minggu 9: banyak kelompok menemukan unggahan yang jalan sempurna di lokal ternyata gagal di server karena `php.ini` produksi berbeda.

---

## 12.2 Prompt Pack — Minggu 12

### A. Prompt Rencana Deployment

```
Konteks: aplikasi Laravel 12 dengan MySQL, Vite, queue database,
dan unggahan berkas privat.

Server: VPS Ubuntu 24.04, akses root, belum ada apa pun terpasang.

Susun rencana deployment lengkap dari nol.

Ketentuan:
- Sajikan sebagai langkah bernomor dengan perintah yang bisa saya
  jalankan.
- Untuk tiap langkah, sebutkan cara MEMVERIFIKASI langkah itu berhasil
  sebelum lanjut.
- Tandai langkah mana yang kalau dilewati akan menyebabkan kegagalan
  yang tidak menampilkan pesan error.
- Sertakan konfigurasi Nginx yang document root-nya mengarah ke folder
  public/, dan jelaskan apa yang bocor kalau salah arah.
```

Instruksi soal verifikasi per langkah sangat berharga: deployment gagal biasanya karena satu langkah terlewat, dan gejalanya baru muncul jauh di belakang.

### B. Prompt Audit Keamanan Produksi

```
Aplikasi Laravel 12 saya sudah live di https://[domain].

Susun daftar periksa keamanan pasca-deployment, lengkap dengan
perintah curl untuk membuktikan setiap poin.

Fokus pada:
- berkas yang seharusnya tidak bisa diakses publik
- header keamanan yang seharusnya ada
- konfigurasi yang membocorkan informasi kalau salah
- endpoint yang seharusnya tidak bisa diakses tanpa autentikasi

Untuk tiap poin, sebutkan hasil yang DIHARAPKAN dan hasil yang
menandakan MASALAH.
```

### C. Prompt Debugging Produksi

```
Aplikasi saya berjalan sempurna di lokal, tetapi di server produksi
menampilkan error 500 tanpa pesan apa pun.

APP_DEBUG bernilai false dan saya TIDAK akan mengubahnya.

JANGAN menyuruh saya menyalakan APP_DEBUG.
1. Sebutkan urutan tempat yang harus saya periksa untuk menemukan
   penyebabnya, dari yang paling sering menjadi penyebab.
2. Beri perintah persis untuk memeriksa tiap tempat.
3. Sebutkan lima penyebab paling umum error 500 pada deployment Laravel
   pertama, beserta gejala khas masing-masing.
4. Tanyakan kepada saya apa yang saya temukan di log sebelum
   menyimpulkan.
```

Instruksi "jangan menyuruh saya menyalakan APP_DEBUG" penting — itu saran pertama yang biasanya diberikan AI, dan itu saran berbahaya untuk server publik.

### D. Prompt Skrip Deployment

```
Buatkan skrip deploy.sh untuk aplikasi Laravel 12 saya.

Ketentuan:
- Berhenti pada kesalahan pertama.
- Mode maintenance selama proses berjalan.
- Sertakan restart queue worker, dan jelaskan mengapa itu wajib.
- Sertakan pembersihan dan pembuatan ulang seluruh cache.
- Beri komentar pada setiap baris yang menjelaskan akibatnya kalau
  baris itu dihilangkan.
- Tambahkan pemeriksaan di akhir yang memverifikasi aplikasi benar-benar
  merespons sebelum keluar dari mode maintenance.
```

---

## 12.3 Read → Break → Fix → Build

### READ — Bedah server (40 menit)

Setelah deployment pertama berhasil, tanpa AI:

1. Temukan konfigurasi Nginx untuk situs Anda. Baris mana yang menentukan document root? Ke folder apa ia mengarah?
2. Jalankan `php artisan about` di server. Bandingkan dengan lokal — apa saja yang berbeda?
3. Jalankan `supervisorctl status`. Berapa worker yang berjalan?
4. Buka `storage/logs/laravel.log`. Ada berapa entri? Baca yang terbaru.
5. Jalankan ketiga perintah `curl -I` dari daftar periksa. Catat hasilnya.
6. Buka aplikasi dari HP Anda, bukan laptop. Ada yang rusak?

### BREAK — Delapan percobaan (50 menit)

> ### ⛔ Baca ini sebelum mulai
>
> Percobaan 1 dan 2 sengaja membuka rahasia aplikasi Anda. Di halaman sebelumnya modul ini menulis bahwa pemindaian internet untuk mencari halaman debug Laravel berlangsung **terus-menerus dan otomatis**. Kalimat itu berlaku juga untuk Anda: paparan tiga menit sudah cukup untuk ditemukan.
>
> Karena itu, percobaan 1–3 **wajib** dilakukan di belakang salah satu pagar berikut, bukan di situs terbuka:
>
> ```nginx
> # nginx: batasi hanya ke alamat IP Anda selama percobaan
> location / {
>     allow 203.0.113.45;      # ganti dengan IP Anda, lihat di https://ifconfig.me
>     deny all;
>     try_files $uri $uri/ /index.php?$query_string;
> }
> ```
>
> Atau pasang basic-auth sementara, atau — paling aman — kerjakan di *virtual host* kedua (`staging.<domain>`) yang memakai basis data dan `APP_KEY` terpisah.
>
> Aturan tambahan yang tidak bisa ditawar:
> - `.env` yang dipakai saat percobaan **tidak boleh memuat kredensial produksi sungguhan**. Pakai basis data throwaway.
> - Setelah percobaan selesai, **ganti kata sandi basis data dan jalankan `php artisan key:generate`**. Rahasia yang pernah terpapar dianggap bocor — ini pelajaran yang sama dengan minggu 13 nomor 2.
> - Catat jam mulai dan jam kembali di `docs/deployment.md`.

Lakukan pada server Anda sendiri di belakang pagar di atas, dan **kembalikan segera** setelah mengamati.

| # | Yang dicoba | Yang harus Anda amati |
|---|-------------|------------------------|
| 1 | Set `APP_DEBUG=true`, picu error, buka dari jendela penyamaran | **Kredensial database Anda di layar** |
| 2 | Arahkan document root Nginx ke akar proyek, akses `/.env` | Seluruh rahasia terunduh |
| 3 | Jalankan `config:cache` dengan `env()` masih terpanggil di controller | Nilai `null` di produksi |
| 4 | Hentikan Supervisor, buat tugas baru | Notifikasi berhenti tanpa jejak |
| 5 | Deploy kode baru tanpa `queue:restart` | Worker menjalankan kode lama |
| 6 | Set izin `storage` menjadi 555 | Error 500, dan lognya pun tidak bisa ditulis |
| 7 | Set `upload_max_filesize=1M`, unggah berkas 3 MB | Oleh-oleh minggu 9 |
| 8 | Akses situs lewat `http://` setelah `SESSION_SECURE_COOKIE=true` | Session tidak terbentuk |

Nomor 1 dan 2 wajib dilakukan lalu **segera dikembalikan**. Melihat isi `.env` sendiri terbuka di internet adalah pelajaran yang tidak bisa digantikan penjelasan apa pun.

### FIX — Repo cacat (30 menit)

Branch `w12` pada repo `kampuslms-broken` berisi konfigurasi deployment dengan **7 masalah**: `.env.example` memuat kredensial sungguhan, `deploy.sh` tanpa `set -e` dan tanpa `queue:restart`, konfigurasi Nginx mengarah ke akar proyek, `APP_DEBUG=true` di `.env.example`, tidak ada konfigurasi Supervisor, `db:seed` dijalankan tiap deploy, dan `.gitignore` yang tidak mengabaikan `.env`.

Perbaiki, kirim PR dengan penjelasan risiko tiap masalah.

### BUILD — Milestone M4

Deliverable yang dinilai sebagai **Tugas 4**:

1. **Aplikasi live di domain publik dengan HTTPS.** Tautannya ada di README.
2. **Seluruh daftar periksa keamanan produksi** terlewati, dibuktikan di `docs/deployment.md` dengan keluaran `curl` yang sesungguhnya.
3. **`deploy.sh`** di repo, teruji, berkomentar.
4. **Supervisor berjalan**, dibuktikan dengan tangkapan layar `supervisorctl status`.
5. **`docs/deployment.md`** memuat: spesifikasi server, langkah dari nol, cara deployment ulang, cara membaca log, dan **daftar masalah yang Anda temui beserta cara mengatasinya** — bagian terakhir ini yang paling bernilai untuk dinilai.
6. **Skrip `test-authz.sh` minggu 7 dijalankan terhadap server produksi**, hasilnya seluruhnya lolos, keluarannya dilampirkan.
7. **Akun demo berfungsi** di server produksi.
8. Aplikasi diuji dari perangkat lain (HP anggota kelompok), tercatat di dokumentasi.

---

## 12.4 Checkpoint Minggu 12 — Gerbang Interview Tugas 4

Sebagian pertanyaan diuji langsung terhadap server produksi Anda.

- [ ] Buka situs Anda, penguji akan menjalankan `curl -I https://domain/.env`. Harus 404.
- [ ] Apa yang terjadi kalau `APP_DEBUG=true` di produksi? Peragakan di lokal, jelaskan risikonya di produksi.
- [ ] Kenapa document root harus mengarah ke `public/`? Apa yang bocor kalau tidak?
- [ ] Tunjukkan `supervisorctl status`. Apa akibatnya kalau worker mati dan tidak ada yang tahu?
- [ ] Kenapa `queue:restart` wajib setelah setiap deployment?
- [ ] Setelah `config:cache`, apa yang terjadi pada `env()` di luar `config/`? Sudahkah kode Anda bersih dari itu?
- [ ] Buka `docs/deployment.md`. Ceritakan satu masalah yang Anda temui dan bagaimana Anda menemukan penyebabnya.
- [ ] Tunjukkan satu bagian yang Anda tulis dengan bantuan AI. Apa yang Anda ubah, dan kenapa?

**Kuis 12:** `APP_DEBUG`, document root, `config:cache`, Supervisor, HTTPS.

---

## Rubrik Ringkas Tugas 4

| Kriteria | Bobot | Yang dilihat |
|----------|-------|--------------|
| Fungsionalitas | 30% | Aplikasi live, HTTPS aktif, seluruh fitur berjalan sama seperti di lokal, akun demo berfungsi |
| Implementasi Konsep | 30% | Daftar periksa keamanan terlewati; Supervisor berjalan; optimisasi produksi diterapkan; `deploy.sh` benar |
| Interview/Pemahaman | 25% | Mampu menjelaskan risiko tiap konfigurasi dan mendiagnosis masalah produksi tanpa menyalakan debug |
| Kualitas Kode | 10% | Dokumentasi deployment jelas dan bisa diikuti orang lain; PR direview |

**Diskualifikasi otomatis** pada komponen Implementasi Konsep: `APP_DEBUG=true` di produksi, `.env` bisa diakses publik, atau berkas submission bisa diunduh tanpa login. Tiga hal ini bukan kekurangan kecil — ketiganya membocorkan data sungguhan kalau aplikasi ini dipakai.

---

*Modul Minggu 13–16 menyusul: kolaborasi & version control, debugging & testing, integrasi proyek, dan UAS.*
