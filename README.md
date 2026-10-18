# BonBon

**Split Bill Calculator**

Web split bill untuk makan bareng. Masukkan menu dari struk, nama yang ikut makan, lalu pilih siapa pesan apa. Hasilnya berupa gambar ringkasan per orang, lengkap dengan rekening tujuan transfer, yang siap dikirim ke grup. Host bisa mencentang siapa yang sudah bayar dan menyalin pesan tagihan untuk yang belum.

**Demo:** https://bonbon-split-bill-calculator.vercel.app/

<!-- Tambahkan screenshot: docs/screenshot.png -->

## Fitur

- **Halaman utama.** Mulai split baru, lanjutkan split yang belum selesai, atau cek siapa yang belum bayar dari satu halaman.
- **Input menu dari struk**: nama menu, qty, dan harga, plus pajak, service charge, dan diskon.
- **Cek total.** Isi total yang tertera di struk, lalu aplikasi memberi tahu apakah hasil hitung sudah cocok atau masih ada selisih (misalnya menu yang terlewat).
- **Assign menu per orang.** Satu menu bisa dimiliki satu orang, beberapa orang, atau semua (harga dibagi rata).
- **Pecah dan gabung porsi.** Item seperti "Es Teh ×5" bisa dipecah jadi 5 porsi untuk orang yang berbeda, lalu digabung lagi kalau perlu.
- **Gambar hasil bergaya struk thermal** berisi rincian per orang dan rekening tujuan. Di HP, gambar bisa langsung dibagikan ke WhatsApp; di laptop, gambar terunduh sebagai PNG.
- **Status pembayaran.** Checklist lunas atau belum, plus pesan tagihan otomatis untuk yang belum bayar.
- **Rekening tersimpan dan riwayat split** di browser pengguna. Split tersimpan otomatis sejak isian pertama, termasuk yang belum selesai.

## Aturan perhitungan

| Komponen | Cara dibagi |
|---|---|
| Menu milik satu orang | Ditanggung orang itu |
| Menu sharing | Dibagi rata ke orang yang dipilih |
| Pajak | Dibagi proporsional sesuai nilai pesanan masing-masing |
| Service | Dibagi rata ke semua peserta |
| Diskon | Dibagi proporsional sesuai nilai pesanan masing-masing (yang pesan lebih banyak, potongannya lebih besar) |
| Pembulatan | Total per orang dibulatkan ke rupiah, dan jumlahnya selalu sama persis dengan total struk |

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

## Riwayat perubahan

### 1.1.0 (18 Oktober 2026)

**Fitur baru**
- Halaman utama saat aplikasi dibuka, berisi tombol buat split baru, daftar split yang belum selesai, dan tiga split terakhir beserta status pembayarannya.
- Split tersimpan otomatis sejak isian pertama. Split yang belum sampai langkah Hasil muncul di bagian "Belum selesai" dan bisa dilanjutkan dari langkah terakhir.
- Tombol "Lihat contoh" untuk pengunjung yang belum punya split.
- Draf yang tidak dipakai bisa dihapus langsung dari halaman utama.

**Perubahan**
- Aplikasi tidak lagi langsung membuka split terakhir saat dibuka.
- Tombol Riwayat di header diganti tombol Beranda. Riwayat lengkap dibuka lewat "Lihat semua" di halaman utama.
- Tombol "Split baru" di langkah Hasil diganti "Selesai", yang kembali ke halaman utama.

**Catatan:** riwayat dari versi sebelumnya tetap terbaca dan muncul di bagian "Split terakhir".

### 1.0.1 (17 Oktober 2026)

**Perhitungan**
- Pajak sekarang dibagi proporsional sesuai nilai pesanan masing-masing. Sebelumnya dibagi rata ke semua peserta.
- Diskon sekarang dibagi proporsional sesuai nilai pesanan masing-masing. Sebelumnya dibagi rata ke semua peserta.
- Service tetap dibagi rata ke semua peserta.
- Jumlah tagihan per orang sekarang selalu sama persis dengan total struk. Sebelumnya bisa selisih Rp1 karena pembulatan.

**Tampilan**
- Gambar hasil dan teks yang disalin ke WhatsApp menampilkan pajak, service, dan diskon di baris terpisah untuk setiap orang.
- Nomor versi tampil di bagian bawah halaman.

**Catatan:** split lama di Riwayat akan dihitung ulang dengan aturan baru saat dibuka. Kalau pajak atau diskonnya tidak nol, nominal per orang bisa berbeda dari yang sudah dibagikan sebelumnya.

### 1.0.0 (14 Oktober 2026)

Rilis pertama: input menu manual dengan cek total struk, assign menu per orang (termasuk sharing), pecah dan gabung porsi, pilihan penerima transfer, gambar hasil yang bisa dibagikan ke WhatsApp, checklist pembayaran dan pesan tagihan, serta rekening dan riwayat yang tersimpan di browser.
