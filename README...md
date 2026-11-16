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

Data produk disimpan dalam beberapa array paralel. Indeks yang sama pada setiap array menunjukkan produk yang sama.

| Array | Tipe | Fungsi |
|-------|------|--------|
| `nama[MAX_PRODUK][PANJANG_NAMA]` | array karakter 2 dimensi | Menyimpan nama produk |
| `harga[MAX_PRODUK]` | array 1 dimensi (`int`) | Menyimpan harga produk |
| `stok[MAX_PRODUK]` | array 1 dimensi (`int`) | Menyimpan stok produk |
| `terjual[MAX_PRODUK]` | array 1 dimensi (`int`) | Menyimpan jumlah unit terjual |
| `pendapatan[MAX_PRODUK]` | array 1 dimensi (`long`) | Menyimpan akumulasi pendapatan per produk |
| `jumlah` | `int` | Jumlah produk aktif (indeks 0 sampai `jumlah - 1`) |

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

### Skenario 1: Tambah Produk
- **Input/Tindakan:** Menu 2, isi nama `Roti Bakar`, harga `7000`, stok awal `10`. Lalu buka menu 1.
- **Hasil yang diharapkan:** Produk "Roti Bakar" muncul di daftar sebagai nomor 7.
- **Keterangan:** Produk disimpan pada indeks `jumlah` (6), lalu `jumlah` menjadi 7.

![Skenario 1 - Tambah produk](screenshots/01-tambah-produk.png)

### Skenario 2: Transaksi
- **Input/Tindakan:** Menu 4, beli Nasi Goreng x2, Es Teh Manis x3, Gorengan x5. Ketik `0` untuk selesai.
- **Hasil yang diharapkan:** Subtotal: 24.000 + 12.000 + 10.000. Total pembayaran 46.000.
- **Keterangan:** Subtotal = harga x jumlah, total = jumlah semua subtotal.

![Skenario 2 - Transaksi](screenshots/02-transaksi.png)

### Skenario 3: Pembaruan Stok
- **Input/Tindakan:** Konfirmasi transaksi skenario 2 dengan `y`, lalu buka menu 1.
- **Hasil yang diharapkan:** Stok Nasi Goreng 20 menjadi 18, Es Teh Manis 30 menjadi 27, Gorengan 40 menjadi 35. Jumlah terjual masing-masing bertambah 2, 3, dan 5.
- **Keterangan:** Stok dikurangi dan `terjual[]` ditambah hanya setelah transaksi dikonfirmasi.

![Skenario 3 - Stok sebelum transaksi](screenshots/03a-stok-sebelum.png)
![Skenario 3 - Stok sesudah transaksi](screenshots/03b-stok-sesudah.png)

### Skenario 4: Stok Tidak Cukup
- **Input/Tindakan:** Menu 4, pilih Air Mineral (stok 4), jumlah beli `10`. Lalu buka menu 1.
- **Hasil yang diharapkan:** Muncul pesan stok tidak cukup, item tidak ditambahkan ke transaksi, stok Air Mineral tetap 4.
- **Keterangan:** Program membandingkan jumlah beli dengan stok sebelum item masuk ke keranjang.

![Skenario 4 - Penolakan](screenshots/04a-stok-tidak-cukup.png)
![Skenario 4 - Stok tetap](screenshots/04b-stok-tetap.png)

### Skenario 5: Laporan
- **Input/Tindakan:** Menu 8 (setelah transaksi skenario 2).
- **Hasil yang diharapkan:** Total unit terjual 10, total pendapatan 46.000, produk terlaris Gorengan (5 unit).
- **Keterangan:** Total unit = 2 + 3 + 5. Gorengan memiliki jumlah terjual tertinggi.

![Skenario 5 - Laporan](screenshots/05-laporan.png)

### Skenario 6: Pengurutan
- **Input/Tindakan:** Menu 7, pilih `1` (harga termurah). Ulangi menu 7, pilih `2` (jumlah terjual terbanyak).
- **Hasil yang diharapkan:** Urutan harga dimulai dari Gorengan (2.000) hingga Nasi Goreng (12.000). Urutan terjual dimulai dari Gorengan (5), Es Teh Manis (3), Nasi Goreng (2).
- **Keterangan:** Pengurutan memakai bubble sort pada array indeks.

![Skenario 6 - Urut harga](screenshots/06a-urut-harga.png)
![Skenario 6 - Urut terjual](screenshots/06b-urut-terjual.png)

### Skenario 7: Pencarian dan Stok
- **Input/Tindakan:** Menu 6, cari `kopi`. Lalu menu 9.
- **Hasil yang diharapkan:** Pencarian menampilkan Kopi Susu. Menu 9 menampilkan peringatan untuk Air Mineral (stok 4).
- **Keterangan:** Pencarian tidak peka huruf besar/kecil. Peringatan muncul untuk setiap produk dengan stok kurang dari 5.

![Skenario 7 - Pencarian](screenshots/07a-cari-produk.png)
![Skenario 7 - Stok menipis](screenshots/07b-stok-menipis.png)

## Struktur Repository

```
Proyek-Array-C-C2583207036/
├── main.c
├── README.md
└── screenshots/
```
