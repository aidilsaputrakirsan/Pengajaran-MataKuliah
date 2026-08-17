# MODUL DMJK — PEKAN 9–10

**SI2514011 | Proyek: NusantaraNet | Cisco Packet Tracer 8.2+**

> **Cara memakai modul ini.** Setiap pekan punya empat bagian: **Konsep** (dibaca sebelum kelas), **Prompt Pack** (alat bantu AI), **READ → BREAK → FIX → BUILD** (dikerjakan saat praktikum), dan **Checkpoint** (instrumen penilaian Praktikum, 2% per pekan).
>
> Tingkat kedua pekan ini **Partial**: modul memberi kebutuhan dan tabel kosong, perintah ada di Lampiran A tanpa urutan pengerjaan.

---
---

# PEKAN 9 — Jaringan Wireless dan Perangkat IoT

**Sub-CPMK 5:** Mahasiswa mampu menganalisis dan merancang jaringan wireless, IoT, dan konektivitas layanan cloud, serta menilai relevansi teknologi generasi lanjut (5G) dan otomasi jaringan bagi organisasi. **(C4)**

**Tingkat:** Partial

**Target akhir pekan:** Empat SSID berjalan di tiga lokasi dengan pemisahan segmen yang sama seperti jaringan kabel, dan tiga sensor di Gudang melaporkan data ke server.

---

## 9.1 Konsep

### Wireless adalah media bersama, dan itu mengubah segalanya

Pada jaringan kabel, setiap perangkat punya jalurnya sendiri ke switch. Pada wireless, semua perangkat pada satu access point berbagi satu ruang udara. Dua konsekuensi yang menentukan seluruh cara merancangnya:

**Bandwidth dibagi, bukan dialokasikan.** Access point berlabel 300 Mbps tidak memberi 300 Mbps kepada setiap klien; angka itu adalah total yang diperebutkan semua klien yang terhubung. Tiga puluh laptop pada satu AP berarti setiap laptop mendapat sepersepuluh dari yang ia harapkan pada hari sibuk.

**Hanya satu perangkat boleh memancar pada satu waktu.** Wireless bekerja setengah dupleks dan memakai CSMA/CA: perangkat menunggu ruang udara sepi sebelum mengirim. Kalau dua perangkat memancar bersamaan, keduanya harus mengulang. Semakin banyak klien, semakin banyak waktu terbuang untuk menunggu dan mengulang — bukan untuk mengirim data.

Karena itu keluhan "WiFi lambat" hampir selalu masalah jumlah klien per AP atau tumpang tindih kanal, bukan masalah bandwidth internet. Ini pembedaan yang perlu Anda bawa ke pekan 10.

### Kanal: satu keputusan yang paling sering diabaikan

Pita 2,4 GHz punya sebelas kanal di Indonesia, tetapi setiap kanal cukup lebar sehingga kanal yang bernomor berdekatan **saling tumpang tindih**. Hanya tiga yang benar-benar tidak bertumpang: **1, 6, dan 11**.

Dua AP bersebelahan pada kanal 1 dan kanal 3 akan saling mengganggu lebih parah daripada dua AP yang keduanya di kanal 1 — pada kanal yang sama mereka setidaknya bisa saling mendengar dan bergantian, sementara pada kanal yang bertumpang sebagian mereka hanya menghasilkan derau bagi satu sama lain.

| Pita | Jangkauan | Kapasitas | Kanal bebas tumpang | Cocok untuk |
|---|---|---|---|---|
| 2,4 GHz | Lebih jauh, tembus dinding | Rendah, padat | 3 (1, 6, 11) | Gudang luas, sensor IoT |
| 5 GHz | Lebih pendek | Tinggi | Banyak | Area kerja padat |

Sensor IoT hampir selalu diletakkan di 2,4 GHz: datanya kecil, dan yang dibutuhkan adalah jangkauan, bukan kecepatan.

### Cakupan versus kapasitas

Ada dua cara merancang penempatan AP, dan keduanya menghasilkan jumlah AP yang berbeda untuk ruangan yang sama:

**Desain cakupan** bertanya: berapa AP agar tidak ada titik mati? Cocok untuk gudang — sedikit orang, area luas.

**Desain kapasitas** bertanya: berapa AP agar setiap klien mendapat bandwidth yang layak? Cocok untuk kantor — banyak orang, area kecil. Biasanya menghasilkan AP lebih banyak dengan daya pancar **lebih rendah**, supaya setiap AP melayani lebih sedikit klien.

Menurunkan daya pancar terasa berlawanan dengan intuisi, tetapi itulah cara memperbesar kapasitas total: sel yang lebih kecil, lebih banyak, dan lebih sedikit klien per sel.

Untuk perpindahan klien antar-AP tanpa koneksi terputus, sel bersebelahan perlu **tumpang tindih sekitar 15–20 persen**. Tanpa tumpang tindih, pengguna yang berjalan akan kehilangan koneksi sebelum AP berikutnya menerimanya.

### SSID bukan VLAN, tetapi harus dipetakan ke VLAN

SSID adalah nama jaringan yang dilihat pengguna. VLAN adalah segmen tempat lalu lintasnya berakhir. Keduanya hal berbeda, dan pemetaan antar keduanya adalah inti keamanan wireless.

```
SSID "NusantaraNet-Staff"  -> VLAN 440 (WiFi-Karyawan)
SSID "NusantaraNet-Guest"  -> VLAN 450 (WiFi-Tamu)
SSID "NusantaraNet-Gudang" -> VLAN 475 (WiFi-Gudang)
SSID "NusantaraNet-IoT"    -> VLAN 470 (IoT)
```

Kalau dua SSID berakhir di VLAN yang sama, keduanya berada di segmen yang sama dan seluruh kebijakan pekan 6 tidak berlaku di antaranya — walaupun namanya berbeda dan passwordnya berbeda. Tamu yang terhubung ke SSID tamu tetapi masuk ke VLAN karyawan adalah kebocoran yang tidak terlihat dari sisi pengguna mana pun.

Uplink dari AP ke switch harus berupa **trunk** yang mengizinkan semua VLAN wireless, dengan alasan yang sama seperti pada pekan 3.

### Keamanan wireless: yang boleh dan tidak boleh dipakai

| Metode | Status | Catatan |
|---|---|---|
| Terbuka, tanpa password | Tidak dipakai | Bahkan untuk tamu — pakai WPA2 dengan password yang dibagikan |
| WEP | Tidak pernah | Dapat dipecahkan dalam hitungan menit |
| WPA2-PSK | Dipakai NusantaraNet | Satu password bersama per SSID |
| WPA2-Enterprise | Ideal untuk staf | Butuh server RADIUS; setiap orang punya kredensial sendiri |

Kelemahan WPA2-PSK yang harus Anda ketahui: satu password dipakai bersama, sehingga ketika seorang karyawan resign, satu-satunya cara mencabut aksesnya adalah mengganti password untuk semua orang. WPA2-Enterprise menyelesaikan ini karena kredensial bersifat per orang.

Dukungan WPA2-Enterprise di Packet Tracer bergantung pada model AP yang Anda pakai. Verifikasi dengan Prompt B sebelum merancang di sekitarnya.

### Menyembunyikan SSID bukan keamanan

Mematikan siaran SSID sering disarankan sebagai langkah pengamanan. Ia bukan: nama jaringan tetap terlihat pada lalu lintas klien yang sudah terhubung, dan alat pemindai biasa dapat menemukannya. Yang benar-benar Anda dapatkan hanyalah kesulitan bagi pengguna yang sah. Sebutkan ini kalau ditanya pada viva.

### IoT: banyak, kecil, dan tidak dapat diperbarui

Sensor suhu di Gudang mengirim beberapa byte setiap beberapa detik. Bandwidth bukan masalahnya. Yang menjadi masalah adalah tiga sifat lain:

**Jumlahnya banyak dan tumbuh cepat.** Rencana alamat harus punya ruang; segmen IoT NusantaraNet dirancang untuk `15 + X` perangkat, dan angka itu akan naik.

**Perangkatnya tidak dijaga.** Sensor menempel di dinding gudang selama lima tahun tanpa ada yang memperbarui perangkat lunaknya. Kerentanan yang ditemukan tahun depan akan tetap ada di sana.

**Perangkat itu tidak pernah butuh mengakses apa pun kecuali satu server.** Sensor suhu tidak punya alasan untuk menjangkau segmen Keuangan.

Ketiganya menghasilkan satu kesimpulan desain: **segmen IoT diisolasi lebih ketat daripada segmen tamu**, dan hanya diizinkan berbicara dengan server pengumpul datanya. Isolasi ini bukan karena IoT berbahaya, melainkan karena ia tidak dapat diperbaiki kalau ternyata berbahaya.

Di Packet Tracer, perangkat IoT mendaftarkan diri ke **IoT Registration Server** dengan akun, lalu dapat dipantau dan dikendalikan lewat interface web. Kondisi otomatis dapat diatur di server — misalnya menyalakan kipas bila suhu melewati ambang. Ini fitur bawaan, bukan simulasi yang Anda karang sendiri.

---

## 9.2 Prompt Pack — Pekan 9

### A. Prompt Perancangan Wireless

```
Saya merancang WLAN untuk tiga lokasi di Packet Tracer:
- Kantor pusat: area kerja 40 x 20 meter, sekitar 114 klien staf
- Gudang: area 60 x 40 meter, langit-langit tinggi, sekitar 47 klien
  bergerak dan 42 sensor IoT
- Cabang: 20 x 15 meter, sekitar 52 klien

JANGAN beri konfigurasi.
1. Untuk setiap lokasi, apakah desainnya digerakkan oleh CAKUPAN atau
   KAPASITAS? Jelaskan alasannya per lokasi.
2. Perkirakan jumlah AP dan sebutkan asumsi jumlah klien per AP yang
   kamu pakai.
3. Usulkan rencana kanal 2,4 GHz, dan jelaskan mengapa kanal yang kamu
   pilih tidak saling bertumpang.
4. Sebutkan satu pertanyaan yang harus saya jawab sendiri sebelum
   rencana ini bisa dipakai.
```

### B. Prompt Verifikasi Dukungan Packet Tracer

```
Untuk WLAN dan IoT di Packet Tracer 8.2, tandai
DIDUKUNG / TIDAK ADA / DISEDERHANAKAN:

- Access Point-PT dan AP-PT-A/N: pilihan keamanan apa saja yang tersedia?
- Apakah WPA2-Enterprise dengan server RADIUS berfungsi?
- Apakah beberapa SSID pada satu AP-PT bisa dipetakan ke VLAN berbeda?
- Apakah WLC (Wireless LAN Controller) tersedia, dan versi mana yang punya?
- Pengaturan kanal dan daya pancar: bisa diubah atau hanya tampilan?
- IoT Registration Server: bagaimana perangkat mendaftar, dan apakah
  perlu DNS?

Kalau ada keterbatasan yang mengubah cara saya merancang, sebutkan.
```

Jawaban pertanyaan ketiga penting sebelum Anda mulai: kalau satu AP-PT hanya mendukung satu SSID dengan satu VLAN, rancangan Anda butuh beberapa AP per lokasi, dan itu mengubah topologi.

### C. Prompt Diagnosis Berjenjang

```
Di Packet Tracer, laptop wireless saya terhubung ke SSID dan mendapat
IP address, tetapi tidak bisa ping ke gateway-nya.

JANGAN memberi solusi.
1. Kalau ia mendapat IP address, bagian mana dari jalur yang sudah
   TERBUKTI berfungsi?
2. Sebutkan 3 hipotesis yang bisa dibantah beserta uji pembedanya.
3. Sebutkan pemeriksaan mana yang paling murah dilakukan lebih dulu.
```

Butir 1 adalah inti pertanyaan ini: mendapat alamat lewat DHCP membuktikan lebih banyak hal daripada yang biasanya disadari.

### D. Prompt Terlarang

Aturan umum berlaku. Tambahan: jangan meminta AI menentukan jumlah AP tanpa Anda memberi jumlah klien dan ukuran area. Angka yang keluar akan terdengar meyakinkan dan tidak berdasar apa pun.

---

## 9.3 READ → BREAK → FIX → BUILD

### READ — Ukur perilaku media bersama (30 menit)

Pasang satu AP di HQ dengan satu SSID sederhana, hubungkan tiga laptop wireless. Tanpa AI.

1. Di Simulation Mode, kirim `ping` dari satu laptop ke gateway. Amati jalurnya: perangkat mana saja yang **menerima** frame wireless itu, walaupun bukan tujuannya?
2. Bandingkan dengan `ping` yang sama pada PC berkabel di switch. Berapa perangkat yang menerima frame di sana?
3. Pada AP, buka pengaturan kanal dan daya. Catat nilai bawaannya.
4. Tambahkan AP kedua dengan kanal yang **sama**, letakkan berdekatan. Amati apakah klien berpindah, dan ke AP mana ia terhubung.
5. Jawab: dari pengamatan nomor 1 dan 2, mengapa penyadapan lalu lintas jauh lebih mudah pada wireless? Apa yang tetap melindungi isi datanya?

Jawaban nomor 5 adalah alasan WPA2 wajib bahkan pada SSID tamu.

### BREAK — Enam percobaan (40 menit)

| # | Yang diubah | Prediksi Anda | Hasil sebenarnya |
|---|---|---|---|
| 1 | Ubah password SSID di AP, jangan ubah di klien | | |
| 2 | Petakan SSID tamu ke VLAN karyawan | | |
| 3 | Hapus VLAN wireless dari daftar trunk uplink AP | | |
| 4 | Ubah keamanan SSID menjadi terbuka, amati di Simulation Mode | | |
| 5 | Letakkan dua AP berdekatan pada kanal 1 dan kanal 3 | | |
| 6 | Matikan siaran SSID, lalu coba sambungkan klien baru | | |

Nomor 2 adalah kebocoran yang paling penting di pekan ini. Dari sudut pandang tamu, tidak ada yang tampak berubah — nama SSID sama, password sama. Buktikan kebocorannya dengan satu `ping`, dan jelaskan mengapa kebijakan ACL pekan 6 tidak menolongnya.

Nomor 6: catat apakah Anda masih bisa menyambungkan klien dengan mengetik nama SSID secara manual. Kalau bisa, apa nilai keamanan yang sebenarnya Anda dapatkan?

### FIX — File cacat (30 menit)

Download `dmjk-broken-p09.pkt`. WLAN tiga lokasi dengan **5 fault**: satu pemetaan SSID ke VLAN yang salah, satu VLAN tidak diizinkan di trunk AP, satu keamanan wireless yang tidak sesuai kebijakan, satu perangkat IoT yang tidak dapat mendaftar, dan satu kanal yang bertumpang.

Dua di antaranya **tidak menyebabkan pengguna kehilangan koneksi** — keduanya masalah keamanan dan kinerja. Menemukannya butuh pemeriksaan aktif, bukan menunggu keluhan.

### BUILD — WLAN dan IoT NusantaraNet

**Kebutuhan:**

1. **Empat SSID** dengan pemetaan VLAN sesuai Lampiran B: staf, tamu, gudang, IoT. Tidak ada dua SSID yang berakhir di VLAN yang sama.
2. **WPA2** pada seluruh SSID, termasuk tamu.
3. **Rencana kanal 2,4 GHz** untuk semua AP di ketiga lokasi, tanpa tumpang tindih antar-AP yang bersebelahan.
4. **Uplink trunk** dari setiap AP, hanya mengizinkan VLAN wireless yang dipakai.
5. **DHCP untuk klien wireless** memakai pool dan relay pekan 5.
6. **Kebijakan pekan 6 tetap berlaku**: tamu wireless tidak dapat menjangkau segmen internal; buktikan lagi setelah WLAN aktif.
7. **Tiga perangkat IoT di Gudang** — minimal satu sensor suhu dan satu aktuator — terdaftar ke IoT Registration Server, dengan satu kondisi otomatis yang berfungsi.
8. **Segmen IoT diisolasi**: hanya boleh menjangkau server pengumpul datanya, tidak boleh internet, tidak boleh segmen lain.

**Isi tabel ini di laporan:**

| SSID | VLAN | Lokasi | Pita | Kanal | Keamanan | Perkiraan klien |
|---|---|---|---|---|---|---|
| | | | | | | |

Tambahkan satu tabel rencana AP: lokasi, jumlah AP, alasan (cakupan atau kapasitas), dan asumsi klien per AP.

**Tantangan wajib (15%).** Manajemen menolak anggaran untuk AP tambahan di Gudang dan meminta satu AP saja melayani seluruh area. Tulis satu halaman: apa konsekuensi teknis yang **terukur** dari keputusan itu (kapasitas per klien, titik mati, dampak pada sensor IoT), dan satu alternatif yang lebih murah daripada menambah AP tetapi lebih baik daripada tidak melakukan apa pun. Jawaban yang hanya menyatakan "tidak disarankan" tanpa angka tidak dinilai.

---

## 9.4 Checkpoint Pekan 9 (2%)

**Checkpoint 1.** Empat SSID aktif, klien di masing-masing mendapat alamat dari segmen yang benar. Asisten memeriksa bahwa alamat yang diterima klien tamu berada di blok WiFi-Tamu Anda, bukan blok lain.

**Checkpoint 2.** Klien tamu wireless gagal menjangkau tiga segmen internal, berhasil ke internet. Tunjukkan rencana kanal Anda dan jelaskan mengapa AP yang bersebelahan tidak bertumpang.

**Checkpoint 3.** Tiga perangkat IoT terdaftar dan satu kondisi otomatis berjalan. Segmen IoT terbukti tidak dapat menjangkau segmen lain selain servernya.

**Viva.** Contoh pertanyaan:

- "SSID ini berakhir di VLAN mana? Tunjukkan buktinya, bukan pengaturannya."
- "Kenapa kanal AP ini 6 dan bukan 3?"
- "Kalau saya sembunyikan SSID, apa yang jadi lebih aman? Jawab jujur."
- "Sensor ini butuh akses ke mana saja? Tunjukkan baris yang membatasinya."
- "Satu AP untuk 114 klien. Apa yang terjadi pada jam sibuk?"

---
---

# PEKAN 10 — Gateway Internet, Redundansi, dan Konektivitas Cloud

**Sub-CPMK 5:** Mahasiswa mampu menganalisis dan merancang jaringan wireless, IoT, dan konektivitas layanan cloud, serta menilai relevansi teknologi generasi lanjut (5G) dan otomasi jaringan bagi organisasi. **(C4)**

**Tingkat:** Partial

**Target akhir pekan:** NusantaraNet tetap terhubung ke internet ketika ISP utama mati, layanan web internal dapat diakses dari luar, dan Anda dapat menjelaskan mengapa dua ISP tidak berarti bandwidth dua kali lipat.

---

## 10.1 Konsep

### Satu ISP berarti satu titik kegagalan

Seluruh jaringan yang Anda bangun sembilan pekan ini bergantung pada satu tautan. Kalau tautan itu mati: email berhenti, aplikasi cloud tidak dapat dijangkau, cabang kehilangan akses ke pusat kalau WAN-nya juga lewat ISP yang sama.

Menambahkan ISP kedua adalah jawaban yang benar, tetapi jawaban itu memunculkan pertanyaan baru yang jauh lebih menarik: **bagaimana router tahu kapan harus berpindah?**

### Failover bukan load balancing

Ini pembedaan yang paling sering salah dipahami, termasuk di modul dan tutorial yang beredar.

| | Failover | Load balancing |
|---|---|---|
| Yang terjadi | ISP-2 dipakai hanya kalau ISP-1 mati | Kedua ISP dipakai bersamaan |
| Bandwidth total | Tetap sebesar satu ISP | Mendekati jumlah keduanya |
| Kesulitan | Rendah | Tinggi |

Load balancing sungguhan pada dua ISP sulit karena tiga hal: lalu lintas keluar lewat ISP-1 tetapi balasannya bisa datang lewat ISP-2 (routing asimetris), keadaan NAT tidak dibagi antar-jalur sehingga sesi terputus saat berpindah, dan koneksi masuk hanya dapat menuju satu alamat publik.

NusantaraNet memakai **failover**, dan Anda harus dapat menjelaskan pilihan itu — bukan mengklaim melakukan load balancing.

### Floating static route

Failover pada routing statis bekerja dengan **administrative distance**. Static route biasa punya AD 1. Kalau Anda menuliskan rute kedua ke tujuan yang sama dengan AD lebih besar, router hanya memasangnya ke tabel rute kalau yang pertama hilang.

```
ip route 0.0.0.0 0.0.0.0 172.16.27.10          <- ISP-1, AD 1
ip route 0.0.0.0 0.0.0.0 172.16.27.14 10       <- ISP-2, AD 10
```

Rute kedua disebut *floating* karena ia mengapung di luar tabel rute sampai dibutuhkan. Pada `show ip route` keadaan normal, hanya satu rute default yang tampak. Ini menjebak: banyak yang menyimpulkan konfigurasinya gagal karena rute keduanya "tidak muncul". Yang membuktikan ia bekerja adalah mematikan tautan pertama dan melihat rute kedua menggantikannya.

Keterbatasan yang harus Anda ketahui: rute mengapung hanya berpindah kalau **interface-nya mati**. Kalau kabel ke ISP tetap hidup tetapi jaringan ISP itu sendiri bermasalah di hulu, router tidak tahu apa-apa dan tetap mengirim lalu lintas ke jalur yang rusak. Mengatasinya butuh mekanisme yang memeriksa keterjangkauan tujuan jauh, dan itu di luar cakupan Packet Tracer — sebutkan keterbatasan ini pada laporan Anda alih-alih berpura-pura tidak ada.

### NAT keluar dan NAT masuk

Pekan 5 Anda memakai PAT untuk lalu lintas keluar. Sekarang ada kebutuhan berlawanan arah: server web internal harus dapat diakses dari internet.

| | PAT (overload) | Static NAT / port forwarding |
|---|---|---|
| Arah | Dari dalam ke luar | Dari luar ke dalam |
| Pemetaan | Banyak privat ke satu publik | Satu publik ke satu privat |
| Dipicu oleh | Klien internal | Klien eksternal |

Static NAT memetakan seluruh alamat publik ke satu host internal. Port forwarding lebih sempit: hanya port tertentu yang diteruskan, sisanya tidak. Untuk NusantaraNet, port forwarding lebih tepat — tidak ada alasan mengekspos seluruh port sebuah server hanya karena port 80-nya perlu diakses.

Setelah port forwarding aktif, server internal Anda dapat dijangkau dari internet. Konsekuensinya: seluruh internet juga dapat mencobanya. Kebijakan ACL pekan 6 harus diperiksa ulang dengan asumsi baru bahwa penyerang berada di luar, bukan di dalam.

### DMZ: mengapa server publik tidak diletakkan bersama server internal

Server yang dapat diakses dari internet punya kemungkinan diretas yang jauh lebih tinggi daripada server yang hanya dapat diakses dari dalam. Kalau ia berada di segmen yang sama dengan server file internal, penyerang yang berhasil menembusnya langsung berada di segmen yang sama dengan data Anda.

DMZ adalah segmen terpisah untuk server yang menghadap publik, dengan aturan: dari internet ke DMZ boleh untuk port tertentu, dari DMZ ke internal **ditolak**. Nilai desainnya bukan mencegah peretasan, melainkan **membatasi kerugiannya**.

NusantaraNet tidak memiliki segmen DMZ pada spesifikasi awalnya. Menilai apakah ia perlu ditambahkan adalah bagian dari tantangan pekan ini.

### Konektivitas layanan cloud

Aplikasi yang dipakai PT Nusantara Digital berpindah ke penyedia layanan luar. Dari sudut pandang jaringan, ini mengubah tiga hal:

**Lalu lintas yang dulu internal menjadi eksternal.** Aplikasi yang dulu di ruang server sekarang diakses lewat internet. Tautan internet berubah dari fasilitas menjadi infrastruktur kritis — kalau ia mati, pekerjaan berhenti, bukan hanya browsing.

**Ketergantungan pada DNS meningkat.** Semua akses ke layanan cloud dimulai dari resolusi nama. DNS yang bermasalah kini terasa seperti seluruh sistem mati.

**Latensi menjadi ukuran yang relevan.** Aplikasi di ruang server merespons dalam satuan milidetik tunggal; layanan cloud melewati beberapa jaringan.

Di Packet Tracer, "cloud" disimulasikan sebagai server di luar router ISP. Yang dapat Anda uji sungguhan adalah keterjangkauan, jalur, dan perilakunya saat failover — bukan kinerjanya. Jangan mengarang angka kinerja; laporkan apa yang benar-benar terukur.

Perangkat IoT Gudang juga masuk ke pembahasan ini: server registrasinya dapat diletakkan di dalam atau di luar. Kalau di luar, sensor bergantung pada internet untuk melapor, dan segmen IoT yang tadinya diisolasi total sekarang butuh satu lubang keluar. Menentukan mana yang dipilih, beserta alasannya, adalah bagian dari BUILD.

---

## 10.2 Prompt Pack — Pekan 10

### A. Prompt Perancangan Redundansi

```
Jaringan saya punya satu router tepi dengan dua tautan ISP di Packet
Tracer. ISP-1 lewat 172.16.27.8/30, ISP-2 lewat 172.16.27.12/30.
Ada satu web server internal yang harus dapat diakses dari internet.

JANGAN beri konfigurasi.
1. Jelaskan perbedaan failover dan load balancing untuk kasus ini, dan
   sebutkan mana yang realistis dengan routing statis.
2. Kalau saya memakai failover, apa yang terjadi pada koneksi yang sedang
   berjalan saat perpindahan? Kenapa?
3. Web server saya dipetakan ke alamat publik ISP-1. Apa yang terjadi
   pada akses dari luar ketika ISP-1 mati? Apakah failover menolongnya?
4. Sebutkan satu hal yang TIDAK dapat dideteksi oleh floating static route.
```

Pertanyaan 3 adalah bagian yang paling sering terlewat dalam rancangan mahasiswa: failover melindungi lalu lintas keluar, tidak otomatis melindungi layanan masuk.

### B. Prompt Verifikasi Dukungan Packet Tracer

```
Tandai DIDUKUNG / TIDAK ADA / DISEDERHANAKAN di Packet Tracer 8.2 pada
router 2911:

- ip route dengan administrative distance (floating static)
- ip nat inside source static tcp <ip> <port> <ip-publik> <port>
- ip nat pool dengan beberapa alamat
- ip sla dan track untuk memeriksa keterjangkauan
- kata kunci "established" pada ACL
- show ip nat translations, show ip nat statistics, clear ip nat translation

Kalau ip sla tidak ada, sebutkan apa artinya bagi keandalan rancangan
failover saya, dan bagaimana saya harus menuliskannya di laporan.
```

### C. Prompt Diagnosis Berjenjang

```
Setelah saya mengaktifkan port forwarding di Packet Tracer, web server
internal tidak dapat diakses dari klien di luar. Dari dalam jaringan,
server dapat diakses normal. Tabel NAT menunjukkan entri statis ada.

JANGAN memberi solusi.
1. Sebutkan 4 penyebab paling mungkin, urut dari yang paling sering.
2. Untuk masing-masing, sebutkan satu perintah show atau satu uji yang
   membantahnya.
3. Sebutkan apa yang dibuktikan oleh fakta bahwa server dapat diakses
   dari dalam.
```

### D. Prompt Terlarang

Aturan umum berlaku. Tambahan: jangan meminta AI menuliskan "hasil uji bandwidth" atau angka latensi untuk topologi Packet Tracer Anda. Angka itu tidak pernah diukur dan memasukkannya ke laporan adalah pemalsuan data.

---

## 10.3 READ → BREAK → FIX → BUILD

### READ — Baca jalur keluar (30 menit)

Konfigurasikan lebih dulu ISP-1 saja beserta PAT dari pekan 5. Tanpa AI.

1. Dari PC di HQ, `tracert` ke alamat di internet. Catat setiap hop. Hop mana yang merupakan batas jaringan Anda?
2. Jalankan `show ip nat translations` saat `ping` berjalan. Catat satu baris lengkap: alamat dan port sebelum dan sesudah translasi.
3. Jalankan `ping` dari **dua** PC berbeda ke tujuan yang sama secara bersamaan. Bandingkan kedua baris di tabel translasi. Apa yang sama, apa yang berbeda? Jelaskan bagaimana router mengembalikan setiap balasan ke pengirim yang benar.
4. Jalankan `show ip route`. Catat rute default. Berapa administrative distance-nya?
5. Dari PC di Gudang, `tracert` ke internet. Bandingkan dengan nomor 1. Berapa hop tambahannya, dan mengapa?

Nomor 3 adalah inti cara kerja PAT. Kalau Anda tidak dapat menjelaskannya dari tabel translasi Anda sendiri, ulangi sebelum lanjut.

### BREAK — Tujuh percobaan (45 menit)

| # | Yang diubah | Prediksi Anda | Hasil sebenarnya |
|---|---|---|---|
| 1 | Tambah rute default kedua dengan AD 10, lihat `show ip route` | | |
| 2 | `shutdown` interface ke ISP-1, lihat `show ip route` lagi | | |
| 3 | Ping panjang ke internet, lalu matikan ISP-1 di tengahnya | | |
| 4 | Hidupkan kembali ISP-1 saat lalu lintas lewat ISP-2 | | |
| 5 | Tambah rute default kedua dengan AD **1**, bukan 10 | | |
| 6 | Aktifkan port forwarding, akses dari luar, lalu matikan ISP-1 | | |
| 7 | Hapus `ip nat outside` dari interface ISP-2, lalu lakukan failover | | |

Nomor 1 penting justru karena **tidak terjadi apa-apa** pada `show ip route`. Catat itu dan jelaskan mengapa itu perilaku yang benar.

Nomor 3 dan 4: catat berapa paket yang hilang pada masing-masing arah perpindahan. Perpindahan kembali ke ISP-1 sering lebih mengganggu daripada perpindahan keluar — jelaskan mengapa, dikaitkan dengan tabel translasi NAT.

Nomor 6 adalah pelajaran utama: layanan masuk Anda mati walaupun failover keluar berhasil. Ini konsekuensi desain yang harus dicatat, bukan bug.

### FIX — File cacat (30 menit)

Download `dmjk-broken-p10.pkt`. Jaringan dengan dua ISP dan port forwarding, berisi **6 fault**: satu administrative distance keliru sehingga failover tidak pernah terjadi, satu interface NAT tidak ditandai, satu pemetaan port forwarding salah port, satu rute balik hilang di sisi ISP, satu ACL yang memblokir lalu lintas masuk yang sah, dan satu yang menyebabkan failover berpindah tetapi tidak pernah kembali.

Uji **kedua arah** dan **kedua keadaan** (normal dan failover). Empat dari enam fault tidak terlihat sama sekali dalam keadaan normal.

### BUILD — Gateway internet NusantaraNet

**Kebutuhan:**

1. **Dua tautan ISP** dengan alamat sesuai rencana pekan 2, keduanya ditandai `ip nat outside`.
2. **Failover** dengan floating static route. Buktikan dengan mematikan ISP-1, dan buktikan lagi bahwa lalu lintas kembali setelah ISP-1 hidup.
3. **PAT** untuk seluruh klien dari ketiga lokasi, memakai alamat publik ISP-1.
4. **Port forwarding** untuk web server internal: hanya port yang dibutuhkan.
5. **Akses cloud** dari ketiga lokasi ke server simulasi di luar ISP, terbukti berfungsi pada keadaan normal maupun failover.
6. **Keputusan IoT**: tentukan apakah IoT Registration Server diletakkan di dalam atau di luar. Terapkan pilihan Anda dan sesuaikan isolasi segmen IoT. Tulis alasannya dalam tiga kalimat.
7. **Tinjau ulang ACL pekan 6** dengan asumsi penyerang berada di luar. Catat perubahan yang Anda buat.

**Isi tabel ini di laporan:**

| Layanan | Alamat publik | Port publik | Alamat internal | Port internal | Dapat diakses saat failover? |
|---|---|---|---|---|---|
| | | | | | |

Kolom terakhir adalah yang paling penting, dan untuk sebagian baris jawabannya adalah "tidak". Menuliskan "tidak" beserta alasannya bernilai lebih tinggi daripada mengklaim semuanya tetap berjalan.

**Tantangan wajib (15%).** Tulis satu halaman berisi dua bagian:

Pertama, **usulkan** apakah NusantaraNet perlu segmen DMZ, dengan alasan berbasis keadaan jaringan Anda sekarang — bukan definisi umum. Kalau ya, sebutkan blok alamat mana yang akan Anda pakai dari sisa blok Anda dan apa yang berpindah ke sana.

Kedua, sebutkan **satu kegagalan yang tidak dapat dideteksi** oleh rancangan failover Anda, jelaskan gejalanya bagi pengguna, dan sebutkan apa yang dibutuhkan untuk mengatasinya beserta alasan mengapa itu tidak dapat disimulasikan di Packet Tracer.

---

## 10.4 Checkpoint Pekan 10 (2%)

**Checkpoint 1.** Failover berfungsi dua arah. Asisten mematikan ISP-1, lalu lintas berpindah; ISP-1 dihidupkan, lalu lintas kembali. Anda menyebutkan lebih dulu berapa paket yang akan hilang.

**Checkpoint 2.** Port forwarding berfungsi dari klien luar, dan hanya untuk port yang dibutuhkan. Port lain pada alamat publik yang sama tidak terjangkau.

**Checkpoint 3.** Tabel layanan terisi termasuk kolom keadaan failover, dan Anda dapat menjelaskan setiap baris yang berisi "tidak".

**Viva.** Contoh pertanyaan:

- "Rute default kedua Anda tidak muncul di `show ip route`. Apakah konfigurasinya gagal?"
- "Kenapa AD-nya 10 dan bukan 1?"
- "Apakah ini load balancing? Kalau bukan, apa yang membuatnya bukan?"
- "Web server ini masih bisa diakses dari luar saat ISP-1 mati? Jawab dulu, baru kita coba."
- "Kegagalan macam apa yang tidak akan disadari router Anda?"

---

## Menuju pekan 11

Tiga pekan berikutnya tidak menambah teknologi baru. Pekan 11 dan 12 adalah konsolidasi: latihan soal, audit jaringan Anda sendiri, dan lab diagnosis dengan jumlah fault terbanyak sepanjang semester. Pekan 13 membahas 5G, teknologi generasi lanjut, dan otomasi jaringan sebagai kuliah teori, karena ketiganya tidak dapat disimulasikan di Packet Tracer.

Sebelum pekan 11, pastikan file NusantaraNet Anda dalam keadaan berfungsi penuh dan dokumentasinya mutakhir. Pekan 11 dimulai dengan mengaudit pekerjaan Anda sendiri, dan audit terhadap jaringan yang setengah jadi tidak menghasilkan apa pun.
