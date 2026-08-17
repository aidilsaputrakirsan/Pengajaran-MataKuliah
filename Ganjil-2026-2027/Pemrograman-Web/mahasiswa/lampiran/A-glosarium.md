# Lampiran A — Glosarium

**Pemrograman Web | SI2514024 | 3 SKS | Ganjil 2026/2027**

---

Lampiran ini tidak untuk dibaca berurutan. Buka saat Anda menemui istilah yang belum jelas, lalu kembali ke modul.

Kolom **Mgg** menunjukkan minggu istilah itu pertama dipakai. Istilah yang belum Anda temui belum perlu dihafal.

## Mengapa banyak istilah dibiarkan berbahasa Inggris

Yang Anda ketik adalah `php artisan make:middleware`, bukan "buat perantara". Nama kelas, pesan error, menu, dan seluruh dokumentasi Laravel berbahasa Inggris. Menerjemahkan `middleware` menjadi "perangkat lunak penengah" hanya menambah satu lapis terjemahan yang harus Anda lakukan sendiri justru saat sedang macet.

Padanan Indonesia tetap dicantumkan karena sebagian dokumen resmi program studi menuntut istilah baku.

---

## Arsitektur dan alur request

| Istilah | Padanan | Artinya | Mgg |
|---|---|---|---:|
| **request** | permintaan | Data yang dikirim browser ke server: URL, metode, header, isi form | 1 |
| **response** | tanggapan | Yang dikembalikan server: HTML, JSON, atau redirect | 1 |
| **frontend** | — | Bagian yang berjalan di browser. Tidak bisa dipercaya — pengguna dapat mengubahnya | 1 |
| **backend** | — | Bagian yang berjalan di server. Hanya di sini keputusan penting boleh diambil | 1 |
| **MVC** | — | Pemisahan Model (data), View (tampilan), Controller (alur) | 1 |
| **route** | rute | Pemetaan URL ke kode yang harus dijalankan | 1 |
| **controller** | — | Kelas yang menerima request, mengatur alur, mengembalikan response | 2 |
| **Blade** | — | Mesin template Laravel; berkas `.blade.php` | 2 |
| **artisan** | — | Perkakas baris perintah Laravel | 1 |
| **`.env`** | — | Berkas kredensial dan konfigurasi. **Tidak pernah** di-commit | 1 |
| **stateless** | tanpa keadaan | Sifat HTTP: tiap request tidak mengingat request sebelumnya | 4 |

## Database dan Eloquent

| Istilah | Padanan | Artinya | Mgg |
|---|---|---|---:|
| **migration** | — | Berkas yang mendefinisikan perubahan struktur tabel; riwayatnya versi-terkontrol | 3 |
| **seeder** | — | Pengisi data awal. Berbeda dari migration: mengisi isi, bukan struktur | 3 |
| **factory** | pabrik data | Pembangkit data palsu untuk seeder dan test | 3 |
| **model** | — | Kelas yang mewakili satu tabel dan aturannya | 3 |
| **Eloquent** | — | ORM Laravel: baris tabel diakses sebagai objek PHP | 3 |
| **relasi** | — | Hubungan antar-model: `hasMany`, `belongsTo`, `belongsToMany` | 3 |
| **pivot table** | tabel penghubung | Tabel perantara relasi banyak-ke-banyak, misalnya `course_user` | 3 |
| **mass assignment** | — | Mengisi banyak kolom sekaligus dari input. Dibatasi `$fillable` agar kolom sensitif tidak ikut terisi | 3 |
| **soft delete** | penghapusan lunak | Baris ditandai terhapus lewat `deleted_at`, datanya tetap ada | 3 |
| **N+1** | — | Satu query daftar diikuti satu query tambahan per baris. Sumber lambat nomor satu | 11 |
| **eager loading** | pemuatan awal | `with()` — mengambil relasi sekaligus, penawar N+1 | 11 |
| **index** | indeks | Struktur bantu database agar pencarian tidak memindai seluruh tabel | 11 |

## Form, validasi, dan state

| Istilah | Padanan | Artinya | Mgg |
|---|---|---|---:|
| **session** | sesi | Penyimpanan data pengguna antar-request di sisi server | 4 |
| **flash message** | pesan sekilas | Data session yang hidup untuk satu request berikutnya saja | 4 |
| **PRG** | — | Pola Post → Redirect → Get; mencegah form terkirim ulang saat halaman di-refresh | 4 |
| **Form Request** | — | Kelas terpisah berisi aturan validasi, agar controller tetap tipis | 4 |
| **old input** | input sebelumnya | Isi form yang dikembalikan setelah validasi gagal | 4 |
| **pagination** | penomoran halaman | Memecah daftar panjang menjadi beberapa halaman | 4 |
| **CSRF** | — | Serangan yang memakai sesi login korban dari situs lain. Ditangkal token `@csrf` | 4 |

## Routing, autentikasi, dan otorisasi

| Istilah | Padanan | Artinya | Mgg |
|---|---|---|---:|
| **middleware** | — | Lapisan pemeriksa yang dilewati request sebelum sampai ke controller | 5 |
| **route model binding** | — | Laravel mengambilkan model dari ID di URL secara otomatis | 5 |
| **route group** | grup rute | Sekumpulan route yang berbagi prefix, middleware, atau nama | 5 |
| **IDOR** | — | *Insecure Direct Object Reference* — data orang lain terbuka hanya dengan mengganti ID di URL | 5 |
| **autentikasi** | — | Membuktikan **siapa** Anda | 7 |
| **otorisasi** | — | Menentukan **apa yang boleh** Anda lakukan | 7 |
| **Gate** | — | Aturan izin sederhana, tidak terikat satu model | 7 |
| **Policy** | — | Kelas berisi aturan izin untuk satu model tertentu | 7 |
| **hashing** | — | Mengubah kata sandi menjadi bentuk yang tidak bisa dikembalikan. Bukan enkripsi | 7 |
| **session fixation** | — | Serangan yang memakai ulang ID session lama setelah korban login | 7 |

## API

| Istilah | Padanan | Artinya | Mgg |
|---|---|---|---:|
| **REST** | — | Gaya perancangan API: sumber daya diakses lewat URL dan metode HTTP baku | 6 |
| **endpoint** | — | Satu alamat API beserta metodenya, misalnya `GET /api/courses` | 6 |
| **API Resource** | — | Kelas pembentuk JSON. Menentukan kolom mana yang keluar — jangan kembalikan model mentah | 6 |
| **token** | — | Kredensial pengganti login untuk API | 6 |
| **Sanctum** | — | Paket Laravel penerbit dan pemeriksa token API | 6 |
| **status code** | kode status | Angka hasil HTTP: 200 berhasil, 401 belum login, 403 tidak berhak, 404 tidak ada, 422 validasi gagal | 6 |
| **rate limiting** | pembatasan laju | Batas jumlah request per satuan waktu | 6 |

## Berkas, asinkron, dan produksi

| Istilah | Padanan | Artinya | Mgg |
|---|---|---|---:|
| **storage** | penyimpanan | Folder berkas unggahan, log, dan cache. Bukan folder publik | 9 |
| **disk** | — | Nama konfigurasi lokasi penyimpanan, misalnya `local` dan `public` | 9 |
| **MIME type** | — | Penanda jenis berkas. Bisa dipalsukan — validasi jangan bergantung padanya saja | 9 |
| **queue** | antrean | Tempat menitipkan pekerjaan lambat agar pengguna tidak menunggu | 10 |
| **job** | pekerjaan | Satu satuan kerja di dalam queue | 10 |
| **worker** | — | Proses yang mengambil dan menjalankan job dari queue | 10 |
| **event / listener** | peristiwa / pendengar | Pemisah sebab dan akibat: sesuatu terjadi, pihak lain menanggapinya | 10 |
| **notification** | notifikasi | Satu pesan yang dapat dikirim lewat beberapa saluran sekaligus | 10 |
| **cache** | tembolok | Simpanan sementara hasil yang mahal dihitung | 11 |
| **deployment** | penggelaran | Memasang aplikasi ke server publik | 12 |
| **produksi** | — | Lingkungan yang dipakai pengguna sungguhan. Lawan dari lokal | 12 |
| **`APP_DEBUG`** | — | Saklar tampilan detail error. **Wajib `false`** di produksi | 12 |
| **SSL / HTTPS** | — | Enkripsi lalu lintas antara browser dan server | 12 |

## Git, testing, dan kolaborasi

| Istilah | Padanan | Artinya | Mgg |
|---|---|---|---:|
| **commit** | — | Satu simpanan perubahan beserta pesannya | 1 |
| **branch** | cabang | Jalur kerja terpisah dari jalur utama | 1 |
| **pull request (PR)** | — | Usulan penggabungan branch, disertai review | 1 |
| **merge conflict** | konflik gabung | Dua branch mengubah baris yang sama; harus diselesaikan manual | 13 |
| **Conventional Commits** | — | Format pesan commit baku, misalnya `feat:`, `fix:`, `refactor:` | 13 |
| **feature test** | — | Test yang menguji satu alur lengkap lewat request HTTP | 14 |
| **unit test** | — | Test yang menguji satu fungsi atau kelas terpisah | 14 |
| **CI** | integrasi berkelanjutan | Test yang berjalan otomatis di server setiap kali kode dikirim | 14 |
| **empty state** | keadaan kosong | Tampilan saat belum ada data sama sekali | 15 |
| **refactor** | penataan ulang | Merapikan kode tanpa mengubah perilakunya | 15 |
