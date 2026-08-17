# Pengajaran — Mata Kuliah

Repositori publik berisi modul praktikum untuk mahasiswa. Diorganisasi per semester, lalu per mata kuliah.

## Semester Ganjil 2026/2027

| Mata Kuliah | Kode | Folder | Proyek |
|---|---|---|---|
| Desain dan Manajemen Jaringan Komputer | SI2514011 | [Ganjil-2026-2027/Desain-dan-Manajemen-Jaringan-Komputer/](Ganjil-2026-2027/Desain-dan-Manajemen-Jaringan-Komputer/) | NusantaraNet (Cisco Packet Tracer) |
| Pemrograman Web | SI2514024 | [Ganjil-2026-2027/Pemrograman-Web/](Ganjil-2026-2027/Pemrograman-Web/) | KampusLMS (Laravel 12) |
| Kapita Selekta: AI Engineering | — | [Ganjil-2026-2027/Kapita-Selekta-AI-Engineering/](Ganjil-2026-2027/Kapita-Selekta-AI-Engineering/) | Produk berbasis AI Agent |

## Struktur

```
Ganjil-2026-2027/
├── Desain-dan-Manajemen-Jaringan-Komputer/
│   ├── README.md
│   └── mahasiswa/          — modul, lampiran, spesifikasi proyek (publik)
├── Pemrograman-Web/
│   ├── README.md
│   └── mahasiswa/
└── Kapita-Selekta-AI-Engineering/
    ├── README.md
    └── mahasiswa/
```

Setiap MK memiliki folder `pengajar/` yang **privat** — kunci jawaban, rubrik internal,
resep fault injection, dan data penilaian. Folder itu di-*ignore* oleh Git dan tidak
pernah ikut ter-*push* ke repo publik ini. Sumber lengkapnya ada di repo privat
`Pengajaran-Modul-Praktikum`.

## Cara Pakai (Mahasiswa)

1. Mulai dari `README.md` mata kuliah masing-masing.
2. Lanjut ke `mahasiswa/00-daftar-modul.md` untuk indeks, aturan main, dan skema penilaian.
3. Modul dibuka bertahap sesuai pekan; ikuti urutan penomoran file.

Untuk menarik pembaruan:

```bash
git pull
```

## Konvensi Penamaan

| Tipe | Format | Contoh |
|---|---|---|
| Folder semester | `Ganjil-YYYY-YYYY` / `Genap-YYYY-YYYY` | `Ganjil-2026-2027` |
| Folder MK | Nama lengkap MK, kebab-case kapital | `Pemrograman-Web`, `Desain-dan-Manajemen-Jaringan-Komputer` |
| File modul | `NN-modul-*.md` | `02-modul-pekan-01-04.md` |
| Lampiran | `<Huruf>-nama-kebab-case.md` | `A-command-reference.md` |

---

Pengampu: Aidil Saputra Kirsan
