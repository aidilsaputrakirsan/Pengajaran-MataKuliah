# Lampiran D — Template Laporan Evaluasi dan Kajian Risiko

**Kapita Selekta: AI Engineering | Ganjil 2026/2027**
Dipakai pada Minggu 14 (`laporan-evaluasi.md`) dan Minggu 15 (`kajian-risiko.md`).

---

## Prinsip

Laporan evaluasi bukan laporan keberhasilan. Ia dokumen yang memungkinkan orang lain **menilai seberapa jauh klaim Anda dapat dipercaya**. Karena itu tiga hal wajib ada di setiap laporan: angkanya, cara mengukurnya, dan keterbatasannya.

Laporan yang menyajikan angka bagus tanpa menyebut keterbatasan bernilai lebih rendah daripada laporan berangka sedang yang jujur tentang apa yang belum terukur.

---
---

# BAGIAN I — Template Laporan Evaluasi (Minggu 14)

```markdown
# LAPORAN EVALUASI — <NAMA PRODUK>

Nama / NIM / K :
Tanggal        :
Versi produk yang dievaluasi :

---

## 1. Apa yang diukur dan apa yang tidak

Yang diukur laporan ini :

Yang TIDAK diukur laporan ini, dan mengapa :

Pengguna sasaran produk ini :


## 2. Set uji

Jumlah kasus : ___ (minimum saya: 12 + (K mod 6) = ___)

| Jenis | Jumlah | Porsi | Dari mana kasusnya berasal |
|---|---:|---:|---|
| Kasus khas |  |  |  |
| Kasus batas |  |  |  |
| Kasus yang harus ditolak |  |  |  |
| Kasus yang pernah gagal |  |  |  |

Siapa yang menyusun jawaban acuan, dan atas dasar apa :


## 3. Kriteria penilaian

| # | Pertanyaan biner | Bagaimana kasus batas diputuskan |
|---|---|---|
| 1 | Menjawab pertanyaan yang benar-benar diajukan |  |
| 2 | Seluruh pernyataan didukung sumber yang dirujuk |  |
| 3 | Tidak ada pernyataan tambahan yang tak berdasar |  |
| 4 | Format keluaran sah menurut skema |  |
| 5 | Menolak dengan benar bila memang seharusnya menolak |  |

Penilai : saya sendiri / model / keduanya
Bila memakai model — kalibrasi:
  Jumlah kasus yang saya nilai sendiri : ___ dari ___
  Jumlah yang penilaiannya SAMA dengan penilaian saya : ___
  Tingkat kesesuaian : ___%
  Kesimpulan saya tentang kelayakan penilai model :


## 4. Hasil

| Kriteria | Lulus | Dari | Persen |
|---|---:|---:|---:|
| 1. Menjawab yang diajukan |  |  |  |
| 2. Didukung sumber |  |  |  |
| 3. Tanpa tambahan tak berdasar |  |  |  |
| 4. Format sah |  |  |  |
| 5. Menolak dengan benar |  |  |  |

Per jenis kasus:

| Jenis | Lulus seluruh kriteria | Dari | Persen |
|---|---:|---:|---:|
| Khas |  |  |  |
| Batas |  |  |  |
| Harus ditolak |  |  |  |
| Pernah gagal |  |  |  |

Variance antar-jalan : set uji dijalankan ___ kali, ___ kasus berubah hasilnya.
Artinya bagi angka di atas :


## 5. Perbandingan terhadap garis dasar Minggu 9

| Ukuran | Minggu 9 | Sekarang | Selisih |
|---|---:|---:|---:|
| Pertanyaan berjawaban yang dijawab benar |  |  |  |
| Pertanyaan tak berjawaban yang ditolak benar |  |  |  |

Apa yang menyebabkan perubahan itu :


## 6. Kegagalan yang tersisa

| # | Kasus | Gagal di kriteria | Diagnosis | Mengapa belum diperbaiki |
|---|---|---|---|---|

Pola yang saya lihat pada kegagalan-kegagalan ini :


## 7. Biaya

| Ukuran | Angka | Cara menghitungnya |
|---|---:|---|
| Biaya per permintaan khas |  |  |
| Biaya per permintaan TERBURUK |  |  |
| Biaya 100 pengguna × 20 permintaan/bulan |  |  |

Bagian yang paling menyumbang biaya :

Penghematan yang saya terapkan :
| Cara | Penghematan | Angka evaluasi sebelum | Sesudah | Kualitas turun? |
|---|---:|---:|---:|---|


## 8. Perbaikan yang diusulkan

Diurutkan menurut dampak, dengan bukti yang mendasarinya.

| # | Perbaikan | Bukti yang mendasari | Perkiraan dampak | Biaya menerapkannya |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |


## 9. Keterbatasan laporan ini

Tiga hal yang membuat angka di atas TIDAK boleh digeneralisasi :
1.
2.
3.

Bila laporan ini dibaca calon pengguna, apa yang perlu ia ketahui
sebelum mempercayai angkanya :
```

---
---

# BAGIAN II — Template Kajian Risiko dan Pernyataan Etis (Minggu 15)

```markdown
# KAJIAN RISIKO — <NAMA PRODUK>

Nama / NIM / K :
Tanggal        :

---

## 1. Risiko keamanan

| Risiko | Wujudnya pada produk saya | Diuji? | Hasil uji | Mitigasi | Di lapis mana |
|---|---|---|---|---|---|
| Prompt injection lewat masukan pengguna |  |  |  |  |  |
| Prompt injection lewat dokumen rujukan |  |  |  |  |  |
| Kebocoran data ke penyedia model |  |  |  |  |  |
| Terungkapnya instruksi sistem |  |  |  |  |  |
| Penyalahgunaan di luar maksud |  |  |  |  |  |
| Kredensial terbuka di repositori |  |  |  |  |  |

Hasil pemeriksaan riwayat repositori atas kredensial :
Tindakan yang saya ambil bila ditemukan :


## 2. Kewenangan tool

| Tool | Boleh | Tidak boleh | Ditegakkan di mana | Butuh persetujuan manusia? |
|---|---|---|---|---|

Baris yang penegakannya HANYA di instruksi (dan karena itu rapuh) :


## 3. Bias

| Pintu | Wujud konkret pada produk saya | Bukti | Yang saya lakukan |
|---|---|---|---|
| Dari model |  |  |  |
| Dari dokumen rujukan saya |  |  |  |
| Dari rancangan saya sendiri |  |  |  |

Kasus yang tidak cocok ke satu pun kategori skema saya, dan ke mana ia
dipaksa masuk :

Siapa yang dirugikan bila itu terjadi berulang :


## 4. Trace data

| Pertanyaan | Jawaban |
|---|---|
| Data apa yang masuk |  |
| Data apa yang dikirim ke penyedia model |  |
| Data apa yang disimpan, di mana, berapa lama |  |
| Siapa pemiliknya, dan apakah ia tahu |  |
| Apa yang terjadi pada data bila produk berhenti dipakai |  |
| Data apa yang seharusnya TIDAK PERNAH masuk |  |

Mekanisme yang MENCEGAH baris terakhir masuk (bukan sekadar harapan) :


## 5. Empat pertanyaan etis

1. Siapa yang dirugikan bila sistem ini salah :

2. Apakah pengguna tahu ia berhadapan dengan sistem AI, dan bahwa
   jawabannya bisa keliru? Bagaimana ia diberi tahu :

3. Data siapa yang diproses, dan apakah pemiliknya tahu :

4. Untuk apa sistem ini tidak boleh dipakai :


## 6. Risiko yang saya TERIMA tanpa mitigasi

| # | Risiko | Mengapa saya menerimanya | Apa yang akan mengubah keputusan ini |
|---|---|---|---|

(Bagian ini wajib berisi sedikitnya satu baris. Setiap sistem punya
risiko yang diterima; menyembunyikannya lebih buruk daripada
menyatakannya.)


## 7. PERNYATAAN ETIS

Ditulis agar dapat diperiksa pihak ketiga. Hindari kalimat yang tidak
dapat dibuktikan benar-salahnya.

### Produk ini dimaksudkan untuk

### Produk ini TIDAK boleh dipakai untuk

### Keputusan yang tidak boleh diserahkan kepada produk ini

### Apa yang produk ini sampaikan kepada penggunanya tentang dirinya

### Kapan produk ini tidak boleh dipercaya
(Satu paragraf, bahasa yang dipahami orang di luar bidang teknis.
Dinilai pada kejujurannya, bukan pada kesan baiknya. Akan diuji
langsung pada UAS dengan kasus dari set uji Anda sendiri.)
```

---

## Kesalahan yang paling sering pada laporan evaluasi

| Kesalahan | Mengapa fatal | Yang seharusnya |
|---|---|---|
| Angka tanpa jumlah kasus | 93% dari 30 dan 93% dari 3 berbeda jauh | Selalu tulis pembilang dan penyebut |
| Penilai model tanpa kalibrasi | Tidak diketahui apakah penilaiannya berarti | Nilai sendiri ≥ sepertiga, laporkan kesesuaiannya |
| "Terbukti andal" | Klaim yang tidak dapat ditopang set uji seukuran ini | Nyatakan andal untuk apa, pada kondisi apa |
| Biaya tanpa kasus terburuk | Produk agentik dibunuh oleh kasus terburuk | Laporkan khas dan terburuk |
| Penghematan tanpa uji ulang | Penurunan mutu yang disamarkan | Jalankan set uji yang sama sebelum dan sesudah |
| Keterbatasan tidak disebut | Pembaca menggeneralisasi lebih jauh dari yang layak | Bagian 9 wajib diisi sungguh-sungguh |
