# LAMPIRAN A — Command Reference

**Cisco IOS pada Packet Tracer 8.2 — router 2911 dan switch 2960**

> Daftar ini disusun **menurut topik, bukan menurut urutan pengerjaan**, dan **tanpa nilai parameter**. Ia sengaja tidak dapat dipakai dengan cara menyalin dari atas ke bawah.
>
> Mulai pekan 5, ini satu-satunya tempat perintah tersedia. Urutan pengerjaan dan nilai setiap parameter adalah bagian yang Anda putuskan sendiri.

Notasi: `<...>` berarti nilai yang Anda tentukan. Perintah yang diberi tanda **(verifikasi)** tidak mengubah apa pun dan aman dijalankan kapan saja.

---

## 1. Dasar dan Navigasi

```
enable                                  masuk mode privileged
configure terminal                      masuk mode konfigurasi global
exit                                    turun satu tingkat
end                                     langsung ke privileged
do <perintah show>                      jalankan show tanpa keluar dari config
write memory                            simpan konfigurasi berjalan ke startup
copy running-config startup-config       sama dengan di atas
reload                                  muat ulang perangkat
```

Perintah yang belum disimpan hilang saat perangkat dimuat ulang. Di Packet Tracer, menyimpan file `.pkt` **tidak** sama dengan menyimpan konfigurasi ke startup-config.

## 2. Penamaan dan Pengerasan Dasar

```
hostname <nama>
enable secret <password>
service password-encryption
banner motd #<teks peringatan>#
no ip domain-lookup                     mencegah jeda saat salah ketik perintah
line console 0
 password <password>
 login
 logging synchronous
 exec-timeout <menit> <detik>
```

## 3. Interface

```
interface <tipe><nomor>                 contoh tipe: gigabitEthernet, fastEthernet
interface range <tipe><awal>-<akhir>
 description <keterangan>
 ip address <ip-address> <mask>
 no shutdown
 shutdown
interface loopback <nomor>
```

Verifikasi:

```
show ip interface brief                 (verifikasi) ringkas semua interface
show interfaces <tipe><nomor>           (verifikasi) rinci satu interface
show interfaces status                  (verifikasi, switch) status dan VLAN per port
show interfaces description             (verifikasi)
```

## 4. VLAN dan Trunk (switch)

```
vlan <id>
 name <nama>
interface <tipe><nomor>
 switchport mode access
 switchport access vlan <id>
 switchport mode trunk
 switchport trunk allowed vlan <daftar>
 switchport trunk native vlan <id>
 switchport nonegotiate
```

SVI dan gateway switch:

```
interface vlan <id>
 ip address <ip-address> <mask>
 no shutdown
ip default-gateway <ip-address>
```

Verifikasi:

```
show vlan brief                         (verifikasi) daftar VLAN dan port anggotanya
show interfaces trunk                   (verifikasi) trunk aktif dan VLAN yang diizinkan
show mac address-table                  (verifikasi) MAC yang dipelajari per port
show mac address-table dynamic
```

Catatan Packet Tracer: `switchport trunk encapsulation dot1q` **tidak ada** pada switch 2960 karena 2960 hanya mendukung dot1Q.

## 5. Router-on-a-Stick

```
interface <tipe><nomor>
 no shutdown
interface <tipe><nomor>.<sub>
 description <keterangan>
 encapsulation dot1Q <vlan-id>
 ip address <ip-address> <mask>
```

Nomor sub-interface hanyalah label; yang menentukan VLAN adalah angka pada `encapsulation dot1Q`. Interface fisik induk harus `no shutdown`.

## 6. Routing Statis

```
ip route <jaringan> <mask> <next-hop>
ip route <jaringan> <mask> <next-hop> <administrative-distance>
ip route 0.0.0.0 0.0.0.0 <next-hop>                       rute default
no ip route <jaringan> <mask> <next-hop>
```

Verifikasi:

```
show ip route                           (verifikasi)
show ip route <jaringan>                (verifikasi) rincian satu rute
show ip protocols                       (verifikasi)
traceroute <ip-address>                     (verifikasi)
```

Rute yang ada di `running-config` tetapi tidak muncul di `show ip route` berarti next-hop-nya tidak terjangkau.

## 7. DHCP

Server pada router:

```
ip dhcp excluded-address <awal> <akhir>
ip dhcp pool <nama>
 network <jaringan> <mask>
 default-router <ip-address>
 dns-server <ip-address>
 domain-name <nama-domain>
 lease <hari> <jam> <menit>
```

Pemesanan alamat tetap untuk satu perangkat:

```
ip dhcp pool <nama-pool-khusus>
 host <ip-address> <mask>
 client-identifier <01 diikuti MAC dalam format titik>
 default-router <ip-address>
```

Relay pada interface segmen jauh:

```
interface <tipe><nomor>[.<sub>]
 ip helper-address <dhcp-server-ip>
```

Verifikasi:

```
show ip dhcp binding                    (verifikasi) alamat yang sudah disewakan
show ip dhcp pool                       (verifikasi) pemakaian dan pengecualian
show ip dhcp conflict                   (verifikasi)
clear ip dhcp binding *                 kosongkan seluruh lease
```

Di sisi klien (Command Prompt PC):

```
ipconfig
ipconfig /all
ipconfig /release
ipconfig /renew
```

## 8. DNS

Layanan DNS di Packet Tracer dikonfigurasi lewat GUI Server-PT: tab **Services**, pilih **DNS**, aktifkan, lalu tambahkan record A berisi nama dan alamat.

Pada router, bila diperlukan:

```
ip domain-name <nama-domain>
ip name-server <ip-address>
ip dns server                           router bertindak sebagai penerus DNS
```

Di sisi klien:

```
nslookup <nama>
ping <nama>
```

## 9. NAT dan PAT

Penandaan arah — wajib, dan tidak dapat diduga router:

```
interface <tipe><nomor>[.<sub>]
 ip nat inside
interface <tipe><nomor>
 ip nat outside
```

PAT (overload) memakai alamat interface:

```
access-list <nomor> permit <jaringan> <wildcard>
ip nat inside source list <nomor> interface <tipe><nomor> overload
```

PAT memakai address pool:

```
ip nat pool <nama> <awal> <akhir> netmask <mask>
ip nat inside source list <nomor> pool <nama> overload
```

Static NAT dan port forwarding:

```
ip nat inside source static <ip-privat> <ip-publik>
ip nat inside source static tcp <ip-privat> <port> <ip-publik> <port>
ip nat inside source static udp <ip-privat> <port> <ip-publik> <port>
```

Verifikasi:

```
show ip nat translations                (verifikasi)
show ip nat statistics                  (verifikasi)
clear ip nat translation *
```

## 10. ACL

Bernomor:

```
access-list <1-99> permit|deny <sumber> <wildcard>                       standard
access-list <100-199> permit|deny <protokol> <sumber> <wildcard> <tujuan> <wildcard> [eq <port>]
no access-list <nomor>                  menghapus SELURUH daftar
```

Bernama:

```
ip access-list standard <nama>
ip access-list extended <nama>
 permit|deny <protokol> <sumber> <wildcard> <tujuan> <wildcard> [eq <port>]
 <nomor-urut> permit|deny ...           menyisipkan pada posisi tertentu
 no <nomor-urut>                        menghapus satu baris
```

Pemasangan:

```
interface <tipe><nomor>[.<sub>]
 ip access-group <nomor-atau-nama> in|out
```

Pembatasan akses administratif:

```
line vty 0 15
 access-class <nomor-atau-nama> in
```

Verifikasi:

```
show ip access-lists                    (verifikasi) termasuk penghitung per baris
show ip interface <tipe><nomor>         (verifikasi) ACL yang terpasang dan arahnya
clear ip access-list counters
```

Kata kunci yang sering dipakai: `any`, `host <ip-address>`, `eq <port>`, `established`. Nama port yang lazim: `www`, `domain`, `ftp`, `telnet`, `ssh`, `smtp`.

Setiap ACL berakhir dengan `deny ip any any` yang tidak tertulis.

### Wildcard mask

Wildcard adalah kebalikan subnet mask: bit `0` berarti harus cocok, bit `1` berarti diabaikan.

| Cakupan | Subnet mask | Wildcard |
|---|---|---|
| Satu host | 255.255.255.255 | 0.0.0.0 |
| /29 | 255.255.255.248 | 0.0.0.7 |
| /28 | 255.255.255.240 | 0.0.0.15 |
| /26 | 255.255.255.192 | 0.0.0.63 |
| /25 | 255.255.255.128 | 0.0.0.127 |
| /24 | 255.255.255.0 | 0.0.0.255 |
| /22 | 255.255.252.0 | 0.0.3.255 |
| /20 | 255.255.240.0 | 0.0.15.255 |
| /8 | 255.0.0.0 | 0.255.255.255 |
| Semua | — | 255.255.255.255 (`any`) |

## 11. SSH dan Akses Administratif

Empat prasyarat SSH, dan salah satunya sering terlupa:

```
hostname <nama>                         tidak boleh default
ip domain-name <nama-domain>
crypto key generate rsa                 pilih 1024 bit atau lebih
username <nama> secret <password>
line vty 0 15
 transport input ssh                    mematikan telnet sekaligus
 login local
```

Verifikasi:

```
show ip ssh                             (verifikasi)
show ssh                                (verifikasi) sesi aktif
show users                              (verifikasi)
```

## 12. Port Security (switch)

```
interface <tipe><nomor>
 switchport mode access
 switchport port-security
 switchport port-security maximum <jumlah>
 switchport port-security mac-address <mac-address>
 switchport port-security mac-address sticky
 switchport port-security violation protect|restrict|shutdown
```

Recovery port yang mati karena pelanggaran:

```
interface <tipe><nomor>
 shutdown
 no shutdown
```

Verifikasi:

```
show port-security                          (verifikasi)
show port-security interface <tipe><nomor>  (verifikasi)
show port-security address                  (verifikasi)
```

## 13. IPv6

```
ipv6 unicast-routing                    wajib, jika tidak paket IPv6 tidak diteruskan
interface <tipe><nomor>[.<sub>]
 ipv6 address <prefix>/<panjang>
 ipv6 address <prefix>/64 eui-64
 ipv6 enable
ipv6 route <prefix>/<panjang> <next-hop>
```

Verifikasi:

```
show ipv6 interface brief               (verifikasi)
show ipv6 route                         (verifikasi)
show ipv6 neighbors                     (verifikasi) padanan tabel ARP
```

## 14. Wireless dan IoT

Access point di Packet Tracer dikonfigurasi lewat GUI, bukan CLI. Tab **Config**, pilih interface wireless, lalu atur SSID, keamanan (WPA2-PSK), kanal, dan pita.

Uplink AP ke switch dikonfigurasi di sisi switch sebagai trunk seperti pada bagian 4.

Perangkat IoT: pada tab **Config** setiap perangkat, isi **Remote Server** dengan alamat IoT Registration Server beserta nama pengguna dan password. Kondisi otomatis diatur pada interface web server tersebut, menu **Conditions**.

## 15. Diagnosis dari Sisi Klien

```
ipconfig /all                           alamat, gateway, DNS yang diterima
ping <ip-address>
ping <ip-address> -t                        terus-menerus, untuk mengamati failover
tracert <ip-address>
arp -a                                  isi cache ARP
nslookup <nama>
telnet <ip-address>                         menguji port terbuka
```

## 16. Urutan Pemeriksaan yang Dianjurkan

Untuk gangguan yang belum diketahui sebabnya, jalankan ini lebih dulu — semuanya aman dan tidak mengubah apa pun:

```
1. show ip interface brief              interface mana yang aktif
2. show vlan brief                      port di VLAN yang benar
3. show interfaces trunk                VLAN lewat trunk
4. show ip route                        rute ada dan next-hop terjangkau
5. show ip access-lists                 penghitung baris yang cocok
6. show ip nat translations             translasi terbentuk
7. show ip dhcp binding                 lease alamat tercatat
```

Pengujian cakupan dari sisi klien selalu lebih murah daripada membuka `show running-config`. Mulailah dari sana.
