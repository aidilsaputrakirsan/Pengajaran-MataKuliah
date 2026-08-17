# MODUL DMJK — PEKAN 5–7

**SI2514011 | Proyek: NusantaraNet | Cisco Packet Tracer 8.2+**

> **Cara memakai modul ini.** Setiap pekan punya empat bagian: **Konsep** (dibaca sebelum kelas), **Prompt Pack** (alat bantu AI), **READ → BREAK → FIX → BUILD** (dikerjakan saat praktikum), dan **Checkpoint** (instrumen penilaian, 2% per pekan).
>
> Anda **boleh dan dianjurkan** memakai AI. Yang dinilai adalah apakah Anda bisa membaca, memverifikasi, dan mempertanggungjawabkan konfigurasi itu. Ingat bahwa Packet Tracer hanya mendukung sebagian perintah Cisco IOS — pakai Prompt B setiap kali AI memberi konfigurasi.

---

## Perubahan mulai pekan ini: modul tidak lagi memberi perintah berurutan

Pekan 1–4 memberi Anda konfigurasi lengkap dan berurutan. Itu wajar: Anda baru berkenalan dengan CLI. Mulai pekan 5 hal itu berhenti.

Yang modul berikan sekarang:

- **Kebutuhan** yang harus dipenuhi, dalam bahasa bisnis
- **Tabel kosong** yang harus Anda isi sendiri dengan parameter turunan X Anda
- **Kriteria sukses** yang harus dapat Anda demonstrasikan
- **Lampiran A — Command Reference**: daftar perintah yang mungkin Anda perlukan, disusun menurut topik, **tanpa nilai parameter dan tidak dalam urutan pengerjaan**

Sintaks tetap tersedia. Yang hilang adalah kemungkinan menempel blok perintah dari atas ke bawah. Kalau ini terasa sulit, kerjakan ulang bagian READ pekan 3–4.

**Aturan tabel prediksi.** Di setiap tahap BREAK ada kolom "Prediksi Anda". Isi kolom itu **sebelum** mencoba. Kolom yang dikosongkan, atau yang isinya selalu tepat sempurna untuk delapan percobaan, sama-sama akan ditanyakan asisten.

---
---

# PEKAN 5 — Layanan Jaringan: DHCP, DNS, dan NAT

**Sub-CPMK 3:** Mahasiswa mampu mengimplementasikan dan memverifikasi infrastruktur jaringan meliputi switching, routing, layanan jaringan (DHCP/DNS/NAT), dan kontrol akses. **(C3)**

**Tingkat scaffolding:** Partial

**Target akhir pekan:** Setiap komputer di seluruh lokasi NusantaraNet menyala dan langsung mendapat alamat tanpa dikonfigurasi manual, bisa membuka `www.nusantaraX.local` dengan nama, dan bisa menjangkau internet lewat satu alamat publik.

---

## 5.1 Konsep

### Tiga layanan, satu benang merah

DHCP, DNS, dan NAT terlihat seperti tiga topik terpisah. Sebenarnya ketiganya menjawab satu pertanyaan yang sama: **bagaimana membuat jaringan bisa dipakai manusia tanpa manusia itu perlu tahu apa pun tentang jaringan.**

Tanpa DHCP, setiap karyawan baru harus menunggu staf IT mengisi empat kolom di pengaturan jaringan. Tanpa DNS, mereka harus menghafal `10.27.1.70`. Tanpa NAT, jaringan berisi 400 perangkat tidak akan pernah bisa menyentuh internet karena alamat publik tidak cukup untuk semuanya.

Ketiganya juga punya sifat yang sama: **kalau salah konfigurasi, gejalanya menyesatkan.** Pengguna melaporkan "internet mati", padahal yang rusak adalah DNS dan internet sendiri baik-baik saja. Sebagian besar pekan ini sesungguhnya adalah latihan membedakan gejala dari sebab.

### DHCP: empat pesan yang harus Anda kenali

Ketika sebuah komputer menyala, ia tidak punya IP address. Ia bahkan tidak tahu siapa yang bisa memberinya. Jadi ia berteriak ke seluruh segmen:

```
Klien  --DISCOVER-->  broadcast ke 255.255.255.255
Server --OFFER----->  "pakai 10.27.0.35, gateway 10.27.0.1"
Klien  --REQUEST-->   "saya ambil yang itu"
Server --ACK------>   "sudah saya catat"
```

Empat pesan ini disingkat **DORA**. Yang perlu Anda pahami bukan singkatannya, tapi konsekuensi dari kata **broadcast** di baris pertama.

**Broadcast tidak melewati router.** Itu memang tugas router: menghentikan broadcast supaya satu segmen yang sibuk tidak membebani seluruh jaringan. Akibatnya, DISCOVER dari komputer di Gudang tidak akan pernah sampai ke server DHCP di HQ.

Ada dua jalan keluar, dan pilihan Anda punya konsekuensi:

| Pendekatan | Cara | Untung | Rugi |
|---|---|---|---|
| DHCP di setiap router | `ip dhcp pool` di router lokal | Berfungsi walau WAN mati | Konfigurasi tersebar di 3 tempat; sulit diaudit |
| DHCP terpusat + relay | Server di HQ + `ip helper-address` di setiap segmen | Satu titik kelola, satu tempat melihat semua lease | Gudang tidak dapat alamat kalau WAN mati |

NusantaraNet memakai **pendekatan kedua** — itu yang dipakai organisasi sungguhan seukuran ini, dan alasannya adalah auditabilitas: manajemen bertanya "siapa yang memakai alamat ini kemarin sore", dan pertanyaan itu hanya bisa dijawab kalau catatannya di satu tempat.

`ip helper-address` bekerja dengan mengubah broadcast menjadi unicast: router menerima DISCOVER, membungkusnya, dan mengirimkannya langsung ke alamat server DHCP. Router juga menyisipkan informasi segmen asal, sehingga server tahu harus memberi alamat dari pool yang mana.

### Yang wajib dikeluarkan dari pool

Pool DHCP `10.27.0.0/25` mencakup 126 alamat — termasuk gateway `10.27.0.1`. Kalau tidak dikecualikan, cepat atau lambat DHCP akan memberikan alamat gateway kepada sebuah laptop, dan segmen itu mati total dengan gejala yang membingungkan: sebagian orang bisa keluar, sebagian tidak, berubah-ubah setiap hari.

Yang selalu dikecualikan: gateway, alamat server, printer, access point, dan rentang statis yang Anda cadangkan. Kebiasaan yang baik: **cadangkan alamat rendah untuk perangkat statis** (misalnya `.1` – `.20`) dan biarkan DHCP memakai sisanya.

### DNS: satu-satunya layanan yang kegagalannya selalu disalahartikan

DNS menerjemahkan nama menjadi alamat. Di Packet Tracer, Server-PT punya layanan DNS bawaan tempat Anda mendaftarkan **record A**: satu nama, satu alamat.

Pola gejala berikut akan muncul lagi di pekan 7 dan di UTS:

| Gejala | Kesimpulan |
|---|---|
| `ping 10.27.1.70` berhasil, `ping www.nusantara27.local` gagal | Jaringan sehat. **DNS** yang bermasalah. |
| `nslookup` menjawab benar tetapi `ping` ke nama gagal | Resolusi sehat. Masalahnya di **routing atau ACL**. |
| Keduanya gagal, dan `ping` ke gateway juga gagal | Masalah **lapisan bawah** — jangan sentuh DNS. |
| Sebagian klien bisa, sebagian tidak, di segmen yang sama | Alamat DNS **tidak disebarkan seragam** oleh DHCP |

Baris terakhir adalah kesalahan paling umum: server DNS berfungsi sempurna, tetapi opsi `dns-server` lupa dicantumkan di pool DHCP. Klien mendapat alamat dan gateway, semua terlihat normal, dan hanya nama yang tidak jalan.

### NAT: menerjemahkan, bukan menyembunyikan

Jaringan Anda memakai `10.X.0.0/8` yang **tidak dapat dirutekan di internet**. Router tepi menerjemahkan alamat privat menjadi alamat publik saat paket keluar, dan membalikkannya saat jawaban datang.

Bentuk yang dipakai NusantaraNet adalah **PAT** (*Port Address Translation*, atau NAT overload): ratusan perangkat berbagi satu alamat publik, dibedakan oleh nomor port sumber.

```
Sebelum:  10.27.0.35:51200  →  8.8.8.8:53
Sesudah:  203.0.113.209:1024 →  8.8.8.8:53
                        ↑ router mencatat pasangan ini di tabel translasi
```

Dua hal yang harus benar dan sering salah:

**Pertama, penandaan arah.** Setiap interface harus dinyatakan `ip nat inside` atau `ip nat outside`. Router tidak bisa menduga mana yang mana. Kalau penandaan ini terlewat, router tetap merutekan paket dengan gembira — keluar dengan alamat privat — dan paket itu dibuang oleh ISP tanpa pesan apa pun. Gejalanya: `ping` ke interface ISP berhasil, `ping` ke internet gagal, `show ip route` tampak sempurna.

**Kedua, ACL yang menentukan siapa yang diterjemahkan.** Kalau ACL-nya tidak mencakup segmen Gudang, maka semua orang bisa berinternet kecuali Gudang — dan Anda akan mencari-cari kesalahan di router Gudang, padahal masalahnya di HQ.

Perintah verifikasi yang harus jadi refleks: `show ip nat translations` dan `show ip nat statistics`. Tabel translasi yang kosong padahal klien sedang mencoba mengakses internet adalah bukti langsung bahwa paket tidak pernah melewati proses NAT.

### Urutan operasi di dalam router

Ini yang menjelaskan banyak kebingungan nanti di pekan 6. Untuk paket dari dalam ke luar, router bekerja dalam urutan:

```
ACL masuk  →  keputusan routing  →  NAT (inside→outside)  →  ACL keluar
```

Dua akibat praktisnya:

1. ACL pada interface masuk melihat alamat **privat**, karena NAT belum terjadi.
2. ACL pada interface keluar melihat alamat **publik**, karena NAT sudah terjadi.

Menulis ACL dengan asumsi urutan yang salah menghasilkan aturan yang tidak pernah cocok dengan apa pun — tanpa pesan error, karena secara sintaks ia benar.

---

## 5.2 Prompt Pack — Pekan 5

### A. Prompt Eksplorasi — untuk memahami, bukan menyalin

```
Saya mahasiswa semester 3 yang sedang belajar jaringan dengan Cisco
Packet Tracer. Saya sudah bisa mengkonfigurasi VLAN dan static routing.

Jelaskan mengapa pesan DHCP DISCOVER dari komputer di satu lokasi tidak
bisa mencapai server DHCP di lokasi lain, dan apa sebenarnya yang
dilakukan "ip helper-address" pada paket tersebut.

Syarat:
- Jangan beri konfigurasi dulu. Saya ingin paham alurnya.
- Jelaskan pada level paket: apa alamat sumber dan tujuannya sebelum
  dan sesudah router memprosesnya.
- Setelah menjelaskan, ajukan 3 pertanyaan kepada saya untuk menguji
  apakah saya benar-benar paham.
```

Instruksi terakhir yang paling menentukan: menyuruh AI menguji Anda lebih berguna daripada menyuruhnya menjawab.

### B. Prompt Verifikasi Dukungan Packet Tracer — pakai setiap kali AI memberi konfigurasi

```
Konfigurasi yang kamu berikan, apakah seluruhnya didukung Cisco Packet
Tracer 8.2 pada router 2911 dan switch 2960?

Periksa satu per satu dan tandai:
- DIDUKUNG
- TIDAK ADA di Packet Tracer (sebutkan penggantinya)
- DITERIMA TAPI PERILAKUNYA DISEDERHANAKAN

Untuk yang tidak didukung, berikan cara mencapai tujuan yang sama
dengan perintah yang memang ada di Packet Tracer. Jangan berasumsi
saya memakai router sungguhan.
```

AI dilatih dari dokumentasi perangkat sungguhan; Packet Tracer hanya mengimplementasikan sebagiannya. Tanpa langkah ini, waktu praktikum habis menatap `% Invalid input detected at '^' marker`.

Jawaban AI untuk prompt ini pun tidak selalu benar — ia bisa mengklaim sesuatu tidak didukung padahal didukung. Verifikasi akhir tetap: coba di Packet Tracer.

### C. Prompt Diagnosis Berjenjang — larang AI langsung menjawab

```
Di Packet Tracer, klien di segmen Gudang saya tidak mendapat alamat
dari DHCP. Klien di segmen HQ mendapat alamat dengan normal.

JANGAN langsung memberi solusi atau konfigurasi.
Sebagai gantinya:
1. Sebutkan 4 kemungkinan penyebab, urut dari yang paling sering terjadi.
2. Untuk setiap kemungkinan, sebutkan SATU perintah show yang bisa
   membantah atau menguatkan kemungkinan itu.
3. Tanyakan kepada saya hasil perintah mana yang ingin kamu lihat lebih
   dulu untuk mempersempit kemungkinan.
```

Gejala ini akan Anda temui puluhan kali semester ini. Kalau AI selalu memperbaikinya untuk Anda, Anda tidak akan bisa memperbaikinya sendiri saat UTS.

### D. Prompt Terlarang

Jangan pernah menempelkan konfigurasi perangkat jaringan kampus yang sungguhan, password `enable` asli, atau data pribadi nyata (NIM dan nama mahasiswa lain) ke layanan AI mana pun. Kalau butuh contoh, ganti dengan nilai palsu.

---

## 5.3 READ → BREAK → FIX → BUILD

### READ — Telusuri satu lease DHCP (30 menit)

Kerjakan **tanpa AI**, pada file NusantaraNet Anda sendiri hasil pekan 4. Konfigurasikan lebih dulu DHCP untuk **satu** segmen saja (WiFi-Karyawan) langsung di router HQ, supaya ada yang bisa dibaca.

1. Nyalakan satu PC, atur ke DHCP. Setelah mendapat alamat, jalankan `ipconfig /all` dan catat **lima** nilai yang diterimanya.
2. Di router, jalankan `show ip dhcp binding`. Cocokkan barisnya dengan langkah 1. MAC address pada tabel itu milik siapa?
3. Jalankan `show ip dhcp pool`. Berapa alamat yang sudah terpakai, dan berapa yang dikecualikan? Apakah angkanya sesuai perhitungan Anda?
4. Pindah ke **Simulation Mode**. Di PC, jalankan `ipconfig /release` lalu `ipconfig /renew`. Perlambat simulasi dan amati keempat paket DORA. Catat alamat **sumber dan tujuan** setiap paket. Paket mana yang memakai `255.255.255.255`? Paket mana yang memakai `0.0.0.0` sebagai sumber, dan mengapa masuk akal bahwa klien belum punya alamat?

Tulis jawaban langkah 4 pada laporan Anda. Ini satu-satunya kesempatan melihat DORA sebagai paket, bukan sebagai singkatan.

### BREAK — Enam kerusakan (45 menit)

Lakukan satu per satu pada file Anda. **Isi kolom prediksi lebih dulu.** Setelah mencatat hasilnya, kembalikan ke keadaan semula sebelum lanjut ke nomor berikutnya.

| # | Yang dirusak | Prediksi Anda | Gejala sebenarnya | Perintah `show` yang membuktikan |
|---|---|---|---|---|
| 1 | Hapus `dns-server` dari pool DHCP, lalu `ipconfig /renew` di klien | | | |
| 2 | Hapus `default-router` dari pool DHCP | | | |
| 3 | Hapus `ip helper-address` dari satu segmen jarak jauh | | | |
| 4 | Hapus `ip nat inside` dari salah satu interface internal | | | |
| 5 | Ubah ACL NAT sehingga hanya mencakup segmen HQ | | | |
| 6 | Matikan layanan DNS di server (bukan menghapus record-nya) | | | |

Setelah selesai, jawab dua pertanyaan berikut di laporan:

- Nomor 1, 2, dan 6 menghasilkan keluhan pengguna yang bunyinya hampir sama. Perintah **apa** yang paling cepat membedakan ketiganya? Sebutkan satu perintah, bukan tiga.
- Nomor 4 dan 5 sama-sama menyebabkan sebagian orang tidak bisa berinternet. Bagaimana Anda membedakannya tanpa membuka konfigurasi router?

### FIX — File cacat (30 menit)

Download `dmjk-broken-p05.pkt` dari LMS. Isinya jaringan tiga lokasi yang sudah berjalan pada pekan 4, kemudian layanan DHCP/DNS/NAT ditambahkan **dengan lima kesalahan**.

Ada **5 fault**. Petunjuknya: satu di pool DHCP, satu di relay, satu di record DNS, dan dua di NAT. Jumlahnya disebutkan supaya Anda tahu kapan berhenti mencari — kalau baru ketemu 3, memang masih ada.

Kumpulkan file yang sudah diperbaiki beserta catatan singkat: untuk setiap fault, tulis **gejala yang Anda lihat lebih dulu**, bukan hanya perbaikannya. Fault yang diperbaiki tanpa bisa menyebutkan gejalanya dianggap ditemukan secara kebetulan dan bernilai setengah.

### BUILD — Layanan untuk seluruh NusantaraNet

Lanjutkan file Anda sendiri. Modul tidak memberikan perintahnya; lihat Lampiran A bila perlu.

**Kebutuhan yang harus dipenuhi:**

1. **DHCP terpusat.** Satu server DHCP di segmen Server HQ melayani seluruh segmen pengguna di **ketiga lokasi**. Segmen Server dan Manajemen tidak memakai DHCP.
2. **Pengecualian alamat.** Setiap pool mengecualikan gateway, server, printer, dan access point.
3. **DNS internal** dengan minimal tiga record: `www`, `erp`, dan `sensor` pada domain `nusantaraX.local`. Alamat server DNS disebarkan lewat DHCP ke semua segmen.
4. **PAT** untuk seluruh klien internal dari ketiga lokasi, memakai satu alamat publik dari blok ISP-1 Anda.
5. **Verifikasi lintas lokasi.** Klien di Gudang harus bisa: mendapat alamat DHCP, membuka `www.nusantaraX.local` dengan nama, dan menjangkau alamat internet di luar.

**Isi tabel ini di laporan Anda** (Lampiran C menyediakan versi kosongnya):

| Segmen | Pool DHCP (jaringan/mask) | Rentang dikecualikan | Gateway | Server DNS | Relay dibutuhkan? |
|---|---|---|---|---|---|
| WiFi-Karyawan | | | | | |
| WiFi-Tamu | | | | | |
| HRD | | | | | |
| Keuangan | | | | | |
| Cabang-Umum | | | | | |
| WiFi-Cabang | | | | | |
| Produksi | | | | | |
| WiFi-Gudang | | | | | |

Kolom terakhir adalah yang paling banyak salah. Pikirkan dulu: relay dibutuhkan pada segmen yang mana, dan **dipasang di perangkat mana** — di router lokasi tersebut, atau di router HQ?

**Tantangan wajib (15% nilai pekan ini).** Manajemen meminta agar setiap laptop staf yang sama selalu mendapat alamat yang sama, supaya bisa dilacak di catatan akses. Terapkan hal ini untuk **dua** perangkat tanpa mengubahnya menjadi alamat statis di sisi klien, lalu jelaskan dalam tiga kalimat mengapa cara ini tetap lebih baik daripada mengisi alamat manual di setiap laptop.

---

## 5.4 Checkpoint Pekan 5 (2%)

Diverifikasi asisten langsung di layar Anda. Tiga checkpoint biner, lalu satu viva 30 detik.

**Checkpoint 1 — DHCP lintas lokasi.** Klien di Gudang dan Cabang mendapat alamat, gateway, dan DNS yang benar lewat relay. Tunjukkan `show ip dhcp binding` di server/router pusat yang memuat klien dari ketiga lokasi.

**Checkpoint 2 — DNS berfungsi dari lokasi terjauh.** Dari PC di Gudang: `nslookup www.nusantaraX.local` menjawab benar, dan halaman web terbuka lewat nama.

**Checkpoint 3 — NAT terbukti bekerja.** `show ip nat translations` memuat entri dari **ketiga** lokasi, dan alamat publiknya sesuai blok ISP-1 Anda.

**Viva.** Satu pertanyaan acak, contohnya:

- "Tunjukkan alamat DNS yang diterima PC ini. Dari baris konfigurasi mana angka itu datang?"
- "Kalau saya hapus `ip nat outside` dari interface ini, apa yang berubah di `show ip route`? Jawab dulu, baru kita coba."
- "Segmen ini relay-nya dipasang di router mana? Kenapa bukan di yang satunya?"
- "Kenapa gateway harus dikecualikan dari pool? Apa gejalanya kalau tidak?"

---
---

# PEKAN 6 — Keamanan Jaringan dan Kontrol Akses

**Sub-CPMK 3:** Mahasiswa mampu mengimplementasikan dan memverifikasi infrastruktur jaringan meliputi switching, routing, layanan jaringan, dan kontrol akses. **(C3)**

**Tingkat scaffolding:** Partial

**Target akhir pekan:** Kebijakan akses NusantaraNet berlaku secara teknis, bukan hanya tertulis di dokumen. Tamu tidak bisa menyentuh apa pun di dalam, Keuangan dan HRD saling tertutup, dan hanya staf IT yang bisa masuk ke perangkat jaringan.

---

## 6.1 Konsep

### Kebijakan yang tidak ditegakkan bukan kebijakan

Pada pekan 3 Anda memisahkan departemen ke dalam VLAN, lalu pada pekan 4 dan 5 Anda menyambungkan semuanya kembali dengan routing dan NAT. Hasil bersihnya: **segmentasi Anda saat ini nyaris tidak memberi keamanan apa pun.** Tamu di WiFi-Tamu bisa melakukan `ping` ke server payroll HRD, karena router dengan patuh merutekannya.

Ini bukan kesalahan desain, ini urutan yang normal: sambungkan dulu, baru batasi. Pekan ini adalah bagian "baru batasi".

Satu hal yang perlu ditegaskan karena sering salah dipahami: **VLAN bukan mekanisme keamanan.** VLAN memisahkan domain broadcast. Yang menegakkan kebijakan adalah ACL pada titik tempat lalu lintas antar-VLAN dipaksa lewat, yaitu router.

### ACL: daftar yang dibaca dari atas, berhenti di kecocokan pertama

Sebuah ACL adalah daftar aturan yang diperiksa **berurutan**. Begitu satu baris cocok, keputusannya diambil dan **sisa daftar tidak dibaca**.

Dua konsekuensi yang menjadi sumber sebagian besar kesalahan mahasiswa:

**Urutan menentukan segalanya.** Aturan yang lebih spesifik harus di atas yang lebih umum. Kalau `permit ip any any` ditempatkan di baris pertama, seluruh baris di bawahnya menjadi hiasan — dan tidak ada pesan error apa pun, karena secara sintaks ia benar.

**Ada `deny ip any any` tersembunyi di akhir setiap ACL.** Baris ini tidak pernah Anda tulis dan tidak muncul di konfigurasi, tetapi selalu ada. Akibatnya, ACL yang hanya berisi satu baris `permit` untuk satu keperluan akan **memblokir semua hal lain**, termasuk hal-hal yang tidak Anda pikirkan: DHCP, DNS, balasan dari server. Gejala khasnya: Anda memasang satu aturan untuk memblokir satu hal, dan yang mati adalah segalanya.

### Arah, dan tempat memasangnya

ACL dipasang pada interface dengan arah `in` atau `out`, dan arah itu **relatif terhadap router**, bukan terhadap Anda.

```
        ┌──────── ROUTER ────────┐
klien → │ in                 out │ → server
        └────────────────────────┘
```

Kesalahan yang paling sering: memasang aturan yang benar dengan arah yang tertukar. Hasilnya ACL tidak pernah cocok dengan apa pun dan lalu lintas lewat begitu saja. Perintah `show ip access-lists` menampilkan penghitung kecocokan per baris — **penghitung yang tetap nol pada baris yang seharusnya sering cocok adalah bukti kuat bahwa arah atau penempatannya salah.** Ini alat diagnosis paling berguna pekan ini.

Aturan praktis penempatan:

| Jenis ACL | Dipasang di dekat | Alasan |
|---|---|---|
| Extended (bisa menyebut tujuan & port) | **sumber** | Buang lalu lintas terlarang sedini mungkin |
| Standard (hanya bisa menyebut sumber) | **tujuan** | Kalau dipasang dekat sumber, ia memblokir tujuan lain yang sah juga |

NusantaraNet hampir seluruhnya memakai extended ACL, karena kebijakannya menyebut layanan tertentu, bukan hanya "boleh" atau "tidak boleh".

### Isolasi tamu: kasus yang perlu dipikirkan terbalik

Kebijakan bunyinya: "WiFi-Tamu boleh internet, tidak boleh apa pun di dalam."

Refleks pertama biasanya menulis daftar `deny` untuk setiap segmen internal. Itu bekerja, tapi rapuh: begitu ada segmen baru ditambahkan tahun depan, tamu langsung bisa mengaksesnya karena tidak ada di daftar. Kebijakan yang aman **gagal ke arah tertutup**, bukan terbuka.

Cara yang benar adalah menolak seluruh ruang alamat privat sekaligus, lalu mengizinkan sisanya. Untuk itu Anda perlu betul-betul mengerti **wildcard mask**, karena satu baris harus mencakup seluruh `10.0.0.0/8` — bukan hanya blok Anda.

Pikirkan juga: tamu tetap butuh DHCP dan DNS, dan keduanya berada **di dalam**. Kebijakan "tidak boleh apa pun di dalam" versi harfiah akan membuat WiFi-Tamu tidak bisa dipakai sama sekali. Menyelesaikan ketegangan ini — mengizinkan tepat dua layanan tanpa membuka jalan lain — adalah inti latihan pekan ini.

### Membatasi akses ke perangkat itu sendiri

Sejauh ini ACL Anda mengatur lalu lintas yang **melewati** router. Lalu lintas yang **menuju** router — sesi SSH ke perangkat — diatur terpisah dengan `access-class` pada baris `vty`.

Kenapa dibedakan? Karena hilangnya akses administratif punya konsekuensi berbeda dari terblokirnya lalu lintas pengguna: kalau Anda salah, Anda mengunci diri sendiri dari perangkat. Di Packet Tracer Anda cukup klik perangkatnya. Di jaringan sungguhan, itu berarti perjalanan ke lokasi.

SSH di Cisco IOS butuh empat hal yang sering terlupa salah satu: hostname yang bukan default, domain name, kunci RSA yang dihasilkan, dan `transport input ssh` pada vty. Telnet harus dimatikan — ia mengirim password sebagai teks terang, dan itu bisa Anda buktikan sendiri di Simulation Mode.

### Port security: pertahanan di lapisan 2

ACL bekerja di lapisan 3. Ia tidak berdaya terhadap orang yang mencabut kabel pemindai barang di gudang dan menancapkan laptopnya sendiri — laptop itu akan mendapat alamat DHCP yang sah dan berada di dalam segmen Produksi.

`switchport port-security` membatasi MAC address mana yang boleh muncul di sebuah port. Tiga pilihan tindakan saat dilanggar:

| Tindakan | Yang terjadi | Cocok untuk |
|---|---|---|
| `protect` | Lalu lintas asing dibuang, tanpa catatan | Hampir tidak pernah — kegagalan sunyi |
| `restrict` | Dibuang **dan** dicatat sebagai pelanggaran | Area kerja biasa |
| `shutdown` | Port dimatikan total sampai dipulihkan manual | Area sensitif |

Pilihan `shutdown` terdengar paling aman dan memang begitu, tetapi punya biaya operasional nyata: setiap kali seorang staf memindahkan komputernya, port mati dan seseorang harus datang memulihkannya. Menentukan pilihan **beserta alasannya** adalah bagian dari nilai pekan ini; kedua jawaban bisa benar.

---

## 6.2 Prompt Pack — Pekan 6

### A. Prompt Perancangan Kebijakan — sebelum menyentuh CLI

```
Saya merancang kontrol akses untuk jaringan perusahaan tiga lokasi di
Packet Tracer. Segmennya: HRD, Keuangan, Server, WiFi-Karyawan,
WiFi-Tamu, Manajemen, Cabang-Umum, WiFi-Cabang, Produksi, IoT,
WiFi-Gudang.

Kebijakannya:
- WiFi-Tamu: boleh internet, tidak boleh apa pun di dalam
- HRD dan Keuangan tidak boleh saling mengakses
- Semua segmen internal boleh mengakses Server, hanya HTTP dan DNS
- Hanya Manajemen boleh SSH ke perangkat jaringan

JANGAN beri konfigurasi ACL.
Sebagai gantinya, buat MATRIKS kontrol akses: baris = segmen sumber,
kolom = tujuan, isi = diizinkan/ditolak/terbatas.

Lalu tunjukkan mana dari kebijakan di atas yang SALING BERTENTANGAN
atau BELUM LENGKAP, dan tanyakan kepada saya keputusan apa yang perlu
saya ambil untuk melengkapinya.
```

Keempat kebijakan di atas memang belum lengkap. Menemukan lubangnya adalah pekerjaan perancang, bukan pekerjaan AI.

### B. Prompt Verifikasi Dukungan Packet Tracer

```
Konfigurasi ACL dan port-security yang kamu berikan, apakah seluruhnya
didukung Cisco Packet Tracer 8.2 pada router 2911 dan switch 2960?

Tandai setiap perintah: DIDUKUNG / TIDAK ADA / DITERIMA TAPI
DISEDERHANAKAN.

Periksa khusus:
1. Apakah named extended ACL didukung, atau saya harus memakai ACL
   bernomor?
2. Apakah kata kunci "established" berfungsi?
3. Apakah penyuntingan ACL dengan sequence number bisa dilakukan?
4. Apakah zone-based firewall tersedia? (saya menduga tidak)

Untuk yang tidak didukung, berikan cara mencapai tujuan yang sama.
```

### C. Prompt Diagnosis Berjenjang

```
Setelah saya memasang ACL di Packet Tracer, klien di segmen tersebut
kehilangan SEMUA konektivitas — bukan hanya yang saya maksud blokir.
Bahkan DHCP tidak lagi berfungsi.

JANGAN langsung memberi konfigurasi yang benar.
Sebagai gantinya:
1. Jelaskan mengapa memasang satu aturan deny bisa mematikan segalanya.
2. Sebutkan 3 penyebab paling mungkin, urut dari yang paling sering.
3. Sebutkan satu perintah show yang memperlihatkan baris ACL mana yang
   sedang mencocoki lalu lintas, dan bagaimana cara membaca hasilnya.
4. Tanyakan kepada saya output mana yang ingin kamu lihat lebih dulu.
```

### D. Prompt Uji Tembus Kebijakan Sendiri

```
Ini matriks kontrol akses yang sudah saya terapkan:

[tempel matriks Anda]

Berperan sebagai penguji. Sebutkan 6 uji konkret yang akan membuktikan
kebijakan ini BOCOR — masing-masing dalam bentuk: dari perangkat mana,
ke alamat mana, dengan protokol apa, dan hasil apa yang membuktikan
adanya kebocoran.

Prioritaskan uji yang menyerang hal-hal yang biasanya dilupakan
perancang, bukan yang jelas-jelas sudah diblokir.
```

Pakai Prompt D sebelum checkpoint: ia menemukan lubang di pekerjaan Anda sebelum asisten menemukannya.

---

## 6.3 READ → BREAK → FIX → BUILD

### READ — Baca kebijakan yang sudah ada (25 menit)

Tanpa AI. Terapkan lebih dulu **satu** extended ACL sederhana pada segmen HRD: izinkan HTTP ke server, tolak sisanya ke segmen internal, izinkan internet.

1. Jalankan `show ip access-lists`. Untuk setiap baris, tulis dalam bahasa Indonesia apa yang diizinkan atau ditolaknya. Termasuk baris yang tidak Anda tulis — sebutkan baris tersembunyi itu.
2. Lakukan `ping` yang seharusnya berhasil, lalu `ping` yang seharusnya gagal. Jalankan `show ip access-lists` lagi. Penghitung baris mana yang naik? Apakah sesuai dugaan Anda?
3. Pindah ke Simulation Mode. Kirim satu paket ICMP yang akan ditolak. Amati di titik mana paket itu dibuang, dan apa yang dikirim router kembali ke pengirim — apakah pengirim tahu bahwa ia diblokir, atau ia hanya melihat *timeout*?
4. Jawab: bagaimana perbedaan antara "diblokir ACL" dan "tujuan tidak ada" terlihat dari sisi klien? Kalau keduanya sama, apa akibatnya bagi seseorang yang sedang mencari-cari kesalahan di pekan 7?

Langkah 3 dan 4 adalah bekal langsung untuk pekan depan.

### BREAK — Delapan percobaan (45 menit)

Isi prediksi lebih dulu. Kembalikan keadaan setelah setiap nomor.

| # | Percobaan | Prediksi Anda | Hasil sebenarnya |
|---|---|---|---|
| 1 | Pindahkan `permit ip any any` ke baris **pertama** ACL | | |
| 2 | Pasang ACL yang sama dengan arah `out`, bukan `in` | | |
| 3 | Hapus baris yang mengizinkan DNS, lalu buka web lewat nama | | |
| 4 | Pasang ACL yang hanya berisi satu baris `deny` untuk satu host | | |
| 5 | Pasang ACL pada segmen yang memakai DHCP relay, tanpa mengizinkan DHCP | | |
| 6 | Aktifkan `port-security` maksimum 1 MAC, lalu ganti PC di port itu | | |
| 7 | Ubah tindakan pelanggaran dari `restrict` ke `shutdown`, ulangi nomor 6 | | |
| 8 | Pasang `access-class` pada vty yang tidak memuat segmen Manajemen | | |

Nomor 4 adalah yang paling penting. Anda memasang satu aturan untuk memblokir satu host, dan yang terjadi adalah seluruh segmen mati. Jelaskan mengapa dalam laporan Anda, dan sebutkan baris apa yang harus ditambahkan.

Nomor 8 adalah simulasi mengunci diri sendiri. Setelah mencatat gejalanya, pulihkan lewat konsol — dan tulis dalam dua kalimat apa yang akan terjadi kalau ini terjadi pada router sungguhan di kantor cabang.

### FIX — File cacat (30 menit)

Download `dmjk-broken-p06.pkt`. Jaringan lengkap dengan kebijakan keamanan yang sudah dipasang, tetapi berisi **6 fault**.

Sebaran petunjuknya: dua kesalahan urutan baris, satu arah `in`/`out` tertukar, satu wildcard mask salah, satu layanan penting terblokir tanpa disengaja, dan satu kebijakan yang **tampak** berfungsi tetapi sebenarnya bocor.

Fault terakhir sengaja tidak menimbulkan keluhan pengguna. Anda hanya bisa menemukannya dengan **menguji secara aktif** apa yang seharusnya tidak boleh — bukan dengan menunggu ada yang rusak. Ini kebiasaan yang membedakan administrator jaringan dari penjaga jaringan.

### BUILD — Kebijakan akses NusantaraNet

**Kebutuhan yang harus dipenuhi:**

1. **WiFi-Tamu terisolasi.** Boleh internet, boleh DHCP dan DNS. Tidak boleh apa pun yang lain di dalam. Tulis dengan pendekatan yang **gagal ke arah tertutup** — segmen baru yang ditambahkan tahun depan harus otomatis terlindungi.
2. **HRD ⇎ Keuangan.** Tidak dapat saling mengakses secara langsung. Keduanya tetap dapat mengakses Server.
3. **Akses Server terbatas layanan.** Segmen internal boleh HTTP dan DNS ke Server; tidak boleh protokol lain.
4. **Manajemen saja untuk SSH.** SSH aktif di ketiga router dan seluruh switch; Telnet mati; hanya segmen Manajemen yang diizinkan.
5. **Port security** pada seluruh port akses di segmen Produksi, dengan tindakan pelanggaran yang Anda pilih **beserta alasannya**.
6. **Pengerasan dasar** di semua perangkat: `enable secret`, `service password-encryption`, banner peringatan, dan penonaktifan port yang tidak dipakai.

**Isi matriks ini di laporan Anda.** Ini deliverable utama pekan ini — lebih penting daripada konfigurasinya, karena inilah yang dibaca manajemen dan auditor:

| Dari ↓ / Ke → | HRD | Keu | Server | Produksi | IoT | Internet | Perangkat jaringan |
|---|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| HRD | — | | | | | | |
| Keuangan | | — | | | | | |
| WiFi-Karyawan | | | | | | | |
| WiFi-Tamu | | | | | | | |
| Produksi | | | | | | | |
| Cabang-Umum | | | | | | | |
| Manajemen | | | | | | | |

Isi setiap sel dengan **Penuh / Terbatas (sebutkan layanannya) / Ditolak**, dan untuk setiap sel "Terbatas" atau "Ditolak" sebutkan **baris ACL mana** yang menegakkannya. Sel tanpa rujukan baris berarti kebijakan itu hanya ada di atas kertas.

**Tantangan wajib (15%).** Uji tembus kebijakan Anda sendiri: lakukan 6 uji yang berusaha membuktikan kebijakan Anda bocor, catat hasilnya, dan perbaiki yang memang bocor. Laporkan **temuan yang Anda perbaiki**, bukan hanya klaim bahwa semuanya aman. Laporan yang menyebutkan satu kebocoran yang ditemukan sendiri dan diperbaiki bernilai lebih tinggi daripada laporan yang mengklaim nol temuan.

---

## 6.4 Checkpoint Pekan 6 (2%)

**Checkpoint 1 — Isolasi tamu terbukti.** Dari PC di WiFi-Tamu: internet berhasil, DNS berhasil, dan `ping` ke **tiga** segmen internal berbeda gagal. Tunjukkan juga baris ACL yang menolaknya, beserta penghitung kecocokan yang naik.

**Checkpoint 2 — Kebijakan antar-departemen.** HRD tidak dapat menjangkau Keuangan; keduanya dapat membuka HTTP ke Server tetapi gagal untuk protokol lain.

**Checkpoint 3 — Akses administratif.** SSH dari segmen Manajemen berhasil; SSH dari segmen lain ditolak; Telnet ditolak dari mana pun.

**Viva.** Contoh pertanyaan:

- "Tunjukkan baris konfigurasi mana yang membuat `ping` ini gagal."
- "Ada `deny` tersembunyi di ACL ini. Di mana, dan apa akibatnya untuk DHCP di segmen ini?"
- "Kalau tahun depan ditambah segmen baru, apakah tamu otomatis bisa mengaksesnya? Tunjukkan barisnya."
- "Anda memilih `restrict`, bukan `shutdown`. Kenapa?"
- "Penghitung pada baris ini nol. Menurut Anda apa artinya?"

---
---

# PEKAN 7 — Dokumentasi As-Built dan Diagnosis Sistematis

**Sub-CPMK 4:** Mahasiswa mampu mendiagnosis gangguan jaringan secara sistematis berbasis lapisan OSI dan mendokumentasikan proses serta hasil perbaikannya. **(C4)**

**Tingkat scaffolding:** Challenge

**Target akhir pekan:** Anda dapat menemukan delapan kesalahan pada jaringan yang belum pernah Anda lihat, dalam waktu terbatas, dan menjelaskan **cara** Anda menemukannya — bukan sekadar bahwa Anda menemukannya.

> Dokumentasi pekan ini tidak dinilai dari kelengkapannya, tetapi dari kegunaannya: pasangan Anda akan memakainya untuk memperbaiki jaringan Anda tanpa bertanya kepada Anda.

---

## 7.1 Konsep

### Kenapa dokumentasi diuji, bukan dinilai kelengkapannya

Dokumentasi jaringan hampir selalu dinilai dengan cara yang salah: dihitung jumlah halamannya, dicek apakah semua tabel terisi, diberi nilai tinggi kalau rapi. Padahal satu-satunya pertanyaan yang penting adalah:

> Pada pukul dua pagi, ketika jaringan mati dan orang yang membangunnya tidak dapat dihubungi, apakah dokumen ini menolong?

Karena itu pada pekan ini dokumentasi Anda **dipakai oleh orang lain**. Pasangan Anda akan merusak jaringan Anda dan mencoba memperbaikinya hanya dengan berbekal dokumentasi Anda. Yang dinilai adalah apakah dokumen itu cukup — bukan apakah ia cantik.

### As-planned versus as-built

Pekan 2 Anda membuat rencana addressing. Sekarang, setelah lima pekan konfigurasi, hampir pasti jaringan Anda **tidak lagi sama dengan rencana itu**: ada alamat yang digeser, ada port yang dipindah, ada satu server yang diberi alamat di luar rentang karena tergesa-gesa.

Dokumen *as-planned* adalah niat. Dokumen *as-built* adalah kenyataan. Yang dipakai saat gangguan adalah yang kedua, dan yang biasanya tersedia adalah yang pertama — inilah sebab mengapa banyak dokumentasi jaringan menyesatkan alih-alih menolong.

Bagian pertama pekan ini adalah **membuat dokumen as-built dengan cara membaca perangkat, bukan membaca catatan Anda sendiri.** Setiap baris tabel harus berasal dari output `show`, bukan dari ingatan. Selisih antara rencana dan kenyataan wajib dicatat, bukan dirapikan diam-diam — selisih itulah informasi paling berharga dalam dokumen Anda.

### Diagnosis bukan bakat, tapi metode

Orang yang tampak "jago troubleshooting" biasanya tidak lebih cerdas; ia hanya tidak melakukan tiga hal yang dilakukan pemula:

1. **Mengubah beberapa hal sekaligus.** Kalau tiga hal diubah dan jaringan membaik, Anda tidak tahu mana yang berpengaruh — dan dua perubahan sisanya kini menjadi utang yang tidak tercatat.
2. **Menebak lalu memperbaiki tebakan.** Menempel konfigurasi dari internet sampai ada yang berhasil kadang menyelesaikan gejala, tetapi tidak menghasilkan pengetahuan, dan sering meninggalkan kerusakan baru.
3. **Mulai dari yang paling menarik.** Pemula memeriksa ACL dan NAT lebih dulu karena di situlah bagian yang rumit. Padahal penyebab paling sering adalah hal yang paling membosankan: kabel salah, interface `shutdown`, mask keliru satu bit.

Metode yang dipakai di mata kuliah ini punya enam langkah, dan langkah ketiga adalah yang paling sering dilewati:

```
1. Gejala      — apa yang tidak bekerja, menurut siapa, sejak kapan
2. Cakupan     — siapa saja yang terdampak, dan siapa yang TIDAK
3. Hipotesis   — dugaan yang BISA DIBANTAH oleh satu uji
4. Uji         — satu perintah yang membantah atau menguatkan
5. Perbaikan   — satu perubahan, lalu verifikasi
6. Dokumentasi — akar masalah, bukan hanya perbaikannya
```

### Cakupan: langkah yang paling banyak menghemat waktu

Sebelum menyentuh satu perintah pun, isi tabel ini:

| Uji | Hasil |
|---|---|
| Klien → gateway sendiri | |
| Klien → klien lain di segmen sama | |
| Klien → segmen lain | |
| Klien → server, memakai IP address | |
| Klien → server, memakai nama | |
| Klien → internet, memakai IP address | |
| Klien lain di segmen sama → tujuan yang sama | |

Pola jawabannya langsung menunjuk lokasi masalah, jauh sebelum Anda membuka konfigurasi apa pun:

| Pola | Hampir pasti |
|---|---|
| Semua gagal, gateway pun gagal | Lapisan 1–2: kabel, VLAN port, interface mati |
| Gateway berhasil, segmen lain gagal | Routing, atau ACL |
| IP berhasil, nama gagal | DNS |
| Internal berhasil, internet gagal | NAT, atau rute default |
| Satu klien gagal, klien lain di segmen sama berhasil | Sisi klien: port akses, port-security, alamat statis keliru |

Baris terakhir adalah yang paling sering diabaikan, padahal ia menghemat waktu paling banyak: **menguji dari klien kedua di segmen yang sama** langsung memisahkan masalah satu perangkat dari masalah jaringan.

### Hipotesis yang bisa dibantah

"Sepertinya ada masalah di routing" bukan hipotesis — tidak ada satu uji pun yang bisa membuktikannya salah. Hipotesis yang bisa dipakai berbunyi seperti ini:

> "Router Gudang tidak punya rute ke `10.27.1.64/28`, sehingga paket dari Produksi ke Server dibuang di R-GD. Kalau benar, `show ip route` di R-GD tidak memuat jaringan itu, dan `traceroute` dari Produksi berhenti di hop pertama."

Perhatikan bentuknya: dugaan penyebab, akibat yang diprediksi, dan **uji yang hasilnya akan berbeda tergantung dugaan itu benar atau salah**. Hipotesis yang selalu benar apa pun hasilnya tidak berguna.

### Naik, turun, atau membelah

Tiga pendekatan menelusuri lapisan OSI, dan pilihannya bergantung pada gejala:

| Pendekatan | Cara | Pakai ketika |
|---|---|---|
| **Bottom-up** | Mulai lapisan 1, naik | Banyak orang terdampak; ada perubahan fisik baru-baru ini |
| **Top-down** | Mulai aplikasi, turun | Satu aplikasi bermasalah, sisanya normal |
| **Divide & conquer** | Mulai lapisan 3, lihat arah | Gejalanya tidak jelas — ini yang paling sering dipakai |

Untuk gangguan pada jaringan yang tidak Anda kenal — persis situasi FIX pekan ini — mulailah dari lapisan 3 dengan `ping` bertingkat, karena satu perintah itu langsung memberi tahu Anda apakah harus naik atau turun.

### Prinsip satu perubahan

Setiap perbaikan dilakukan **satu per satu**, diverifikasi, lalu dicatat. Kalau sebuah perubahan tidak memperbaiki apa pun, **kembalikan** sebelum mencoba yang lain. Konfigurasi yang penuh sisa-sisa percobaan yang tidak berhasil adalah bagaimana jaringan PT Nusantara Digital menjadi seperti di awal cerita.

---

## 7.2 Prompt Pack — Pekan 7

### A. Prompt Penyusunan Hipotesis — bukan minta jawaban

```
Saya menghadapi gangguan pada jaringan Packet Tracer yang bukan saya
yang membangunnya. Hasil pengujian cakupan saya:

- Klien Produksi → gateway sendiri: BERHASIL
- Klien Produksi → klien lain segmen sama: BERHASIL
- Klien Produksi → Server HQ (IP address): GAGAL
- Klien Produksi → Server HQ (nama): GAGAL
- Klien Produksi → internet: BERHASIL
- Klien HQ → Server HQ: BERHASIL

JANGAN memberi solusi atau konfigurasi.
Sebagai gantinya:
1. Rumuskan 3 hipotesis yang BISA DIBANTAH, masing-masing dalam bentuk
   "penyebab → akibat yang diprediksi → uji yang membedakan".
2. Urutkan dari yang paling murah diuji.
3. Sebutkan satu hipotesis yang mungkin saya lewatkan karena terlalu
   membosankan untuk dipikirkan.
```

Butir 3 sengaja diminta. Penyebab yang membosankan adalah penyebab yang paling sering.

### B. Prompt Verifikasi Dukungan Packet Tracer

```
Untuk mendiagnosis masalah di Packet Tracer 8.2, perintah diagnosis apa
saja yang benar-benar tersedia pada router 2911 dan switch 2960?

Tandai DIDUKUNG / TIDAK ADA / DISEDERHANAKAN untuk masing-masing:
show ip route, show ip interface brief, show vlan brief,
show interfaces trunk, show ip access-lists, show ip nat translations,
show ip dhcp binding, show mac address-table, show port-security,
show cdp neighbors, traceroute, debug ip packet, terminal monitor

Untuk yang tidak tersedia, sebutkan apa yang bisa saya pakai sebagai
gantinya di Packet Tracer — termasuk fitur GUI seperti Simulation Mode.
```

AI cenderung menyarankan perintah `debug` yang tidak berperilaku sama di Packet Tracer.

### C. Prompt Uji Dokumentasi Sendiri

```
Berikut dokumentasi as-built jaringan saya:

[tempel tabel addressing, matriks akses, dan daftar perangkat Anda]

Berperan sebagai teknisi yang dipanggil pukul dua pagi dan belum pernah
melihat jaringan ini.

1. Sebutkan 5 pertanyaan yang TIDAK bisa saya jawab dengan dokumen ini.
2. Sebutkan informasi mana yang ada tetapi tidak berguna saat gangguan.
3. Kalau hanya boleh menambah SATU tabel, tabel apa yang paling
   menolong, dan kenapa?
```

### D. Prompt Terlarang

Selain aturan umum: **jangan menempelkan seluruh `show running-config` ke AI lalu meminta "cari yang salah".** Bukan karena dilarang secara teknis, tetapi karena itu melewati keseluruhan keterampilan yang dinilai pekan ini — dan asisten akan menanyakan cakupan pengujian Anda, bukan daftar perbaikannya. Mahasiswa yang menempuh jalan itu biasanya bisa menyebutkan delapan perbaikan tetapi tidak satu pun gejalanya.

---

## 7.3 READ → BREAK → FIX → BUILD

### READ — Membangun dokumen as-built dari perangkat (40 menit)

Tanpa AI. Bekerjalah dari file NusantaraNet Anda sendiri, dan **jangan buka catatan pekan 2–6.** Seluruh isi tabel harus berasal dari output perintah.

1. Untuk setiap perangkat, jalankan `show ip interface brief`, `show vlan brief` (switch), dan `show ip route` (router). Susun **tabel addressing as-built**.
2. Bandingkan dengan rencana pekan 2 Anda. Catat setiap selisih di tabel terpisah: apa yang berbeda, dan mengapa jadi berbeda. **Jangan diperbaiki dulu.**
3. Jalankan `show cdp neighbors` di setiap perangkat. Susun **peta koneksi fisik**: perangkat, port lokal, perangkat seberang, port seberang.
4. Jawab: adakah perangkat yang tidak muncul di `show cdp neighbors` mana pun? Kalau ada, apa artinya?

Laporan "tidak ada selisih" untuk lima pekan konfigurasi akan diminta dibuktikan baris per baris.

### BREAK — Merusak jaringan pasangan Anda (40 menit)

Bagian ini menggantikan tabel percobaan pekan-pekan sebelumnya, dan dikerjakan berpasangan.

**Babak 1 (15 menit).** Tukar file `.pkt` dengan pasangan Anda. Pada file **milik pasangan**, tanamkan **3 fault**:

- satu di lapisan 1–2 (port, VLAN, trunk)
- satu di lapisan 3 (rute, mask, gateway)
- satu di layanan atau ACL

Aturannya: fault harus **menimbulkan gejala yang dapat diamati**, dan **tidak boleh** berupa penghapusan seluruh blok konfigurasi. Fault yang baik itu satu baris, satu angka, atau satu kata yang salah. Catat ketiganya di kertas terpisah — jangan diberikan.

**Babak 2 (25 menit).** Terima kembali file Anda yang sudah dirusak, beserta **dokumentasi as-built Anda sendiri** dari tahap READ. Temukan dan perbaiki ketiganya. Ukur waktunya.

Setelah selesai, jawab bersama pasangan:

- Fault mana yang paling lama ditemukan? Apakah karena tersembunyi, atau karena Anda mencari di tempat yang salah?
- Informasi apa yang **tidak ada** di dokumentasi Anda tetapi Anda butuhkan? Tambahkan sekarang.
- Adakah fault yang ditemukan secara kebetulan? Kalau ya, uji apa yang **seharusnya** menemukannya secara sistematis?

Pertanyaan kedua adalah inti latihan ini: dokumentasi Anda baru saja diuji oleh orang lain.

### FIX — File cacat besar (35 menit)

Download `dmjk-broken-p07.pkt`. Ini jaringan tiga lokasi yang lengkap — VLAN, routing, DHCP, DNS, NAT, ACL — dan berisi **8 fault** yang tersebar di seluruh lapisan.

File ini **tidak** memakai parameter NIM Anda; ia jaringan orang lain, sebagaimana yang akan Anda hadapi di dunia kerja. Anda diberi dokumentasi as-built yang menyertainya, tetapi **dokumentasi itu sengaja dibuat tidak sepenuhnya akurat** — persis seperti dokumentasi jaringan sungguhan.

Kumpulkan **Fault Report** memakai template Lampiran D. Untuk setiap fault:

| Kolom | Isi |
|---|---|
| Gejala | Apa yang tidak bekerja, dari sudut pandang pengguna |
| Cakupan | Siapa terdampak, siapa tidak |
| Hipotesis | Dugaan + uji yang membedakan |
| Uji | Perintah yang dijalankan + output yang relevan |
| Akar masalah | Baris konfigurasi yang salah, dan mengapa ia menimbulkan gejala itu |
| Perbaikan | Perubahan yang dilakukan |
| Verifikasi | Bukti bahwa gejala hilang **dan** tidak ada yang rusak baru |

**Penilaian bagian ini tidak berdasarkan jumlah fault yang ditemukan.** Enam fault dengan penalaran yang terdokumentasi bernilai lebih tinggi daripada delapan fault yang "ketemu saja". Kolom Hipotesis yang kosong membuat fault tersebut dihitung setengah.

Satu di antara delapan fault **tidak menimbulkan keluhan pengguna sama sekali** — ia adalah kebocoran kebijakan keamanan. Menemukannya hanya mungkin dengan menguji apa yang seharusnya dilarang.

### BUILD — Dokumentasi yang lulus uji

Perbaiki dokumentasi as-built NusantaraNet Anda berdasarkan apa yang baru Anda pelajari dari BREAK dan FIX. Deliverable pekan ini:

1. **Diagram topologi fisik** — perangkat, port, media. Diberi label sesuai penamaan Lampiran B.
2. **Diagram topologi logis** — segmen, VLAN, subnet, titik kontrol akses.
3. **Tabel addressing as-built** — dari output perangkat, bukan dari rencana.
4. **Tabel selisih rencana vs kenyataan** — beserta keputusan: diperbaiki, atau diterima (dengan alasan).
5. **Peta koneksi fisik** dari `show cdp neighbors`.
6. **Matriks kontrol akses** pekan 6, diperbarui dengan rujukan baris ACL.
7. **Tiga prosedur recovery**, masing-masing maksimal satu halaman:
   - WAN ke Cabang mati
   - Server DHCP tidak merespons
   - Seluruh segmen kehilangan internet

Prosedur recovery ditulis dengan asumsi pembacanya **bukan Anda** dan sedang panik. Urutan langkah, perintah yang tepat untuk dijalankan, dan kondisi kapan harus berhenti dan meminta bantuan.

**Tantangan wajib (15%).** Berikan ketiga prosedur recovery Anda kepada pasangan. Ia menjalankan salah satunya pada jaringan Anda yang sudah dirusak sesuai skenario tersebut, **tanpa bertanya kepada Anda**. Catat di titik mana ia berhenti atau salah arah, lalu perbaiki prosedurnya. Laporkan versi sebelum dan sesudah, beserta apa yang membuat versi pertama gagal.

---

## 7.4 Checkpoint Pekan 7 (2%)

**Checkpoint 1 — Dokumen as-built cocok dengan kenyataan.** Asisten memilih **tiga baris acak** dari tabel addressing Anda dan memverifikasinya langsung pada perangkat. Ketiganya harus cocok. Satu ketidakcocokan yang **sudah tercatat** di tabel selisih tetap dihitung lulus; yang tidak tercatat tidak.

**Checkpoint 2 — Fault Report lengkap.** Minimal 6 dari 8 fault dilaporkan dengan kolom Gejala, Hipotesis, dan Akar Masalah terisi. Asisten memilih satu fault dan meminta Anda menceritakan urutan penemuannya.

**Checkpoint 3 — Prosedur recovery teruji.** Tunjukkan versi sebelum dan sesudah, dan sebutkan apa yang membuat pasangan Anda tersesat pada versi pertama.

**Viva.** Contoh pertanyaan:

- "Fault mana yang pertama Anda temukan? Kenapa yang itu lebih dulu?"
- "Uji apa yang membedakan masalah DNS dari masalah routing? Jalankan sekarang."
- "Satu fault tidak menimbulkan keluhan. Bagaimana Anda menemukannya?"
- "Klien ini gagal, klien di sebelahnya berhasil. Apa yang langsung tersingkir dari daftar dugaan Anda?"
- "Tunjukkan satu selisih antara rencana pekan 2 dan kenyataan hari ini. Kenapa Anda putuskan untuk menerimanya?"

---

## Menuju UTS (Pekan 8)

UTS adalah ujian praktik individual 150 menit yang mengevaluasi Sub-CPMK 1–4, dengan parameter NIM yang **berbeda** dari proyek Anda — jadi tidak ada bagian yang bisa disalin dari file sendiri.

Yang menentukan hasil Anda bukan hafalan perintah, melainkan tiga hal yang justru dilatih pada pekan 5–7:

- kemampuan menghitung VLSM dengan cepat dan benar dari kebutuhan host
- refleks memakai `show` yang tepat, bukan mengubah konfigurasi secara spekulatif
- disiplin satu perubahan pada satu waktu ketika ada yang tidak jalan

Bagian yang paling sering menghabiskan waktu peserta ujian adalah mencari kesalahannya sendiri di akhir. Latih tahap READ pekan 7 — pengujian cakupan tujuh baris itu — sampai ia menjadi kebiasaan yang bisa Anda jalankan dalam dua menit.
