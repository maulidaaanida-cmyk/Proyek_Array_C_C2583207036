# Sistem Monitoring Stok dan Penjualan Kantin Kampus

## Identitas

| Keterangan   | Isi                                   |
|--------------|---------------------------------------|
| Nama Lengkap | [ISI NAMA LENGKAP]                    |
| NIM          | [ISI NIM]                             |
| Kelas        | [PTI3A / PTI3B]                       |
| Mata Kuliah  | Algoritma dan Pemrograman Lanjutan    |
| Dosen        | Taofik Muhammad, M.Kom                |
| Nama Proyek  | Sistem Monitoring Stok dan Penjualan Kantin Kampus |

## Deskripsi Singkat

Aplikasi console berbahasa C untuk membantu pengelola kantin kampus mencatat produk, memantau stok, memproses transaksi penjualan, serta melihat laporan penjualan. Seluruh data disimpan di dalam array selama program berjalan (tanpa database atau file eksternal). Kapasitas maksimal 50 produk dengan [ISI JUMLAH] produk sebagai data awal.

## Daftar Fitur

1. Lihat Daftar Produk (nama, harga, stok)
2. Tambah Produk
3. Ubah Produk (harga dan stok)
4. Transaksi Penjualan (beberapa jenis produk dalam satu transaksi)
5. Tambah Stok
6. Cari Produk berdasarkan nama
7. Urutkan Produk (harga termurah dan jumlah terjual terbanyak)
8. Laporan Penjualan (jumlah terjual per produk, total unit, total pendapatan, produk terlaris)
9. Cek Stok Menipis (stok < 5)
0. Keluar

Validasi input: pilihan menu tidak tersedia, nomor produk tidak ditemukan, jumlah beli nol/negatif, harga/stok negatif, jumlah beli melebihi stok, dan kapasitas array penuh.

## Penjelasan Penggunaan Array

Data disimpan dalam array dengan indeks yang saling berkaitan (indeks yang sama = produk yang sama).

| Array | Tipe | Fungsi |
|-------|------|--------|
| `[NAMA_ARRAY_NAMA]` | array karakter 2 dimensi | Menyimpan nama produk |
| `[NAMA_ARRAY_HARGA]` | array 1 dimensi | Menyimpan harga produk |
| `[NAMA_ARRAY_STOK]` | array 1 dimensi | Menyimpan stok produk |
| `[NAMA_ARRAY_TERJUAL]` | array 1 dimensi | Menyimpan jumlah terjual |
| `[VARIABEL_JUMLAH]` | integer | Jumlah data produk aktif |

Contoh deklarasi (sesuaikan dengan kode Anda):

```c
#define MAKS_PRODUK 50
#define PANJANG_NAMA [ISI]

char   [array_nama][MAKS_PRODUK][PANJANG_NAMA];
[tipe] [array_harga][MAKS_PRODUK];
int    [array_stok][MAKS_PRODUK];
int    [array_terjual][MAKS_PRODUK];
int    [jumlah_produk];
```

**Cara kerja singkat:**
- Produk baru ditambahkan pada indeks `[jumlah_produk]`, lalu `[jumlah_produk]` bertambah satu.
- Penambahan ditolak jika `[jumlah_produk]` sudah mencapai 50.
- Pencarian: [JELASKAN ALGORITMA ANDA, misalnya sequential search]
- Pengurutan: [JELASKAN ALGORITMA ANDA, misalnya bubble sort] pada array harga dan jumlah terjual, dengan semua array ikut ditukar agar data tetap sinkron.

## Cara Kompilasi dan Menjalankan

Prasyarat: compiler GCC.

```bash
gcc [nama_file].c -o kantin
```

Menjalankan di Linux/Mac:

```bash
./kantin
```

Menjalankan di Windows:

```bash
kantin.exe
```

## Pengujian

### Skenario 1: Tambah Produk
- **Input/Tindakan:** Menu 2, masukkan produk baru: [NAMA, HARGA, STOK]. Lalu buka menu 1.
- **Hasil yang diharapkan:** Produk muncul dalam daftar.
- **Keterangan:** [JELASKAN SINGKAT]

![Skenario 1](screenshots/01-tambah-produk.png)

### Skenario 2: Transaksi
- **Input/Tindakan:** Menu 4, beli beberapa produk: [PRODUK x JUMLAH, ...].
- **Hasil yang diharapkan:** Subtotal dan total benar.
- **Keterangan:** [JELASKAN SINGKAT, sertakan hitungan manual]

![Skenario 2](screenshots/02-transaksi.png)

### Skenario 3: Pembaruan Stok
- **Input/Tindakan:** Selesaikan transaksi pada skenario 2, lalu buka menu 1.
- **Hasil yang diharapkan:** Stok dan jumlah terjual berubah.
- **Keterangan:** [STOK SEBELUM -> SESUDAH]

![Skenario 3 - sebelum](screenshots/03a-stok-sebelum.png)
![Skenario 3 - sesudah](screenshots/03b-stok-sesudah.png)

### Skenario 4: Stok Tidak Cukup
- **Input/Tindakan:** Menu 4, beli melebihi stok: [PRODUK x JUMLAH].
- **Hasil yang diharapkan:** Transaksi ditolak; stok tetap.
- **Keterangan:** [JELASKAN SINGKAT]

![Skenario 4 - penolakan](screenshots/04a-stok-tidak-cukup.png)
![Skenario 4 - stok tetap](screenshots/04b-stok-tetap.png)

### Skenario 5: Laporan
- **Input/Tindakan:** Menu 8.
- **Hasil yang diharapkan:** Unit, pendapatan, dan produk terlaris benar.
- **Keterangan:** [JELASKAN SINGKAT]

![Skenario 5](screenshots/05-laporan.png)

### Skenario 6: Pengurutan
- **Input/Tindakan:** Menu 7, urutkan berdasarkan harga lalu jumlah terjual.
- **Hasil yang diharapkan:** Harga dari termurah; jumlah terjual dari terbanyak.
- **Keterangan:** [JELASKAN SINGKAT]

![Skenario 6 - harga](screenshots/06a-urut-harga.png)
![Skenario 6 - terjual](screenshots/06b-urut-terjual.png)

### Skenario 7: Pencarian dan Stok
- **Input/Tindakan:** Menu 6 cari produk: [NAMA]. Lalu menu 9 untuk cek stok < 5.
- **Hasil yang diharapkan:** Hasil pencarian dan peringatan stok menipis tepat.
- **Keterangan:** [JELASKAN SINGKAT]

![Skenario 7 - pencarian](screenshots/07a-cari-produk.png)
![Skenario 7 - stok menipis](screenshots/07b-stok-menipis.png)

## Struktur Repository

```
Proyek-Array-C-[NIM]/
├── [nama_file].c
├── README.md
└── screenshots/
```
