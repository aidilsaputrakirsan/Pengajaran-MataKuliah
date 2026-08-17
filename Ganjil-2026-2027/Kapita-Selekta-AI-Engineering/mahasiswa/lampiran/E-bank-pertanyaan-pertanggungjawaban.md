# Lampiran E — Bank Pertanyaan Pertanggungjawaban

**Kapita Selekta: AI Engineering | Ganjil 2026/2027**

Seluruh pertanyaan yang mungkin diajukan pada UTS dan UAS ada di sini. Dibagikan terbuka sejak Minggu 1.

---

## Mengapa tidak dirahasiakan

Tidak satu pun pertanyaan di bawah dapat dijawab dengan menghafal. Semuanya menunjuk ke **produk Anda sendiri** dan menanyakan keputusan yang **Anda** ambil. Membacanya lebih dulu tidak memberi keuntungan kepada orang yang tidak mengerjakan produknya sendiri; ia justru memberi tahu apa yang perlu Anda pahami sambil membangun.

Cara memakai lampiran ini: bacalah setiap akhir blok, dan bila ada pertanyaan yang belum dapat Anda jawab, itu daftar pekerjaan Anda — bukan bahan hafalan menjelang ujian.

---

## Cara jawaban dinilai

| Jawaban | Nilai |
|---|---|
| Menunjuk bukti pada produk sendiri, menyebut alternatif yang ditolak dan alasannya | Penuh |
| Benar tetapi tanpa alasan; "karena begitu contohnya" | Separuh |
| Berbeda dari yang diharapkan penanya, tetapi berlasan kuat dan konsisten dengan rancangan | **Penuh** |
| Tidak dapat menjelaskan bagian karyanya sendiri | Nol untuk aspek itu |
| "Saya tidak tahu, tetapi dugaan saya begini, dan cara memeriksanya begini" | Sebagian besar |

Baris terakhir penting. Mengakui batas pengetahuan lalu menunjukkan cara mencari tahu dinilai jauh lebih tinggi daripada mengarang jawaban yang terdengar meyakinkan — kelas ini menghabiskan enam belas minggu mempelajari bahaya persis itu.

---
---

# BAGIAN I — Bank Pertanyaan UTS (Minggu 8)

Menguji Sub-CPMK 1–3: fondasi, kendali keluaran, dan rancangan grounding.

## A. Persoalan dan kelayakan

1. Siapa persisnya yang mengalami persoalan yang Anda pilih? Bagaimana Anda tahu?
2. Bagaimana persoalan ini diselesaikan sekarang, dan mengapa cara itu tidak memadai?
3. Mengapa persoalan ini tidak cukup diselesaikan dengan basis data atau formulir?
4. Bagian mana dari persoalan Anda yang sebenarnya **tidak** butuh model bahasa?
5. Bila anggaran Anda nol rupiah, bagian mana dari produk ini yang tetap dapat berjalan?
6. Siapa yang dapat memeriksa keluaran produk Anda benar atau salah? Bila hanya Anda, apa akibatnya bagi evaluasi nanti?
7. Apa yang terjadi bila produk Anda salah, dan siapa yang menanggungnya?
8. Sebutkan satu tanda yang akan membuat Anda menyimpulkan tema ini keliru dipilih.

## B. Karakteristik model

9. Jelaskan apa yang dilakukan model bahasa besar tanpa memakai kata "berpikir" atau "memahami".
10. Mengapa halusinasi tidak dapat dihapus? Apa tiga cara mengelolanya, dan mana yang Anda pakai?
11. Berapa perkiraan token satu permintaan khas pada produk Anda? Tunjukkan cara Anda menghitungnya.
12. Suhu berapa yang Anda pakai, dan mengapa bukan yang lebih tinggi atau lebih rendah?
13. Apa perbedaan jendela konteks dan ingatan? Mana yang dimiliki produk Anda?
14. Model apa yang Anda pakai, dan model apa yang Anda tolak? Atas dasar apa?
15. Tunjukkan satu tugas pada produk Anda yang model termurah sudah memadai. Bagaimana Anda membuktikannya?

## C. Kendali keluaran

16. Tunjukkan instruksi sistem Anda. Petakan setiap bagiannya ke enam sumbu.
17. Bagian mana dari instruksi Anda yang bila dihapus akan paling merusak? Sudahkah Anda mencobanya?
18. Berapa versi instruksi yang sudah Anda buat? Apa yang berubah dari v1 ke versi sekarang, dan mengapa?
19. Tunjukkan skema keluaran Anda. Mengapa medan ini ada dan medan itu tidak?
20. Nilai apa yang tersedia untuk kasus "tidak dapat ditentukan"? Apa yang terjadi bila ia tidak ada?
21. Mengapa medan bukti berupa kutipan langsung, bukan ringkasan?
22. Apa yang terjadi bila keluaran tidak sah menurut skema Anda? Tunjukkan mekanismenya berjalan.
23. Tunjukkan satu masukan yang membuat produk Anda menghasilkan keluaran tak sah. Mengapa ia lolos?

## D. Tool

24. Sebutkan tool Anda dan kapan masing-masing **tidak** boleh dipakai.
25. Tool mana yang hanya membaca dan mana yang mengubah sesuatu? Bagaimana perbedaan itu tercermin pada rancangan?
26. Tunjukkan satu pertanyaan yang membuat sistem Anda memilih tool keliru. Apa sebabnya?
27. Apa yang terjadi bila tool Anda gagal? Tunjukkan.
28. Mengapa tool yang mengembalikan hasil kosong lebih berbahaya daripada yang mengembalikan error?

## E. Rancangan RAG

29. Berapa ukuran chunk Anda, dan mengapa bukan setengahnya atau dua kalinya?
30. Mengapa Anda memotong menurut struktur / menurut jumlah huruf? Apa yang Anda korbankan?
31. Berapa chunk yang Anda ambil per pertanyaan? Apa yang terjadi bila diambil satu, dan bila diambil sepuluh?
32. Apa yang dilakukan sistem Anda bila tidak ada chunk yang cukup mirip? Mengapa itu keputusan etis, bukan teknis?
33. Dari mana dokumen rujukan Anda? Apakah Anda berhak memakainya?
34. Tunjukkan satu pertanyaan yang jawabannya tersebar di dua dokumen. Apakah rancangan Anda menanganinya?
35. Metadata apa yang ikut disimpan bersama chunk, dan mengapa itu penting saat menjawab?

## F. Proses dan kejujuran

36. Tunjukkan satu masukan yang membuat sistem Anda gagal, dan jelaskan sebabnya sampai ke akar.
37. Sebutkan satu keputusan rancangan yang dapat dibuat berbeda, dan mengapa Anda memilih yang ini.
38. Bagian mana dari karya Anda yang dibuat dengan bantuan AI, dan bagaimana Anda memverifikasinya?
39. Sebutkan satu hal yang AI berikan kepada Anda dan ternyata keliru. Bagaimana Anda menemukannya?
40. Prediksi Anda pada tahap PATAHKAN minggu ke berapa yang paling meleset? Apa yang salah dari cara Anda berpikir waktu itu?

---
---

# BAGIAN II — Bank Pertanyaan UAS (Minggu 16)

Menguji Sub-CPMK 4–6: agentic, evaluasi, risiko, dan pertanggungjawaban. Pertanyaan UTS tetap dapat diajukan kembali.

## G. Keputusan agentic

41. Bagian mana dari produk Anda yang agentik dan bagian mana yang **sengaja tidak**? Mengapa?
42. Apa yang Anda tukar dengan menjadikannya agen? Sebutkan kerugiannya, bukan hanya keuntungannya.
43. Berapa batas langkah Anda, dan mengapa angka itu? Apa yang terjadi saat batas tercapai?
44. Berapa biaya terburuk satu permintaan pada produk Anda? Tunjukkan angkanya.
45. Bila tugas yang sama dijalankan tiga kali, apakah jalurnya sama? Apa akibatnya bagi pengujian Anda?
46. Di mana keadaan tugas disimpan? Mengapa bukan di dalam riwayat percakapan?
47. Tunjukkan trace satu tugas yang gagal. Pada langkah mana kegagalan bermula — dan bagaimana Anda tahu itu bukan langkah tempat ia terlihat?
48. Alasan yang dinyatakan agen Anda pada tiap langkah — seberapa jauh ia dapat dipercaya sebagai penyebab tindakannya?
49. Bila Anda memakai multi-agen: tunjukkan bahwa satu agen dengan tool yang baik sudah dicoba dan tidak memadai.
50. Apa yang terjadi bila tool mengembalikan hasil yang **salah tetapi masuk akal**? Apakah sistem Anda menangkapnya?

## H. Guardrails

51. Tunjukkan tabel kewenangan tool Anda. Baris mana yang penegakannya hanya bersandar pada instruksi?
52. Mengapa penegakan di lapis instruksi lebih lemah daripada di lapis kewenangan?
53. Tindakan apa pada produk Anda yang tidak akan pernah dijalankan tanpa persetujuan manusia? Mengapa itu yang dipilih?
54. Tunjukkan bentuk persetujuannya. Apakah pengguna punya cukup informasi untuk **menolak**?
55. Serangan apa yang berhasil menembus produk Anda? Bagaimana Anda menutupnya?
56. Mengapa serangan lewat dokumen rujukan lebih berhasil daripada lewat masukan pengguna?
57. Lubang keamanan apa yang Anda ketahui **masih ada**? Mengapa Anda menerimanya?
58. Apa yang akan mengubah keputusan Anda untuk menerima risiko itu?

## I. Evaluasi

59. Berapa kasus pada set uji Anda, dan bagaimana komposisinya? Mengapa proporsi itu?
60. Siapa yang menyusun jawaban acuan, dan atas dasar apa?
61. Tunjukkan satu kriteria penilaian Anda dan satu kasus batas yang membuat dua penilai bisa berbeda pendapat.
62. Bila Anda memakai penilai model: berapa tingkat kesesuaiannya dengan penilaian Anda sendiri? Apakah itu cukup?
63. Berapa variance antar-jalan pada set uji Anda? Apa artinya bagi angka yang Anda laporkan?
64. Sebutkan satu klaim pada laporan evaluasi Anda, dan tunjukkan buktinya sekarang juga.
65. Bagaimana angka Anda berubah dari garis dasar Minggu 9? Apa yang menyebabkannya?
66. Kasus jenis apa yang paling sering gagal? Apa polanya?
67. Penghematan apa yang Anda terapkan, dan bagaimana Anda membuktikan kualitas tidak turun?
68. Tiga hal apa yang membuat angka Anda tidak boleh digeneralisasi?
69. Bila saya memberi Anda sepuluh kasus baru dari bidang Anda sekarang juga, berapa yang Anda perkirakan lolos? Atas dasar apa perkiraan itu?

## J. Etika dan tanggung jawab

70. Siapa yang dirugikan bila produk Anda salah?
71. Apakah pengguna tahu ia berhadapan dengan sistem AI dan bahwa jawabannya bisa keliru? Tunjukkan di mana ia diberi tahu.
72. Data siapa yang diproses, dan apakah pemiliknya tahu?
73. Untuk apa produk Anda **tidak boleh** dipakai? Apa yang mencegah orang memakainya untuk itu?
74. Tunjukkan bias yang masuk lewat rancangan Anda sendiri — bukan lewat model, bukan lewat dokumen.
75. Kasus apa yang tidak cocok ke satu pun kategori skema Anda? Ke mana ia dipaksa masuk, dan siapa yang dirugikan?
76. Bila produk Anda dipakai orang sungguhan mulai besok, apa yang paling membuat Anda khawatir?
77. Bila sistem Anda memberi rekomendasi keliru yang merugikan seseorang, siapa yang bertanggung jawab?

## K. Pertanggungjawaban menyeluruh

78. Tunjukkan satu bagian sistem Anda dan jelaskan mengapa dirancang begitu, serta alternatif apa yang Anda tolak.
79. Bagian mana dari karya ini yang tidak dapat Anda jelaskan sepenuhnya?
80. Keputusan rancangan apa yang paling Anda sesali, dan apa yang akan Anda lakukan berbeda?
81. Kesalahan apa sepanjang semester yang tidak tertangkap siapa pun kecuali Anda sendiri?
82. Di titik mana AI menyesatkan Anda, dan bagaimana Anda menyadarinya?
83. Bila Anda punya satu semester lagi, apa yang Anda kerjakan pertama?
84. Sebutkan satu kritik dari peer review yang Anda tolak, dan bantahan Anda.
85. Andaikan seseorang di luar bidang Anda bertanya "dari mana Anda tahu ini bekerja?" — jawab dalam satu menit.

---

## Empat pertanyaan yang pasti diajukan

Dari daftar panjang di atas, empat ini diajukan kepada **setiap** peserta pada UAS:

- **Nomor 78** — jelaskan satu keputusan rancangan beserta alternatif yang ditolak
- **Nomor 64** — tunjukkan bukti satu klaim laporan Anda, sekarang juga
- **Nomor 57** — lubang keamanan yang Anda ketahui masih ada, dan mengapa diterima
- **Nomor 79** — bagian yang tidak dapat Anda jelaskan sepenuhnya

Untuk UTS, tiga ini diajukan kepada setiap peserta: **nomor 37, 36, dan 38**.

---

## Cara berlatih

Jalankan pola **F3 — Penguji pemahaman** pada Lampiran A terhadap bagian-bagian produk Anda, sepekan sebelum ujian. Ia menghasilkan pertanyaan yang bentuknya serupa dengan yang ada di sini.

Latihan yang lebih baik lagi: mintalah rekan yang me-review produk Anda mengajukan lima pertanyaan dari daftar ini, dan jawablah tanpa membuka catatan. Bagian yang tersendat adalah bagian yang belum Anda kuasai — dan Anda punya waktu memperbaikinya sebelum penanya yang sesungguhnya datang.
