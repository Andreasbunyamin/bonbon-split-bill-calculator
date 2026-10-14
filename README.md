# BonBon

**Split Bill Calculator**

Web split bill untuk makan bareng. Masukkan menu dari struk, nama yang ikut makan, lalu pilih siapa pesan apa. Hasilnya berupa gambar ringkasan per orang, lengkap dengan rekening tujuan transfer, yang siap dikirim ke grup. Host bisa mencentang siapa yang sudah bayar dan menyalin pesan tagihan untuk yang belum.

**Demo:** `https://<nama-project>.vercel.app` *(ganti setelah deploy)*

<!-- Tambahkan screenshot: docs/screenshot.png -->

## Fitur

- **Input menu dari struk**: nama menu, qty, dan harga, plus pajak, service charge, dan diskon.
- **Cek total.** Isi total yang tertera di struk, lalu aplikasi memberi tahu apakah hasil hitung sudah cocok atau masih ada selisih (misalnya menu yang terlewat).
- **Assign menu per orang.** Satu menu bisa dimiliki satu orang, beberapa orang, atau semua (harga dibagi rata).
- **Pecah dan gabung porsi.** Item seperti "Es Teh ×5" bisa dipecah jadi 5 porsi untuk orang yang berbeda, lalu digabung lagi kalau perlu.
- **Gambar hasil bergaya struk thermal** berisi rincian per orang dan rekening tujuan. Di HP, gambar bisa langsung dibagikan ke WhatsApp; di laptop, gambar terunduh sebagai PNG.
- **Status pembayaran.** Checklist lunas atau belum, plus pesan tagihan otomatis untuk yang belum bayar.
- **Rekening tersimpan dan riwayat split** di browser pengguna.

## Aturan perhitungan

| Komponen | Cara dibagi |
|---|---|
| Menu milik satu orang | Ditanggung orang itu |
| Menu sharing | Dibagi rata ke orang yang dipilih |
| Pajak, service, diskon | Dibagi rata ke semua peserta |
| Pembulatan | Total per orang dibulatkan ke rupiah terdekat |

**Penerima transfer.** Di langkah Rekening, pilih peserta mana yang menerima transfer. Peserta itu ditandai sebagai **Penerima** dan tidak masuk daftar tagihan. Kalau nama pemilik rekening sama dengan nama peserta (tanpa membedakan huruf besar-kecil), pilihan terisi otomatis. Kalau berbeda, misalnya peserta "Andreas" dan rekening atas nama "Andreas Natanael", pilih manual. Nama pemilik rekening di gambar hasil tetap mengikuti yang diisi, sesuai buku tabungan.

## Privasi

Tidak ada login, server, atau database. Semua data (menu, nama, rekening, riwayat) disimpan di `localStorage` browser pengguna dan tidak pernah dikirim ke mana pun. Konsekuensinya, data tidak ikut pindah ke perangkat lain, dan akan hilang kalau data browser dihapus.

## Struktur repo

```
.
├── index.html   # Seluruh aplikasi: HTML, CSS, dan JavaScript, tanpa framework dan tanpa build step
└── README.md
```

## Deploy ke Vercel

1. Push repo ini ke GitHub.
2. Buka [vercel.com/new](https://vercel.com/new), lalu import repo-nya.
3. Framework Preset: **Other**. Build Command dan Output Directory dibiarkan kosong.
4. Klik **Deploy**. Setiap push ke branch `main` akan otomatis men-deploy ulang.

Tidak ada environment variable yang perlu diisi.

## Menjalankan di lokal

Buka `index.html` langsung di browser, atau jalankan server statis:

```bash
python -m http.server 8000   # buka http://localhost:8000
```

## Tech stack

HTML, CSS, dan JavaScript tanpa framework, Canvas API untuk membuat gambar hasil, Web Share API untuk berbagi ke aplikasi chat, dan `localStorage` untuk penyimpanan.

## Roadmap

- [ ] Baca struk otomatis dari foto (kodenya sudah disiapkan di balik flag `SCAN_ENABLED` di `index.html`)
- [ ] Pajak dan service proporsional sebagai opsi
- [ ] QRIS atau tautan e-wallet di gambar hasil
- [ ] Link split bersama: peserta memilih menunya sendiri
