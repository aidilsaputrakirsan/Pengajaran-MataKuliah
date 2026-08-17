# LAMPIRAN E — Bank Pertanyaan Viva

**Dibagikan terbuka kepada mahasiswa.**

> Pertanyaan ini tidak dirahasiakan, karena tidak satu pun dapat dijawab dengan menghafal. Semuanya menunjuk ke layar mahasiswa dan menanyakan jaringan miliknya sendiri.
>
> **Cara pakai bagi asisten.** Pada checkpoint terakhir setiap sesi, ambil satu pertanyaan secara acak. Batas waktu 30 detik. Yang dinilai bukan kelancaran, melainkan apakah mahasiswa dapat menunjuk bukti di layar. Jawaban "saya tidak tahu, tetapi saya akan memeriksanya dengan perintah ini" lebih bernilai daripada tebakan yang terdengar meyakinkan.
>
> Untuk pertanyaan bertanda **(P)**, minta mahasiswa **memprediksi lebih dulu**, baru mencobanya bersama. Prediksi yang salah tetapi disertai alasan tetap mendapat nilai; menolak memprediksi tidak.

---

## Pekan 1 — Fundamental dan Analisis Paket

1. Tunjukkan pada tabel PDU Anda: MAC tujuan paket ini milik siapa? Kenapa bukan milik tujuan akhirnya?
2. Pada baris ARP request, kenapa MAC tujuan berisi `FFFF.FFFF.FFFF`?
3. **(P)** Kalau saya hapus cache ARP sekarang, apa yang berubah pada ping berikutnya?
4. Perangkat ini tidak muncul di `show mac address-table`. Apa artinya?
5. Berapa baris di tabel MAC switch Anda, dan kenapa jumlahnya begitu?
6. IP address berubah atau tetap sepanjang perjalanan paket ke segmen lain? Bagaimana dengan MAC?
7. **(P)** Kalau saya ganti switch ini dengan hub, apa yang berubah pada percobaan tadi?
8. Tunjukkan bagaimana Anda membuat tabel MAC hanya berisi satu baris padahal kedua PC menyala.

## Pekan 2 — Addressing

1. Kenapa segmen ini Anda beri `/26` dan bukan `/25`?
2. Berapa alamat yang tersisa di blok Gudang Anda? Tunjukkan perhitungannya.
3. Alamat `10.X.1.63` pada jaringan Anda: host, alamat jaringan, atau broadcast? Di segmen mana?
4. **(P)** Kalau HRD tumbuh dua kali lipat tahun depan, apa yang Anda ubah?
5. Kenapa tautan antar-router `/30` dan bukan `/24`?
6. Kenapa alokasi VLSM harus dari kebutuhan terbesar lebih dulu?
7. Kenapa `ipv6 unicast-routing` dibutuhkan padahal IPv6 address sudah terpasang?
8. Manajemen Anda beri `/29` untuk 6 host, tanpa ruang tumbuh. Kenapa itu pilihan yang Anda ambil?

## Pekan 3 — Switching dan VLAN

1. VLAN ID Anda 420. Dari mana angka itu?
2. Tunjukkan baris yang membuat PC ini bisa mencapai segmen Server.
3. **(P)** Kalau saya hapus satu VLAN dari daftar trunk, siapa yang terdampak dan siapa yang tidak?
4. `interface vlan 499` ini untuk apa? Apa akibatnya kalau saya matikan?
5. Apakah VLAN membuat segmen Keuangan Anda aman dari HRD sekarang? Buktikan.
6. Di titik mana tag 802.1Q ditambahkan, dan di titik mana ia dibuang?
7. **(P)** Kalau `encapsulation dot1Q` pada sub-interface ini saya ubah satu angka, apa gejalanya, dan perintah apa yang menunjukkannya?
8. Ada port yang masih di VLAN 1. Kenapa itu masalah?

## Pekan 4 — Routing Statis

1. **(P)** Kalau saya hapus baris rute ini, siapa yang kehilangan akses ke mana?
2. Kenapa router cabang Anda hanya punya satu static route?
3. Rute ini ada di `running-config` tetapi tidak muncul di `show ip route`. Apa artinya?
4. Ping ke internet gagal. Buktikan bahwa penyebabnya bukan routing.
5. Anda meringkas tiga segmen Gudang menjadi satu baris rute. Apa yang membuat itu mungkin?
6. Untuk apa rute `/32` ke loopback? Apa yang tidak berfungsi tanpanya?
7. **(P)** Kalau tautan WAN ke Cabang saya matikan, baris apa yang hilang dari `show ip route` di kedua router?
8. Komunikasi dua arah butuh berapa rute? Tunjukkan keduanya.

## Pekan 5 — DHCP, DNS, NAT

1. Tunjukkan alamat DNS yang diterima PC ini. Dari baris konfigurasi mana angka itu datang?
2. **(P)** Kalau saya hapus `ip nat outside` dari interface ini, apa yang berubah di `show ip route`?
3. Segmen ini relay-nya dipasang di router mana? Kenapa bukan di yang satunya?
4. Kenapa gateway harus dikecualikan dari pool? Apa gejalanya kalau tidak?
5. Tunjukkan satu baris `show ip nat translations` dan jelaskan setiap kolomnya.
6. Ping ke IP berhasil, ping ke nama gagal. Perintah apa yang paling cepat memastikan penyebabnya?
7. Dua klien mengakses server yang sama. Apa yang membedakan kedua baris translasi mereka?
8. **(P)** Kalau ACL NAT saya ubah sehingga hanya mencakup HQ, siapa yang kehilangan internet?

## Pekan 6 — Keamanan dan Kontrol Akses

1. Tunjukkan baris konfigurasi mana yang membuat ping ini gagal.
2. Ada `deny` tersembunyi di ACL ini. Di mana, dan apa akibatnya untuk DHCP di segmen ini?
3. **(P)** Kalau tahun depan ditambah segmen baru, apakah tamu otomatis bisa mengaksesnya? Tunjukkan barisnya.
4. Anda memilih `restrict`, bukan `shutdown`. Kenapa?
5. Penghitung pada baris ini nol. Menurut Anda apa artinya?
6. Kenapa ACL untuk vty memakai `access-class`, bukan `ip access-group`?
7. Tamu butuh DHCP dan DNS yang keduanya ada di dalam. Bagaimana Anda mengizinkan tepat dua itu tanpa membuka jalan lain?
8. **(P)** Kalau saya tukar arah ACL ini dari `in` ke `out`, apa yang terjadi?

## Pekan 7 — Dokumentasi dan Diagnosis

1. Fault mana yang pertama Anda temukan? Kenapa yang itu lebih dulu?
2. Uji apa yang membedakan masalah DNS dari masalah routing? Jalankan sekarang.
3. Satu fault tidak menimbulkan keluhan pengguna. Bagaimana Anda menemukannya?
4. Klien ini gagal, klien di sebelahnya berhasil. Apa yang langsung tersingkir dari daftar dugaan Anda?
5. Tunjukkan satu selisih antara rencana pekan 2 dan kenyataan hari ini. Kenapa Anda putuskan menerimanya?
6. Saya ambil tiga baris acak dari tabel as-built Anda. Buktikan ketiganya di perangkat.
7. Prosedur recovery Anda membuat pasangan tersesat di langkah mana? Apa yang Anda ubah?
8. Hipotesis Anda untuk fault ini apa, dan uji apa yang bisa membantahnya?

## Pekan 9 — Wireless dan IoT

1. SSID ini berakhir di VLAN mana? Tunjukkan buktinya, bukan pengaturannya.
2. Kenapa kanal AP ini 6 dan bukan 3?
3. Kalau saya sembunyikan SSID, apa yang jadi lebih aman? Jawab jujur.
4. Sensor ini butuh akses ke mana saja? Tunjukkan baris yang membatasinya.
5. **(P)** Satu AP untuk seluruh klien staf Anda. Apa yang terjadi pada jam sibuk?
6. Kenapa segmen IoT Anda dibatasi lebih ketat daripada segmen tamu?
7. Desain wireless Gudang Anda digerakkan cakupan atau kapasitas? Kenapa?
8. **(P)** Kalau SSID tamu saya petakan ke VLAN karyawan, apa yang berubah dari sisi pengguna tamu?

## Pekan 10 — Gateway dan Redundansi

1. Rute default kedua Anda tidak muncul di `show ip route`. Apakah konfigurasinya gagal?
2. Kenapa administrative distance-nya 10 dan bukan 1?
3. Apakah ini load balancing? Kalau bukan, apa yang membuatnya bukan?
4. **(P)** Web server ini masih bisa diakses dari luar saat ISP-1 mati?
5. Kegagalan macam apa yang tidak akan disadari router Anda?
6. Kenapa Anda pakai port forwarding dan bukan static NAT seluruh alamat?
7. Perpindahan kembali ke ISP-1 lebih mengganggu daripada perpindahan keluar. Kenapa?
8. IoT Registration Server Anda di dalam atau di luar? Apa konsekuensinya bagi isolasi segmen IoT?

## Pekan 11 — Konsolidasi Addressing

1. Tunjukkan satu temuan audit Anda dan jelaskan bagaimana Anda menemukannya.
2. Fault mana yang paling lama? Anda mencari di mana lebih dulu, dan kenapa itu keliru?
3. Gejalanya berubah-ubah. Apa yang langsung Anda curigai?
4. Butir 8 audit Anda lulus. Berapa alamat yang terbuang di segmen terbesar Anda?
5. Keluhan berbunyi "kadang bisa kadang tidak". Apa yang Anda periksa lebih dulu?
6. Satu VLAN bermasalah, VLAN lain normal. Apa yang tersingkir dari daftar dugaan?
7. Semua VLAN mati sekaligus. Apa yang Anda periksa lebih dulu?
8. Ada temuan audit yang Anda putuskan tidak diperbaiki. Kenapa?

## Pekan 12 — Konsolidasi Layanan dan Kontrol Akses

1. Sebutkan pola gejala yang membedakan masalah NAT dari masalah rute default.
2. Penghitung baris ini nol. Wajar atau temuan?
3. Fault yang satu arah itu, apa yang membuat Anda menguji dari sisi sebaliknya?
4. Kebocoran kebijakan tidak menimbulkan keluhan. Uji apa yang menemukannya?
5. Tunjukkan rute yang ada di `running-config` tetapi tidak di `show ip route`. Apa artinya?
6. ACL keluar pada interface ISP melihat alamat privat atau publik? Apa akibatnya bagi cara Anda menulisnya?
7. Anda memasang satu baris `deny` dan seluruh segmen mati. Kenapa?
8. **(P)** Kalau `ip helper-address` di segmen ini saya hapus, siapa yang terdampak dan kapan mereka menyadarinya?

---

## Pertanyaan Lintas Pekan

Dipakai bila mahasiswa terlalu cepat menjawab pertanyaan pekan berjalan, atau pada UAS.

1. Sebutkan satu keputusan desain Anda pada pekan 2 yang menyulitkan Anda pada pekan 6. Apa yang akan Anda ubah?
2. Bagian mana dari jaringan Anda yang paling rapuh, dan apa yang akan Anda perbaiki lebih dulu kalau ada waktu satu jam?
3. Perintah ini Anda dapat dari AI. Bagaimana Anda tahu ia didukung Packet Tracer?
4. Sebutkan satu hal yang AI beri tahu Anda dan ternyata salah. Bagaimana Anda menemukannya?
5. Satu segmen baru harus ditambahkan besok. Sebutkan lima hal yang harus Anda ubah dan di perangkat mana.
6. Manajemen bertanya siapa yang memakai alamat ini kemarin. Bisa Anda jawab? Dari mana?
7. Sebutkan urutan pemeriksaan Anda kalau seseorang melapor "internet mati" tanpa keterangan lain.
8. Jaringan ini akan Anda serahkan kepada orang lain besok. Apa yang paling mungkin ia salah pahami?
