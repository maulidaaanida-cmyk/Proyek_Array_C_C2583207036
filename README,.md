# Sistem Monitoring Stok dan Penjualan Kantin Kampus

## Identitas

| Keterangan   | Isi                                                |
|--------------|----------------------------------------------------|
| Nama Lengkap | Nida Sri Nurmaulida                                |
| NIM          | C2583207036                                        |
| Kelas        | PTI3B                                              |
| Mata Kuliah  | Algoritma dan Pemrograman Lanjutan                 |
| Dosen        | Taofik Muhammad, M.Kom                             |
| Nama Proyek  | Sistem Monitoring Stok dan Penjualan Kantin Kampus |

## Deskripsi Singkat

Aplikasi console berbahasa C untuk membantu pengelola kantin kampus mencatat produk, memantau stok, memproses transaksi penjualan, dan melihat laporan penjualan. Seluruh data disimpan di dalam array selama program berjalan (tanpa database atau file eksternal). Kapasitas maksimal 50 produk, dengan 6 produk sebagai data awal.

**Data awal:**

| No | Nama Produk  | Harga  | Stok |
|----|--------------|--------|------|
| 1  | Nasi Goreng  | 12.000 | 20   |
| 2  | Mie Ayam     | 10.000 | 15   |
| 3  | Es Teh Manis | 4.000  | 30   |
| 4  | Gorengan     | 2.000  | 40   |
| 5  | Air Mineral  | 3.000  | 4    |
| 6  | Kopi Susu    | 8.000  | 12   |

## Daftar Fitur

| Menu | Fitur | Keterangan |
|------|-------|------------|
| 1 | Lihat Daftar Produk | Menampilkan nomor, nama, harga, dan stok seluruh produk |
| 2 | Tambah Produk | Menambah produk baru (nama, harga, stok awal); nama yang sama ditolak |
| 3 | Ubah Produk | Mengubah harga dan stok produk berdasarkan nomor |
| 4 | Transaksi Penjualan | Membeli beberapa jenis produk dalam satu transaksi, menampilkan rincian subtotal dan total, dengan konfirmasi (y/n) |
| 5 | Tambah Stok | Menambah stok produk yang sudah ada |
| 6 | Cari Produk | Mencari produk berdasarkan nama (tidak peka huruf besar/kecil, cukup sebagian kata) |
| 7 | Urutkan Produk | Urut harga termurah, atau urut jumlah terjual terbanyak |
| 8 | Laporan Penjualan | Jumlah terjual dan pendapatan per produk, total unit terjual, total pendapatan, produk terlaris |
| 9 | Cek Stok Menipis | Menampilkan produk dengan stok kurang dari 5 |
| 0 | Keluar | Mengakhiri program |

**Validasi input yang ditangani:**
- Pilihan menu tidak tersedia atau bukan angka
- Nomor produk tidak ditemukan
- Jumlah pembelian nol atau negatif
- Harga atau stok bernilai negatif
- Jumlah pembelian melebihi stok yang tersedia (termasuk jumlah yang sudah dipilih dalam keranjang transaksi yang sama)
- Kapasitas array sudah penuh (50 produk)
- Nama produk kosong atau sudah ada

## Penjelasan Penggunaan Array

Data produk disimpan dalam array paralel. Indeks yang sama pada setiap array menunjukkan produk yang sama.

| No | Nama Array | Tipe Data | Ukuran | Fungsi | Contoh Isi (indeks 0) |
|----|------------|-----------|--------|--------|-----------------------|
| 1 | `nama` | array karakter 2 dimensi (`char`) | [50][30] | Menyimpan nama produk | "Nasi Goreng" |
| 2 | `harga` | array 1 dimensi (`int`) | [50] | Menyimpan harga produk | 12000 |
| 3 | `stok` | array 1 dimensi (`int`) | [50] | Menyimpan stok produk | 20 |
| 4 | `terjual` | array 1 dimensi (`int`) | [50] | Menyimpan jumlah unit terjual | 0 |
| 5 | `pendapatan` | array 1 dimensi (`long`) | [50] | Menyimpan akumulasi pendapatan per produk | 0 |
| 6 | `jumlah` | variabel `int` | - | Jumlah produk aktif (indeks 0 sampai `jumlah - 1`) | 6 |

Konstanta: `MAX_PRODUK = 50`, `PANJANG_NAMA = 30`, `BATAS_MENIPIS = 5`.

```c
char nama[MAX_PRODUK][PANJANG_NAMA];
int  harga[MAX_PRODUK];
int  stok[MAX_PRODUK];
int  terjual[MAX_PRODUK];
long pendapatan[MAX_PRODUK];
int  jumlah = 0;
```

**Cara kerja array:**
- **Tambah produk:** data baru disimpan pada indeks `jumlah`, lalu `jumlah` bertambah satu. Jika `jumlah` sudah mencapai 50, penambahan ditolak.
- **Transaksi:** program memakai array lokal `qtyBeli[MAX_PRODUK]` sebagai keranjang. Stok baru dikurangi setelah pengguna mengonfirmasi, sehingga transaksi yang dibatalkan tidak mengubah data.
- **Pencarian:** sequential search (linear) dari indeks 0 sampai `jumlah - 1`, memakai `strstr` pada nama yang sudah diubah ke huruf kecil.
- **Pengurutan:** bubble sort pada array indeks `urut[]`, bukan menukar isi array data. Data asli tetap pada posisinya, hanya urutan tampilannya yang berubah. Mode 1 mengurutkan harga dari termurah, mode 2 mengurutkan jumlah terjual dari terbanyak.
- **Laporan:** perulangan melewati seluruh array untuk menjumlahkan `terjual[]` dan `pendapatan[]`, lalu mencari nilai `terjual` terbesar sebagai produk terlaris.

## Cara Kompilasi dan Menjalankan

Prasyarat: compiler GCC (atau Code::Blocks dengan compiler GCC/MinGW).

Kompilasi:

```bash
gcc main.c -o kantin
```

Menjalankan di Windows:

```bash
kantin.exe
```

Menjalankan di Linux/Mac:

```bash
./kantin
```

Atau buka `main.c` di Code::Blocks, lalu tekan **Build and Run** (F9).

## Pengujian

Pengujian dilakukan berurutan dalam satu sesi program (Code::Blocks 25.03, Windows), sehingga hasil tiap skenario memakai data dari skenario sebelumnya.

### Skenario 1: Tambah Produk
- **Input/Tindakan:** Menu 2, isi nama `Bakso`, harga `10000`, stok awal `20`.
- **Hasil yang diharapkan:** Produk muncul dalam daftar.
- **Hasil pengujian:** Muncul pesan `Produk "Bakso" berhasil ditambahkan.` Pada daftar produk, Bakso tampil sebagai nomor 7 (harga 10000, stok 20).
- **Keterangan:** Produk baru disimpan pada indeks `jumlah` (6), lalu `jumlah` menjadi 7.

![Skenario 1 - Tambah produk](screenshots/01-tambah-produk.png)

### Skenario 2: Transaksi
- **Input/Tindakan:** Menu 4, beli Nasi Goreng x3, Es Teh Manis x3, Bakso x3, lalu ketik `0` untuk selesai dan `y` untuk konfirmasi.
- **Hasil yang diharapkan:** Subtotal dan total benar.
- **Hasil pengujian:** Subtotal Nasi Goreng 36000 (3 x 12000), Es Teh Manis 12000 (3 x 4000), Bakso 30000 (3 x 10000). Total pembayaran 78000.
- **Keterangan:** Subtotal = harga x jumlah beli, total = jumlah seluruh subtotal.

![Skenario 2 - Transaksi](screenshots/02-transaksi.png)

### Skenario 3: Pembaruan Stok
- **Input/Tindakan:** Setelah transaksi skenario 2 berhasil, buka menu 1.
- **Hasil yang diharapkan:** Stok dan jumlah terjual berubah.
- **Hasil pengujian:** Stok Nasi Goreng 20 menjadi 17, Es Teh Manis 30 menjadi 27, Bakso 20 menjadi 17. Jumlah terjual masing-masing menjadi 3 (terlihat pada laporan skenario 5).
- **Keterangan:** Stok dikurangi dan `terjual[]` ditambah hanya setelah transaksi dikonfirmasi.

Stok sebelum transaksi (stok awal pada daftar produk):

![Skenario 3 - Stok sebelum](screenshots/01-tambah-produk.png)

Stok sesudah transaksi (menu 1):

![Skenario 3 - Stok sesudah](screenshots/03-stok-sesudah.png)

### Skenario 4: Stok Tidak Cukup
- **Input/Tindakan:** Menu 4, pilih nomor 5 (Air Mineral, stok 4), jumlah beli `5`.
- **Hasil yang diharapkan:** Transaksi ditolak; stok tetap.
- **Hasil pengujian:** Muncul pesan `Stok Air Mineral tidak cukup (tersedia 4, sudah dipilih 0).` Produk tidak masuk transaksi, dan transaksi dibatalkan karena tidak ada produk yang dipilih. Stok Air Mineral tetap 4 (terlihat pada menu 9 di skenario 7).
- **Keterangan:** Program membandingkan jumlah beli dengan stok sebelum item masuk ke keranjang.

![Skenario 4 - Stok tidak cukup](screenshots/04-stok-tidak-cukup.png)

### Skenario 5: Laporan
- **Input/Tindakan:** Menu 8.
- **Hasil yang diharapkan:** Unit, pendapatan, dan produk terlaris benar.
- **Hasil pengujian:** Terjual Nasi Goreng 3 (36000), Es Teh Manis 3 (12000), Bakso 3 (30000), produk lain 0. Total unit terjual 9, total pendapatan 78000. Produk terlaris: Nasi Goreng, Es Teh Manis, dan Bakso dengan 3 unit.
- **Keterangan:** Total unit = 3 + 3 + 3 = 9. Total pendapatan = 36000 + 12000 + 30000 = 78000. Ketiga produk memiliki jumlah terjual yang sama, sehingga ketiganya ditampilkan sebagai produk terlaris.

![Skenario 5 - Laporan](screenshots/05-laporan.png)

### Skenario 6: Pengurutan
- **Input/Tindakan:** Menu 7, pilih `1` (harga termurah). Ulangi menu 7, pilih `2` (jumlah terjual terbanyak).
- **Hasil yang diharapkan:** Urutan sesuai ketentuan.
- **Hasil pengujian:**
  - Urut harga: Gorengan (2000), Air Mineral (3000), Es Teh Manis (4000), Kopi Susu (8000), Mie Ayam (10000), Bakso (10000), Nasi Goreng (12000).
  - Urut terjual: Nasi Goreng (3), Es Teh Manis (3), Bakso (3), lalu Mie Ayam, Gorengan, Air Mineral, Kopi Susu (masing-masing 0).
- **Keterangan:** Pengurutan memakai bubble sort pada array indeks, sehingga data asli tidak berpindah.

![Skenario 6 - Pengurutan](screenshots/06-urut-produk.png)

### Skenario 7: Pencarian dan Stok
- **Input/Tindakan:** Menu 6, cari `Kopi Susu`. Lalu menu 9.
- **Hasil yang diharapkan:** Hasil dan peringatan tepat.
- **Hasil pengujian:** Pencarian menampilkan Kopi Susu (nomor 6, harga 8000, stok 12) dan pesan `Ditemukan 1 produk.` Menu 9 menampilkan peringatan untuk Air Mineral dengan stok 4.
- **Keterangan:** Pencarian tidak peka huruf besar/kecil. Peringatan muncul untuk setiap produk dengan stok kurang dari 5.

![Skenario 7 - Pencarian dan stok menipis](screenshots/07-cari-dan-stok.png)

### Pengujian Tambahan: Validasi Menu
- **Input/Tindakan:** Memilih menu `15`, lalu `0` untuk keluar.
- **Hasil pengujian:** Muncul pesan `[!] Pilihan menu tidak tersedia.` dan program kembali ke menu. Pilihan `0` mengakhiri program dengan pesan `Terima kasih. Program selesai.`

![Validasi menu dan keluar](screenshots/08-validasi-menu.png)

## Struktur Repository

```
Proyek-Array-C-C2583207036/
├── main.c
├── README.md
└── screenshots/
    ├── 01-tambah-produk.png
    ├── 02-transaksi.png
    ├── 03-stok-sesudah.png
    ├── 04-stok-tidak-cukup.png
    ├── 05-laporan.png
    ├── 06-urut-produk.png
    ├── 07-cari-dan-stok.png
    └── 08-validasi-menu.png
```
