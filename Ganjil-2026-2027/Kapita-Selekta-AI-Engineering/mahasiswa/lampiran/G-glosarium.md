# Lampiran G — Glosarium

**Kapita Selekta: AI Engineering | Ganjil 2026/2027**

---

## Mengapa modul ini memakai istilah Inggris

Modul memakai istilah yang lazim dipakai di dunia kerja dan dokumentasi teknis, bukan padanan Indonesianya. Alasannya satu dan bersifat praktis:

> **Anda akan mencari `chunking`, bukan "pemenggalan".**

Semua dokumentasi, tutorial, forum, dan pesan error yang akan Anda temui memakai istilah Inggris. Kelas ini tidak memiliki asisten, sehingga kemampuan mencari rujukan sendiri adalah keterampilan bertahan hidup, bukan pelengkap. Mengajarkan padanan Indonesia akan membuat Anda tidak dapat menemukan apa pun ketika macet.

Padanan Indonesia tetap dicantumkan di sini karena dua alasan: peserta kelas ini datang dari berbagai program studi, dan sebagian dokumen resmi program studi menuntut istilah baku.

---

## Istilah inti

| Istilah dipakai modul | Padanan Indonesia | Artinya |
|---|---|---|
| **prompt** | instruksi | Teks yang dikirim ke model untuk mengarahkan keluarannya |
| **system prompt** | instruksi sistem | Instruksi tetap yang mengatur peran dan batas perilaku model |
| **token** | — | Potongan teks sebagai satuan hitung model; menentukan biaya dan batas |
| **context window** | jendela konteks | Jumlah token maksimum yang dapat dilihat model dalam satu pemanggilan |
| **temperature** | suhu | Pengatur seberapa berani model memilih kemungkinan yang tidak paling atas |
| **hallucination** | halusinasi | Keluaran yang terdengar masuk akal tetapi tidak berdasar |
| **prototipe** | purwarupa | Versi awal produk yang belum teruji untuk pemakaian sungguhan |
| **structured output** | keluaran terstruktur | Keluaran yang wajib mengikuti satu format tetap |
| **schema** | skema | Kontrak yang menetapkan medan, tipe, dan nilai yang sah pada keluaran |
| **tool** | perkakas | Fungsi di luar model yang dapat diminta model untuk dijalankan |
| **tool calling** / **function calling** | pemanggilan perkakas | Mekanisme model meminta sistem menjalankan sebuah tool |
| **model gateway** | gerbang model | Lapisan perantara: satu kredensial, banyak model di belakangnya |
| **API key** | kunci akses | Kredensial yang terhubung ke tagihan. Diperlakukan seperti kata sandi |
| **error** | galat | Pesan kegagalan dari sistem |
| **grounding** | pembumian | Mengikat jawaban model pada sumber pengetahuan yang sahih |
| **retrieval** | temu-kembali | Mengambil bagian dokumen yang relevan dengan sebuah pertanyaan |
| **RAG** (*Retrieval-Augmented Generation*) | pembangkitan berbantuan temu-kembali | Alur: cari chunk relevan → masukkan ke konteks → model menjawab |
| **embedding** | representasi makna | Perubahan teks menjadi deretan angka; teks bermakna mirip berdekatan |
| **chunking** | pemenggalan dokumen | Memotong dokumen menjadi bagian-bagian sebelum diberi embedding |
| **chunk** | potongan / penggalan | Satu bagian hasil chunking |
| **vector database** | basis data vektor | Tempat menyimpan embedding dan mencarinya berdasarkan kemiripan |
| **agent** | agen | Sistem yang menentukan sendiri langkah berikutnya, bukan mengikuti urutan tetap |
| **agentic** | keagenan | Sifat sebuah sistem yang menentukan langkahnya sendiri |
| **workflow** | alur kerja | Rangkaian langkah yang urutannya sudah Anda tetapkan sejak awal |
| **reason–act** (**ReAct**) | siklus nalar–tindak | Nalar → tindak → amati → nilai, berulang sampai berhenti |
| **trace** | jejak eksekusi | Rekaman langkah demi langkah yang dilakukan sistem |
| **tracing** | penelusuran jejak | Praktik merekam trace agar sistem dapat didiagnosis |
| **guardrails** | pengaman | Mekanisme yang membatasi apa yang boleh dilakukan sistem |
| **human-in-the-loop** | keterlibatan manusia | Titik yang menuntut persetujuan manusia sebelum tindakan dijalankan |
| **prompt injection** | penyusupan instruksi | Serangan berupa teks yang berperan sebagai instruksi bagi model |
| **LLM-as-a-judge** | model sebagai penilai | Memakai model untuk menilai keluaran model lain pada set uji |
| **variance** | variasi / derau | Perbedaan hasil antar-jalan pada masukan yang sama |
| **reproducibility** | keterulangan | Sejauh mana hasil yang sama dapat diperoleh ulang |
| **caching** | penyimpanan sementara | Menyimpan hasil yang berulang agar tidak dihitung ulang |
| **peer review** | telaah sejawat | Menelaah dan mengkritik karya rekan secara terstruktur |
| **latency** | latensi | Waktu tanggap sistem |

---

## Istilah yang **tidak** diterjemahkan, dan mengapa

| Istilah | Alasan |
|---|---|
| **luaran** | Istilah baku RPS dan dokumen akademik program studi; dipertahankan agar modul selaras dengan format prodi |
| **kredensial** | Ini memang istilah teknis Indonesianya; tidak ada versi Inggris yang lebih dikenal di kelas |
| **daftar periksa** | Transparan, dan tidak menghalangi pencarian rujukan |
| **catatan proses** | Nama artefak khas mata kuliah ini, bukan istilah teknis umum |
| **AMATI · PATAHKAN · PERBAIKI · RAKIT** | Nama tahap khas mata kuliah ini |

---

## Kaidah penulisan pada modul

1. Istilah Inggris ditulis **tanpa dicetak miring** kalau sudah lazim di kelas (prompt, token, tool, chunk, agent).
2. Padanan Indonesia diberikan **sekali**, di titik istilah itu pertama kali diperkenalkan, dalam kurung. Contoh: *"chunking (pemenggalan dokumen)"*. Setelah itu istilah Inggrisnya dipakai konsisten.
3. Kalau Anda menulis laporan atau catatan proses, **pakai istilah yang sama dengan modul.** Konsistensi memudahkan Anda mencari rujukan sendiri, dan memudahkan pembaca laporan Anda.
4. Kalau program studi Anda menuntut istilah baku Indonesia pada dokumen resmi, pakailah kolom "Padanan Indonesia" di atas.

---

## Untuk peserta dari luar bidang teknologi informasi

Tidak ada satu pun istilah di halaman ini yang menuntut kemampuan pemrograman untuk dipahami. Semuanya adalah nama untuk gagasan yang dijelaskan di dalam modul.

Kalau sebuah istilah terasa asing saat Anda membaca modul, cara tercepat bukan menghafal tabel ini, tapi membaca bagian **Konsep** pada minggu tempat istilah itu diperkenalkan:

| Istilah | Diperkenalkan pada |
|---|---|
| token, context window, temperature, hallucination | Minggu 2 |
| model gateway, API key | Minggu 3 |
| prompt, system prompt | Minggu 4 |
| structured output, schema | Minggu 5 |
| tool, tool calling | Minggu 6 |
| grounding, retrieval, RAG, embedding, chunking, chunk | Minggu 7 |
| agent, agentic, workflow, reason–act | Minggu 10 |
| trace, memory | Minggu 11 |
| guardrails, human-in-the-loop, prompt injection | Minggu 13 |
| LLM-as-a-judge, variance, caching | Minggu 14 |
