# Lampiran A — Pustaka Prompt

**Kapita Selekta: AI Engineering | Ganjil 2026/2027**

Mulai Minggu 7, ini satu-satunya tempat contoh prompt tersedia. Pola di bawah **tidak berurutan** dan tidak dipetakan ke minggu tertentu — Anda yang memilih mana yang relevan dan menyesuaikannya dengan kasus Anda.

Semua prompt di sini adalah kerangka. Menempelkannya apa adanya tanpa menyesuaikan konteks akan menghasilkan jawaban umum yang tidak berguna. Bagian bertanda `<...>` wajib Anda isi.

---

## Aturan pemakaian

**Tiga syarat yang berlaku pada setiap prompt di lampiran ini:**

1. Setiap penggunaan dicatat pada catatan proses: apa yang diminta, apa yang dikembalikan, apa yang Anda ubah.
2. Anda wajib dapat menjelaskan setiap bagian hasilnya.
3. Kesalahan pada karya tetap tanggung jawab Anda.

**Yang tidak boleh ditempel ke layanan AI mana pun:** API key, data pribadi orang lain, dokumen rahasia organisasi, data penelitian pihak lain yang belum dipublikasikan.

**Satu kebiasaan yang menghemat banyak waktu:** hampir setiap prompt di bawah diakhiri dengan larangan langsung memberi solusi. Itu disengaja. AI yang langsung memberi jawaban membuat Anda melewatkan tahap mendiagnosis, dan diagnosis adalah bagian yang dinilai.

---

## A. Pola Merancang

### A1 — Penggali persoalan

```
Saya mahasiswa <PRODI> semester <N>. Saya mencari persoalan nyata di
bidang saya yang layak diselesaikan dengan AI generatif.

Ajukan 8 pertanyaan, SATU PER SATU, untuk menggali persoalan yang
benar-benar saya alami.

Aturan:
- Jangan menyarankan ide sebelum 8 pertanyaan selesai.
- Kalau jawaban saya kabur, gali lebih dalam, jangan diterima.
- Setelah selesai, sebutkan 3 persoalan dari jawaban saya. Untuk
  MASING-MASING, sebutkan satu alasan mengapa AI generatif MUNGKIN
  BUKAN jawaban yang tepat.
```

### A2 — Penguji kelayakan tema

```
Tema proyek saya: <DESKRIPSI>
Bidang saya: <PRODI>
Dokumen rujukan yang saya miliki: <DAFTAR>

Uji tema ini terhadap lima kriteria penolakan berikut, satu per satu,
dan berikan vonis LOLOS / RAGU / GUGUR beserta alasan:
1. AI dipaksakan pada persoalan yang sebenarnya deterministik
2. Keluaran tidak dapat dibuktikan benar atau salah
3. Tidak ada sumber pengetahuan yang sah dan dapat saya akses
4. Bergantung pada data pribadi orang sungguhan
5. Terlalu luas untuk dikerjakan seorang diri dalam 12 minggu

Untuk setiap vonis RAGU atau GUGUR, sebutkan penyempitan tema
yang membuatnya LOLOS.
```

### A3 — Pemecah tugas

```
Tugas produk saya: <DESKRIPSI>
Saat ini saya mengerjakannya dalam SATU prompt besar.

1. Pecah menjadi langkah terkecil yang masuk akal.
2. Untuk tiap langkah: masukan, keluaran, dan apakah ia benar-benar
   memerlukan model bahasa atau cukup aturan biasa.
3. Tandai langkah yang paling mungkin gagal, dan mengapa.
4. Sebutkan satu KERUGIAN pemecahan ini dibanding satu prompt besar.
```

### A4 — Penentu workflow atau agen

```
Tugas: <DESKRIPSI TUGAS BERLANGKAH MAJEMUK>
Tool yang saya punya: <DAFTAR>

Rancang tugas ini dalam DUA bentuk: workflow tetap dan agen.
Bandingkan pada lima sumbu: keterdugaan, biaya, kemudahan pengujian,
penanganan kegagalan, mutu hasil.

Jangan merekomendasikan salah satu sebelum kelima sumbu selesai
dibandingkan. Setelah itu, sebutkan bentuk mana yang kamu pilih
DAN pertukaran apa yang saya terima dengan memilihnya.
```

---

## B. Pola Mengendalikan Keluaran

### B1 — Pengkritik instruksi

```
Berikut instruksi sistem saya:
<TEMPEL>

JANGAN menulis ulang.
1. Periksa terhadap enam sumbu: peran, konteks, tugas, batasan,
   contoh, format. Sebutkan yang lemah atau hilang.
2. Sebutkan 3 masukan yang akan MEMBUATNYA GAGAL, dan gagalnya
   seperti apa.
3. Tunjukkan bagian yang ambigu dan bisa ditafsirkan dua cara.
4. Baru setelah itu tanyakan apakah saya mau versi perbaikannya.
```

### B2 — Perancang contoh sulit

```
Tugas produk saya: <DESKRIPSI>

Rancang 5 contoh pasangan masukan-keluaran untuk few-shot, dengan
komposisi:
- 1 kasus khas
- 2 kasus batas (data tak lengkap, format tak wajar)
- 1 kasus yang SEHARUSNYA ditolak sistem
- 1 kasus ambigu yang menuntut klarifikasi

Untuk tiap contoh, jelaskan APA yang diajarkannya kepada model.
```

### B3 — Perancang skema

```
Keluaran yang saya inginkan: <DESKRIPSI>
Keluaran ini dipakai untuk: <APA SETELAHNYA>

1. Rancang skema: medan, tipe, wajib/opsional, nilai yang sah.
2. Untuk medan bernilai terbatas, sebutkan daftarnya dan pastikan
   ADA nilai untuk kasus "tidak dapat ditentukan".
3. Sebutkan medan yang saya LUPA dan biasanya diperlukan.
4. Tunjukkan satu masukan yang membuat skema ini tidak memadai.
```

### B4 — Perancang tool

```
Produk saya: <DESKRIPSI>
Tugas yang harus selesai: <TUGAS>

1. Tool apa saja yang dibutuhkan?
2. Untuk tiap tool: nama, kegunaan, parameter, keluaran, dan
   SATU KALIMAT tentang kapan ia TIDAK boleh dipakai.
3. Tandai mana yang hanya membaca dan mana yang mengubah/mengirim.
4. Sebutkan bagian tugas yang sebenarnya tidak butuh tool
   maupun model bahasa, cukup aturan biasa.
```

---

## C. Pola Membumikan (RAG)

### C1 — Penimbang strategi chunking

```
Dokumen rujukan saya: <JENIS, JUMLAH, PANJANG, STRUKTURNYA>
Pertanyaan khas pengguna: <3 CONTOH>

1. Usulkan 3 strategi chunking yang berbeda untuk dokumen ini.
2. Untuk tiap strategi: ukuran, tumpang tindih, metadata yang ikut
   disimpan, dan JENIS PERTANYAAN APA yang akan gagal olehnya.
3. Jangan pilih satu; sebutkan cara saya MENGUJI mana yang terbaik
   untuk kasus saya.
```

### C2 — Pembangkit pertanyaan uji RAG

```
Isi ringkas dokumen rujukan saya:
<RINGKASAN PER DOKUMEN>

Buat 10 pertanyaan uji dengan komposisi:
- 6 yang jawabannya jelas ada pada satu dokumen
- 2 yang jawabannya tersebar di dua dokumen berbeda
- 2 yang jawabannya TIDAK ADA tetapi terdengar seolah-olah ada

Untuk tiap pertanyaan, sebutkan jawaban acuan dan dokumen sumbernya.
Untuk 2 terakhir, jelaskan mengapa ia terdengar seolah-olah ada.
```

### C3 — Pendiagnosis kegagalan RAG

```
Pertanyaan  : <PERTANYAAN>
Chunk yang terambil: <TEMPEL POTONGAN>
Jawaban sistem: <TEMPEL JAWABAN>
Jawaban yang benar: <JAWABAN ACUAN>

JANGAN langsung memberi perbaikan.
1. Tentukan jenis kegagalannya: gagal retrieval, gagal kesetiaan,
   atau gagal cakupan. Jelaskan dasarnya.
2. Sebutkan satu pemeriksaan yang bisa MEMBANTAH diagnosis itu.
3. Baru setelah saya jawab, usulkan perbaikan — dan sebutkan
   perbaikan itu menyentuh bagian mana dari alur.
```

---

## D. Pola Merakit Agen

### D1 — Peninjau kriteria berhenti

```
Agen saya bertugas: <DESKRIPSI>
Tool yang tersedia: <DAFTAR>
Kriteria berhenti saat ini: <TEMPEL>
Batas langkah: <N>

1. Sebutkan 3 keadaan yang membuat agen ini BERHENTI TERLALU CEPAT.
2. Sebutkan 3 keadaan yang membuatnya TIDAK PERNAH BERHENTI.
3. Untuk masing-masing, sebutkan mekanisme pencegahnya beserta
   kerugian mekanisme itu.
```

### D2 — Pembaca trace agen

```
Berikut trace agen saya pada satu tugas yang gagal:
<TEMPEL JEJAK LANGKAH DEMI LANGKAH>

JANGAN langsung memperbaiki.
1. Tentukan langkah mana kegagalan BERMULA — bukan langkah mana ia
   terlihat.
2. Jelaskan dasar penentuan itu.
3. Sebutkan satu hal pada trace ini yang TIDAK bisa saya percaya
   sebagai alasan sebenarnya dari tindakan agen, dan mengapa.
4. Tanyakan informasi tambahan apa yang kamu butuhkan.
```

### D3 — Perancang serangan

```
Produk saya: <DESKRIPSI>
Tool beserta kewenangannya: <TABEL>
Sumber dokumen rujukan: <ASAL DOKUMEN>

Rancang 6 cara membuat sistem ini melakukan hal yang tidak saya
inginkan. Sedikitnya 3 di antaranya harus berupa serangan lewat
DOKUMEN RUJUKAN, bukan lewat masukan pengguna.

Untuk tiap serangan: bagian mana yang tumbang, dan pada lapis
guardrails mana ia seharusnya dihentikan.
Jangan memberi perbaikannya.
```

---

## E. Pola Mengevaluasi

### E1 — Penyusun set uji

```
Produk saya: <DESKRIPSI>
Jumlah kasus uji yang saya butuhkan: <N>
Kegagalan yang pernah saya temukan: <DAFTAR>

Susun rancangan set uji dengan komposisi: 40% kasus khas, 25% kasus
batas, 20% kasus yang HARUS DITOLAK, 15% dari daftar kegagalan saya.

Untuk tiap kasus: masukan, jawaban acuan, dan apa yang diukur kasus
itu. Jangan membuat jawaban acuan untuk kasus dari bidang saya —
tandai saja bahwa saya yang harus mengisinya.
```

### E2 — Perumus kriteria biner

```
Set uji saya menilai: <APA YANG DIUKUR>

Rumuskan 5 pertanyaan penilaian BINER (ya/tidak) yang bisa
diterapkan konsisten oleh dua penilai berbeda.

Untuk tiap pertanyaan, tunjukkan satu kasus batas di mana dua
penilai bisa berbeda pendapat, dan perbaiki rumusannya sampai
perbedaan itu hilang.
```

### E3 — Penghitung biaya

```
Instruksi sistem saya: <N> kata
Masukan pengguna khas: <N> kata
Konteks RAG khas: <N> chunk × <N> kata
Keluaran khas: <N> kata
Langkah agen khas / terburuk: <N> / <N>
Tarif model: masukan <X>, keluaran <Y> per juta token

1. Perkirakan token tiap bagian (teks Indonesia).
2. Hitung biaya per permintaan khas dan per permintaan TERBURUK.
3. Hitung untuk 100 pengguna × 20 permintaan per bulan.
4. Sebutkan bagian yang paling menyumbang biaya dan 3 cara
   menurunkannya, beserta apa yang dikorbankan masing-masing.
Tunjukkan perhitungannya, bukan hanya hasil.
```

### E4 — Pengkritik laporan evaluasi

```
Berikut laporan evaluasi saya:
<TEMPEL>

Berperanlah sebagai penguji yang skeptis.
1. Sebutkan klaim mana yang TIDAK didukung angka yang saya sajikan.
2. Sebutkan angka mana yang bisa menyesatkan karena cara
   pengukurannya tidak dijelaskan.
3. Ajukan 5 pertanyaan yang akan sulit saya jawab saat ujian.
Jangan memperbaiki laporan saya.
```

---

## F. Pola Mendiagnosis

### F1 — Pembaca error

```
Saya menemui error berikut. API key SUDAH saya sensor.
<TEMPEL PESAN GALAT>

JANGAN langsung memberi perbaikan.
1. Terjemahkan ke bahasa manusia: apa yang gagal, di lapisan mana.
2. Sebutkan 3 penyebab paling mungkin, urut dari yang paling sering.
3. Untuk tiap penyebab, satu pemeriksaan yang bisa MEMBANTAHNYA.
4. Tanyakan hasil pemeriksaan mana yang ingin kamu lihat dulu.
```

### F2 — Pendamping tanpa jawaban

```
Saya sedang belajar dan TIDAK ingin diberi jawaban langsung.

Persoalan saya: <DESKRIPSI>
Yang sudah saya coba: <DAFTAR>
Yang saya duga penyebabnya: <DUGAAN>

Balas HANYA dengan pertanyaan yang mengarahkan saya menemukan
sendiri. Maksimal 2 pertanyaan per balasan. Baru berikan jawaban
bila saya menulis kata "MENYERAH".
```

### F3 — Penguji pemahaman

```
Saya baru saja merakit bagian berikut pada produk saya:
<TEMPEL / DESKRIPSIKAN>

Ujilah pemahaman saya. Ajukan 5 pertanyaan berjenjang dari mudah
ke sulit tentang MENGAPA bagian ini dirancang begitu — bukan
tentang apa fungsinya.

Ajukan satu per satu. Jangan beri tahu jawaban benarnya sebelum
saya mencoba. Setelah kelimanya, sebutkan bagian mana yang
pemahaman saya paling lemah.
```

F3 layak dijalankan sebelum UTS dan UAS. Ia mendekati bentuk pertanyaan yang akan diajukan dosen.

---

## G. Pola Meminta Bantuan di Forum Kelas

Bukan prompt untuk AI, melainkan format pertanyaan yang akan dijawab di forum kelas. Pertanyaan yang tidak memakai format ini tidak dijawab.

```
JUDUL   : <gejala dalam satu kalimat, bukan "tolong bantu">

Konteks : minggu ke-<N>, tahap <AMATI/PATAHKAN/PERBAIKI/RAKIT>
Yang saya harapkan terjadi :
Yang sebenarnya terjadi    :
Yang sudah saya coba (min. 3) :
  1.
  2.
  3.
Dugaan saya tentang penyebabnya :
Yang saya butuhkan : <arah, bukan jawaban jadi>
```

Berlaku **aturan tiga puluh menit**: coba mandiri 30 menit sebelum bertanya. Karena kelas ini tidak memiliki asisten, forum adalah saluran bantuan utama di luar jam kuliah, dan kualitasnya bergantung pada kualitas pertanyaan yang masuk.
