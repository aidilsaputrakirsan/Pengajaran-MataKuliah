# Kapita Selekta: AI Engineering
### Merancang dan Membangun Produk Berbasis AI Agent

**Mata Kuliah Pengayaan Lintas Program Studi**
Semester Ganjil 2026/2027 · 2 SKS · Semester 5–7

---

## 0. Struktur File

Dokumen ini adalah rancangan pembelajaran. Modul pelaksanaannya terpisah dalam dua folder.

| Folder | Pembaca | Boleh diunggah ke LMS |
|---|---|---|
| [mahasiswa/](mahasiswa/) | Mahasiswa | **Ya, seluruhnya** |
| pengajar/ (privat — repo dosen) | Dosen | **Tidak** |

Aturannya sederhana: seluruh isi `mahasiswa/` aman diunggah tanpa perlu diperiksa lagi. Tidak ada kunci jawaban, bank soal kuis, maupun catatan internal di dalamnya.

### Untuk mahasiswa

| File | Dibagikan | Isi |
|---|---|---|
| [mahasiswa/00-daftar-modul.md](mahasiswa/00-daftar-modul.md) | Minggu 1 | Indeks, aturan main, penilaian |
| [mahasiswa/01-spesifikasi-proyek.md](mahasiswa/01-spesifikasi-proyek.md) | Minggu 1 | Produk berlapis lima tahap |
| [mahasiswa/02-modul-minggu-01-03.md](mahasiswa/02-modul-minggu-01-03.md) | Minggu 1 | Blok A — Fondasi |
| [mahasiswa/03-modul-minggu-04-06.md](mahasiswa/03-modul-minggu-04-06.md) | Minggu 3 | Blok B — Kendali |
| [mahasiswa/04-modul-minggu-07-09-uts.md](mahasiswa/04-modul-minggu-07-09-uts.md) | Minggu 6 | Blok C — Grounding dan aturan UTS |
| [mahasiswa/05-modul-minggu-10-13.md](mahasiswa/05-modul-minggu-10-13.md) | Minggu 9 | Blok D — Agentic |
| [mahasiswa/06-modul-minggu-14-16-uas.md](mahasiswa/06-modul-minggu-14-16-uas.md) | Minggu 13 | Blok E — Kematangan dan aturan UAS |
| [mahasiswa/lampiran/](mahasiswa/lampiran/) | Minggu 1 | A sampai G, ketujuhnya untuk mahasiswa |

Kode peserta **K** dibagikan Minggu 1, satu per mahasiswa, dan menurunkan besaran lingkup wajib produknya (aturan pada Lampiran B).

### Untuk pengajar — jangan dibagikan

| File | Alasan |
|---|---|
| pengajar/01-rancangan-modul.md (privat — repo dosen) | Dokumen keputusan internal dan peta beban kerja |
| pengajar/02-panduan-sesi.md (privat — repo dosen) | Rundown, petunjuk saat kelas macet, titik rawan |
| pengajar/03-rubrik-dan-penilaian.md (privat — repo dosen) | **Memuat bank soal kuis** dan lembar penilaian |
| pengajar/penilaian/ (privat — repo dosen) | Lembar nilai CSV berisi nama dan nilai mahasiswa |
| pengajar/tools/ (privat — repo dosen) | Skrip rekap nilai |

### Penyesuaian terhadap kendala mata kuliah

Modul diturunkan dari struktur DMJK (4 SKS, dua asisten) dengan empat penyesuaian pokok — alasan lengkapnya di pengajar/01-rancangan-modul.md (privat — repo dosen):

| Kendala | Penyesuaian |
|---|---|
| 2 SKS, 100 menit/minggu | Konsep dibaca sebelum kelas; tatap muka dipakai untuk mengerjakan. Tahap RAKIT sepenuhnya mandiri |
| Tanpa asisten | Verifikasi checkpoint di layar diganti daftar periksa mandiri + catatan proses wajib + viva pada UTS/UAS. Kasus untuk diperbaiki sudah tertulis di modul, sehingga beban persiapan mingguan mendekati nol |
| Tanpa prasyarat pemrograman | Bobot "sistem berfungsi" 25%; "rancangan dan alasannya" 35% |
| Lintas program studi | Tidak ada kasus tunggal; setiap mahasiswa membawa persoalan dari bidangnya, dengan lingkup diturunkan dari kode peserta K |

Siklus mingguan **AMATI → PATAHKAN → PERBAIKI → RAKIT** adalah padanan siklus READ/BREAK/FIX/BUILD pada DMJK.

---

## 1. Identitas Mata Kuliah

| Komponen | Keterangan |
|---|---|
| Nama Mata Kuliah | Kapita Selekta: AI Engineering |
| Kode Mata Kuliah | *(menyusul)* |
| Dosen Pengampu | Aidil Saputra Kirsan, S.ST., M.Tr.Kom. |
| Tanggal Penetapan | *(menyusul)* |
| Bobot | 2 SKS (Teori) |
| Sifat | Pilihan / Pengayaan, terbuka lintas program studi |
| Semester Sasaran | 5, 6, atau 7 |
| Prasyarat Formal | Tidak ada |
| Prasyarat Teknis | **Tidak diperlukan kemampuan pemrograman.** Cukup literasi komputer dasar dan kemauan bereksperimen. |
| Bentuk Pembelajaran | Kuliah, studi kasus, diskusi kritis, dan proyek terbimbing berbasis *AI-assisted development* |
| Praktikum Terpisah | Tidak ada (seluruh aktivitas praktik terintegrasi dalam jam kuliah dan tugas mandiri) |

---

## 2. Deskripsi Mata Kuliah

Mata kuliah ini membekali mahasiswa dari beragam latar belakang keilmuan dengan kemampuan **merancang, membangun, dan mengevaluasi produk digital yang ditenagai kecerdasan buatan generatif**, tanpa mensyaratkan kemampuan pemrograman sebagai titik awal.

Fokusnya bukan pada pembuatan model kecerdasan buatan dari nol — itu ranah *Machine Learning*. Fokusnya adalah **AI Engineering**: disiplin merekayasa sistem dan produk di atas *foundation model* yang sudah tersedia. Mahasiswa belajar memperlakukan model bahasa besar sebagai sebuah komponen sistem yang memiliki karakteristik, keterbatasan, biaya, dan risiko yang harus dikelola secara sadar.

Perjalanan kelas bergerak dari mengendalikan keluaran model, membumikannya pada sumber pengetahuan yang sahih, hingga merakitnya menjadi **AI Agent** — sistem yang mampu bernalar, menggunakan tool, dan menyelesaikan tugas bertahap secara mandiri. Sepanjang semester mahasiswa mengerjakan **satu produk yang sama secara berlapis**, sehingga pada akhir kuliah mereka memiliki satu karya utuh yang dapat dipertanggungjawabkan.

Karena mata kuliah ini bersifat pengayaan lintas prodi, setiap mahasiswa didorong mengangkat persoalan nyata dari **bidang keilmuannya masing-masing** sebagai kasus proyek. Keberagaman latar belakang peserta diperlakukan sebagai kekuatan, bukan hambatan.

Sebagai konsekuensi dari asumsi tanpa prasyarat pemrograman, kelas ini menggunakan pendekatan ***AI-assisted development***: mahasiswa merakit sistem dengan bantuan asisten pemrograman berbasis AI. Yang dinilai adalah **ketepatan rancangan, kualitas evaluasi, dan kematangan pertimbangan etis** — bukan kefasihan menulis sintaks.

---

## 3. Capaian Pembelajaran Lulusan (CPL) yang Dititipkan

Kapita Selekta adalah mata kuliah pilihan bertopik berganti: pengampu dan temanya dapat berbeda tiap semester, sementara **RPS-nya satu dan dipakai bersama**. Karena itu CPL, CPMK, dan Bahan Kajian dirumuskan **netral terhadap topik**. Yang menyesuaikan tiap semester hanyalah Sub-CPMK beserta isian bahan kajiannya.

Satu CPL saja, diambil dari daftar CPL Program Studi Sistem Informasi.

| Kode | Rumusan |
|---|---|
| **CPL-6** | Mampu menghasilkan rekomendasi manajemen dan SI/TI dengan mempertimbangkan keselarasan bisnis dan SI/TI serta mengevaluasi dampak dan penggunaannya untuk mencapai tujuan organisasi |

### Kenapa CPL-6

Tiga alasan, dan ketiganya bertahan meski topik mata kuliah berganti:

**Kata kerjanya sepasang, bukan tunggal.** CPL-6 menuntut *menghasilkan* sekaligus *mengevaluasi dampak dan penggunaannya*. Itu persis bentuk Kapita Selekta: mahasiswa membangun sesuatu, lalu menilai keandalan dan risikonya. CPL yang hanya menuntut satu kata kerja akan menyisakan separuh mata kuliah tanpa payung.

**Frasa "keselarasan bisnis dan SI/TI ... tujuan organisasi" menjaga arahnya tetap Sistem Informasi.** Inilah yang diminta koordinator prodi — generik, tetapi tidak melayang ke ranah teknik murni.

**Tidak berbenturan dengan mata kuliah lain.** CPL-8 sudah dipakai Pemrograman Web dan CPL-9 oleh DMJK. Memakai CPL-6 memperluas cakupan CPL yang terlayani kurikulum, bukan menumpuk di CPL yang sama.

### Yang ditimbang dan tidak dipilih

Daftar CPL prodi terbagi dua: **CPL-1 sampai 5** bersifat generik lintas bidang, **CPL-6 sampai 10** bersifat teknis Sistem Informasi.

| CPL | Kenapa tidak dipilih |
|:--:|---|
| 1 — komunikasi efektif | Terlayani sebagai bagian dari CPMK, tetapi terlalu sempit untuk menaungi seluruh mata kuliah |
| 2 — berpikir kritis dan penyelesaian masalah kompleks | Sangat cocok dan netral topik, tetapi generik lintas bidang — koordinator prodi meminta arah Sistem Informasi |
| 3 — kolaborasi tim multidisiplin | Kelas ini dinilai individual; kerja tim bukan bentuk pokoknya |
| 4 — nilai Pancasila, etika, tanggung jawab profesional | Terlayani lewat kajian risiko, tetapi hanya sebagian mata kuliah |
| 5 — belajar mandiri sepanjang hayat | Pasangan alami untuk mata kuliah bertopik berganti; kandidat terkuat **bila** prodi mengizinkan CPL kedua |
| 7 — analisis data | Terikat pengolahan data, cocok hanya bila topik semester kebetulan analitik |
| 8 — membangun sistem informasi | Menuntut metodologi pengembangan dan alat pemodelan yang belum tentu diajarkan topik lain; sudah dipakai Pemrograman Web |
| 10 — manajemen proyek SI | Berbicara tentang mengelola proyek, bukan menghasilkan solusinya |

**CPL-2 dan CPL-5 adalah pesaing terdekat.** Keduanya netral topik secara bawaan — justru itu kekuatannya untuk mata kuliah bertopik berganti. Yang membuat CPL-6 menang adalah arahan koordinator prodi agar rumusan condong ke Sistem Informasi, dan CPL-2 maupun CPL-5 tidak menyebut SI/TI sama sekali. Kalau suatu saat prodi mengizinkan dua CPL dititipkan, **CPL-5** adalah tambahan yang paling masuk akal.

Kalau prodi menghendaki kata kerja "membangun" tampil eksplisit, **CPL-8** adalah alternatif terdekat — dengan konsekuensi rumusannya menuntut metodologi dan pemodelan sistem yang harus benar-benar diajarkan tiap semester.

---

## 4. Capaian Pembelajaran Mata Kuliah (CPMK)

Satu CPMK, seperti pada DMJK dan Pemrograman Web.

| Kode | Rumusan | CPL |
|---|---|:--:|
| **CPMK-1** | Mahasiswa mampu menerapkan teknologi, kerangka kerja, atau paradigma mutakhir pada domain keilmuan Sistem Informasi untuk menjawab kebutuhan organisasi, serta mengevaluasi kesesuaian, dampak, risiko, dan implikasi etis penerapannya dan mengomunikasikannya secara bertanggung jawab kepada khalayak lintas disiplin. **(C5)** | CPL-6 |

Tidak satu pun kata dalam rumusan itu terikat pada kecerdasan buatan. Semester dengan topik Business Intelligence, Enterprise Architecture, atau apa pun dapat memakainya apa adanya — yang berganti cukup Sub-CPMK-nya.

Rumusannya sengaja menggemakan kosakata CPL-6: *menghasilkan rekomendasi SI/TI* → "menerapkan ... untuk menjawab kebutuhan organisasi"; *keselarasan bisnis dan SI/TI* → "pada domain keilmuan Sistem Informasi"; *mengevaluasi dampak dan penggunaannya* → "mengevaluasi kesesuaian, dampak, risiko, dan implikasi etis penerapannya". Penelusuran CPMK ke CPL dapat ditunjukkan kata per kata, bukan sekadar diklaim.

### Kenapa C5, bukan C6

Tiga alasan, dan yang ketiga yang paling mengikat.

**Beban 2 SKS.** Menempatkan puncak di C6 berarti menuntut tiap mahasiswa mencipta artefak utuh dalam 100 menit tatap muka per minggu. Untuk mata kuliah pengayaan tanpa prasyarat, itu terlalu tinggi sebagai janji kurikulum.

**Arah yang terbaca dari penyelenggaraan sebelumnya** adalah *menerapkan* teknologi terkini pada domain keilmuan Sistem Informasi — bukan menciptakan teknologi barunya. Kata kerja "menerapkan" (C3) menjadi lantai, dan "mengevaluasi" (C5) menjadi puncak.

**RPS ini dipakai bersama.** CPMK harus dapat dicapai oleh penyelenggaraan yang paling sedikit membangun, bukan hanya oleh semester yang kebetulan berbasis produk. Semester bertopik kerangka kerja atau paradigma mungkin sama sekali tidak menghasilkan artefak yang dibangun — tetapi tetap dapat menerapkan, menguji kesesuaian, dan mengevaluasi dampaknya. C5 menampung keduanya; C6 hanya menampung yang membangun.

Konsekuensinya untuk semester ini: Sub-CPMK-4 diberi label **C5**, bukan C6. Yang dinilai pada perakitan AI Agent memang keputusan rancangannya — batas kewenangan, guardrails, kapan manusia dilibatkan — bukan keberhasilan mengetik kodenya. Sub-CPMK tidak boleh melampaui level CPMK yang menaunginya, dan dengan C5 seluruh rantainya sah.

### Kenapa bukan hanya "teknologi"

Rumusan menyebut **teknologi, kerangka kerja, atau paradigma**. Kapita Selekta di Sistem Informasi tidak selalu berarti perkakas baru — ia bisa berarti cara berpikir baru: arsitektur enterprise, metodologi tata kelola, pendekatan berbasis data, atau paradigma perancangan. Mengunci rumusan pada kata "teknologi" akan memaksa semester bertopik kerangka kerja menuliskan RPS yang tidak jujur.

### Peta CPL–CPMK–Sub-CPMK

Keenam Sub-CPMK berikut adalah **instansiasi semester Ganjil 2026/2027 dengan topik AI Engineering**, dan boleh diganti seluruhnya bila pengampu atau topiknya berubah.

| Sub-CPMK | Rumusan Ringkas | Taksonomi | Minggu |
|:--:|---|:--:|:--:|
| Sub-CPMK-1 | Menjelaskan lanskap AI generatif dan karakteristik operasional model bahasa besar | C2 · A2 · P2 | 1–3 |
| Sub-CPMK-2 | Mengendalikan keluaran model melalui instruksi terstruktur, keluaran berformat, dan pemanggilan tool | C3 · A3 · P3 | 4–6 |
| Sub-CPMK-3 | Membumikan sistem pada sumber pengetahuan sahih melalui alur *Retrieval-Augmented Generation* | C4 · A3 · P3 | 7–9 |
| Sub-CPMK-4 | Merancang dan merakit AI Agent dengan penalaran bertahap, tool, memori, dan guardrails | C5 · A4 · P4 | 10–13 |
| Sub-CPMK-5 | Mengevaluasi kualitas, keandalan, dan efisiensi biaya sistem secara sistematis | C5 · A4 · P4 | 14–15 |
| Sub-CPMK-6 | Menilai risiko etis dan keamanan serta mengomunikasikan karya secara bertanggung jawab | C5 · A5 · P4 | 15–16 |

Seluruhnya bermuara pada CPMK-1, dan tidak satu pun melampaui level C5 yang menaunginya. Tingkat kognitifnya menanjak C2 → C3 → C4 → C5 → C5 → C5: memahami mesinnya, menerapkan kendali, menganalisis alur pengetahuan, lalu tiga tahap yang seluruhnya menuntut penilaian — atas rancangan agen, atas keandalan sistem, dan atas risiko serta etikanya.

Tiap Sub-CPMK memuat tepat satu taksonomi kognitif, satu afektif, dan satu psikomotorik — sesuai batas yang diberlakukan borang SIMKUR.

---

## 5. Bahan Kajian

Lima bahan kajian, dirumuskan pendek sebagai **kategori fungsional** dan bukan sebagai topik. Inilah yang membuat satu RPS dapat dipakai bersama: topik apa pun mengisi kelima kategori yang sama.

| # | Bahan Kajian | Study Material | Sub-CPMK | Diisi semester ini dengan |
|:--:|---|---|:--:|---|
| 1 | Lanskap Teknologi Mutakhir | Emerging Technology Landscape | 1 | Blok A — fondasi model bahasa besar |
| 2 | Analisis Kebutuhan dan Perancangan | Requirements Analysis and Design | 2, 3 | Lembar tema minggu 4; Blok B kendali dan Blok C grounding |
| 3 | Purwarupa dan Implementasi | Prototyping and Implementation | 4 | Blok D — perakitan AI Agent |
| 4 | Evaluasi dan Pengujian | Evaluation and Testing | 5 | Blok E — laporan evaluasi dan set uji |
| 5 | Risiko, Etika, dan Komunikasi | Risk, Ethics, and Communication | 6 | Kajian risiko minggu 15; presentasi UTS dan pertanggungjawaban UAS |

Kolom kanan berganti tiap semester; kolom tengah tidak.

Bahan kajian 3 dibaca luas: **purwarupa, implementasi, atau studi penerapan**. Semester bertopik kerangka kerja atau paradigma mungkin tidak menghasilkan artefak yang dibangun — dan tetap dapat mengisinya dengan studi penerapan pada organisasi nyata.

**Lima, bukan tujuh.** Kepadatannya 2,5 bahan kajian per SKS, di antara DMJK (1,5) dan Pemrograman Web (3,0). Versi tujuh sebelumnya mencapai 3,5 per SKS — lebih padat daripada mata kuliah 4 SKS, dan itu tidak sepadan untuk mata kuliah pengayaan 2 SKS.

### Instansiasi Semester Ini — AI Engineering

Rincian materi kelima blok yang mengisi ketujuh bahan kajian di atas.

### Blok A — Fondasi: Memahami Mesinnya
- Lanskap AI generatif dan posisi AI Engineering terhadap Data Science, Machine Learning, dan Rekayasa Perangkat Lunak
- Anatomi *foundation model*: apa yang sebenarnya dilakukan model bahasa besar
- Konsep operasional: token, jendela konteks, suhu (*temperature*), latensi, dan biaya
- Kriteria pemilihan model dan strategi abstraksi penyedia layanan (*model gateway*)
- Halusinasi sebagai sifat bawaan, bukan sekadar cacat

### Blok B — Kendali: Mengarahkan Keluaran
- Perancangan instruksi terstruktur: peran, konteks, batasan, contoh, dan format
- Teknik penalaran: *chain-of-thought*, *few-shot*, dekomposisi tugas
- Keluaran terstruktur (*structured output*) dan skema data sebagai kontrak antar-komponen
- *Function calling* / *tool calling*: menjembatani model dengan dunia luar
- Pola kegagalan umum dan strategi mitigasinya

### Blok C — Grounding: Menjawab dengan Sumber
- Mengapa model tidak tahu data organisasi Anda
- Representasi makna (*embedding*) dan basis data vektor secara konseptual
- Chunking dokumen (*chunking*) dan strategi retrieval
- Merancang alur *Retrieval-Augmented Generation* utuh
- Menelusuri dan mengatasi jawaban yang tidak berdasar

### Blok D — Agentic: Dari Tool Menjadi Pelaku
- Perbedaan mendasar *workflow* dan *agent*
- Siklus nalar–tindak (*reason–act*) dan orkestrasi langkah majemuk
- Tool, memori jangka pendek dan panjang, serta manajemen keadaan
- Pola multi-agen dan pembagian peran
- *Guardrails*, batas kewenangan, dan keterlibatan manusia (*human-in-the-loop*)
- **Studi kasus praktisi:** pembedahan arsitektur ASDOS-AI dan SkripsiPintar/GuruPintar

### Blok E — Kematangan: Dari Prototipe ke Produk
- Mengapa prototipe yang terasa mengesankan sering gagal di lapangan
- Evaluasi sistematis: penyusunan set uji, metrik kualitas, dan *LLM-as-a-judge*
- Observabilitas, tracing (penelusuran jejak eksekusi), dan penanganan error
- Pengendalian biaya: *caching*, pemilihan model berjenjang, dan efisiensi konteks
- Keamanan: *prompt injection*, kebocoran data, dan perlindungan informasi pribadi
- Etika, keberpihakan (*bias*), transparansi, dan tanggung jawab profesional

---

## 6. Peta Pertemuan

Enam belas minggu, mencakup dua pekan asesmen tengah dan akhir semester.

| Mgg | Pokok Bahasan | Sub-CPMK | Aktivitas & Luaran |
|:--:|---|:--:|---|
| 1 | Orientasi kelas, kontrak kuliah, dan lanskap AI generatif | 1 | Diskusi pemetaan harapan; penjajakan bidang minat lintas prodi |
| 2 | Anatomi model bahasa besar: token, konteks, biaya, dan halusinasi | 1 | Eksplorasi terpandu; laporan pengamatan perilaku model |
| 3 | Penyiapan lingkungan kerja dan model gateway (*model gateway*) | 1 | Setiap mahasiswa berhasil melakukan pemanggilan model pertamanya |
| 4 | Perancangan instruksi terstruktur dan teknik penalaran | 2 | **Penetapan tema proyek**; pustaka instruksi versi awal |
| 5 | Keluaran terstruktur dan skema data sebagai kontrak | 2 | Prototipe menghasilkan keluaran berformat yang konsisten |
| 6 | Pemanggilan tool: menghubungkan model dengan dunia luar | 2 | Prototipe memanggil sedikitnya satu tool eksternal |
| 7 | Grounding pengetahuan: konsep *embedding* dan retrieval | 3 | Rancangan alur RAG untuk kasus masing-masing |
| 8 | **Ujian Tengah Semester** | 1–3 | **Presentasi rancangan sistem** dan pembelaan keputusan desain |
| 9 | Merakit alur RAG utuh dan menangani jawaban tak berdasar | 3 | Produk mampu menjawab berbasis dokumen milik mahasiswa |
| 10 | Dari *workflow* ke *agent*: siklus nalar–tindak | 4 | Analisis komparatif dua pendekatan pada kasus sendiri |
| 11 | Tool, memori, dan orkestrasi langkah majemuk | 4 | Produk menyelesaikan tugas bertahap secara mandiri |
| 12 | Studi kasus praktisi: pembedahan arsitektur agen produksi | 4 | Kritik arsitektur; identifikasi keputusan desain dan alasannya |
| 13 | *Guardrails*, batas kewenangan, dan *human-in-the-loop* | 4, 6 | Penambahan mekanisme guardrails pada produk |
| 14 | Evaluasi sistematis dan pengendalian biaya | 5 | Set uji dan laporan hasil evaluasi produk |
| 15 | Keamanan, etika, bias, dan tanggung jawab profesional | 6 | Kajian risiko dan pernyataan etis atas produk sendiri |
| 16 | **Ujian Akhir Semester** | 4–6 | **Demonstrasi produk, portofolio, dan pertanggungjawaban** |

### Rincian Rencana Kegiatan per Rentang Minggu

Format ini disiapkan untuk pengisian sistem RPS program studi.

#### Minggu 1–3 · Sub-CPMK-1 · CPMK-1
*Fondasi: memahami mesinnya*

| Bentuk Kegiatan | Metode Pembelajaran | Jenis | Durasi | Instrumen Penilaian | Bobot | Kategori Nilai |
|---|---|---|:--:|---|:--:|---|
| Kegiatan Belajar Terbimbing | Kuliah interaktif | Tatap Muka | 300 menit | Kuis konsep di akhir Minggu 3 | 5% | Tes / Ujian (Quiz) |
| Kegiatan Penugasan Terstruktur | Eksplorasi terpandu | Tugas | 180 menit | Laporan pengamatan perilaku model | 5% | Tugas |
| Kegiatan Mandiri | Telaah pustaka | Mandiri | 180 menit | Partisipasi dan keaktifan diskusi | 3% | Sikap dan Profesionalisme |

#### Minggu 4–6 · Sub-CPMK-2 · CPMK-1
*Kendali: mengarahkan keluaran*

| Bentuk Kegiatan | Metode Pembelajaran | Jenis | Durasi | Instrumen Penilaian | Bobot | Kategori Nilai |
|---|---|---|:--:|---|:--:|---|
| Kegiatan Belajar Terbimbing | Kuliah interaktif dan demonstrasi | Tatap Muka | 300 menit | Kuis perancangan instruksi | 5% | Tes / Ujian (Quiz) |
| Kegiatan Penugasan Terstruktur | Pembelajaran Berbasis Proyek | Proyek | 360 menit | Prototipe berkeluaran terstruktur dan memanggil tool | 10% | Tugas |
| Kegiatan Mandiri | Belajar mandiri | Mandiri | 240 menit | Kemajuan berkala proyek | 3% | Sikap dan Profesionalisme |

#### Minggu 7–9 · Sub-CPMK-3 · CPMK-1
*Grounding: menjawab dengan sumber* · **Minggu 8: Ujian Tengah Semester**

| Bentuk Kegiatan | Metode Pembelajaran | Jenis | Durasi | Instrumen Penilaian | Bobot | Kategori Nilai |
|---|---|---|:--:|---|:--:|---|
| Kegiatan Belajar Terbimbing | Kuliah interaktif | Tatap Muka | 200 menit | Presentasi rancangan sistem | 10% | Tes / Ujian (UTS) |
| Kegiatan Penugasan Terstruktur | Pembelajaran Berbasis Proyek | Proyek | 360 menit | Produk menjawab berbasis dokumen rujukan | 10% | Tugas |
| Kegiatan Kolaboratif | Peer Review | Kolaboratif | 180 menit | Kualitas kritik terhadap rancangan rekan | 5% | Tugas |

#### Minggu 10–13 · Sub-CPMK-4 · CPMK-1
*Agentic: dari tool menjadi pelaku*

| Bentuk Kegiatan | Metode Pembelajaran | Jenis | Durasi | Instrumen Penilaian | Bobot | Kategori Nilai |
|---|---|---|:--:|---|:--:|---|
| Kegiatan Belajar Terbimbing | Kuliah dan studi kasus praktisi | Tatap Muka | 400 menit | — | — | — |
| Kegiatan Penugasan Terstruktur | Pembelajaran Berbasis Proyek | Proyek | 480 menit | Agen menyelesaikan tugas berlangkah majemuk | 15% | Proyek |
| Kegiatan Kolaboratif | Diskusi kritis arsitektur | Kolaboratif | 300 menit | Kritik arsitektur agen produksi | 4% | Sikap dan Profesionalisme |

#### Minggu 14–16 · Sub-CPMK-5 dan Sub-CPMK-6 · CPMK-2
*Kematangan: dari prototipe ke produk* · **Minggu 16: Ujian Akhir Semester**

| Bentuk Kegiatan | Metode Pembelajaran | Jenis | Durasi | Instrumen Penilaian | Bobot | Kategori Nilai |
|---|---|---|:--:|---|:--:|---|
| Kegiatan Belajar Terbimbing | Kuliah interaktif | Tatap Muka | 200 menit | — | — | — |
| Kegiatan Penugasan Terstruktur | Pembelajaran Berbasis Proyek | Proyek | 420 menit | Laporan evaluasi dan kajian risiko etis | 15% | Proyek |
| Kegiatan Mandiri | Penyusunan portofolio | Mandiri | 300 menit | Demonstrasi produk dan pertanggungjawaban | 10% | Tes / Ujian (UAS) |

---

## 7. Peta Kompetensi

Kompetensi dibangun berlapis. Setiap lapis menjadi prasyarat lapis berikutnya, dan seluruhnya bermuara pada satu produk yang dipertanggungjawabkan.

```
                    ┌───────────────────────────────────────────────┐
                    │   CPMK-2 · Mengevaluasi & Mempertanggung-     │
                    │   jawabkan  (Sub-CPMK-5, Sub-CPMK-6)          │
                    │   Evaluasi · Biaya · Keamanan · Etika         │
                    └───────────────────────┬───────────────────────┘
                                            │  ditopang oleh
                    ┌───────────────────────┴───────────────────────┐
                    │   Sub-CPMK-4 · Merakit AI Agent               │
                    │   Reason-Act · Tool · Memori · Guardrails     │
                    └───────────────────────┬───────────────────────┘
                                            │
                    ┌───────────────────────┴───────────────────────┐
                    │   Sub-CPMK-3 · Membumikan Pengetahuan (RAG)   │
                    │   Embedding · Retrieval · Sumber rujukan   │
                    └───────────────────────┬───────────────────────┘
                                            │
                    ┌───────────────────────┴───────────────────────┐
                    │   Sub-CPMK-2 · Mengendalikan Keluaran         │
                    │   Instruksi · Keluaran terstruktur · Tool │
                    └───────────────────────┬───────────────────────┘
                                            │
                    ┌───────────────────────┴───────────────────────┐
                    │   Sub-CPMK-1 · Memahami Model Bahasa Besar    │
                    │   Token · Konteks · Biaya · Halusinasi        │
                    └───────────────────────────────────────────────┘

                         CPMK-1 mencakup Sub-CPMK-1 s.d. Sub-CPMK-4
```

---

## 8. Indikator Ketercapaian Sub-CPMK

| Kode | Sub-CPMK | Indikator Ketercapaian |
|---|---|---|
| Sub-1 | Menjelaskan posisi AI Engineering dan karakteristik operasional model bahasa besar | Mampu membedakan AI Engineering dari ML; menjelaskan pengaruh token, konteks, dan suhu terhadap perilaku serta biaya |
| Sub-2 | Merancang instruksi dan kontrak keluaran yang andal | Menghasilkan keluaran terstruktur yang konsisten pada masukan beragam; mampu memanggil tool eksternal |
| Sub-3 | Merancang dan merakit alur retrieval berbasis dokumen | Sistem menjawab berdasarkan dokumen rujukan disertai sumber; mampu menjelaskan sebab jawaban tak berdasar |
| Sub-4 | Merakit AI Agent untuk persoalan nyata bidang keilmuannya | Agen menyelesaikan tugas berlangkah majemuk dengan tool dan guardrails yang memadai |
| Sub-5 | Mengevaluasi kualitas, keandalan, dan biaya sistem | Menyusun set uji, melaporkan metrik, dan merumuskan perbaikan berbasis bukti |
| Sub-6 | Menilai risiko etis dan mengomunikasikan karya | Menyajikan kajian risiko yang jujur serta presentasi yang runtut dan meyakinkan bagi khalayak lintas disiplin |

---

## 9. Bentuk dan Metode Pembelajaran

Mata kuliah ini menolak pola ceramah satu arah. Setiap pertemuan menggabungkan penjelasan konsep singkat dengan pengerjaan langsung pada produk mahasiswa sendiri.

- **Kuliah interaktif** — pemaparan konsep disertai demonstrasi langsung
- **Belajar berbasis proyek** — satu produk dikembangkan berlapis sejak Minggu 4 hingga akhir semester
- **Studi kasus praktisi** — pembedahan sistem AI Agent yang benar-benar beroperasi
- **Diskusi kritis dan peer review** — mahasiswa saling mengkritisi rancangan lintas disiplin
- ***AI-assisted development*** — asisten pemrograman berbasis AI digunakan secara terbuka dan terdokumentasi sebagai tool belajar

### Ketentuan Penggunaan AI dalam Pengerjaan Tugas

Penggunaan AI **diizinkan dan diharapkan**, dengan tiga syarat yang tidak dapat ditawar:

1. **Transparan.** Setiap penggunaan dicantumkan dalam catatan proses.
2. **Terpahami.** Mahasiswa wajib mampu menjelaskan setiap bagian karyanya. Ketidakmampuan menjelaskan dinilai sebagai tidak menguasai, terlepas dari kualitas luaran.
3. **Bertanggung jawab.** Kesalahan pada karya tetap menjadi tanggung jawab mahasiswa, bukan alat yang digunakan.

---

## 10. Penilaian

Karena luaran mata kuliah berupa karya, penilaian bertumpu pada produk dan pertanggungjawabannya. Kategori penilaian mengikuti ketentuan baku program studi.

| No | Kategori Nilai | Bentuk Penilaian | Bobot | Sub-CPMK |
|:--:|---|---|:--:|:--:|
| 1 | Tes / Ujian (Quiz) | Kuis pemahaman konsep di awal pertemuan | 10% | 1, 2 |
| 2 | Tugas | Luaran bertahap proyek dan peer review | 25% | 2, 3 |
| 3 | Sikap dan Profesionalisme | Partisipasi, diskusi kritis, dan kejujuran proses | 15% | 6 |
| 4 | Proyek | Produk akhir, laporan evaluasi, dan kajian risiko etis | 30% | 4, 5, 6 |
| 5 | Tes / Ujian (UTS) | Presentasi rancangan sistem | 10% | 1, 2, 3 |
| 6 | Tes / Ujian (UAS) | Demonstrasi produk dan pertanggungjawaban | 10% | 4, 5, 6 |
| | **Total** | | **100%** | |

### Beban Belajar

| Jenis Beban Belajar | Satuan | Nilai |
|---|:--:|:--:|
| Proyek | Jam | 40,00 |
| Tugas | Jam | 16,00 |
| Tatap Muka | Jam | 20,00 |
| SCL | Jam | 7,00 |
| Kolaboratif | Jam | 8,00 |

### Rubrik Penilaian Produk Akhir

| Aspek | Bobot | Sangat Baik (85–100) | Cukup (65–84) | Kurang (<65) |
|---|:--:|---|---|---|
| Ketepatan rumusan masalah | 20% | Persoalan nyata dan spesifik dari bidang keilmuan; AI terbukti solusi yang tepat | Persoalan jelas namun umum | Persoalan dipaksakan agar terlihat memakai AI |
| Kualitas rancangan sistem | 25% | Setiap keputusan arsitektur dapat dijelaskan alasannya | Rancangan berjalan namun sebagian keputusan tanpa alasan | Rancangan meniru contoh tanpa pemahaman |
| Keandalan dan evaluasi | 25% | Set uji memadai, metrik jelas, perbaikan berbasis bukti | Pengujian terbatas dan tidak sistematis | Tidak ada pengujian; hanya demonstrasi kasus ideal |
| Kesadaran risiko dan etika | 15% | Risiko diidentifikasi jujur beserta mitigasinya | Risiko disebut secara permukaan | Risiko diabaikan |
| Komunikasi dan pertanggungjawaban | 15% | Runtut, meyakinkan, mampu menjawab pertanyaan mendalam | Cukup jelas, sebagian pertanyaan tak terjawab | Tidak mampu menjelaskan karyanya sendiri |

---

## 11. Sarana Pembelajaran dan Ketentuan Biaya

| Kebutuhan | Ketentuan |
|---|---|
| Perangkat | Komputer jinjing atau akses laboratorium komputer |
| Akses model | Melalui satu model gateway terpadu (*model gateway*), sehingga mahasiswa cukup mengelola satu kredensial dan bebas membandingkan berbagai model |
| Tool pengembangan | Tool berbantuan AI yang tersedia gratis atau berlisensi pendidikan |
| Dokumen kerja | Repositori bersama untuk pengumpulan tugas dan portofolio |

> **Catatan mengenai biaya.**
> Penggunaan model berbayar dikelola dengan batas anggaran yang ditetapkan di awal semester. Tugas rutin diarahkan pada model bertarif rendah atau tanpa biaya, sementara model berkemampuan tinggi digunakan terbatas pada tahap evaluasi akhir. **Tidak ada mahasiswa yang dirugikan karena keterbatasan biaya**; alternatif tanpa biaya selalu disediakan. Pengelolaan anggaran ini sekaligus menjadi bahan ajar tersendiri pada Blok E.

---

## 12. Referensi

**Acuan Utama**
1. Huyen, C. *AI Engineering: Building Applications with Foundation Models*. O'Reilly Media.
2. Bouchard, L. & Peters, L. *Building LLMs for Production*. Towards AI.

**Acuan Pendukung**
3. Dokumentasi resmi penyedia *foundation model* dan model gateway terpadu.
4. Anthropic. *Building Effective Agents* — panduan pola arsitektur agen.
5. OWASP. *Top 10 for Large Language Model Applications* — rujukan keamanan.
6. Studi kasus internal: arsitektur **ASDOS-AI** dan **SkripsiPintar / GuruPintar**.

---

## 13. Catatan Penutup untuk Mahasiswa

Mata kuliah ini tidak menjanjikan Anda menjadi ahli kecerdasan buatan dalam satu semester. Yang dijanjikan lebih sederhana sekaligus lebih berguna: **Anda akan mampu melihat sebuah persoalan di bidang Anda sendiri, menilai apakah AI benar-benar merupakan jawabannya, lalu merancang dan membuktikan solusinya.**

Kelas ini terbuka bagi siapa pun tanpa syarat kemampuan pemrograman. Namun keterbukaan itu bukan berarti tuntutan diturunkan. Yang dituntut bergeser — dari kefasihan menulis kode menjadi **ketajaman berpikir, kejujuran dalam menguji, dan keberanian mempertanggungjawabkan karya sendiri.**

---

*Dokumen ini merupakan rancangan pembelajaran ringkas. Rincian teknis pelaksanaan disampaikan pada pertemuan pertama.*
