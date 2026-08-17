# MODUL DMJK — PEKAN 11–13

**SI2514011 | Proyek: NusantaraNet | Cisco Packet Tracer 8.2+**

---

## Tentang tiga pekan ini

Tidak ada teknologi baru di pekan 11–13. Yang ada tiga hal: **latihan soal** untuk memantapkan materi sebelumnya, **audit terhadap jaringan Anda sendiri**, dan **lab diagnosis dengan jumlah fault terbanyak sepanjang semester**.

Sepuluh pekan pertama memberi Anda sepuluh topik yang masing-masing dikerjakan sekali. Dikerjakan sekali bukan berarti dikuasai, dan tiga pekan ini ada untuk menutup jarak itu sebelum proyek akhir.

Susunan tiap pekan berbeda dari pekan 1–10:

| Tahap | Pekan 1–10 | Pekan 11–13 |
|---|---|---|
| READ | Membaca konfigurasi yang diberikan | **Audit** jaringan sendiri terhadap checklist |
| BREAK | Merusak dari tabel percobaan | **Latihan soal** desain dan prediksi, dikerjakan di kertas |
| FIX | Fault lab | Fault lab, jumlah fault terbanyak |
| BUILD | Menambah lapisan baru | **Memperbaiki** temuan audit sendiri |

Semua soal memakai parameter turunan X, sehingga jawaban setiap mahasiswa berbeda dan asisten memverifikasinya dengan menghitung ulang.

**Tingkat ketiga pekan ini: Challenge.** Tidak ada perintah yang diberikan.

---
---

# PEKAN 11 — Konsolidasi: Addressing dan Switching

**Sub-CPMK 4:** Mahasiswa mampu mendiagnosis gangguan jaringan secara sistematis berbasis lapisan OSI dan mendokumentasikan proses serta hasil perbaikannya. **(C4)**

**Cakupan materi:** pekan 1, 2, 3.

**Target akhir pekan:** Anda dapat menghitung VLSM tanpa alat bantu dalam waktu terbatas, dan menemukan tujuh fault pada lapisan 1–3 di jaringan yang belum pernah Anda lihat.

---

## 11.1 Yang Paling Sering Salah

Bagian ini bukan ringkasan materi. Isinya kesalahan yang benar-benar muncul pada pekerjaan pekan 1–3, disusun agar Anda memeriksanya lebih dulu saat mendiagnosis.

### Addressing

| Kesalahan | Gejala yang muncul | Cara tercepat memastikan |
|---|---|---|
| Mask klien berbeda dari mask gateway | Sebagian tujuan terjangkau, sebagian tidak | `ipconfig` di klien, bandingkan dengan `show ip interface brief` |
| Alamat jaringan atau broadcast dipakai sebagai host | Klien tidak dapat berkomunikasi sama sekali | Hitung ulang batas subnet |
| Dua segmen dengan blok bertumpang | Lalu lintas ke salah satunya hilang tanpa pola jelas | Bandingkan seluruh tabel addressing |
| Gateway di klien bukan alamat sub-interface | Dalam segmen berhasil, keluar segmen gagal | `ipconfig` lalu `ping` gateway |

Baris pertama adalah yang paling mahal waktunya, karena gejalanya sebagian berhasil. Kalau sebuah keluhan berbunyi "kadang bisa kadang tidak", periksa mask sebelum apa pun.

### Switching dan VLAN

| Kesalahan | Gejala | Perintah pembukti |
|---|---|---|
| Port tertinggal di VLAN 1 | Perangkat seperti tidak tersambung, lampu hijau | `show vlan brief` |
| VLAN tidak diizinkan di trunk | Satu VLAN gagal antar-switch, VLAN lain normal | `show interfaces trunk` |
| `encapsulation dot1Q` beda ID dari VLAN switch | Segmen berkomunikasi internal, gateway tidak terjangkau | `show vlan brief` dibanding `show running-config` |
| Port trunk dikonfigurasi sebagai access | Semua VLAN kecuali satu mati | `show interfaces trunk` kosong |
| Interface fisik induk masih `shutdown` | Semua sub-interface mati sekaligus | `show ip interface brief` |

Pola yang perlu dihafal: **kalau satu VLAN bermasalah dan lainnya normal**, periksa daftar VLAN pada trunk dan penugasan port. **Kalau semua VLAN mati sekaligus**, periksa interface fisik dan trunk.

---

## 11.2 Prompt Pack — Pekan 11

### A. Prompt Latihan Mandiri

```
Buat 5 soal latihan VLSM setingkat ujian untuk saya kerjakan, dengan
blok 10.77.0.0/21 dan kebutuhan host yang bervariasi antara 5 dan 200.

Aturan:
- Berikan SOALNYA SAJA, jangan jawabannya.
- Setelah saya kirim jawaban, periksa dan tunjukkan baris mana yang salah
  TANPA memberi jawaban yang benar, supaya saya perbaiki sendiri.
- Kalau saya salah dua kali pada soal yang sama, baru jelaskan konsep
  yang saya lewatkan, tetap tanpa memberi angka jawabannya.
```

Pola ini mengubah AI dari pemberi jawaban menjadi pemeriksa. Untuk pekan konsolidasi, itu satu-satunya pemakaian yang berguna.

### B. Prompt Verifikasi Dukungan Packet Tracer

```
Saya mendiagnosis masalah VLAN dan addressing di Packet Tracer 8.2.
Urutkan perintah show berikut dari yang paling informatif ke yang paling
sempit untuk kasus "satu VLAN tidak bisa antar-switch, VLAN lain normal":

show vlan brief, show interfaces trunk, show mac address-table,
show interfaces status, show ip interface brief, show running-config

Untuk masing-masing, sebutkan satu hal yang HANYA bisa dibuktikan oleh
perintah itu dan tidak oleh yang lain.
```

### C. Prompt Audit Silang

```
Ini tabel addressing jaringan saya: [tempel tabel as-built Anda].

Berperan sebagai auditor. Jangan memperbaiki apa pun.
1. Adakah blok yang tumpang tindih? Sebutkan barisnya saja.
2. Adakah alamat gateway yang tidak konsisten dengan blok segmennya?
3. Adakah VLAN ID yang tidak mengikuti pola basis yang saya sebutkan?
4. Sebutkan 3 hal yang tidak dapat kamu periksa dari tabel ini saja,
   dan sebutkan apa yang harus saya lampirkan agar bisa diperiksa.
```

### D. Prompt Terlarang

Jangan meminta AI mengerjakan soal latihan pekan ini untuk Anda. Soal ini adalah simulasi UTS dan UAS, dan satu-satunya nilainya adalah mengetahui bagian mana yang belum Anda kuasai selagi masih ada waktu memperbaikinya.

---

## 11.3 AUDIT → SOAL → FIX → PERBAIKAN

### AUDIT — Periksa jaringan Anda sendiri (35 menit)

Tanpa AI, tanpa membuka modul pekan 1–3. Jalankan setiap butir terhadap file NusantaraNet Anda dan catat hasilnya sebagai **Lulus** atau **Temuan**.

| # | Butir periksa | Cara memeriksa |
|---|---|---|
| 1 | Tidak ada port aktif di VLAN 1 | `show vlan brief` di setiap switch |
| 2 | Setiap trunk hanya mengizinkan VLAN yang memang dipakai | `show interfaces trunk` |
| 3 | Setiap `encapsulation dot1Q` cocok dengan VLAN di switch | Bandingkan dua output |
| 4 | Semua port yang tidak dipakai dalam keadaan `shutdown` | `show interfaces status` |
| 5 | Setiap gateway sama dengan yang tercantum di tabel addressing | `show ip interface brief` |
| 6 | Tidak ada blok yang tumpang tindih di ketiga lokasi | Hitung ulang batas setiap subnet |
| 7 | Setiap VLAN ID mengikuti basis X Anda | Periksa terhadap Lampiran B |
| 8 | Prefix setiap segmen sesuai kebutuhan host, tidak berlebihan | Hitung pemborosan alamat per segmen |
| 9 | Setiap switch punya SVI manajemen dan `ip default-gateway` | `show running-config` |
| 10 | IPv6 aktif di dua segmen dan dapat saling ping | `show ipv6 interface brief` |

Audit yang menghasilkan nol temuan untuk sepuluh butir akan diminta dibuktikan butir per butir. Untuk kebanyakan mahasiswa, butir 2, 4, dan 8 akan menghasilkan temuan.

### SOAL — Latihan setingkat ujian (40 menit)

Dikerjakan **di kertas, tanpa Packet Tracer dan tanpa kalkulator subnet**. Ganti setiap `X` dengan angka Anda.

**Soal 1 — VLSM.** Anda diberi blok `10.(X+50).0.0/21`. Rancang alokasi untuk lima segmen dengan kebutuhan: 300 host, 120 host, 60 host, 25 host, dan 2 host. Sebutkan untuk masing-masing: prefix, alamat jaringan, host pertama, host terakhir, dan broadcast. Berapa alamat yang tersisa?

**Soal 2 — Analisis alamat.** Untuk alamat `10.X.1.(X+35)` dengan mask `255.255.255.192`: sebutkan alamat jaringannya, alamat broadcastnya, dan apakah alamat tersebut sah dipakai sebagai host. Tunjukkan langkah perhitungannya.

**Soal 3 — Diagnosis dari gejala.** Sebuah PC dapat melakukan ping ke seluruh perangkat di segmennya, dapat ping ke gatewaynya, tetapi tidak dapat ping ke segmen mana pun yang lain. PC lain di segmen yang sama berfungsi normal ke semua tujuan. Sebutkan tiga kemungkinan penyebab beserta satu uji pembeda untuk masing-masing, urut dari yang paling mungkin.

**Soal 4 — Diagnosis VLAN.** Dua PC pada VLAN yang sama, di dua switch berbeda yang terhubung trunk, tidak dapat saling ping. Kedua PC dapat ping ke gateway masing-masing. Apa satu penyebab yang menjelaskan **seluruh** fakta ini, dan perintah apa yang membuktikannya?

**Soal 5 — Perancangan.** Segmen dengan kebutuhan `40 + X` host diberi prefix `/24`. Sebutkan dua kerugian konkret dari keputusan itu, dan satu keadaan di mana keputusan itu justru dapat dibenarkan.

**Soal 6 — Prediksi.** Pada router-on-a-stick dengan empat sub-interface, `encapsulation dot1Q` pada satu sub-interface diubah ke ID VLAN yang tidak ada di switch. Sebutkan apa yang terjadi pada: klien di segmen tersebut, klien di segmen lain, output `show ip route`, dan output `show ip interface brief`.

**Soal 7 — Tag 802.1Q.** Sebuah frame berjalan dari PC di VLAN A ke PC di VLAN B melalui satu switch dan satu router-on-a-stick. Sebutkan di titik mana tag ditambahkan, di titik mana ia dibuang, dan berapa kali frame itu ditulis ulang MAC address-nya.

Kumpulkan lembar jawaban. Soal 3, 4, dan 6 memiliki bobot dua kali soal lainnya — ketiganya menguji diagnosis, yang merupakan Sub-CPMK pekan ini.

### FIX — Fault lab (40 menit)

Download `dmjk-broken-p11.pkt`. Jaringan dua lokasi lengkap dengan **7 fault**, seluruhnya pada lapisan 1 sampai 3: penugasan VLAN, daftar VLAN trunk, enkapsulasi sub-interface, mask yang tidak konsisten, alamat gateway, interface yang belum aktif, dan satu blok yang bertumpang dengan blok lain.

Kerjakan dengan urutan yang benar: **pengujian cakupan lebih dulu** (tujuh baris dari modul pekan 7), baru hipotesis, baru perbaikan. Kumpulkan Fault Report memakai template Lampiran D.

Fault yang bertumpang blok adalah yang paling sulit, karena gejalanya tidak konsisten dan berubah tergantung urutan perangkat berkomunikasi. Kalau Anda menemukan gejala yang "berubah-ubah", periksa tabel addressing secara menyeluruh, bukan satu perangkat.

### PERBAIKAN — Tutup temuan audit Anda

Perbaiki setiap temuan dari tahap AUDIT pada file NusantaraNet Anda. Untuk setiap temuan, catat di laporan: butir nomor berapa, apa temuannya, apa yang Anda ubah, dan bagaimana Anda memverifikasi perbaikannya.

Untuk temuan yang Anda putuskan **tidak** diperbaiki — misalnya prefix yang berlebihan tetapi mengubahnya akan memengaruhi seluruh rencana — tulis alasannya. Keputusan menerima sebuah kekurangan dengan alasan yang jelas adalah keputusan teknis yang sah; yang tidak sah adalah tidak menyadarinya.

---

## 11.4 Checkpoint Pekan 11 (2%)

**Checkpoint 1.** Lembar audit sepuluh butir terisi, dengan temuan yang nyata. Asisten memeriksa dua butir secara acak langsung di perangkat.

**Checkpoint 2.** Lembar jawaban soal dikumpulkan. Soal 1 dan 2 diperiksa terhadap kunci parameter X Anda.

**Checkpoint 3.** Minimal 5 dari 7 fault dilaporkan dengan kolom Gejala dan Hipotesis terisi.

**Viva.** Contoh pertanyaan:

- "Tunjukkan satu temuan audit Anda dan jelaskan bagaimana Anda menemukannya."
- "Fault mana yang paling lama? Anda mencari di mana lebih dulu, dan kenapa itu keliru?"
- "Gejalanya berubah-ubah. Apa yang langsung Anda curigai?"
- "Butir 8 audit Anda lulus. Berapa alamat yang terbuang di segmen terbesar Anda?"

---
---

# PEKAN 12 — Konsolidasi: Routing, Layanan, dan Kontrol Akses

**Sub-CPMK 4:** Mahasiswa mampu mendiagnosis gangguan jaringan secara sistematis berbasis lapisan OSI dan mendokumentasikan proses serta hasil perbaikannya. **(C4)**

**Cakupan materi:** pekan 4, 5, 6.

**Target akhir pekan:** Anda dapat memisahkan masalah routing, NAT, layanan, dan ACL hanya dari pola gejala, sebelum membuka konfigurasi apa pun.

---

## 12.1 Yang Paling Sering Salah

### Routing

| Kesalahan | Gejala | Perintah pembukti |
|---|---|---|
| Rute hanya dikonfigurasi satu arah | Ping gagal total, tabel rute sisi pengirim tampak benar | `traceroute` dari kedua arah |
| Next-hop tidak terjangkau | Rute ada di `running-config`, tidak ada di `show ip route` | Bandingkan kedua output |
| Mask rute lebih sempit dari jaringan sebenarnya | Sebagian segmen tujuan terjangkau, sebagian tidak | Hitung cakupan rute |
| Rute default hilang di cabang | Segmen lokal normal, semua tujuan luar gagal | `show ip route` mencari `S*` |

### Layanan

| Kesalahan | Gejala | Perintah pembukti |
|---|---|---|
| `ip helper-address` hilang | Segmen jauh tidak dapat alamat, segmen lokal server normal | `show running-config interface` |
| Opsi `dns-server` tidak ada di pool | Alamat didapat, ping IP berhasil, nama gagal | `ipconfig /all` di klien |
| Gateway tidak dikecualikan dari pool | Gangguan berpindah-pindah, sebagian klien putus | `show ip dhcp binding` |
| Interface NAT tidak ditandai | Ping ke interface ISP berhasil, ke internet gagal | `show ip nat translations` kosong |
| ACL NAT tidak mencakup semua segmen | Satu lokasi tidak bisa internet, lokasi lain bisa | `show ip nat statistics` |

### Kontrol akses

| Kesalahan | Gejala | Perintah pembukti |
|---|---|---|
| Urutan baris terbalik | Aturan spesifik tidak pernah berlaku | `show ip access-lists`, lihat penghitung |
| Arah `in`/`out` tertukar | ACL tidak berpengaruh sama sekali | Penghitung tetap nol |
| Lupa mengizinkan DHCP atau DNS | Seluruh segmen mati setelah ACL dipasang | Uji per layanan |
| Wildcard mask salah | Cakupan lebih luas atau lebih sempit dari maksud | Hitung ulang cakupan |
| Kebijakan bocor tanpa gejala | Tidak ada keluhan; akses terlarang tetap berhasil | Uji aktif yang seharusnya ditolak |

### Pohon keputusan dari gejala

Hafalkan urutan ini. Ia menggantikan kebiasaan langsung membuka `running-config`.

```
Ping ke gateway sendiri gagal?
  ya  -> lapisan 1-2: kabel, VLAN port, interface, mask klien
  tidak
    Ping ke segmen lain gagal, semua tujuan?
      ya  -> rute, atau ACL pada arah masuk
      tidak
        Ping IP berhasil, nama gagal?
          ya  -> DNS: layanan, record, atau opsi DHCP
          tidak
            Internal berhasil, internet gagal?
              ya  -> NAT: penandaan interface, ACL NAT, rute default
              tidak
                Sebagian tujuan atau sebagian layanan saja gagal?
                  ya  -> ACL: urutan, arah, wildcard
```

---

## 12.2 Prompt Pack — Pekan 12

### A. Prompt Latihan Diagnosis

```
Buat 6 skenario gangguan jaringan untuk saya latih, masing-masing dalam
bentuk DAFTAR HASIL PENGUJIAN saja — tanpa menyebutkan penyebabnya.

Setiap skenario harus memuat hasil dari: ping ke gateway, ping antar
segmen, ping dengan IP, ping dengan nama, ping ke internet, dan hasil
dari klien kedua di segmen yang sama.

Aturan:
- Jangan beri jawaban.
- Setelah saya menebak penyebabnya, katakan hanya BENAR atau BELUM TEPAT,
  dan kalau belum tepat, sebutkan satu hasil pengujian yang saya
  abaikan — bukan jawabannya.
- Sertakan satu skenario yang penyebabnya di sisi klien, bukan jaringan.
```

### B. Prompt Verifikasi Dukungan Packet Tracer

```
Untuk mendiagnosis ACL dan NAT di Packet Tracer 8.2, tandai
DIDUKUNG / TIDAK ADA / DISEDERHANAKAN:

show ip access-lists dengan penghitung kecocokan,
clear ip access-list counters, show ip nat translations,
clear ip nat translation *, show ip nat statistics,
debug ip nat, debug ip packet, show ip dhcp conflict.

Untuk yang tidak ada, sebutkan cara lain memperoleh informasi yang sama
di Packet Tracer.
```

### C. Prompt Uji Kebocoran

```
Ini matriks kontrol akses jaringan saya: [tempel matriks Anda].

Berperan sebagai penguji penetrasi internal. Sebutkan 8 uji yang paling
mungkin membuktikan kebijakan ini bocor, masing-masing dalam bentuk:
dari segmen mana, ke alamat mana, protokol dan port apa, dan hasil apa
yang membuktikan kebocoran.

Prioritaskan yang menyerang asumsi yang biasanya tidak diuji perancang,
misalnya arah balik, protokol selain ICMP, dan segmen yang baru
ditambahkan.
```

### D. Prompt Terlarang

Jangan menempelkan seluruh `show running-config` lalu meminta AI mencari kesalahannya. Yang dinilai pekan ini adalah kemampuan menyempitkan kemungkinan dari pola gejala, dan viva menanyakan urutan pengujian Anda — bukan daftar perbaikan.

---

## 12.3 AUDIT → SOAL → FIX → PERBAIKAN

### AUDIT — Periksa jaringan Anda sendiri (35 menit)

| # | Butir periksa | Cara memeriksa |
|---|---|---|
| 1 | Setiap tujuan terjangkau dari setiap lokasi, dua arah | Matriks ping antar ketiga lokasi |
| 2 | Rute di HQ berbentuk ringkas, bukan satu baris per segmen | `show ip route` |
| 3 | Setiap rute di `running-config` muncul di `show ip route` | Bandingkan kedua output |
| 4 | Cabang dan Gudang memakai rute default, bukan rute rinci | `show ip route` |
| 5 | Semua segmen pengguna mendapat alamat, gateway, dan DNS | `ipconfig /all` di satu klien per segmen |
| 6 | Gateway dan alamat statis dikecualikan di setiap pool | `show ip dhcp pool` |
| 7 | Semua interface internal ditandai `ip nat inside` | `show ip nat statistics` |
| 8 | ACL NAT mencakup ketiga lokasi | Uji internet dari ketiga lokasi |
| 9 | Setiap baris ACL yang seharusnya sering cocok punya penghitung tidak nol | `show ip access-lists` |
| 10 | Tamu tidak dapat menjangkau segmen internal mana pun | Uji ke seluruh segmen, bukan sebagian |
| 11 | Telnet mati di seluruh perangkat, SSH hanya dari Manajemen | Uji dari dua segmen berbeda |
| 12 | Tidak ada baris ACL yang tidak pernah cocok tanpa alasan | `show ip access-lists` |

Butir 9 dan 12 adalah yang paling sering menghasilkan temuan, dan keduanya memakai output yang sama. Penghitung nol tidak selalu berarti salah — baris `deny` yang jarang dilanggar memang wajar nol — tetapi Anda harus dapat menjelaskan setiap nol yang Anda temukan.

### SOAL — Latihan setingkat ujian (40 menit)

Di kertas, tanpa simulator. Ganti `X` dengan angka Anda.

**Soal 1 — Perancangan rute.** Empat lokasi: pusat `10.X.0.0/20`, dan tiga cabang `10.X.16.0/22`, `10.X.20.0/22`, `10.X.24.0/22`, semuanya terhubung hub-and-spoke ke pusat. Tulis daftar rute yang dibutuhkan di setiap router, dalam bentuk tujuan dan next-hop. Tandai mana yang dapat diringkas dan mana yang cukup rute default. Berapa total baris rute di seluruh jaringan?

**Soal 2 — Diagnosis satu arah.** PC di Cabang tidak dapat ping server di pusat. Dari router Cabang, ping ke interface WAN router pusat berhasil. Dari router pusat, ping ke server berhasil. Dari router pusat, ping ke PC di Cabang gagal. Sebutkan penyebab tunggal yang menjelaskan seluruh fakta ini, dan sebutkan perintah yang membuktikannya di router mana.

**Soal 3 — Diagnosis layanan.** Klien di satu segmen mendapat IP address yang benar, dapat ping gateway, dapat ping IP address server web, tetapi tidak dapat membuka halaman web lewat nama. Klien di segmen lain berfungsi normal untuk semuanya. Sebutkan tiga kemungkinan penyebab, urut dari yang paling mungkin, beserta uji pembedanya.

**Soal 4 — ACL.** Tulis kebijakan berikut sebagai daftar baris ACL berurutan, dalam bahasa Indonesia (bukan sintaks Cisco), untuk segmen tamu `10.X.0.128/26`: boleh DHCP, boleh DNS ke server `10.X.1.70`, boleh seluruh internet, tidak boleh apa pun yang lain di dalam. Sebutkan urutan barisnya dan jelaskan mengapa urutan itu tidak boleh ditukar. Sebutkan juga baris tersembunyi yang berlaku dan akibatnya.

**Soal 5 — NAT.** Pada PAT dengan satu alamat publik, tiga klien internal mengakses server web yang sama pada waktu yang sama. Sebutkan apa yang sama dan apa yang berbeda di antara ketiga baris tabel translasi, dan jelaskan bagaimana router mengembalikan setiap balasan ke klien yang benar. Apa yang terjadi kalau tabel translasi dikosongkan saat ketiga sesi sedang berjalan?

**Soal 6 — Prediksi berlapis.** Sebuah ACL dipasang pada interface masuk router untuk memblokir satu host tertentu, dan segmen itu memakai DHCP relay ke server di lokasi lain. Sebutkan apa yang terjadi pada: host yang dimaksud, host lain di segmen yang sama, klien baru yang menyala setelah ACL dipasang, dan penghitung `show ip access-lists`.

**Soal 7 — Urutan operasi.** Untuk paket dari klien internal ke internet, sebutkan urutan pemrosesan di router tepi antara: ACL masuk, keputusan routing, NAT, dan ACL keluar. Lalu jawab: ACL keluar pada interface ISP melihat alamat sumber privat atau publik? Apa akibatnya bagi cara Anda menulis aturannya?

Soal 2, 3, dan 6 berbobot dua kali.

### FIX — Fault lab (40 menit)

Download `dmjk-broken-p12.pkt`. Jaringan tiga lokasi lengkap dengan **7 fault** pada lapisan 3 dan di atasnya: dua pada routing, dua pada layanan, dua pada ACL, dan satu pada NAT.

Dua ketentuan:

Salah satu fault menyebabkan kegagalan **hanya satu arah**. Menemukannya mustahil kalau Anda hanya menguji dari satu sisi.

Salah satu fault **tidak menimbulkan keluhan pengguna** — ia kebocoran kebijakan. Uji apa yang seharusnya ditolak.

Kumpulkan Fault Report. Untuk setiap fault, kolom Cakupan wajib terisi: siapa terdampak dan siapa **tidak**. Fault Report tanpa kolom Cakupan dinilai setengah, karena tanpa itu tidak ada bukti bahwa Anda menyempitkan kemungkinan alih-alih menebak.

### PERBAIKAN — Tutup temuan audit Anda

Perbaiki setiap temuan tahap AUDIT. Untuk setiap perbaikan pada ACL, sertakan output `show ip access-lists` sebelum dan sesudah, dengan penghitung sebagai bukti bahwa baris yang Anda perbaiki sekarang benar-benar dilewati lalu lintas.

---

## 12.4 Checkpoint Pekan 12 (2%)

**Checkpoint 1.** Lembar audit dua belas butir terisi. Asisten memilih butir 9 atau 10 dan memverifikasinya langsung.

**Checkpoint 2.** Lembar jawaban soal dikumpulkan; soal 2 dan 4 dibahas lisan.

**Checkpoint 3.** Minimal 5 dari 7 fault dilaporkan dengan kolom Cakupan terisi, termasuk fault satu arah **atau** fault kebocoran.

**Viva.** Contoh pertanyaan:

- "Sebutkan pola gejala yang membedakan masalah NAT dari masalah rute default."
- "Penghitung baris ini nol. Wajar atau temuan?"
- "Fault yang satu arah itu, apa yang membuat Anda menguji dari sisi sebaliknya?"
- "Kebocoran kebijakan tidak menimbulkan keluhan. Uji apa yang menemukannya?"
- "Tunjukkan rute yang ada di `running-config` tetapi tidak di `show ip route`. Apa artinya?"

---
---

# PEKAN 13 — Teknologi Generasi Lanjut dan Otomasi Jaringan

**Sub-CPMK 5:** Mahasiswa mampu menganalisis dan merancang jaringan wireless, IoT, dan konektivitas layanan cloud, serta menilai relevansi teknologi generasi lanjut (5G) dan otomasi jaringan bagi organisasi. **(C4)**

**Cakupan materi:** konsolidasi pekan 7, 9, 10, ditambah bahan kajian *Jaringan 5G, Generasi Selanjutnya, dan Automasi Jaringan*.

**Bentuk:** kuliah dan analisis, **bukan praktikum simulasi**.

---

## 13.1 Mengapa pekan ini tidak ada praktikumnya

Bahan kajian mata kuliah ini mencakup jaringan 5G, teknologi generasi selanjutnya, dan otomasi jaringan. Ketiganya tidak dapat disimulasikan di Packet Tracer: tidak ada perangkat 5G, tidak ada shell yang menjalankan skrip otomasi, tidak ada pengontrol SDN.

Menuliskan skrip otomasi yang tidak dapat dijalankan tidak mengajarkan apa pun: yang terjadi hanya penyalinan teks ke laporan. Karena itu pekan ini dinilai dari hal lain yang memang dapat dikerjakan dan diperiksa: **kemampuan menilai apakah sebuah teknologi relevan untuk organisasi tertentu.**

Bagi lulusan Sistem Informasi, keterampilan itu lebih sering dibutuhkan daripada hafalan sintaks alat otomasi yang akan berganti dalam tiga tahun. Anda akan lebih sering diminta menjawab "apakah kita perlu ini" daripada diminta mengonfigurasinya sendiri.

Deliverable pekan ini adalah **Tugas 4**.

---

## 13.2 Jaringan 5G

### Tiga janji, bukan satu

5G sering diperkenalkan sebagai "internet lebih cepat". Kecepatan hanya satu dari tiga hal yang dirancangnya, dan bagi organisasi seukuran PT Nusantara Digital, kecepatan adalah yang paling tidak relevan.

| Kategori | Yang dioptimalkan | Contoh pemakaian |
|---|---|---|
| **eMBB** | Bandwidth besar per pengguna | Video definisi tinggi, download besar |
| **URLLC** | Latensi sangat rendah dan andal | Kendali mesin, kendaraan otonom, bedah jarak jauh |
| **mMTC** | Sangat banyak perangkat berdaya rendah | Sensor tersebar, meteran, pertanian |

Untuk NusantaraNet, yang berpotensi relevan adalah **mMTC** untuk sensor gudang yang berada di luar jangkauan WiFi, dan **eMBB** sebagai tautan backup internet. URLLC tidak relevan: tidak ada proses yang gagal karena latensi 20 milidetik.

### Network slicing

Kemampuan 5G yang paling relevan untuk perusahaan adalah *network slicing*: satu jaringan fisik operator dibagi menjadi beberapa jaringan logis dengan jaminan kinerja berbeda.

Ini konsep yang sudah Anda kenal. Slicing pada jaringan operator adalah VLAN dan QoS pada skala nasional, dengan jaminan kontraktual. Kalau Anda dapat menjelaskan mengapa VLAN tamu dan VLAN karyawan dipisahkan, Anda sudah memahami dasar slicing.

### Edge computing

Latensi rendah tidak dapat dicapai kalau data harus menempuh jarak ke pusat data yang jauh — batasnya kecepatan cahaya, bukan teknologi. Karena itu 5G hadir bersama pemrosesan yang didekatkan ke pengguna.

Konsekuensi arsitektural bagi seorang perancang sistem informasi: sebagian logika aplikasi berpindah keluar dari pusat data. Itu memunculkan pertanyaan baru soal konsistensi data dan keamanan, dan pertanyaan itulah yang relevan bagi Anda — bukan detail radionya.

### Yang perlu disikapi hati-hati

Angka pemasaran 5G hampir selalu berupa kondisi laboratorium: satu perangkat, jarak dekat, pita frekuensi tinggi yang tidak menembus dinding. Di lapangan, 5G pada pita rendah sering hanya sedikit lebih cepat daripada 4G yang baik.

Untuk sebuah perusahaan, pertanyaan yang benar bukan "seberapa cepat 5G" melainkan "apakah ada masalah yang saya miliki hari ini yang hanya dapat diselesaikan 5G". Untuk sebagian besar perusahaan menengah, jawabannya belum.

---

## 13.3 Teknologi Generasi Selanjutnya

### Wi-Fi 6 dan seterusnya

Peningkatan utama Wi-Fi 6 bukan kecepatan puncak, melainkan **perilaku di keadaan padat**. OFDMA memungkinkan satu transmisi melayani beberapa klien sekaligus, sehingga masalah media bersama dari pekan 9 menjadi tidak seburuk sebelumnya.

Ini contoh bagus bahwa nomor generasi tidak selalu berarti "lebih cepat": Wi-Fi 6 pada satu klien tunggal sering hampir sama dengan Wi-Fi 5. Keunggulannya baru muncul pada empat puluh klien di satu ruangan.

### IPv6 dan penerapannya

IPv4 address sudah habis di tingkat regional. IPv6 bukan hal yang akan datang; ia sudah berjalan dan sebagian besar lalu lintas ke penyedia besar sudah melewatinya. Yang menghambat penerapan di perusahaan bukan teknologi, melainkan biaya melatih ulang orang dan memperbarui alat yang berasumsi alamat 32 bit.

Anda sudah menerapkan IPv6 pada dua segmen di pekan 2. Pertanyaan yang harus dapat Anda jawab: apa yang menghalangi menerapkannya di seluruh NusantaraNet, dan apa yang harus disiapkan lebih dulu.

### SDN dan pengelolaan berbasis maksud

Sepanjang semester Anda mengkonfigurasi perangkat satu per satu lewat CLI. Untuk tiga router itu masuk akal. Untuk tiga ratus perangkat, ia tidak dapat dipertahankan: setiap perubahan kebijakan berarti tiga ratus sesi manual, dan setiap sesi adalah kesempatan untuk salah.

SDN memisahkan keputusan dari perangkat: satu pengontrol memegang kebijakan, perangkat menjalankannya. Pengelolaan berbasis maksud melangkah lebih jauh — administrator menyatakan tujuan ("tamu tidak boleh menjangkau segmen internal"), sistem yang menyusun konfigurasinya.

Yang perlu Anda catat: matriks kontrol akses yang Anda buat pada pekan 6 **adalah** pernyataan maksud. Perbedaannya hanya siapa yang menerjemahkannya menjadi konfigurasi — Anda, atau perangkat lunak.

### Zero trust

Model keamanan yang Anda bangun sepanjang semester berbasis lokasi: yang di dalam dipercaya, yang di luar tidak. Asumsi itu melemah ketika karyawan bekerja dari rumah, aplikasi berada di cloud, dan perangkat IoT tidak dapat diperbarui.

Zero trust menggantinya: tidak ada yang dipercaya karena lokasinya, setiap permintaan diverifikasi. Segmen IoT Anda yang diisolasi ketat pada pekan 9 adalah penerapan prinsip ini dalam skala kecil — Anda tidak mempercayai sensor itu walaupun ia berada di dalam jaringan Anda.

---

## 13.4 Otomasi Jaringan

### Masalah yang diselesaikannya

Otomasi jaringan sering diperkenalkan sebagai cara menghemat waktu. Manfaat yang lebih penting adalah **konsistensi**: ketika sepuluh switch dikonfigurasi dengan tangan, kesepuluhnya akan berbeda dalam hal-hal kecil, dan perbedaan itu baru muncul sebagai gangguan berbulan-bulan kemudian.

Anda sudah mengalami versi kecil dari masalah ini pada pekan 7: selisih antara rencana pekan 2 dan kenyataan setelah lima pekan konfigurasi manual. Otomasi adalah cara menghilangkan selisih itu secara sistematis, bukan dengan lebih berhati-hati.

### Empat tingkat, dari yang paling sederhana

| Tingkat | Bentuknya | Sudah Anda lakukan? |
|---|---|---|
| 1. Template | Konfigurasi baku dengan variabel yang diganti per perangkat | Ya, secara manual, dengan parameter X |
| 2. Skrip | Program yang mengirimkan konfigurasi ke banyak perangkat | Belum |
| 3. Konfigurasi sebagai kode | Konfigurasi disimpan di sistem versi, perubahan lewat peninjauan | Belum |
| 4. Berbasis maksud | Kebijakan dinyatakan, konfigurasi dihasilkan | Belum |

Tingkat 1 tidak butuh alat apa pun dan sudah memberi sebagian besar manfaat. Ini yang paling sering dilewati orang yang langsung ingin memakai alat canggih.

### SNMP, NETCONF, dan telemetri

| Pendekatan | Cara kerja | Keterbatasan |
|---|---|---|
| SNMP polling | Server bertanya berkala | Data hanya sebaru interval polling; berat pada skala besar |
| NETCONF/RESTCONF | Konfigurasi dan pembacaan terstruktur | Butuh dukungan perangkat |
| Telemetri | Perangkat mengirim sendiri secara terus-menerus | Butuh infrastruktur penerima |

Perbedaan pokok antara polling dan telemetri: pada polling, Anda hanya mengetahui keadaan pada saat bertanya. Gangguan yang berlangsung tiga puluh detik dengan interval polling lima menit tidak akan pernah terlihat.

### Risiko yang harus disebutkan

Otomasi memperbesar akibat kesalahan. Satu perintah salah yang dijalankan secara manual merusak satu perangkat; satu template salah yang disebarkan otomatis merusak tiga ratus perangkat sekaligus, dalam beberapa detik.

Karena itu praktik otomasi jaringan selalu disertai tiga hal: validasi sebelum penerapan, penerapan bertahap pada sebagian kecil perangkat lebih dulu, dan kemampuan mengembalikan keadaan. Rancangan otomasi tanpa ketiganya lebih berbahaya daripada konfigurasi manual.

---

## 13.5 Konsolidasi Pekan 7, 9, 10

### Latihan soal (30 menit, di kertas)

**Soal 1 — Diagnosis wireless.** Klien wireless terhubung ke SSID, mendapat IP address, tetapi tidak dapat menjangkau segmen mana pun termasuk gatewaynya. Sebutkan apa yang **sudah terbukti berfungsi** oleh fakta bahwa ia mendapat alamat, lalu sebutkan dua penyebab yang masih mungkin.

**Soal 2 — Kapasitas wireless.** Satu AP melayani `60 + 2X` klien di satu ruangan. Sebutkan tiga gejala yang akan dilaporkan pengguna, dan jelaskan mengapa menambah bandwidth internet tidak menyelesaikan satu pun di antaranya.

**Soal 3 — Kanal.** Tiga AP bersebelahan diberi kanal 1, 3, dan 6. Sebutkan pasangan mana yang saling mengganggu paling parah dan mengapa, lalu berikan penetapan kanal yang benar.

**Soal 4 — Failover.** Rute default kedua dengan administrative distance 10 tidak muncul di `show ip route`. Jelaskan apakah ini kegagalan konfigurasi, dan sebutkan cara membuktikan bahwa failover berfungsi.

**Soal 5 — Batas failover.** Sebutkan satu jenis kegagalan ISP yang tidak akan menyebabkan floating static route berpindah, jelaskan gejalanya bagi pengguna, dan sebutkan apa yang dibutuhkan untuk mendeteksinya.

**Soal 6 — Layanan masuk.** Sebuah web server dipetakan lewat port forwarding ke alamat publik ISP-1. Jelaskan apa yang terjadi pada akses dari internet ketika ISP-1 mati dan failover ke ISP-2 berhasil untuk lalu lintas keluar. Mengapa demikian?

**Soal 7 — Dokumentasi.** Sebutkan tiga informasi yang wajib ada pada dokumen as-built agar seseorang yang belum pernah melihat jaringan Anda dapat memulihkan gangguan pukul dua pagi, dan untuk masing-masing sebutkan perintah `show` mana yang menghasilkannya.

---

## 13.6 Tugas 4 — Analisis Relevansi Teknologi

**Bobot:** 2,5% dari nilai akhir. Dikumpulkan akhir pekan 13.

Tulis **maksimal empat halaman** yang menilai relevansi teknologi pekan ini bagi PT Nusantara Digital, berdasarkan jaringan yang **Anda sendiri** bangun.

### Yang harus ada

**Bagian 1 — Keadaan sekarang (setengah halaman).** Ringkas jaringan NusantaraNet Anda: jumlah lokasi, jumlah segmen, perkiraan jumlah perangkat, dan tiga keterbatasan nyata yang Anda temukan sendiri sepanjang semester.

**Bagian 2 — Penilaian teknologi (dua halaman).** Untuk masing-masing dari lima teknologi berikut, berikan **satu** dari tiga putusan, dengan alasan berbasis keadaan jaringan Anda:

- **Terapkan sekarang** — sebutkan langkah pertama yang konkret
- **Siapkan, terapkan nanti** — sebutkan pemicu yang membuatnya perlu
- **Tidak relevan** — sebutkan syarat apa yang harus berubah agar ia menjadi relevan

Kelima teknologi: 5G sebagai tautan backup atau untuk sensor, Wi-Fi 6, IPv6 menyeluruh, otomasi konfigurasi tingkat 1 atau 2, dan prinsip zero trust.

**Bagian 3 — Satu usulan yang dikembangkan (satu halaman).** Pilih **satu** teknologi yang Anda putuskan "terapkan sekarang" dan kembangkan: apa yang berubah pada jaringan Anda, perangkat atau layanan apa yang dibutuhkan, apa risikonya, dan bagaimana Anda membuktikan bahwa penerapannya berhasil.

**Bagian 4 — Audit klaim pemasaran (setengah halaman).** Berikut tiga klaim yang diambil dari materi pemasaran vendor jaringan. Untuk masing-masing, sebutkan apa yang benar, apa yang menyesatkan, dan satu pertanyaan yang harus diajukan sebelum mempercayainya:

1. "5G memberikan latensi 1 milidetik untuk aplikasi kritis Anda."
2. "Dengan dua tautan ISP, bandwidth Anda menjadi dua kali lipat."
3. "Otomasi jaringan menghilangkan kesalahan manusia."

### Cara penilaian

| Kriteria | Bobot |
|---|---|
| Putusan berbasis keadaan jaringan sendiri, bukan definisi umum | 30% |
| Alasan yang menyebut angka atau keterbatasan konkret dari jaringan Anda | 25% |
| Usulan yang dikembangkan: dapat dilaksanakan dan risikonya disebutkan | 25% |
| Audit klaim: menemukan bagian yang menyesatkan, bukan menolak seluruhnya | 20% |

**Putusan "tidak relevan" yang beralasan bernilai sama tinggi dengan putusan "terapkan sekarang".** Tugas yang menyatakan kelima teknologi perlu diterapkan segera hampir pasti tidak dikerjakan berdasarkan keadaan jaringan mana pun, dan akan dinilai rendah pada kriteria pertama.

Tulisan yang mengulang penjelasan teknologi dari modul ini tanpa menghubungkannya ke jaringan Anda mendapat nol pada kriteria pertama dan kedua, walaupun uraiannya benar.

---

## 13.7 Menuju pekan 14

Pekan 14 dan 15 adalah proyek akhir berkelompok: setiap tim memilih skenario industri, memperluas rancangan NusantaraNet ke skala baru, dan mempertanggungjawabkannya pada UAS.

Tiga hal yang perlu selesai sebelum pekan 14:

File NusantaraNet Anda berfungsi penuh dan seluruh temuan audit pekan 11–12 sudah ditutup atau dicatat alasannya. Dokumentasi as-built mutakhir. Dan Anda sudah memutuskan satu hal dari Tugas 4 yang layak dibawa ke proyek kelompok sebagai nilai tambah rancangan.
