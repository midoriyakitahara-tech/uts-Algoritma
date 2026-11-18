# uts-Algoritma
# Kantin Kampus - Monitoring Stok & Penjualan

Aplikasi konsol berbahasa **C** untuk memantau stok dan penjualan sebuah kantin kampus.

## Deskripsi

Program ini memungkinkan pengguna melihat, menambah, mengubah, dan mencari produk, mencatat transaksi dengan banyak item sekaligus, mengurutkan produk, melihat laporan penjualan, serta mengecek stok yang menipis. Data awal berisi 13 produk (makanan Jepang dan minuman tradisional) yang disimpan di memori selama program berjalan.

## Daftar Fitur

| No | Fitur | Penjelasan singkat |
|----|-------|--------------------|
| 1 | Lihat Daftar Produk | Menampilkan semua produk beserta nomor, harga, dan stok dalam bentuk tabel. |
| 2 | Tambah Produk | Menambah produk baru (nama, harga, stok awal). Menolak nama kosong atau duplikat (huruf besar/kecil diabaikan), angka negatif, dan kapasitas penuh. |
| 3 | Ubah Produk | Mengubah harga dan stok produk yang dipilih berdasarkan nomor. |
| 4 | Transaksi Penjualan | Memilih beberapa produk dalam satu transaksi, menampilkan rincian dan total, lalu meminta konfirmasi. Stok baru berkurang jika dikonfirmasi `y`. |
| 5 | Tambah Stok | Menambah stok produk yang dipilih dengan jumlah lebih dari 0. |
| 6 | Cari Produk | Mencari produk berdasarkan sebagian nama, tanpa memedulikan huruf besar/kecil. |
| 7 | Urutkan Produk | Mengurutkan tampilan berdasarkan harga termurah atau jumlah terjual terbanyak (bubble sort). |
| 8 | Laporan Penjualan | Menampilkan unit terjual dan pendapatan per produk, total keseluruhan, dan produk terlaris. |
| 9 | Cek Stok Menipis | Menampilkan produk yang stoknya kurang dari 5 (`BATAS_MENIPIS`). |
| 0 | Keluar | Mengakhiri program. |

## Menu Awal Produk

- **Makanan:** Shio Ramen, Miso Ramen, Yakisoba, Tempura, Onigiri Tuna, Onigiri Ayam, Takoyaki, Okonomiyaki
- **Minuman:** Air Mineral, Bandrek, Bajigur, Kopi Tubruk, Lahang

Harga dan stok awal dapat diubah pada fungsi `isiDataAwal()` (array `h[]` dan `s[]`).

## Penggunaan Array

### 1. Array paralel untuk data produk

Data produk disimpan dalam beberapa array global yang indeksnya saling berpasangan. Elemen ke-`i` dari semua array menggambarkan produk yang sama.

| Array | Tipe | Isi |
|-------|------|-----|
| `nama[MAX_PRODUK][PANJANG_NAMA]` | array 2 dimensi `char` | Nama produk (baris = produk, kolom = karakter, maksimal 29 karakter + `\0`) |
| `harga[MAX_PRODUK]` | `int` | Harga satuan |
| `stok[MAX_PRODUK]` | `int` | Stok tersedia |
| `terjual[MAX_PRODUK]` | `int` | Total unit terjual |
| `pendapatan[MAX_PRODUK]` | `long` | Akumulasi pendapatan per produk |

Contoh: `nama[2]`, `harga[2]`, dan `stok[2]` semuanya milik produk nomor 3. Variabel `jumlah` mencatat banyaknya produk aktif, sehingga perulangan cukup berjalan dari `0` sampai `jumlah - 1`. Kapasitas maksimal ditentukan oleh `MAX_PRODUK` (50).

### 2. Array bantu di dalam fungsi

- `qtyBeli[MAX_PRODUK]` (di `transaksiPenjualan`): menyimpan jumlah beli per produk dalam satu transaksi, sehingga stok asli baru diubah setelah transaksi dikonfirmasi. Array ini juga dipakai untuk memeriksa apakah total pembelian melebihi stok.
- `urut[MAX_PRODUK]` (di `urutkanProduk`): menyimpan **indeks** produk, bukan datanya. Yang diurutkan hanya indeksnya, sehingga array utama tidak berubah dan nomor produk di menu tetap konsisten.
- `buf[64]`, `namaBaru[]`, `kata[]`, dan sejenisnya: array `char` sebagai buffer input teks, dibaca dengan `fgets` agar aman dari buffer overflow.

### 3. Array saat inisialisasi

Di `isiDataAwal()`, array `n[]`, `h[]`, dan `s[]` menampung data awal, lalu disalin ke array utama dengan perulangan. Jumlah produk dihitung otomatis dengan `sizeof(n) / sizeof(n[0])`.

## Kompilasi dan Menjalankan

Simpan kode dengan nama `kantin.c`, lalu kompilasi.

### Linux / macOS / WSL (GCC)

```bash
gcc kantin.c -o kantin -Wall -Wextra
./kantin
```

### Windows (MinGW/GCC)

```bash
gcc kantin.c -o kantin.exe -Wall -Wextra
kantin.exe
```

### Code::Blocks / Dev-C++ / VS Code

Buka file `kantin.c`, lalu pilih *Build and Run* (biasanya F9 atau Ctrl+F10).

## Cara Penggunaan

1. Setelah program berjalan, menu utama tampil. Ketik angka menu (0-9) lalu tekan Enter.
2. Untuk transaksi (menu 4), masukkan nomor produk dan jumlah beli. Ulangi untuk produk lain, ketik `0` jika sudah selesai, lalu ketik `y` untuk memproses atau `n` untuk membatalkan.
3. Ketik `0` di menu utama untuk keluar.

## Catatan

Data hanya tersimpan di memori. Semua perubahan hilang saat program ditutup, dan program kembali ke data awal ketika dijalankan lagi.

## Hasil Skenario

- skenario 1: tambah produk

  sebelum:
  <img width="959" height="539" alt="Screenshot 2026-11-18 194252" src="https://github.com/user-attachments/assets/63abbb6a-2b3d-438a-9e2c-920a82ae5cc1" />

  sesudah:
  <img width="959" height="539" alt="Screenshot 2026-11-18 194347" src="https://github.com/user-attachments/assets/62e76052-35d0-4019-8de5-b3d77086e929" />
  
- skenario 2: transaksi produk
  <img width="959" height="539" alt="Screenshot 2026-11-18 195155" src="https://github.com/user-attachments/assets/d137a656-358f-4421-bf2c-a0a9ed9f5741" />

- skenario 3: perbaruan stok

  stok sebelum diubah setelah penjualan:
  <img width="959" height="535" alt="Screenshot 2026-11-18 195635" src="https://github.com/user-attachments/assets/b681053c-8332-44cf-9387-8796c003ccc8" />

  stok setelah diubah setelah penjualan:
  <img width="960" height="540" alt="Screenshot 2026-11-18 200032" src="https://github.com/user-attachments/assets/4f827bdc-58e3-48b8-ab89-265d291cb6b4" />

- skenario 4: Pembelian melebihi stok dan stok tetap setelah transaksi dibatalkan

  stok sebelum dibatalkan:
  
  <img width="515" height="539" alt="Screenshot 2026-11-18 201032" src="https://github.com/user-attachments/assets/e94dbebb-9d62-4548-9923-63c785c41f61" />

  stok setelah dibatalkan(stok okonomiyaki(no.8)tetap):
  
  <img width="610" height="539" alt="Screenshot 2026-11-18 201024" src="https://github.com/user-attachments/assets/d8e8ca27-9e1b-4dde-8777-76903792c418" />

- skenario 5: Laporan penjualan

  <img width="960" height="540" alt="Screenshot 2026-11-18 201444" src="https://github.com/user-attachments/assets/cc67c684-df3b-42c0-bce7-f51aa00b25f4" />

- skenario 6: pengurutan produk (berdasarkan yang termurah)
  
  <img width="959" height="539" alt="Screenshot 2026-11-18 201618" src="https://github.com/user-attachments/assets/0d3c28ff-2580-479b-bf48-f107017f0f2d" />

- skenario 7: pencarian produk dan peringatan saat stok produk <5

  <img width="614" height="401" alt="Screenshot 2026-11-18 202017" src="https://github.com/user-attachments/assets/8dcdca2a-ff13-4d5b-84dd-08c08962d219" />


