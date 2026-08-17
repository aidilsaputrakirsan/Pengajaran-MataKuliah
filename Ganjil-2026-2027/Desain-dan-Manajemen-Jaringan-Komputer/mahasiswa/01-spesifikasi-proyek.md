# SPESIFIKASI PROYEK — "NusantaraNet"

**SI2514011 | Desain dan Manajemen Jaringan Komputer | Cisco Packet Tracer 8.2+**

> Dokumen ini adalah acuan tunggal untuk seluruh praktikum pekan 2–15. Setiap pekan menambahkan satu lapisan ke jaringan yang sama — bukan memulai topologi baru. Di akhir semester Anda memiliki **satu** file Packet Tracer yang tumbuh dari sebuah rencana addressing menjadi jaringan enterprise multi-lokasi lengkap.
>
> Setiap mahasiswa mengerjakan jaringan dengan **parameter berbeda**, diturunkan dari angka `X` — nomor urut Anda pada daftar peserta. Tidak ada dua mahasiswa dengan IP address yang sama. Lihat Lampiran B.

---

### Glosarium — satu istilah untuk satu hal

Beberapa istilah di bawah sering dipakai bergantian untuk hal yang sama, dan itu menyulitkan saat asistensi. Di mata kuliah ini setiap istilah punya satu arti:

| Istilah | Artinya | Bukan |
|---|---|---|
| **Segmen** | Satu subnet IP + satu VLAN | "jaringan", "departemen" |
| **Lokasi** | Satu tempat fisik: HQ, Cabang, atau Gudang | "site", "kantor" |
| **Perangkat tepi** | Router yang menghadap ISP | "gateway", "edge" |
| **Uplink** | Tautan trunk dari switch ke router | "backbone" |
| **Fault** | Satu kesalahan konfigurasi yang menyebabkan gejala | "error", "bug" |
| **X** | Nomor urut Anda pada daftar peserta (01–40) | dua digit NIM |

---

## 1. Latar Belakang

**PT Nusantara Digital** adalah perusahaan distribusi yang bertumbuh cepat di Kalimantan Timur. Jaringannya tumbuh tanpa perencanaan: satu subnet `192.168.1.0/24` datar untuk semua orang, switch tak dikelola bertumpuk di bawah meja, tidak ada dokumentasi, dan satu-satunya orang yang paham konfigurasinya sudah resign.

Gejala yang dilaporkan manajemen:

- Staf gudang bisa membuka file payroll HRD dari komputer mana pun.
- Jaringan melambat drastis setiap pagi tanpa sebab yang diketahui.
- Ketika koneksi internet mati, tidak ada yang tahu bagian mana yang rusak.
- Cabang Samarinda mengakses aplikasi pusat lewat internet publik tanpa perlindungan.

Anda diminta merancang ulang jaringannya dari nol. Ini bukan proyek tambal-sulam.

---

## 2. Lokasi dan Kebutuhan

### 2.1 Kantor Pusat (HQ) — Balikpapan

| Segmen | Peruntukan | Kebutuhan host | Catatan |
|---|---|---:|---|
| HRD | Data karyawan, payroll | **25 + X** | Data rahasia |
| Keuangan | Akuntansi, faktur | **12 + X** | Data sangat rahasia |
| Server | Web, DNS, file internal | **8 + (X mod 5)** | Diakses semua lokasi |
| WiFi-Karyawan | Laptop & ponsel staf | **60 + 2X** | Segmen terbesar |
| WiFi-Tamu | Pengunjung, vendor | **30 + X** | Wajib terisolasi total |
| Manajemen | Akses admin perangkat | **6** | Hanya untuk staf IT |

### 2.2 Cabang — Samarinda

| Segmen | Peruntukan | Kebutuhan host |
|---|---|---:|
| Cabang-Umum | Staf penjualan & administrasi | **30 + X** |
| WiFi-Cabang | Laptop staf cabang | **25 + X** |

### 2.3 Gudang — Balikpapan Utara

| Segmen | Peruntukan | Kebutuhan host |
|---|---|---:|
| Produksi | Terminal pemindai barang | **40 + X** |
| IoT | Sensor suhu & kelembapan (mulai pekan 9) | **15 + X** |
| WiFi-Gudang | Perangkat bergerak | **20 + X** |

Karena kebutuhan host bergantung pada X, **panjang prefix setiap segmen berbeda antar mahasiswa**. Tidak ada satu jawaban `/24` yang benar untuk semua orang. Inilah yang membuat VLSM di pekan 2 menjadi latihan sungguhan, bukan penyalinan.

---

## 3. Blok Alamat yang Diberikan

| Peruntukan | Blok | Sumber |
|---|---|---|
| HQ | `10.X.0.0/20` | Privat RFC 1918 |
| Cabang | `10.X.16.0/22` | Privat RFC 1918 |
| Gudang | `10.X.20.0/22` | Privat RFC 1918 |
| Tautan WAN | `172.16.X.0/24` (dibagi /30) | Privat RFC 1918 |
| Loopback router | `172.31.X.0/24` (dibagi /32) | Privat RFC 1918 |
| IP publik ISP-1 | `203.0.113.X` | RFC 5737 (dokumentasi) |
| IP publik ISP-2 | `198.51.100.X` | RFC 5737 (dokumentasi) |
| IPv6 | `2001:db8:X::/48` | RFC 3849 (dokumentasi) |

Alamat publik diambil dari blok yang **dicadangkan RFC untuk dokumentasi**, bukan alamat sungguhan. Ini praktik yang benar saat menulis dokumen desain jaringan: jangan pernah memakai IP publik milik organisasi lain sebagai contoh. Sebutkan alasan ini saat viva jika ditanya.

Aturan lengkap penurunan parameter dari X, beserta contoh terisi penuh, ada di **Lampiran B**.

---

## 4. Target Akhir Semester

File `.pkt` Anda pada pekan 15 harus memenuhi seluruh butir berikut. Daftar ini tidak berubah sepanjang semester, jadi Anda bisa memakainya sebagai peta.

**Lapisan 2 — Switching**
- Semua segmen terpisah sebagai VLAN dengan ID turunan NIM
- Uplink trunk yang hanya mengizinkan VLAN yang memang dibutuhkan
- VLAN Manajemen terpisah dan tidak dipakai sebagai VLAN data
- Port akses tidak pernah dibiarkan pada VLAN 1

**Lapisan 3 — Routing**
- Inter-VLAN routing di HQ dan Gudang
- Konektivitas penuh HQ ↔ Cabang ↔ Gudang lewat tautan WAN
- Static routing dengan rute ringkas (bukan satu rute per subnet bila bisa diringkas)
- Rute default menuju ISP-1, dengan backup ke ISP-2

**Layanan**
- DHCP untuk semua segmen pengguna; server DHCP terpusat di HQ dengan `ip helper-address`
- Alamat statis untuk server, router, switch, dan printer
- DNS internal: `www.nusantaraX.local`, `erp.nusantaraX.local`, `sensor.nusantaraX.local`
- PAT untuk akses internet; port forwarding untuk web server yang dapat diakses publik

**Keamanan**
- WiFi-Tamu terisolasi: boleh ke internet, **tidak boleh** ke segmen internal mana pun
- Keuangan dan HRD tidak dapat saling mengakses secara langsung
- Hanya segmen Manajemen yang boleh SSH ke perangkat jaringan
- Port security pada port akses di segmen Produksi
- Password terenkripsi, banner peringatan, dan `service password-encryption` aktif

**Wireless & IoT**
- WLAN dengan SSID terpisah per segmen dan keamanan WPA2
- Minimal 3 sensor IoT di Gudang yang melaporkan ke server registrasi IoT

**Dokumentasi**
- Diagram topologi fisik dan logis
- Tabel addressing lengkap (as-built, bukan as-planned)
- File konfigurasi seluruh perangkat
- Matriks kontrol akses
- Prosedur recovery untuk tiga skenario kegagalan

---

## 5. Yang **Tidak** Dikerjakan

Empat butir pertama sering muncul di materi jaringan pada umumnya, tetapi tidak dapat dijalankan di Packet Tracer:

| Jangan kerjakan | Alasan |
|---|---|
| SNMP/NMS sungguhan, skrip `snmpwalk` | PT hanya menyimulasikan SNMP secara terbatas; tidak ada NMS nyata |
| Skrip otomasi bash/Python untuk konfigurasi | Tidak ada shell yang mengeksekusinya di PT |
| Load balancer, API gateway, microservices | Tidak ada perangkatnya di PT |
| Payment gateway, deteksi fraud, CDN | Di luar bahan kajian dan tidak dapat disimulasikan |
| Routing dinamis (OSPF/EIGRP/BGP) | Di luar cakupan RPS mata kuliah ini — static routing sudah cukup untuk skala ini |
| IPv6 penuh di seluruh jaringan | Cukup dua segmen sebagai bukti pemahaman (pekan 2) |

Materi 5G, teknologi generasi lanjut, dan otomasi jaringan **tetap dipelajari** — sebagai kuliah teori pekan 13 dan Tugas 4 berupa analisis, bukan sebagai praktikum simulasi. Anda tidak dinilai dari kemampuan menyalin skrip yang tidak pernah dijalankan.

---

## 6. Aturan File dan Pengumpulan

**Penamaan file.** `nusantaranet-<NIM>-p<pekan>.pkt` — contoh `nusantaranet-2126250027-p05.pkt`.

**Setiap pekan dikumpulkan tiga hal:**

1. File `.pkt` kumulatif (bukan hanya bagian pekan itu)
2. `konfigurasi-p<pekan>.txt` — hasil `show running-config` seluruh perangkat, dipisahkan komentar `! ===== R-HQ-27 =====`
3. Laporan 2 halaman memakai template Lampiran C

File nomor 2 diperiksa **otomatis** oleh skrip: parameter dicocokkan terhadap NIM Anda dan dibandingkan antar-mahasiswa. Konfigurasi dengan blok IP milik orang lain akan tertangkap tanpa perlu dibaca manual.

**Pekerjaan berpasangan, penilaian individual.** Anda bekerja berdua di satu meja, tetapi **masing-masing membangun jaringan dengan parameter NIM sendiri**. Pasangan adalah rekan diskusi dan penguji silang, bukan pembagi pekerjaan. Peran wajib bertukar pada pertengahan sesi dan dicatat di lembar checkpoint.

---

## 7. Kebijakan Penggunaan AI

Anda **boleh dan dianjurkan** memakai AI. Yang dinilai adalah apakah Anda bisa membaca, memverifikasi, dan mempertanggungjawabkan konfigurasi tersebut.

Ada satu kenyataan teknis yang perlu Anda ketahui sejak awal: **Packet Tracer hanya mengimplementasikan sebagian kecil perintah Cisco IOS.** AI dilatih dari dokumentasi perangkat sungguhan, jadi ia akan rutin memberi Anda perintah yang valid di router asli tetapi ditolak Packet Tracer dengan `% Invalid input detected at '^' marker`.

Contoh yang hampir pasti Anda temui:

| Diberikan AI | Status di Packet Tracer |
|---|---|
| `switchport trunk encapsulation dot1q` | Tidak ada pada switch 2960 — 2960 hanya mendukung dot1Q, jadi perintahnya dihilangkan |
| `zone-security` / zone-based firewall | Tidak ada; pakai ACL |
| `monitor session` (SPAN), `ip sla`, `netflow` | Tidak ada |
| `ip nat inside source list 1 interface Gi0/0 overload` | Didukung |
| `ip helper-address` | Didukung |
| `spanning-tree mode rapid-pvst` | Diterima, tetapi perilakunya disederhanakan |

Daftar ini tidak lengkap dan **tidak perlu dihafal** — yang perlu Anda bangun adalah kebiasaan memverifikasi, bukan hafalan. Versi Packet Tracer pun berbeda-beda dukungannya, jadi jawaban paling andal selalu: coba di PT, baca pesan errornya.

Karena itu setiap pekan menyediakan **Prompt Pack** dengan empat jenis prompt, dan salah satunya — Prompt B, Verifikasi Dukungan Packet Tracer — wajib Anda pakai setiap kali AI memberi konfigurasi. Kemampuan mengenali perintah yang tidak akan jalan adalah salah satu hal yang membedakan Anda dari orang yang sekadar menempel jawaban.

**Yang dilarang keras:** menempelkan konfigurasi perangkat jaringan kampus yang sungguhan, password enable asli, atau data pribadi nyata (NIM dan nama mahasiswa lain) ke layanan AI mana pun.

---

## 8. Yang Akan Ditanyakan Saat Viva

Viva berlangsung 30 detik pada checkpoint terakhir setiap sesi. Asisten mengambil satu pertanyaan acak. Bentuknya selalu **menunjuk ke layar Anda**, bukan menghafal definisi:

- "Tunjukkan baris konfigurasi mana yang membuat ping ini gagal."
- "Kalau saya cabut kabel ini, apa yang berubah di `show ip route`? Jawab dulu, baru kita coba."
- "Kenapa subnet ini Anda beri `/26` dan bukan `/24`?"
- "Segmen mana yang bisa mencapai server ini, dan baris mana yang mengizinkannya?"
- "VLAN ID Anda 420. Dari mana angka itu?"
- "Perintah ini Anda dapat dari AI. Bagaimana Anda tahu ia didukung Packet Tracer?"

Bank pertanyaan lengkap per pekan ada di Lampiran E — **dibagikan terbuka kepada mahasiswa.** Pertanyaan yang bisa dipelajari sebelumnya tetap tidak bisa dijawab tanpa memahami jaringan sendiri.
