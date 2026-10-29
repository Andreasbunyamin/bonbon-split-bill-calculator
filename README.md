# BonBon

**Split Bill Calculator**

Web split bill untuk makan bareng. Masukkan menu dari struk, nama yang ikut makan, lalu pilih siapa pesan apa. Hasilnya berupa gambar ringkasan per orang, lengkap dengan rekening tujuan transfer, yang siap dikirim ke grup. Host bisa mencentang siapa yang sudah bayar dan menyalin pesan tagihan untuk yang belum.

**Demo:** https://bonbon-split-bill-calculator.vercel.app/

<!-- Tambahkan screenshot: docs/screenshot.png -->

## Fitur

- **Halaman utama.** Mulai split baru, lanjutkan split yang belum selesai, atau cek siapa yang belum bayar dari satu halaman.
- **Input menu dari struk**: nama menu, qty, dan harga, plus pajak, service charge, dan diskon. Harga bisa diisi sebagai subtotal atau harga satuan (subtotal dihitung otomatis dari qty × harga). Setiap menu wajib punya nama dan harga, dan angka yang formatnya salah ditolak dengan pesan.
- **Cek total.** Isi total yang tertera di struk, lalu aplikasi memberi tahu apakah hasil hitung sudah cocok atau masih ada selisih (misalnya menu yang terlewat).
- **Assign menu per orang.** Satu menu bisa dimiliki satu orang, beberapa orang, atau semua (harga dibagi rata).
- **Isi jumlah per orang.** Menu dengan qty lebih dari 1, misalnya sate 20 tusuk, bisa dibagi sesuai jumlah yang diambil tiap orang (Nb 5, Nico 10, Theo 5). Jumlahnya bisa diketik atau diatur dengan tombol − dan +, termasuk pecahan seperti 0,5 atau 1/3. Sisa yang belum dibagi ditandai sebelum hasil dibuat.
- **Gambar hasil bergaya struk thermal** berisi rincian per orang dan rekening tujuan. Di HP, gambar bisa langsung dibagikan ke WhatsApp; di laptop, gambar terunduh sebagai PNG.
- **Status pembayaran.** Checklist lunas atau belum, plus pesan tagihan otomatis untuk yang belum bayar.
- **Gabungkan beberapa split.** Dari Riwayat, pilih beberapa split (misalnya semua split dalam satu hari) lalu gabungkan jadi satu tagihan. Total setiap orang dijumlahkan berdasarkan nama, dan tagihan dikelompokkan per penerima transfer: satu gambar untuk setiap penerima. Orang yang saling berutang cukup membayar selisihnya.
- **Rekening tersimpan dan riwayat split** di browser pengguna. Split tersimpan otomatis sejak isian pertama, termasuk yang belum selesai.

## Aturan perhitungan

| Komponen | Cara dibagi |
|---|---|
| Menu milik satu orang | Ditanggung orang itu |
| Menu sharing | Dibagi rata ke orang yang dipilih |
| Menu dengan jumlah per orang | Harga menu × jumlah yang diambil ÷ qty. Nominal setiap orang dibulatkan ke bawah ke rupiah, lalu sisa rupiahnya diberikan satu per satu ke pecahan terbesar (kalau sama, ke nama paling atas), jadi jumlahnya pas dengan harga menu |
| Pajak | Dibagi proporsional sesuai nilai pesanan masing-masing |
| Service | Pilih per split: **rata per orang** (cocok untuk ongkos kirim atau biaya layanan aplikasi seperti GrabFood) atau **proporsional** sesuai nilai pesanan (cocok untuk service charge restoran). Bawaannya rata. |
| Diskon | Dibagi proporsional sesuai nilai pesanan masing-masing (yang pesan lebih banyak, potongannya lebih besar) |
| Pembulatan | Total per orang dibulatkan ke rupiah, dan jumlahnya selalu sama persis dengan total struk |

**Penerima transfer.** Di langkah Rekening, pilih peserta mana yang menerima transfer. Peserta itu ditandai sebagai **Penerima** dan tidak masuk daftar tagihan. Kalau nama pemilik rekening sama dengan nama peserta (tanpa membedakan huruf besar-kecil), pilihan terisi otomatis. Kalau berbeda, misalnya peserta "Andreas" dan rekening atas nama "Andreas Natanael", pilih manual. Nama pemilik rekening di gambar hasil tetap mengikuti yang diisi, sesuai buku tabungan.

**Tagihan gabungan.** Setiap split dihitung sendiri dulu dengan aturan di atas, lalu nominal per orang dijumlahkan berdasarkan nama (tanpa membedakan huruf besar-kecil) dan dikelompokkan per rekening tujuan. Split yang sudah ditandai lunas untuk seseorang tetap tampil, tapi tidak ikut dijumlahkan. Kalau dua orang saling berutang, opsi **Sederhanakan transfer** (aktif secara bawaan) menghitung selisihnya saja: yang utangnya lebih besar mentransfer selisih itu, dan yang lain tidak perlu transfer. Penerima transfer lainnya tidak berubah.

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

### 1.3.2 (29 Oktober 2026)

**Tampilan**
- Format pesan tagihan untuk yang belum bayar dibuat lebih ringkas: diawali "Reminder patungan", tanpa "Halo!" dan "Terima kasih!".
- Nomor rekening sekarang ada di baris sendiri, di bawah nama bank dan nama pemilik, jadi bisa ditekan lama dan disalin langsung di WhatsApp.

```
Reminder patungan *Ayam Bawang Putih*
- Andreas: Rp20.000
- David: Rp20.000

Transfer ke:
BCA a.n. Marshelly Theresia
4790163279
```

### 1.3.1 (22 Oktober 2026)

**Perbaikan**
- Pindah antara "Subtotal" dan "Harga satuan" sekarang mengonversi angkanya: subtotal ÷ qty jadi harga per item, dan harga per item × qty jadi subtotal. Subtotal setiap menu tidak berubah. Sebelumnya angka yang sudah diketik tetap dan hanya artinya yang berubah.
- Kalau subtotal tidak habis dibagi qty, harga per item ditampilkan dibulatkan (misalnya 10.000 ÷ 3 = 3.333), tapi subtotalnya tetap 10.000 selama angkanya tidak diketik ulang.

**Tampilan**
- Pilihan "Total per baris" diganti "Subtotal", termasuk judul kolomnya.

### 1.3.0 (22 Oktober 2026)

**Fitur baru**
- "Isi jumlah per orang" di langkah Menu, untuk menu dengan qty lebih dari 1. Setiap peserta punya tombol − dan + yang angkanya juga bisa diketik, dan nominalnya langsung terhitung. Contohnya sate 20 tusuk: Nb 5, Nico 10, Theo 5.
- Jumlah bisa berupa pecahan: 0,5, 1/3, atau 1 1/2. Angka seperti 0,33 dibaca 1/3, jadi tiga orang yang masing-masing mengisi 0,33 pas membagi satu porsi.
- Nominal per orang dibulatkan ke rupiah dan jumlahnya selalu pas dengan harga menu. Contohnya Rp10.000 dibagi 1/6 ke enam orang: empat orang Rp1.667 dan dua orang Rp1.666.
- Total jumlah tidak bisa melebihi qty. Kalau masih kurang, kartu menu ditandai oranye "Sisa … belum dibagi", dan sisanya ikut muncul di peringatan di langkah Hasil.
- "Bagi rata saja" kembali ke pilihan nama. Pilihan nama dan jumlah per orang sama-sama tetap tersimpan saat berpindah.

**Perubahan**
- "Pecah jadi N porsi" dan "Gabungkan lagi" dihapus, digantikan "Isi jumlah per orang". Untuk membagi porsi tertentu ke kelompok yang berbeda (misalnya 2 pizza untuk 2 kelompok), masukkan sebagai 2 baris di langkah Struk.
- Langkah Struk selalu menampilkan satu baris per menu, sama seperti di struk.
- Qty di langkah Struk tidak bisa dikurangi di bawah jumlah yang sudah dibagi di langkah Menu.
- Gambar hasil dan teks WhatsApp menulis jumlahnya, misalnya "Sate Daging Ayam / Tusuk ×10".

**Catatan:** split lama yang sudah dipecah per porsi dikonversi otomatis jadi jumlah per orang, dan porsi yang dimiliki beberapa orang dihitung sebagai pecahan (misalnya 1/2 untuk dua orang). Nominal per orang tetap sama; paling-paling bergeser Rp1 antarorang kalau harga per porsinya tidak bulat.

### 1.2.2 (19 Oktober 2026)

**Fitur baru**
- Pilihan "Harga yang diisi" di langkah Struk: **Total per baris** (bawaan) atau **Harga satuan**. Di mode harga satuan, total per baris dihitung otomatis dari qty × harga.
- Mengganti pilihan tidak mengubah angka yang sudah diketik, hanya artinya. Kalau salah pilih, semua baris terkoreksi dengan sekali tekan.
- Baris dengan qty lebih dari 1 menampilkan hitungannya, misalnya "= Rp46.848" atau "Rp2.928/item".

**Perbaikan**
- Qty hanya menerima angka bulat 1–999. Sebelumnya "-3" terbaca 3 dan "1,5" terbaca 15.
- Kolom harga, pajak, service, diskon, dan total di struk mengenali format struk seperti "125.000,00", "125,000", dan "Rp 25.000". Sebelumnya "125.000,00" terbaca 12.500.000.
- Angka desimal (misalnya "12,5") dan angka minus ditolak dengan pesan. Isiannya kembali ke angka terakhir yang benar, dan alasannya tampil di bawah kolom. Khusus kolom diskon, tanda minus diabaikan.
- Setiap menu wajib punya nama dan harga sebelum lanjut dari langkah Struk. Baris yang kosong sama sekali dihapus otomatis, dan menu Rp0 (misalnya tas plastik) tidak perlu dimasukkan.
- Langkah Menu, hasil, gambar, dan tagihan gabungan tidak lagi menampilkan atau menghitung baris tanpa harga.
- Jumlah menu di gambar hasil menghitung menu yang dipecah jadi porsi sebagai satu menu.

**Catatan:** split lama terbuka dengan pilihan "Total per baris", jadi nominalnya tidak berubah.

### 1.2.1 (19 Oktober 2026)

**Fitur baru**
- Opsi "Sederhanakan transfer" di tagihan gabungan, aktif secara bawaan. Kalau dua orang saling berutang, yang dihitung hanya selisihnya: orang dengan utang lebih besar mentransfer selisih itu, dan orang yang lain tidak perlu transfer.
- Potongan tampil sebagai baris hijau "Potong utang ..." di rincian orang yang membayar. Halaman penerima yang lain memberi catatan siapa yang tidak perlu transfer dan alasannya.
- Penerima transfer lain tidak berubah, jadi setiap halaman tetap sesuai dengan split asalnya. Opsi ini bisa dimatikan untuk melihat tagihan tanpa potongan.

### 1.2.0 (19 Oktober 2026)

**Fitur baru**
- Gabungkan beberapa split jadi satu tagihan, lewat tombol "Gabungkan beberapa split" di Riwayat. Split dikelompokkan per tanggal, dengan tombol "Pilih semua" untuk setiap hari.
- Setiap split dihitung sendiri dulu, lalu total setiap orang dijumlahkan berdasarkan nama dan dikelompokkan per penerima transfer. Setiap penerima punya gambar sendiri, berisi rincian per split, total per orang, dan rekening tujuan.
- Judul tagihan gabungan bisa diubah, dan seluruh tagihan bisa disalin sebagai satu teks untuk WhatsApp.
- Peringatan saat ada nama yang mirip tapi ditulis berbeda (misalnya "Nico" dan "Nicolas"), split tanpa rekening penerima, atau menu yang belum ada pemiliknya.

**Catatan**
- Nama dicocokkan tanpa membedakan huruf besar-kecil, jadi "Nb" dan "NB" dianggap orang yang sama.
- Split yang sudah ditandai lunas untuk seseorang tetap ditampilkan, tapi tidak ikut dijumlahkan.

### 1.1.1 (18 Oktober 2026)

**Perhitungan**
- Service sekarang bisa dibagi rata per orang atau proporsional sesuai nilai pesanan. Pilihannya muncul di bawah kolom Service setelah nominalnya diisi.
- Rata cocok untuk ongkos kirim atau biaya layanan aplikasi pesan-antar. Proporsional cocok untuk service charge restoran, dan hasilnya sama dengan menghitung persentase service dari pesanan masing-masing.
- Bawaannya tetap rata, jadi split yang sudah tersimpan tidak berubah nominalnya.

**Tampilan**
- Gambar hasil dan teks WhatsApp menulis "service (proporsional)" atau "service (÷jumlah orang)" sesuai pilihan.

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
