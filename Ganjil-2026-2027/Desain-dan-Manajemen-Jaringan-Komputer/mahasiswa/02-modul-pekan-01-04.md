# MODUL DMJK — PEKAN 1–4

**SI2514011 | Proyek: NusantaraNet | Cisco Packet Tracer 8.2+**

> **Cara memakai modul ini.** Setiap pekan punya empat bagian: **Konsep** (dibaca sebelum kelas), **Prompt Pack** (alat bantu AI), **READ → BREAK → FIX → BUILD** (dikerjakan saat praktikum), dan **Checkpoint** (instrumen penilaian Praktikum, 2% per pekan).
>
> Anda boleh dan dianjurkan memakai AI. Yang dinilai adalah apakah Anda bisa membaca, memverifikasi, dan mempertanggungjawabkan konfigurasi itu. Packet Tracer hanya mendukung sebagian perintah Cisco IOS, jadi pakai Prompt B setiap kali AI memberi konfigurasi.

---

## Tentang pekan 1–4: konfigurasi diberikan lengkap

Empat pekan pertama bertingkat **Guided**: seluruh perintah diberikan berurutan dan siap dijalankan. Tugas Anda bukan menemukan perintahnya, melainkan menjelaskan mengapa ia bekerja dan memprediksi apa yang terjadi kalau ia diubah. Itulah yang diuji pada tahap BREAK dan pada viva.

Mulai pekan 5 hal ini berhenti: modul hanya memberi kebutuhan dan tabel kosong, dan perintah dipindah ke Lampiran A tanpa urutan. Empat pekan ini adalah persiapan untuk itu.

**Semua contoh konfigurasi di modul ini memakai X = 27.** Angka Anda berbeda. Setiap alamat, VLAN ID, dan hostname harus Anda hitung dari X Anda sendiri menurut Lampiran B. Menyalin contoh apa adanya akan tertangkap pemeriksa otomatis pada pengumpulan pertama.

---
---

# PEKAN 1 — Fondasi: Apa yang Sebenarnya Terjadi Saat Dua Komputer Berkomunikasi

**Sub-CPMK 1:** Mahasiswa mampu menjelaskan konsep fundamental dan arsitektur jaringan serta menganalisis proses enkapsulasi dan protokol komunikasi melalui observasi paket pada simulator. **(C2, C4)**

**Tingkat:** Guided

**Target akhir pekan:** Anda dapat menjelaskan setiap lapisan yang dilewati satu paket `ping`, dengan bukti dari Simulation Mode, bukan dari slide.

**Catatan penilaian:** Checkpoint pekan 1 tidak berskor. Ia syarat untuk melanjutkan, dan tempat Anda menerima lembar parameter X.

---

## 1.1 Konsep

### Empat pertanyaan yang harus dijawab setiap paket

Setiap kali satu komputer mengirim data ke komputer lain, ada empat pertanyaan yang harus terjawab, dan masing-masing dijawab oleh lapisan yang berbeda:

| Pertanyaan | Dijawab oleh | Bentuk jawabannya |
|---|---|---|
| Aplikasi mana yang dituju? | Transport (L4) | Nomor port |
| Komputer mana, di jaringan mana? | Network (L3) | IP address |
| Perangkat mana di segmen ini? | Data Link (L2) | MAC address |
| Lewat media apa? | Physical (L1) | Sinyal listrik atau cahaya |

IP address dan MAC address keduanya "alamat", tetapi jangkauannya berbeda, dan perbedaan itu adalah sumber sebagian besar kebingungan pemula:

- **IP address berlaku ujung ke ujung.** Ia tidak berubah sepanjang perjalanan paket dari Balikpapan ke Samarinda.
- **MAC address berlaku satu langkah.** Ia ditulis ulang di setiap router yang dilewati.

Akibat praktisnya: ketika PC di segmen HRD mengirim paket ke server di segmen lain, IP address tujuan adalah alamat server, tetapi MAC address tujuan adalah alamat **router**, bukan server. PC tahu bahwa tujuan berada di jaringan lain, jadi ia menyerahkan paket ke gateway. Anda akan melihat ini langsung di Simulation Mode nanti.

### ARP: bagaimana IP menemukan MAC

PC punya IP address tujuan, tetapi frame Ethernet butuh MAC address. Jembatan antara keduanya adalah ARP.

```
PC: "Siapa pemilik 10.27.0.193? Beri tahu 10.27.0.35."   (broadcast)
Router: "10.27.0.193 ada di MAC 00D0.BA12.3456."          (unicast)
```

Dua hal yang perlu dicatat:

Pertanyaan ARP dikirim sebagai **broadcast** — semua perangkat di segmen menerimanya, hanya satu yang menjawab. Jawaban disimpan di cache ARP selama beberapa menit. Inilah sebabnya `ping` pertama ke sebuah tujuan sering lebih lambat atau bahkan gagal pada percobaan pertama di Packet Tracer, sementara `ping` berikutnya lancar: yang pertama harus menunggu ARP selesai.

### Switch belajar, hub tidak

Switch tidak dikonfigurasi untuk tahu perangkat mana ada di port mana. Ia mempelajarinya: setiap frame yang masuk mengajarkan satu pasangan MAC address dan nomor port. Frame ke tujuan yang belum dikenal di-flood ke semua port kecuali port asalnya.

Konsekuensi yang akan Anda pakai berkali-kali semester ini: `show mac address-table` adalah cara tercepat membuktikan bahwa sebuah perangkat benar-benar terhubung dan sudah pernah mengirim sesuatu. Perangkat yang tidak muncul di tabel itu belum pernah bicara.

### Topologi fisik dan topologi logis bukan hal yang sama

Satu switch bisa berisi tiga jaringan yang saling terisolasi. Tiga switch bisa membentuk satu jaringan datar. Diagram fisik menunjukkan kabel; diagram logis menunjukkan segmen dan batas. Anda akan membuat keduanya, dan pada pekan 7 keduanya akan diuji terhadap kenyataan.

---

## 1.2 Prompt Pack — Pekan 1

### A. Prompt Eksplorasi

```
Saya mahasiswa semester 3, baru pertama kali memakai Cisco Packet Tracer.

Jelaskan apa yang terjadi ketika PC A melakukan ping ke PC B yang berada
di JARINGAN LAIN, mulai dari perintah ditekan sampai balasan diterima.

Syarat:
- Sebutkan IP address sumber/tujuan DAN MAC address sumber/tujuan pada
  setiap tahap, termasuk saat paket berada di dalam router.
- Jelaskan di titik mana ARP terlibat, dan siapa yang bertanya kepada siapa.
- Jangan beri konfigurasi. Saya ingin memahami alurnya lebih dulu.
- Setelah menjelaskan, ajukan 3 pertanyaan untuk menguji pemahaman saya.
```

### B. Prompt Verifikasi Dukungan Packet Tracer

```
Perintah diagnosis apa saja yang benar-benar tersedia di Packet Tracer 8.2
pada Command Prompt PC dan pada CLI switch 2960?

Tandai DIDUKUNG / TIDAK ADA / DISEDERHANAKAN untuk: ipconfig, ipconfig /all,
arp -a, ping, tracert, nslookup, telnet, show mac address-table,
show interfaces status, show cdp neighbors.

Untuk yang tidak tersedia, sebutkan penggantinya di Packet Tracer,
termasuk fitur GUI seperti Simulation Mode.
```

### C. Prompt Diagnosis Berjenjang

```
Di Packet Tracer, dua PC saya terhubung ke satu switch tetapi tidak bisa
saling ping. Lampu link pada kedua kabel berwarna hijau.

JANGAN langsung memberi solusi.
1. Sebutkan 4 kemungkinan penyebab, urut dari yang paling sering terjadi.
2. Untuk setiap kemungkinan, sebutkan satu hal yang bisa saya periksa
   untuk membantahnya.
3. Tanyakan hasil pemeriksaan mana yang ingin kamu lihat lebih dulu.
```

### D. Prompt Terlarang

Jangan menempelkan konfigurasi perangkat jaringan kampus yang sungguhan, password `enable` asli, atau data pribadi nyata milik orang lain ke layanan AI mana pun.

---

## 1.3 READ → BREAK → FIX → BUILD

### READ — Bedah satu ping (40 menit)

Bangun topologi minimal: dua PC dan satu switch 2960, alamat `10.27.0.10` dan `10.27.0.11` dengan mask `/24`. Kerjakan **tanpa AI**.

1. Sebelum melakukan apa pun, jalankan `show mac address-table` di switch. Catat isinya. Mengapa kosong?
2. Dari PC1: `arp -a`. Catat isinya. Lakukan `ping` ke PC2, lalu `arp -a` lagi. Apa yang berubah?
3. Jalankan `show mac address-table` di switch sekali lagi. Berapa baris sekarang, dan mengapa jumlahnya begitu?
4. Masuk **Simulation Mode**. Hapus semua event, lalu kirim satu `ping` dari PC1 ke PC2. Perlambat simulasi. Klik amplop pertama dan buka tab **Inbound/Outbound PDU Details**. Catat untuk paket ARP dan paket ICMP pertama:

| Paket | MAC sumber | MAC tujuan | IP sumber | IP tujuan |
|---|---|---|---|---|
| ARP request | | | | |
| ARP reply | | | | |
| ICMP echo request | | | | |
| ICMP echo reply | | | | |

5. Jawab: pada baris ARP request, mengapa MAC tujuan berisi `FFFF.FFFF.FFFF`? Pada baris mana IP sumber kosong, dan mengapa itu masuk akal?

Tabel langkah 4 dikumpulkan. Ini satu-satunya kesempatan Anda melihat isi frame secara langsung; setelah pekan ini semuanya menjadi abstraksi.

### BREAK — Lima percobaan (30 menit)

Isi kolom prediksi **sebelum** mencoba. Kembalikan keadaan setelah setiap nomor.

| # | Yang diubah | Prediksi Anda | Hasil sebenarnya |
|---|---|---|---|
| 1 | Ubah mask PC2 menjadi `/25`, ping dari PC1 | | |
| 2 | Beri PC2 alamat di jaringan berbeda, tanpa router | | |
| 3 | Ganti switch dengan hub, ulangi ping di Simulation Mode | | |
| 4 | Cabut kabel PC2, `ping`, lalu `show mac address-table` | | |
| 5 | Beri kedua PC IP address yang sama | | |

Nomor 1 dan 2 menghasilkan gejala yang mirip tetapi sebabnya berbeda. Jelaskan bedanya dalam laporan: pada nomor 1, siapa yang salah menyimpulkan bahwa tujuan berada di jaringan lain?

Nomor 3 dikerjakan di Simulation Mode, bukan Realtime. Yang diamati bukan berhasil atau gagalnya ping, melainkan **berapa perangkat yang menerima frame** yang seharusnya bukan untuk mereka.

### FIX — Tidak ada pada pekan ini

Tahap FIX dimulai pekan 3, setelah Anda punya cukup dasar untuk mengenali gejala.

### BUILD — Fondasi NusantaraNet

1. Terima lembar parameter X Anda dari asisten. Catat X, blok HQ, blok Cabang, blok Gudang, dan basis VLAN Anda.
2. Buat file `nusantaranet-<NIM>-p01.pkt`. Susun kerangka fisik ketiga lokasi: satu router dan satu switch per lokasi, belum dikonfigurasi, belum tersambung antar-lokasi.
3. Beri nama setiap perangkat sesuai pola Lampiran B (`R-HQ-27`, `SW-HQ-27-01`, dan seterusnya).
4. Pasang dua PC di HQ dengan alamat statis sementara dari blok HQ Anda, dan buktikan keduanya bisa saling ping.
5. Buat diagram fisik awal di Draw.io: perangkat, port yang dipakai, jenis media.

**Tantangan wajib.** Pada topologi dua PC Anda, buatlah keadaan di mana `show mac address-table` menampilkan **satu** baris saja padahal kedua PC menyala dan kabelnya terhubung. Jelaskan cara Anda melakukannya dan mengapa tabelnya begitu.

---

## 1.4 Checkpoint Pekan 1 (tidak berskor)

**Checkpoint 1.** Tabel PDU dari READ langkah 4 terisi lengkap dan benar.

**Checkpoint 2.** Kerangka tiga lokasi ada, penamaan perangkat sesuai X Anda.

**Checkpoint 3.** Anda dapat menjelaskan isi `arp -a` dan `show mac address-table` pada topologi Anda sendiri.

**Viva.** Contoh pertanyaan:

- "Tunjukkan di layar, MAC tujuan pada paket ini milik siapa? Kenapa bukan milik tujuan akhirnya?"
- "Kalau saya hapus cache ARP sekarang, apa yang berubah pada ping berikutnya?"
- "Perangkat ini tidak muncul di `show mac address-table`. Apa artinya?"

---
---

# PEKAN 2 — Perencanaan Alamat: VLSM dan IPv6

**Sub-CPMK 2:** Mahasiswa mampu merancang skema addressing IPv4/IPv6 menggunakan subnetting/VLSM yang efisien terhadap batasan jumlah host dan pertumbuhan organisasi. **(C3)**

**Tingkat:** Guided

**Target akhir pekan:** Seluruh rencana addressing NusantaraNet selesai untuk sebelas segmen di tiga lokasi, tanpa tumpang tindih, dan dapat Anda pertanggungjawabkan angka per angka.

---

## 2.1 Konsep

### Kenapa tidak semua segmen diberi /24 saja

Memberi `/24` ke setiap segmen memang paling mudah dan tidak akan salah hitung. Alasan untuk tidak melakukannya bukan penghematan alamat privat, karena `10.0.0.0/8` praktis tidak akan habis. Alasannya ada tiga:

**Peringkasan rute.** Kalau seluruh segmen Gudang berada dalam satu blok yang bersebelahan, router HQ hanya butuh satu baris rute untuk mencapai seluruh Gudang. Dengan alamat yang tersebar acak, ia butuh satu baris per segmen. Anda akan merasakan bedanya langsung pada pekan 4.

**Batas keamanan yang bisa ditulis dalam satu baris.** ACL yang menolak `10.27.20.0/22` mencakup seluruh Gudang sekaligus. Kalau segmen Gudang tersebar, kebijakan yang sama butuh empat baris, dan baris keempat akan lupa ditambahkan ketika ada segmen baru.

**Ukuran domain broadcast.** Satu `/24` berisi 254 alamat. Menempatkan enam perangkat di dalamnya bukan pemborosan alamat, melainkan pemborosan diagnosis: ketika terjadi banjir broadcast, Anda punya 254 kemungkinan sumber untuk diperiksa.

### Cara mengerjakan VLSM tanpa salah

Prosedurnya selalu sama, dan urutannya tidak boleh ditukar:

1. **Daftarkan kebutuhan host setiap segmen**, lalu urutkan dari besar ke kecil.
2. **Tentukan ukuran blok** untuk masing-masing: cari pangkat 2 terkecil yang menampung `kebutuhan + 2` (alamat jaringan dan broadcast).
3. **Alokasikan dari atas**, dimulai dari segmen terbesar, berurutan tanpa celah.
4. **Verifikasi**: broadcast setiap blok harus lebih kecil dari alamat jaringan blok berikutnya.

Langkah 1 adalah yang paling sering dilanggar. Kalau alokasi dilakukan dari kecil ke besar, blok besar tidak akan menemukan ruang yang sejajar dan Anda harus mulai dari awal.

Tabel yang harus Anda hafal:

| Prefix | Jumlah alamat | Host dapat dipakai |
|---|---:|---:|
| /24 | 256 | 254 |
| /25 | 128 | 126 |
| /26 | 64 | 62 |
| /27 | 32 | 30 |
| /28 | 16 | 14 |
| /29 | 8 | 6 |
| /30 | 4 | 2 |

`/30` dipakai untuk tautan antar-router: hanya dua alamat yang dibutuhkan, satu di setiap ujung.

### Kebutuhan host bukan jumlah orang hari ini

Segmen dengan 52 pengguna tidak butuh 52 alamat. Setiap orang membawa laptop dan ponsel; ada printer, access point, kamera. Aturan praktis di industri: rancang untuk **kebutuhan sekarang ditambah pertumbuhan tiga tahun**, biasanya 30 sampai 50 persen.

Di NusantaraNet, angka pada Lampiran B **sudah** merupakan kebutuhan akhir. Jangan menambahkan margin lagi di atasnya, tetapi Anda harus dapat menjelaskan apa yang akan Anda lakukan kalau segmen HRD bertambah dua kali lipat tahun depan. Jawaban yang benar melibatkan sisa blok yang Anda sisakan, bukan mengubah seluruh rencana.

### IPv6: cukup dua segmen, tetapi harus benar

IPv6 address berukuran 128 bit, ditulis dalam delapan kelompok heksadesimal. Aturan penyingkatan:

```
2001:0db8:0027:0410:0000:0000:0000:0001
2001:db8:27:410::1              (nol di depan dibuang, satu blok nol diringkas ::)
```

Tanda `::` hanya boleh muncul **sekali** dalam satu alamat. Kalau muncul dua kali, jumlah blok nol menjadi ambigu dan alamatnya tidak sah.

Tiga hal yang berbeda dari IPv4 dan perlu Anda ketahui:

Subnet IPv6 hampir selalu `/64`, bahkan untuk tautan antar-router yang hanya butuh dua alamat. Tidak ada penghematan yang perlu dilakukan.

Tidak ada alamat broadcast; fungsinya digantikan multicast. Tidak ada ARP; penggantinya Neighbor Discovery.

Routing IPv6 harus diaktifkan secara eksplisit di router dengan `ipv6 unicast-routing`. Tanpa itu, router menerima IPv6 address tetapi tidak meneruskan paket IPv6, dan gejalanya menyesatkan: interface tampak benar, ping ke gateway berhasil, ping ke segmen lain gagal.

Di NusantaraNet, IPv6 hanya diterapkan pada dua segmen — HRD dan Keuangan — sebagai bukti pemahaman. Subnet ID-nya memakai angka VLAN: `2001:db8:27:410::/64` untuk VLAN 410.

---

## 2.2 Prompt Pack — Pekan 2

### A. Prompt Verifikasi Perhitungan Sendiri

```
Saya sudah menghitung VLSM untuk blok 10.27.0.0/20 dengan kebutuhan
berikut: [daftar segmen dan kebutuhan host Anda].

Ini hasil saya: [tabel Anda].

JANGAN menghitung ulang dari awal untuk saya.
Sebagai gantinya, PERIKSA pekerjaan saya:
1. Adakah blok yang tumpang tindih? Sebutkan barisnya.
2. Adakah prefix yang terlalu besar untuk kebutuhannya? Berapa alamat
   yang terbuang?
3. Adakah celah yang tidak terpakai di antara blok?
4. Apakah urutan alokasinya dari besar ke kecil?

Untuk setiap temuan, sebutkan barisnya saja, jangan langsung diperbaiki.
```

Urutan ini penting: minta AI **memeriksa** hasil Anda, bukan mengerjakannya. Kemampuan menghitung VLSM di bawah tekanan waktu adalah salah satu dari tiga hal yang menentukan hasil UTS Anda.

### B. Prompt Verifikasi Dukungan Packet Tracer

```
Untuk konfigurasi IPv6 di Packet Tracer 8.2 pada router 2911, tandai
DIDUKUNG / TIDAK ADA / DISEDERHANAKAN:

ipv6 unicast-routing, ipv6 address <prefix>/64, ipv6 address autoconfig,
ipv6 address <prefix> eui-64, show ipv6 interface brief, show ipv6 route,
ping <alamat ipv6> dari Command Prompt PC.

Sebutkan juga cara mengisi IPv6 address pada PC di Packet Tracer, dan
apakah SLAAC berfungsi di simulator ini.
```

### C. Prompt Konsekuensi Desain

```
Ini rencana addressing saya: [tabel Anda].

Berperan sebagai auditor jaringan. Jangan memuji dan jangan memperbaiki.
Ajukan 5 pertanyaan yang akan sulit saya jawab tentang rencana ini,
terutama menyangkut:
- apa yang terjadi kalau satu segmen tumbuh dua kali lipat
- apakah rute ke setiap lokasi bisa diringkas menjadi satu baris
- apakah kebijakan keamanan per lokasi bisa ditulis dalam satu baris ACL
```

### D. Prompt Terlarang

Jangan meminta AI mengerjakan seluruh perhitungan VLSM Anda dari nol. Bukan karena hasilnya salah, tetapi karena keterampilan inilah yang diuji pada UTS tanpa bantuan alat apa pun.

---

## 2.3 READ → BREAK → FIX → BUILD

### READ — Baca rencana yang sudah ada (25 menit)

Buka contoh terisi untuk X = 27 di Lampiran B bagian 5. Tanpa AI:

1. Untuk segmen HRD pada contoh itu, hitung sendiri: alamat jaringan, alamat host pertama, alamat host terakhir, dan broadcast. Cocokkan dengan tabel.
2. Segmen Manajemen diberi `/29` untuk kebutuhan 6 host, sehingga tidak ada ruang tumbuh sama sekali. Sebutkan satu argumen untuk memilih `/29` dan satu argumen untuk memilih `/28`.
3. Berapa alamat yang tersisa di blok HQ setelah keenam segmen dialokasikan? Tunjukkan perhitungannya.
4. Kalau segmen WiFi-Karyawan pada contoh itu tumbuh dari 114 menjadi 200 host, apakah ia bisa diperbesar **tanpa** memindahkan segmen lain? Jelaskan mengapa.

Jawaban nomor 4 menentukan apakah rencana Anda sendiri nanti tahan terhadap pertumbuhan.

### BREAK — Enam percobaan (30 menit)

Pakai topologi dua PC pekan 1, tambahkan satu router sebagai gateway.

| # | Yang diubah | Prediksi Anda | Hasil sebenarnya |
|---|---|---|---|
| 1 | Beri PC alamat jaringan (`.0`) sebagai alamat host | | |
| 2 | Beri PC alamat broadcast subnet sebagai alamat host | | |
| 3 | Beri dua segmen blok yang tumpang tindih, ping antar keduanya | | |
| 4 | Beri PC mask `/26` sementara gateway `/24`, ping ke luar segmen | | |
| 5 | Konfigurasi IPv6 di kedua PC dan router, **tanpa** `ipv6 unicast-routing` | | |
| 6 | Tulis IPv6 address dengan dua tanda `::` | | |

Nomor 4 adalah kesalahan yang paling sering terjadi di dunia nyata dan paling sulit didiagnosis, karena sebagian tujuan tetap terjangkau. Catat **tujuan mana** yang masih berhasil dan mana yang gagal, lalu jelaskan pola tersebut.

Nomor 5 menghasilkan gejala yang akan Anda temui lagi pada pekan 4: interface terlihat benar tetapi paket tidak diteruskan.

### FIX — Tidak ada pada pekan ini

### BUILD — Rencana addressing lengkap

Kerjakan untuk **X Anda sendiri**, bukan 27. Deliverable ini juga menjadi **Tugas 1** (bobot 2,5% dari nilai akhir).

1. Tabel VLSM untuk ketiga lokasi, sebelas segmen, dengan kolom: segmen, kebutuhan host, prefix, alamat jaringan, host pertama, host terakhir, broadcast, gateway, VLAN ID.
2. Tabel empat tautan WAN `/30` beserta alamat kedua ujungnya.
3. Tabel loopback ketiga router.
4. Dua prefix IPv6 `/64` untuk HRD dan Keuangan.
5. Alamat sisa yang belum terpakai di setiap blok, disebutkan eksplisit sebagai ruang pertumbuhan.
6. **Justifikasi tiga keputusan** dalam masing-masing dua sampai tiga kalimat: mengapa prefix segmen terbesar Anda sebesar itu, mengapa Manajemen diberi ukuran itu, dan segmen mana yang paling mungkin tumbuh lebih dulu beserta rencana Anda menghadapinya.

Terapkan alamat statis untuk router dan dua PC uji di HQ pada file `.pkt` Anda. Segmen lain belum perlu diisi; VLAN baru dibuat pekan depan.

**Tantangan wajib.** Manajemen meminta ruang untuk satu lokasi keempat berukuran sama dengan Cabang, dalam blok `10.X` Anda, **tanpa mengubah** alokasi yang sudah ada. Tunjukkan blok mana yang Anda pilih dan buktikan ia tidak bertabrakan dengan apa pun.

---

## 2.4 Checkpoint Pekan 2 (2%)

**Checkpoint 1.** Tabel VLSM lengkap, tanpa tumpang tindih, prefix sesuai kebutuhan host X Anda. Asisten memeriksa tiga baris acak dengan menghitung ulang.

**Checkpoint 2.** Empat tautan `/30` dan tiga loopback benar; IPv6 aktif di dua segmen dan `ping` IPv6 antar keduanya berhasil.

**Checkpoint 3.** Tiga justifikasi tertulis, dan Anda dapat mempertahankannya secara lisan.

**Viva.** Contoh pertanyaan:

- "Kenapa segmen ini Anda beri `/26` dan bukan `/25`?"
- "Berapa alamat yang tersisa di blok Gudang Anda? Tunjukkan perhitungannya."
- "Kalau HRD tumbuh dua kali lipat, apa yang Anda ubah?"
- "Alamat ini `10.27.1.63`. Ia host, jaringan, atau broadcast? Di segmen mana?"
- "Kenapa tautan antar-router `/30` dan bukan `/24`?"

---
---

# PEKAN 3 — Switching dan VLAN

**Sub-CPMK 3:** Mahasiswa mampu mengimplementasikan dan memverifikasi infrastruktur jaringan meliputi switching, routing, layanan jaringan, dan kontrol akses. **(C3)**

**Tingkat:** Guided

**Target akhir pekan:** Keenam segmen HQ terpisah sebagai VLAN, dan komunikasi antar keduanya hanya mungkin lewat router.

---

## 3.1 Konsep

### VLAN memisahkan, bukan mengamankan

VLAN membagi satu switch fisik menjadi beberapa switch logis. Perangkat di VLAN 410 tidak dapat berbicara langsung dengan perangkat di VLAN 420, walaupun kabelnya menancap di switch yang sama.

Kalimat berikut perlu ditegaskan sejak sekarang karena sering disalahpahami sepanjang semester: **VLAN bukan mekanisme keamanan.** Yang dilakukan VLAN adalah memisahkan domain broadcast. Begitu Anda menambahkan router untuk menyambungkan VLAN pada bagian berikutnya, seluruh isolasi itu hilang kembali — semua VLAN dapat saling menjangkau lewat router. Yang menegakkan kebijakan adalah ACL, dan itu pekerjaan pekan 6.

Manfaat VLAN yang nyata: domain broadcast lebih kecil, segmentasi tidak bergantung lokasi fisik, dan tersedianya satu titik tempat seluruh lalu lintas antar-segmen dapat diperiksa.

### Port akses dan port trunk

| Jenis port | Membawa | Dipakai untuk |
|---|---|---|
| Access | Satu VLAN, frame tanpa tag | Menyambung PC, printer, server |
| Trunk | Banyak VLAN, frame diberi tag 802.1Q | Menyambung switch ke switch, atau switch ke router |

Tag 802.1Q adalah empat byte yang disisipkan ke dalam frame Ethernet, berisi nomor VLAN. Tag hanya ada di dalam tautan trunk. Switch menambahkannya saat frame masuk trunk dan membuangnya saat frame keluar ke port akses — PC tidak pernah melihat tag dan tidak perlu tahu tentang VLAN.

Dua kesalahan yang menghasilkan gejala paling membingungkan:

**VLAN tidak diizinkan di trunk.** Konfigurasi VLAN di kedua switch benar, port akses benar, tetapi VLAN itu tidak tercantum pada daftar `switchport trunk allowed vlan`. Akibatnya perangkat di satu switch tidak bisa mencapai perangkat di switch lain pada VLAN yang sama, sementara VLAN lain berfungsi normal.

**Port dibiarkan di VLAN 1.** Port akses yang tidak diberi VLAN secara eksplisit berada di VLAN 1. Perangkat di situ tampak "tidak terhubung ke mana pun" padahal lampu link hijau. Karena itu VLAN 1 tidak pernah dipakai untuk data di NusantaraNet.

### Router-on-a-stick

Untuk menyambungkan VLAN, dibutuhkan perangkat lapisan 3. Cara yang dipakai NusantaraNet adalah satu tautan fisik dari router ke switch yang dijadikan trunk, lalu dibagi menjadi beberapa **sub-interface**, satu per VLAN.

```
                       Gi0/0.410  <- VLAN 410 (HRD)
R-HQ-27  Gi0/0 ======= Gi0/0.420  <- VLAN 420 (Keuangan)
                trunk  Gi0/0.430  <- VLAN 430 (Server)
```

Setiap sub-interface memegang gateway satu segmen. Tiga hal yang harus konsisten dan menjadi sumber kesalahan kalau tidak:

Nomor sub-interface (`.410`) hanyalah label bagi manusia; yang menentukan adalah `encapsulation dot1Q 410`. Angka pada `encapsulation` inilah yang harus sama dengan VLAN di switch. Kalau labelnya `.410` tetapi enkapsulasinya `420`, konfigurasi tetap diterima tanpa keluhan dan gejalanya sulit dilacak.

Interface fisik induk harus `no shutdown`. Sub-interface tidak akan aktif tanpa itu, walaupun sub-interface-nya sendiri tidak di-`shutdown`.

Alamat pada setiap sub-interface harus sama dengan gateway yang Anda cantumkan di rencana pekan 2. Selisih satu angka di sini menghasilkan segmen yang bisa berkomunikasi di dalam dirinya sendiri tetapi tidak bisa keluar.

---

## 3.2 Prompt Pack — Pekan 3

### A. Prompt Eksplorasi

```
Jelaskan apa yang terjadi pada sebuah frame Ethernet ketika ia berjalan
dari PC di VLAN 410 ke PC di VLAN 420, melalui satu switch dan satu
router yang dikonfigurasi router-on-a-stick.

Syarat:
- Sebutkan di titik mana tag 802.1Q ditambahkan dan di titik mana ia dibuang.
- Sebutkan MAC address tujuan pada setiap tahap.
- Jangan beri konfigurasi.
- Ajukan 3 pertanyaan untuk menguji pemahaman saya.
```

### B. Prompt Verifikasi Dukungan Packet Tracer

```
Konfigurasi VLAN dan trunk yang kamu berikan, apakah didukung Packet
Tracer 8.2 pada switch 2960 dan router 2911?

Periksa khusus:
1. Apakah "switchport trunk encapsulation dot1q" ada pada switch 2960?
   (saya menduga tidak, dan ingin tahu alasannya)
2. Apakah VTP berfungsi di Packet Tracer?
3. Apakah "switchport nonegotiate" tersedia?

Tandai setiap perintah DIDUKUNG / TIDAK ADA / DISEDERHANAKAN.
```

### C. Prompt Diagnosis Berjenjang

```
Di Packet Tracer, dua PC di VLAN yang sama tetapi di switch berbeda
tidak bisa saling ping. PC dalam satu switch yang sama bisa.

JANGAN memberi solusi.
1. Sebutkan 4 penyebab paling mungkin, urut dari yang paling sering.
2. Untuk masing-masing, sebutkan satu perintah show yang membantahnya.
3. Tanyakan output mana yang ingin kamu lihat lebih dulu.
```

### D. Prompt Terlarang

Aturan umum berlaku. Tambahan untuk pekan ini: jangan meminta AI menuliskan seluruh konfigurasi VLAN untuk sebelas segmen sekaligus. Anda akan mendapat blok panjang yang benar secara sintaks tetapi memakai VLAN ID milik contoh, bukan milik Anda, dan kesalahan itu baru terlihat tiga pekan kemudian.

---

## 3.3 READ → BREAK → FIX → BUILD

### READ — Baca konfigurasi yang diberikan (30 menit)

Konfigurasi berikut lengkap dan berfungsi, untuk **X = 27**. Terapkan lebih dulu untuk tiga VLAN, lalu baca dan jawab pertanyaannya. Ganti setiap angka dengan milik Anda.

Pada switch:

```
enable
configure terminal
hostname SW-HQ-27-01

vlan 410
 name HRD
vlan 420
 name KEUANGAN
vlan 430
 name SERVER
vlan 499
 name MANAJEMEN
exit

interface range fastEthernet0/2-5
 switchport mode access
 switchport access vlan 410
exit

interface range fastEthernet0/6-9
 switchport mode access
 switchport access vlan 420
exit

interface fastEthernet0/10
 switchport mode access
 switchport access vlan 430
exit

interface gigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 410,420,430,499
exit

interface vlan 499
 ip address 10.27.1.82 255.255.255.248
 no shutdown
exit

ip default-gateway 10.27.1.81
end
write memory
```

Pada router:

```
enable
configure terminal
hostname R-HQ-27

interface gigabitEthernet0/0
 no shutdown
exit

interface gigabitEthernet0/0.410
 description Gateway HRD
 encapsulation dot1Q 410
 ip address 10.27.0.193 255.255.255.192
exit

interface gigabitEthernet0/0.420
 description Gateway Keuangan
 encapsulation dot1Q 420
 ip address 10.27.1.1 255.255.255.192
exit

interface gigabitEthernet0/0.430
 description Gateway Server
 encapsulation dot1Q 430
 ip address 10.27.1.65 255.255.255.240
exit

interface gigabitEthernet0/0.499
 description Gateway Manajemen
 encapsulation dot1Q 499
 ip address 10.27.1.81 255.255.255.248
exit
end
write memory
```

Jawab tanpa AI:

1. Mask `255.255.255.192` pada sub-interface `.410` setara prefix berapa? Cocokkan dengan tabel VLSM Anda sendiri — apakah angka Anda sama?
2. `interface vlan 499` pada switch bukan gateway untuk pengguna. Untuk apa ia ada? Apa yang hilang kalau ia dihapus?
3. Jalankan `show vlan brief`. Port mana yang berada di VLAN 1, dan mengapa masih ada yang di sana?
4. Jalankan `show interfaces trunk`. Kolom mana yang membuktikan VLAN 430 diizinkan lewat? Apa isi kolom "Native"?
5. Jalankan `show ip route` di router. Ada empat jaringan terhubung. Mengapa router tahu keempatnya tanpa satu pun perintah `ip route`?

Pertanyaan 5 adalah dasar dari seluruh pekan 4.

### BREAK — Enam percobaan (35 menit)

| # | Yang diubah | Prediksi Anda | Hasil sebenarnya |
|---|---|---|---|
| 1 | Ubah `encapsulation dot1Q 420` menjadi `421` | | |
| 2 | Hapus VLAN 430 dari `switchport trunk allowed vlan` | | |
| 3 | Ubah satu port akses HRD menjadi VLAN 420 | | |
| 4 | `shutdown` pada interface fisik Gi0/0 router | | |
| 5 | Ubah port trunk menjadi `switchport mode access` | | |
| 6 | Hapus `no shutdown` pada `interface vlan 499` switch | | |

Nomor 1 diterima router tanpa pesan error apa pun. Catat bagaimana Anda akhirnya bisa mengetahuinya, dan perintah `show` mana yang paling cepat menunjukkannya.

Nomor 3 adalah simulasi kesalahan patching yang lazim terjadi. Dari sudut pandang pengguna di PC tersebut, apa yang ia laporkan ke helpdesk?

### FIX — File cacat (25 menit)

Download `dmjk-broken-p03.pkt`. Topologi VLAN satu lokasi dengan **4 fault**: satu di penugasan port akses, satu di daftar VLAN trunk, satu di enkapsulasi sub-interface, dan satu di alamat gateway.

Untuk setiap fault, catat **gejala yang Anda lihat lebih dulu**, bukan hanya perbaikannya. Fault yang diperbaiki tanpa gejala yang tercatat dihitung setengah.

### BUILD — Segmentasi HQ

Terapkan pada file Anda sendiri, seluruh enam segmen HQ:

1. Enam VLAN dengan ID dan nama sesuai X Anda, ditambah VLAN Manajemen.
2. Port akses ditugaskan per segmen; port yang tidak dipakai di-`shutdown`.
3. Trunk router-ke-switch yang hanya mengizinkan VLAN yang memang ada.
4. Sub-interface router untuk keenam segmen dengan gateway sesuai rencana pekan 2.
5. SVI manajemen di switch beserta `ip default-gateway`.
6. Dua PC uji per segmen dengan alamat statis, membuktikan komunikasi di dalam segmen dan antar segmen berfungsi.
7. Diagram logis: segmen, VLAN ID, subnet, dan letak gateway.

**Tantangan wajib.** Tambahkan switch kedua di HQ (`SW-HQ-27-02`), pindahkan dua PC HRD ke sana, dan buktikan keduanya tetap berada di segmen HRD yang sama. Lalu jawab: perubahan apa yang dibutuhkan pada trunk antar-switch, dan apa yang terjadi kalau daftar VLAN yang diizinkan di kedua ujung trunk berbeda?

---

## 3.4 Checkpoint Pekan 3 (2%)

**Checkpoint 1.** `show vlan brief` menampilkan enam segmen dengan ID sesuai X Anda; tidak ada port aktif yang tertinggal di VLAN 1.

**Checkpoint 2.** Komunikasi dalam segmen berhasil, komunikasi antar segmen berhasil lewat router, dan `show interfaces trunk` membuktikan VLAN mana yang lewat.

**Checkpoint 3.** Dua PC di segmen yang sama pada dua switch berbeda dapat saling ping.

**Viva.** Contoh pertanyaan:

- "VLAN ID Anda 420. Dari mana angka itu?"
- "Tunjukkan baris yang membuat PC ini bisa mencapai segmen Server."
- "Kalau saya hapus satu VLAN dari daftar trunk, siapa yang terdampak dan siapa yang tidak?"
- "`interface vlan 499` ini untuk apa? Apa akibatnya kalau saya matikan?"
- "Apakah VLAN membuat segmen Keuangan Anda aman dari HRD sekarang? Buktikan."

---
---

# PEKAN 4 — Routing Statis dan Konektivitas Multi-Lokasi

**Sub-CPMK 3:** Mahasiswa mampu mengimplementasikan dan memverifikasi infrastruktur jaringan meliputi switching, routing, layanan jaringan, dan kontrol akses. **(C3)**

**Tingkat:** Guided

**Target akhir pekan:** Ketiga lokasi NusantaraNet saling terhubung, dan Anda dapat menjelaskan setiap baris pada `show ip route` ketiga router.

---

## 4.1 Konsep

### Router hanya tahu apa yang diberitahukan

Router mengenal dua jenis jaringan tanpa diberi tahu: jaringan yang terhubung langsung ke interface-nya yang aktif. Semua jaringan lain harus diberitahukan, entah oleh administrator (static route) atau oleh protokol routing.

Perilaku router terhadap paket yang tujuannya tidak ada di tabel rute adalah **membuangnya**, bukan menebak atau meneruskan ke sembarang arah. Karena itu satu rute yang hilang di satu router sudah cukup untuk mematikan komunikasi ke satu arah, sementara arah kebalikannya tampak normal.

### Routing itu satu arah

Ini penyebab kegagalan paling sering pada pekan ini. Komunikasi dua arah membutuhkan **dua** rute: satu untuk paket pergi, satu untuk paket balik. Setiap router di sepanjang jalur harus punya keduanya.

Gejala khas kalau hanya satu arah yang dikonfigurasi: `ping` gagal total, tetapi `show ip route` di router asal tampak benar, dan `traceroute` berhenti di suatu titik di tengah. Paket ICMP echo request tiba dengan sukses; yang tidak pernah kembali adalah balasannya.

Cara memeriksanya: jalankan `traceroute` dari kedua arah. Titik tempat kedua jejak berhenti menunjukkan router yang kehilangan rute.

### Menulis static route

```
ip route <jaringan-tujuan> <mask> <next-hop>
```

`next-hop` adalah IP address router **tetangga** yang akan meneruskan paket, bukan alamat tujuan akhir, dan harus berada di jaringan yang terhubung langsung. Kalau next-hop tidak terjangkau, rute tetap diterima dan tercantum di konfigurasi, tetapi tidak akan muncul di `show ip route` — dan itu petunjuk diagnosis yang berguna: **rute yang ada di `running-config` tetapi tidak ada di `show ip route` berarti next-hop-nya bermasalah.**

### Rute ringkas dan rute default

Kalau Gudang memakai tiga segmen di dalam `10.27.20.0/22`, router HQ tidak perlu tiga baris:

```
ip route 10.27.20.0 255.255.252.0 172.16.27.6
```

Satu baris menggantikan tiga. Inilah manfaat konkret dari perencanaan pekan 2 yang menempatkan segmen satu lokasi dalam blok yang bersebelahan. Kalau Anda menulis satu rute per segmen, konfigurasi tetap bekerja tetapi Anda kehilangan salah satu tujuan utama latihan ini.

Untuk arah sebaliknya, router Cabang dan Gudang tidak perlu tahu detail apa pun tentang HQ dan internet. Mereka cukup punya **rute default**:

```
ip route 0.0.0.0 0.0.0.0 172.16.27.5
```

Artinya: apa pun yang tidak saya kenali, kirim ke sana. Ini bukan kemalasan, melainkan desain yang benar untuk topologi hub-and-spoke — cabang memang hanya punya satu jalan keluar, dan menuliskan rute rinci di sana hanya menambah tempat untuk salah.

### Membaca show ip route

```
C    10.27.0.192/26 is directly connected, GigabitEthernet0/0.410
S    10.27.20.0/22 [1/0] via 172.16.27.6
S*   0.0.0.0/0 [1/0] via 172.16.27.9
```

| Kode | Arti |
|---|---|
| `C` | Terhubung langsung; router tahu tanpa diberi tahu |
| `L` | Alamat lokal interface itu sendiri |
| `S` | Statis; Anda yang menuliskannya |
| `S*` | Statis, dan menjadi rute default |

Angka `[1/0]` adalah administrative distance dan metric. Administrative distance static route adalah 1, dan angka inilah yang nanti dipakai pada pekan 10 untuk membuat rute backup ke ISP kedua: rute dengan angka lebih besar hanya dipakai kalau yang utama hilang.

### Internet belum akan berfungsi pekan ini

Setelah rute selesai, `ping` dari PC ke interface router ISP akan berhasil, tetapi `ping` ke alamat di internet tetap gagal. Ini **bukan** kesalahan Anda: paket keluar dengan alamat sumber privat `10.X`, dan alamat itu tidak dapat dirutekan balik. Yang menyelesaikannya adalah NAT, pekan depan.

Bedakan dua hal ini dengan jelas sekarang, karena keduanya sering dikira sama: routing menentukan **ke mana** paket pergi; NAT menentukan **dengan alamat apa** ia pergi.

---

## 4.2 Prompt Pack — Pekan 4

### A. Prompt Perancangan Rute

```
Saya punya tiga lokasi di Packet Tracer:
- HQ: 10.27.0.0/20, router R-HQ-27
- Cabang: 10.27.16.0/22, router R-BR-27
- Gudang: 10.27.20.0/22, router R-GD-27
Tautan WAN: HQ-Cabang 172.16.27.0/30, HQ-Gudang 172.16.27.4/30,
HQ-ISP 172.16.27.8/30. Topologi hub-and-spoke, HQ di tengah.

JANGAN tulis perintahnya dulu.
Buat TABEL rute yang dibutuhkan: untuk setiap router, tujuan mana yang
harus ada di tabel rutenya, dan lewat next-hop mana.

Tandai mana yang bisa DIRINGKAS menjadi satu baris dan mana yang cukup
memakai rute default. Lalu jelaskan mengapa jumlah baris di R-HQ berbeda
jauh dari jumlah baris di R-BR.
```

### B. Prompt Verifikasi Dukungan Packet Tracer

```
Tandai DIDUKUNG / TIDAK ADA / DISEDERHANAKAN di Packet Tracer 8.2 pada
router 2911:

ip route dengan next-hop IP, ip route dengan exit-interface,
floating static route dengan administrative distance,
show ip route, show ip route summary, traceroute, show cdp neighbors detail,
ping dengan source interface tertentu.
```

### C. Prompt Diagnosis Berjenjang

```
Di Packet Tracer, PC di Cabang tidak bisa ping server di HQ. Dari router
Cabang, ping ke interface WAN router HQ BERHASIL. Dari router HQ, ping ke
server HQ BERHASIL.

JANGAN memberi solusi.
1. Berdasarkan tiga fakta itu, bagian mana dari jalur yang sudah
   TERBUKTI sehat?
2. Sebutkan 3 hipotesis yang bisa dibantah, beserta uji pembedanya.
3. Sebutkan satu perintah yang paling cepat mempersempit kemungkinan,
   dan jelaskan cara membaca hasilnya.
```

### D. Prompt Terlarang

Aturan umum berlaku. Tambahan: jangan menempelkan seluruh `show running-config` ketiga router lalu meminta AI mencari rute yang hilang. Menyusun tabel rute yang dibutuhkan adalah pekerjaan perancang, dan itu yang diuji pada UTS.

---

## 4.3 READ → BREAK → FIX → BUILD

### READ — Baca tabel rute (30 menit)

Terapkan konfigurasi berikut untuk **X = 27**, ganti dengan angka Anda.

Pada R-HQ:

```
interface gigabitEthernet0/1
 description WAN ke Cabang
 ip address 172.16.27.1 255.255.255.252
 no shutdown
exit

interface gigabitEthernet0/2
 description WAN ke Gudang
 ip address 172.16.27.5 255.255.255.252
 no shutdown
exit

interface loopback0
 ip address 172.31.27.1 255.255.255.255
exit

ip route 10.27.16.0 255.255.252.0 172.16.27.2
ip route 10.27.20.0 255.255.252.0 172.16.27.6
ip route 172.31.27.2 255.255.255.255 172.16.27.2
ip route 172.31.27.3 255.255.255.255 172.16.27.6
```

Pada R-BR:

```
interface gigabitEthernet0/1
 description WAN ke HQ
 ip address 172.16.27.2 255.255.255.252
 no shutdown
exit

interface loopback0
 ip address 172.31.27.2 255.255.255.255
exit

ip route 0.0.0.0 0.0.0.0 172.16.27.1
```

Jawab tanpa AI:

1. Jalankan `show ip route` di R-HQ. Hitung baris berkode `C`, `L`, dan `S`. Mengapa jumlah `C` sebanyak itu?
2. Jalankan `show ip route` di R-BR. Mengapa jumlah barisnya jauh lebih sedikit padahal ia menjangkau tujuan yang sama?
3. R-HQ punya dua rute `/22` dan dua rute `/32`. Untuk apa yang `/32`? Apa yang tidak berfungsi kalau keduanya dihapus?
4. Dari PC di Cabang, jalankan `tracert` ke server HQ. Catat setiap hop dan cocokkan dengan tabel rute yang baru Anda baca.
5. Dari PC di Cabang, `ping` ke alamat internet. Ia gagal. Tunjukkan dengan `show ip route` bahwa penyebabnya **bukan** rute yang hilang.

Pertanyaan 5 memisahkan masalah routing dari masalah NAT — pembedaan yang akan Anda pakai sepanjang sisa semester.

### BREAK — Tujuh percobaan (40 menit)

| # | Yang diubah | Prediksi Anda | Hasil sebenarnya |
|---|---|---|---|
| 1 | Hapus rute ke Cabang di R-HQ, biarkan default di R-BR | | |
| 2 | Ubah next-hop satu rute menjadi alamat yang tidak terjangkau | | |
| 3 | `shutdown` interface WAN di R-BR, lihat `show ip route` R-HQ | | |
| 4 | Ganti rute `/22` di R-HQ menjadi tiga rute per segmen | | |
| 5 | Tulis rute dengan mask `/24` untuk jaringan yang sebenarnya `/22` | | |
| 6 | Hapus rute default di R-BR, ping dari Cabang ke HQ | | |
| 7 | Tambah rute kedua ke tujuan yang sama lewat next-hop berbeda | | |

Nomor 1 adalah pelajaran utama pekan ini: catat arah mana yang berhasil dan arah mana yang gagal, lalu jelaskan mengapa `ping` dari Cabang gagal padahal R-BR punya rute yang benar.

Nomor 2 menghasilkan rute yang ada di `running-config` tetapi tidak ada di `show ip route`. Pastikan Anda melihat sendiri perbedaan kedua output itu.

Nomor 5 menghasilkan konektivitas yang berhasil untuk sebagian tujuan saja. Catat pola tujuan mana yang berhasil.

### FIX — File cacat (30 menit)

Download `dmjk-broken-p04.pkt`. Tiga lokasi dengan **5 fault**: dua rute hilang atau salah arah, satu next-hop tidak terjangkau, satu mask rute keliru, dan satu interface WAN yang belum aktif.

Kumpulkan catatan per fault: gejala, uji yang Anda jalankan, akar masalah, perbaikan. Salah satu fault hanya menyebabkan kegagalan **satu arah** — pastikan Anda menguji dari kedua sisi.

### BUILD — Konektivitas penuh NusantaraNet

1. Alamat pada empat tautan WAN sesuai rencana pekan 2.
2. Loopback di ketiga router.
3. Segmentasi VLAN di Gudang (Produksi, IoT, WiFi-Gudang) dan Cabang, dengan pola yang sama seperti HQ pekan 3.
4. Static route: HQ mengenal seluruh blok Cabang dan Gudang dalam **rute ringkas**; Cabang dan Gudang memakai rute default ke HQ.
5. Rute default di HQ menuju ISP-1.
6. Bukti konektivitas: `ping` berhasil dari setiap lokasi ke setiap lokasi lain, dan `tracert` yang menunjukkan jalur sesuai dugaan Anda.
7. Diagram logis diperbarui dengan tautan WAN dan alamatnya.

Deliverable ini menjadi **Tugas 2** (2,5% dari nilai akhir).

**Tantangan wajib.** Manajemen bertanya apa yang terjadi kalau tautan WAN ke Gudang terputus. Simulasikan dengan `shutdown`, catat perubahan pada `show ip route` di kedua router, catat apa yang masih berfungsi dan apa yang tidak, lalu jawab dalam tiga kalimat: apakah lalu lintas Gudang bisa dialihkan lewat Cabang dengan topologi Anda saat ini? Kalau tidak, satu tautan tambahan mana yang akan memungkinkannya?

---

## 4.4 Checkpoint Pekan 4 (2%)

**Checkpoint 1.** `ping` berhasil dua arah antara ketiga lokasi, termasuk antara Cabang dan Gudang.

**Checkpoint 2.** Rute di R-HQ memakai bentuk ringkas, bukan satu baris per segmen; Cabang dan Gudang memakai rute default. Anda dapat menjelaskan setiap baris `show ip route`.

**Checkpoint 3.** `tracert` dari Gudang ke server HQ menampilkan jalur yang Anda prediksi lebih dulu secara lisan.

**Viva.** Contoh pertanyaan:

- "Kalau saya hapus baris ini, siapa yang kehilangan akses ke mana? Jawab dulu, baru kita coba."
- "Kenapa R-BR hanya punya satu static route?"
- "Rute ini ada di `running-config` tetapi tidak muncul di `show ip route`. Apa artinya?"
- "Ping ke internet gagal. Buktikan bahwa penyebabnya bukan routing."
- "Anda meringkas tiga segmen Gudang menjadi satu baris. Apa yang membuat itu mungkin?"

---

## Menuju pekan 5

Mulai pekan depan modul tidak lagi memberi perintah berurutan. Yang Anda terima adalah kebutuhan, tabel kosong, dan Lampiran A. Dua kebiasaan dari empat pekan ini yang akan menentukan kelancaran Anda:

Menjalankan `show` sebelum mengubah apa pun, dan mengubah satu hal pada satu waktu. Keduanya terlihat lambat dan justru menghemat waktu paling banyak — terutama pada UTS, ketika waktu 150 menit dan tidak ada yang bisa ditanyai.
