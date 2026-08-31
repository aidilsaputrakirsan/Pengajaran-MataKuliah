# Kapita Selekta: AI Engineering — Daftar Modul dan Cara Memakainya

**Merancang dan Membangun Produk Berbasis AI Agent | 2 SKS | Ganjil 2026/2027**
**Dosen: Aidil Saputra Kirsan, S.ST., M.Tr.Kom. | Tanpa asisten dosen**

---

## Bacalah berurutan

| File | Dibaca sebelum | Isi |
|---|---|---|
| [01-spesifikasi-proyek.md](01-spesifikasi-proyek.md) | Minggu 1 | Produk berlapis yang Anda bangun sepanjang semester |
| [02-modul-minggu-01-03.md](02-modul-minggu-01-03.md) | Minggu 1 | Blok A — Fondasi: memahami mesinnya |
| [03-modul-minggu-04-06.md](03-modul-minggu-04-06.md) | Minggu 4 | Blok B — Kendali: mengarahkan keluaran |
| [04-modul-minggu-07-09-uts.md](04-modul-minggu-07-09-uts.md) | Minggu 6 | Blok C — Grounding (RAG) dan aturan UTS |
| [05-modul-minggu-10-13.md](05-modul-minggu-10-13.md) | Minggu 9 | Blok D — Agentic: dari tool menjadi pelaku |
| [06-modul-minggu-14-16-uas.md](06-modul-minggu-14-16-uas.md) | Minggu 13 | Blok E — Kematangan, evaluasi, etika, dan UAS |

Baca bagian **Konsep** *sebelum* pertemuan. Tatap muka hanya 100 menit per minggu dan hampir seluruhnya dipakai untuk mengerjakan produk Anda sendiri, bukan mendengarkan ceramah. Itulah alasan bagian Konsep dibaca di rumah: waktu di kelas terlalu berharga untuk dipakai mengejar bacaan.

## Lampiran

| File | Dipakai untuk |
|---|---|
| [lampiran/A-pustaka-prompt.md](lampiran/A-pustaka-prompt.md) | Kumpulan pola prompt. Mulai Minggu 7, ini satu-satunya tempat contoh prompt tersedia |
| [lampiran/B-lembar-tema-proyek.md](lampiran/B-lembar-tema-proyek.md) | Format penetapan tema dan aturan penurunan lingkup dari kode peserta **K** |
| [lampiran/C-template-catatan-proses.md](lampiran/C-template-catatan-proses.md) | Catatan proses mingguan, maksimal 2 halaman. Wajib setiap minggu |
| [lampiran/D-template-laporan-evaluasi.md](lampiran/D-template-laporan-evaluasi.md) | Laporan evaluasi dan kajian risiko, dipakai Minggu 14–16 |
| [lampiran/E-bank-pertanyaan-pertanggungjawaban.md](lampiran/E-bank-pertanyaan-pertanggungjawaban.md) | Seluruh pertanyaan yang mungkin diajukan saat UTS dan UAS, dibagikan terbuka |
| [lampiran/F-panduan-tool.md](lampiran/F-panduan-tool.md) | Penyiapan lingkungan kerja, model gateway, dan pengendalian biaya |
| [lampiran/G-glosarium.md](lampiran/G-glosarium.md) | Padanan istilah Inggris dan Indonesia, serta di minggu mana tiap istilah diperkenalkan |
| [lampiran/H-rencana-instrumen-dan-rubrik.md](lampiran/H-rencana-instrumen-dan-rubrik.md) | Seluruh yang dinilai sepanjang semester beserta rubriknya, dalam satu tempat |

Lampiran E memuat semua pertanyaan pertanggungjawaban. Ia tidak dirahasiakan karena tidak satu pun dapat dijawab dengan menghafal — semuanya menunjuk ke produk Anda sendiri dan menanyakan keputusan yang Anda ambil di dalamnya.

---

## Enam hal yang perlu Anda ketahui sebelum Minggu 1

**1. Anda tidak perlu bisa memrogram, tetapi Anda wajib bisa menjelaskan.**
Kelas ini memakai *AI-assisted development*: Anda merakit sistem dengan bantuan asisten pemrograman berbasis AI. Jadi yang dinilai bukan kemampuan mengetik kode, tapi ketepatan rancangan, kualitas evaluasi, dan kematangan pertimbangan etis Anda. Kemampuan menjelaskan karya sendiri adalah inti dari semuanya — dan itu keterampilan yang bisa dilatih, bukan bakat.

**2. Kelas ini tidak punya asisten.** Konsekuensinya tiga:

| Konsekuensi | Artinya bagi Anda |
|---|---|
| Tidak ada yang mengecek pekerjaan Anda baris per baris di kelas | Verifikasi dilakukan **oleh Anda sendiri** memakai daftar periksa di akhir tiap minggu |
| Bantuan teknis satu-per-satu terbatas | Pertanyaan teknis dibawa ke forum kelas dulu; yang sudah terjawab di sana tidak diulang di kelas |
| Penilaian bertumpu pada artefak, bukan pengamatan langsung | Repositori dan catatan proses adalah satu-satunya jendela dosen ke kerja Anda — kerja bagus yang tidak tercatat di sana tidak bisa dinilai |

Aturan **tiga puluh menit**: kalau macet, coba mandiri 30 menit dulu, lalu tulis pertanyaan di forum memakai format di Lampiran F. Format itu meminta Anda menyebutkan apa yang sudah dicoba — bukan untuk mempersulit, tapi karena separuh persoalan biasanya terpecahkan saat Anda menuliskannya, dan sisanya jadi jauh lebih cepat dijawab.

**3. Produk Anda berasal dari bidang keilmuan Anda sendiri.**
Kelas ini terbuka lintas prodi. Anda tidak mengerjakan kasus contoh dari dosen; Anda mengangkat persoalan nyata dari prodi Anda. Tema ditetapkan Minggu 4 dan tidak boleh diganti setelah Minggu 6. Aturan lengkap ada di Lampiran B.

**4. Satu produk, dikerjakan berlapis, tidak diulang dari nol.**
Setiap blok menambah satu lapisan ke produk yang sama:

```
Minggu 1–3   Fondasi        →  Anda memahami mesin dan berhasil memanggil model
Minggu 4–6   Kendali        →  Produk menghasilkan keluaran terstruktur dan memanggil tool
Minggu 7–9   Grounding      →  Produk menjawab berdasarkan dokumen milik Anda, dengan sumber
Minggu 10–13 Agentic       →  Produk menyelesaikan tugas bertahap secara mandiri, dengan guardrails
Minggu 14–16 Kematangan     →  Produk teruji, terukur biayanya, dan dipertanggungjawabkan
```

Tiap lapisan berdiri di atas lapisan sebelumnya, jadi tidak ada jalan pintas ke Minggu 13. Sisi baiknya: Anda tidak pernah memulai dari nol, dan kerja Minggu 2 masih terpakai di Minggu 15.

**5. Modul akan berhenti memberi contoh.**
Minggu 1–6 memberi prompt dan langkah yang siap pakai. Mulai Minggu 7 contoh prompt dipindah ke Lampiran A tanpa urutan pengerjaan. Mulai Minggu 10 yang Anda terima hanya skenario dan kriteria sukses. Enam minggu pertama adalah persiapan untuk itu.

**6. Setiap minggu punya empat tahap.**

| Tahap | Yang dikerjakan | Waktu khas |
|---|---|---|
| **AMATI** | Menjalankan sesuatu yang sudah bekerja dan menjelaskan *mengapa* ia bekerja. Dikerjakan **tanpa AI** | 20 menit |
| **PATAHKAN** | Merusak sendiri dari tabel percobaan. **Kolom prediksi diisi sebelum mencoba** | 25 menit |
| **PERBAIKI** | Memperbaiki kasus rusak yang disediakan dosen; jumlah kesalahannya selalu diberitahukan | 20 menit |
| **RAKIT** | Menambah satu lapisan ke produk Anda sendiri | 35 menit + mandiri |

Kolom prediksi tidak dinilai benar atau salahnya. Yang dinilai adalah apakah Anda mengisinya **sebelum** mencoba. Prediksi yang meleset dan tercatat jujur justru bahan terbaik untuk catatan proses Anda — di situlah terlihat apa yang tadinya Anda kira, dan apa yang ternyata terjadi.

Tahap AMATI dikerjakan tanpa AI karena ia satu-satunya bagian yang mengukur apakah Anda benar-benar paham. Tiga tahap lain boleh, bahkan dianjurkan, memakai AI.

---

## Ketentuan penggunaan AI

Penggunaan AI **diizinkan dan diharapkan**, dengan tiga syarat:

1. **Transparan.** Setiap penggunaan dicantumkan pada catatan proses (Lampiran C): apa yang Anda minta, apa yang dikembalikan, dan apa yang Anda ubah.
2. **Terpahami.** Anda mampu menjelaskan setiap bagian karya Anda. Kalau ada bagian yang belum bisa Anda jelaskan, itu tanda bagian itu belum selesai dipelajari — dan lebih baik ketahuan sekarang daripada di Minggu 16.
3. **Bertanggung jawab.** Kesalahan pada karya tetap tanggung jawab Anda, bukan alat yang Anda pakai.

**Satu batas yang tidak boleh dilewati:** jangan menempelkan data pribadi orang lain, dokumen rahasia organisasi, kredensial asli, atau data penelitian pihak lain yang belum dipublikasikan ke layanan AI mana pun. Ini bukan soal nilai; ini kebiasaan profesi yang akan Anda bawa keluar dari kelas ini.

---

## Penilaian

| No | Kategori Nilai | Bentuk Penilaian | Bobot |
|:--:|---|---|---:|
| 1 | Tes / Ujian (Quiz) | Kuis konsep di awal pertemuan Minggu 3 dan Minggu 6 | 10% |
| 2 | Tugas | Luaran bertahap produk dan peer review | 25% |
| 3 | Sikap dan Profesionalisme | Partisipasi, diskusi kritis, kejujuran catatan proses | 15% |
| 4 | Proyek | Produk akhir, laporan evaluasi, kajian risiko etis | 30% |
| 5 | Tes / Ujian (UTS) | Presentasi rancangan sistem, Minggu 8 | 10% |
| 6 | Tes / Ujian (UAS) | Demonstrasi produk dan pertanggungjawaban, Minggu 16 | 10% |
| | **Total** | | **100%** |

Bobot di dalam setiap luaran mingguan: **rancangan dan alasannya 35%, bukti bahwa ia bekerja 25%, catatan proses 25%, tantangan wajib 15%.**

Perhatikan bahwa "sistemnya jalan" hanya bernilai seperempat. Sistem yang jalan tanpa alasan yang dapat dijelaskan adalah kebetulan, dan kebetulan tidak dinilai.

## Yang dikumpulkan setiap minggu

Satu file terkompresi bernama `<NIM>-m<minggu>.zip`, berisi:

1. **Artefak produk** — file kerja Anda, kumulatif, bukan hanya bagian minggu itu
2. **`catatan-proses.md`** — memakai template Lampiran C, maksimal 2 halaman
3. **`bukti/`** — tangkapan layar atau rekaman keluaran yang membuktikan sistem berjalan, beserta masukan yang dipakai

Batas pengumpulan: **H+2 setelah pertemuan, pukul 23.59.**

---

## Aturan main

Enam poin di atas menjelaskan cara kelas ini bekerja. Bagian ini menjelaskan batasnya. Semuanya dikumpulkan di satu tempat supaya tidak ada yang tersembunyi di tengah modul, dan supaya Anda bisa merujuknya kapan pun.

| Hal | Ketentuan |
|---|---|
| Karya yang tidak dapat dijelaskan | Dinilai nol pada komponen bersangkutan, berapa pun kualitas luarannya. Ini berlaku sama untuk semua orang dan tidak dinegosiasikan per kasus |
| Keterlambatan | Sampai 24 jam: maksimal 80%. Lebih dari itu: nol, kecuali ada alasan sah yang disampaikan **sebelum** tenggat |
| Tema proyek | Ditetapkan Minggu 4, dapat diganti sampai Minggu 6, terkunci setelahnya |
| Data sensitif ke layanan AI | Ditangani terpisah dari penilaian, sebagai persoalan etika profesi |
| Klaim keberhasilan tanpa bukti | Dinilai lebih rendah daripada laporan kegagalan yang jujur. Ini disengaja |

Kalau ada keadaan yang membuat salah satu ketentuan ini memberatkan Anda secara tidak wajar — sakit, urusan keluarga, kendala perangkat — bicarakan lebih awal. Yang sulit ditolong adalah kabar yang datang setelah tenggat lewat.

---

## Kalender ringkas

| Mgg | Pokok Bahasan | Tahap | Yang dikumpulkan |
|:--:|---|---|---|
| 1 | Orientasi, kontrak kuliah, lanskap AI generatif | AMATI | Peta minat dan calon persoalan |
| 2 | Anatomi model: token, konteks, suhu, halusinasi | AMATI–PATAHKAN | Laporan pengamatan perilaku model |
| 3 | Lingkungan kerja dan model gateway | RAKIT | Bukti pemanggilan model pertama · **Kuis 1** |
| 4 | Instruksi terstruktur dan teknik penalaran | Empat tahap | **Lembar tema proyek** + pustaka instruksi v1 |
| 5 | Keluaran terstruktur dan skema sebagai kontrak | Empat tahap | Prototipe berkeluaran berformat |
| 6 | Pemanggilan tool | Empat tahap | Prototipe memanggil tool · **Kuis 2** |
| 7 | Konsep *embedding* dan retrieval | Empat tahap | Rancangan alur RAG |
| 8 | **UTS** — presentasi rancangan sistem | — | Dokumen rancangan + presentasi |
| 9 | Merakit RAG utuh, menangani jawaban tak berdasar | Empat tahap | Produk menjawab berbasis dokumen |
| 10 | Dari *workflow* ke *agent* | Empat tahap | Analisis komparatif dua pendekatan |
| 11 | Tool, memori, orkestrasi langkah majemuk | Empat tahap | Agen menyelesaikan tugas bertahap |
| 12 | Studi kasus praktisi: arsitektur agen produksi | AMATI | Kritik arsitektur + peer review |
| 13 | *Guardrails* dan *human-in-the-loop* | Empat tahap | Mekanisme guardrails pada produk |
| 14 | Evaluasi sistematis dan pengendalian biaya | Empat tahap | Set uji dan laporan evaluasi |
| 15 | Keamanan, etika, bias, tanggung jawab profesional | Empat tahap | Kajian risiko dan pernyataan etis |
| 16 | **UAS** — demonstrasi dan pertanggungjawaban | — | Portofolio lengkap |
