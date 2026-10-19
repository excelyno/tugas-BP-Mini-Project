# Sistem Kasir Sederhana Warung Makan

> **Mata kuliah:** Pemrograman Dasar (C++)
> **Kelompok:** 4 orang, 1 anggota = 1 fitur = 1 `case`
> **Materi yang dipakai:** `struct`, array, array 2D, `for`, `while`, `do-while`, `if-else`

---

## 1. Studi Kasus

Sebuah warung makan memiliki **5 menu** yang dijual. Buat program C++ sederhana yang membantu kasir:

1. menampilkan daftar menu,
2. mencatat pesanan pelanggan,
3. menghitung total belanja beserta diskon dan pajak,
4. mencetak struk dan menghitung kembalian.

Program berjalan melalui menu dan terus menampilkan menu sampai pengguna memilih keluar. Setiap anggota kelompok mengerjakan **1 fitur**, yaitu **1 case** pada `switch`.

---

## 2. Ketentuan Program

1. Gunakan **array of struct** dengan tepat 5 slot untuk menyimpan data menu. Indeks yang valid adalah `0` sampai `4`.
2. Gunakan `struct Menu` untuk menyimpan data menu.
3. Data menu hanya terdiri dari `id`, `nama`, `harga`, dan `stok`.
4. Gunakan **array 2D** `int keranjang[10][2]` untuk menyimpan pesanan.
   - Kolom `0` berisi index menu pada `daftarMenu`.
   - Kolom `1` berisi jumlah yang dibeli.
   - Keranjang maksimal 10 baris.
5. Gunakan `do-while` untuk mengulang menu utama sampai pengguna memilih keluar (sudah disediakan).
6. Gunakan `for` untuk menampilkan daftar menu, mencari ID menu, dan membaca isi keranjang.
7. Gunakan `while (true)` untuk input pesanan dan `do-while` untuk validasi uang bayar.
8. Gunakan `if-else` untuk memeriksa ID menu, jumlah, stok, keranjang kosong, status hitung total, dan uang bayar.
9. Fitur 3 dan fitur 4 saling bergantung melalui variabel `sudahHitung`. Fitur 4 hanya boleh berjalan setelah total dihitung.
10. **Dilarang** memakai fungsi (`void`) maupun `vector`. Semua logika ditulis langsung di dalam `case`.
11. Jika pilihan menu tidak valid, tampilkan `Pilihan tidak valid!`.
12. Pesan yang ditampilkan harus sesuai dengan **Expected Output**.

---

## 3. Struktur Data yang Digunakan

```cpp
struct Menu {
    int id;
    string nama;
    int harga;
    int stok;
};

Menu daftarMenu[5] = {
    {1, "Nasi Goreng", 15000, 10},
    {2, "Mie Ayam",    12000, 8},
    {3, "Es Teh",       4000, 20},
    {4, "Kopi Hitam",   6000, 15},
    {5, "Roti Bakar",  10000, 5}
};
int jumlahMenu = 5;

// kolom 0 = index menu di daftarMenu, kolom 1 = jumlah beli
int keranjang[10][2];
int jumlahItem = 0;

int subtotal = 0, diskon = 0, pajak = 0, totalBayar = 0;
bool sudahHitung = false;

int pilihan;
int i, id, jumlah, idx, persen, bayar, kembalian;
```

**Data awal menu:**

| Index | ID | Nama        | Harga  | Stok |
|:-----:|:--:|-------------|-------:|-----:|
| 0     | 1  | Nasi Goreng | 15.000 | 10   |
| 1     | 2  | Mie Ayam    | 12.000 | 8    |
| 2     | 3  | Es Teh      | 4.000  | 20   |
| 3     | 4  | Kopi Hitam  | 6.000  | 15   |
| 4     | 5  | Roti Bakar  | 10.000 | 5    |

> Kerangka program (`main`, menu utama, dan deklarasi variabel di atas) **sudah disediakan**. Mahasiswa hanya mengisi bagian `case 1` sampai `case 4`.

---

## 4. Rincian Menu dan Alur Kerja

### 4.1 Daftar Menu & Harga (`case 1`)

1. Menggunakan perulangan `for (i = 0; i < jumlahMenu; i++)`.
2. Untuk setiap menu, cetak satu baris dengan format:

   ```text
   ID. Nama - RpHarga (stok: Stok)
   ```

### 4.2 Input Pesanan (`case 2`)

1. Menggunakan `while (true)`. Tampilkan `Masukkan ID menu (0 = selesai): ` lalu baca `id`. Jika `id == 0`, hentikan perulangan.
2. Cari `id` pada `daftarMenu` menggunakan `for`, simpan index-nya ke `idx` (nilai awal `-1`). Jika `idx` masih `-1`, tampilkan `ID tidak ditemukan!` lalu ulangi input.
3. Jika keranjang sudah penuh (`jumlahItem == 10`), tampilkan `Keranjang penuh!` lalu hentikan perulangan.
4. Minta input `jumlah`:
   - Jika `jumlah <= 0`, tampilkan `Jumlah harus lebih dari 0!`.
   - Jika `jumlah` lebih besar dari stok, tampilkan `Stok tidak cukup! Sisa stok: X`.
5. Jika semua valid:
   - simpan `idx` ke `keranjang[jumlahItem][0]` dan `jumlah` ke `keranjang[jumlahItem][1]`,
   - tambah `jumlahItem` sebanyak 1,
   - kurangi stok menu sebesar `jumlah`,
   - ubah `sudahHitung = false`,
   - tampilkan `<jumlah> x <nama> masuk keranjang.`

### 4.3 Hitung Total + Diskon + Pajak (`case 3`)

1. Jika `jumlahItem == 0`, tampilkan `Keranjang masih kosong! Isi pesanan dulu (menu 2).` dan selesai.
2. Jika ada isi, hitung `subtotal` = jumlah dari (`harga` x `jumlah`) setiap baris keranjang menggunakan `for`.
3. Tentukan `persen` diskon menggunakan `if - else if - else`:

   | Kondisi subtotal    | Diskon |
   |---------------------|:------:|
   | `>= 100000`         | 10%    |
   | `>= 50000`          | 5%     |
   | selain itu          | 0%     |

4. Hitung:

   ```cpp
   diskon     = subtotal * persen / 100;
   pajak      = (subtotal - diskon) * 10 / 100;
   totalBayar = subtotal - diskon + pajak;
   ```

   Semua bertipe `int`, jadi pembagian membulatkan ke bawah.
5. Ubah `sudahHitung = true`, lalu tampilkan rincian subtotal, diskon, pajak, dan total bayar.

### 4.4 Cetak Struk & Kembalian (`case 4`)

1. Jika `jumlahItem == 0`, tampilkan `Keranjang kosong, tidak ada yang dicetak!` dan selesai.
2. Jika `sudahHitung == false`, tampilkan `Total belum dihitung! Jalankan menu 3 dulu.` dan selesai.
3. Tampilkan total yang harus dibayar, lalu minta uang bayar menggunakan `do-while`. Jika `bayar < totalBayar`, tampilkan `Uang kurang Rp<selisih>, silakan masukkan lagi.` dan minta input ulang.
4. Hitung `kembalian = bayar - totalBayar`.
5. Cetak struk: daftar item (`nama: jumlah x harga = total item`) menggunakan `for`, lalu subtotal, diskon, pajak, total, bayar, dan kembalian.
6. Reset transaksi:
   - `jumlahItem = 0`
   - `subtotal = 0`, `diskon = 0`, `pajak = 0`, `totalBayar = 0`
   - `sudahHitung = false`

   Stok menu **tidak** dikembalikan.

---

## 5. Expected Output

> Teks setelah `Pilih: `, `Masukkan ID menu (0 = selesai): `, `Jumlah: `, dan `Uang bayar: Rp` adalah **input dari pengguna**.

### Contoh 1: Alur normal

Pesan, hitung total, bayar kurang lalu cukup, dan stok berkurang setelah transaksi.

```text

===== KASIR SEDERHANA =====
1. Daftar Menu & Harga
2. Input Pesanan
3. Hitung Total
4. Cetak Struk & Kembalian
0. Keluar
Pilih: 1

=== DAFTAR MENU ===
1. Nasi Goreng - Rp15000 (stok: 10)
2. Mie Ayam - Rp12000 (stok: 8)
3. Es Teh - Rp4000 (stok: 20)
4. Kopi Hitam - Rp6000 (stok: 15)
5. Roti Bakar - Rp10000 (stok: 5)

===== KASIR SEDERHANA =====
1. Daftar Menu & Harga
2. Input Pesanan
3. Hitung Total
4. Cetak Struk & Kembalian
0. Keluar
Pilih: 2

Masukkan ID menu (0 = selesai): 1
Jumlah: 3
3 x Nasi Goreng masuk keranjang.

Masukkan ID menu (0 = selesai): 2
Jumlah: 2
2 x Mie Ayam masuk keranjang.

Masukkan ID menu (0 = selesai): 0

===== KASIR SEDERHANA =====
1. Daftar Menu & Harga
2. Input Pesanan
3. Hitung Total
4. Cetak Struk & Kembalian
0. Keluar
Pilih: 3

=== RINCIAN TOTAL ===
Subtotal    : Rp69000
Diskon 5%   : Rp3450
Pajak 10%   : Rp6555
TOTAL BAYAR : Rp72105

===== KASIR SEDERHANA =====
1. Daftar Menu & Harga
2. Input Pesanan
3. Hitung Total
4. Cetak Struk & Kembalian
0. Keluar
Pilih: 4

Total yang harus dibayar: Rp72105
Uang bayar: Rp70000
Uang kurang Rp2105, silakan masukkan lagi.
Uang bayar: Rp100000

==============================
        STRUK BELANJA
==============================
Nasi Goreng: 3 x 15000 = 45000
Mie Ayam: 2 x 12000 = 24000
------------------------------
Subtotal  : Rp69000
Diskon    : Rp3450
Pajak     : Rp6555
TOTAL     : Rp72105
Bayar     : Rp100000
Kembalian : Rp27895
==============================
   Terima kasih, datang lagi!

===== KASIR SEDERHANA =====
1. Daftar Menu & Harga
2. Input Pesanan
3. Hitung Total
4. Cetak Struk & Kembalian
0. Keluar
Pilih: 1

=== DAFTAR MENU ===
1. Nasi Goreng - Rp15000 (stok: 7)
2. Mie Ayam - Rp12000 (stok: 6)
3. Es Teh - Rp4000 (stok: 20)
4. Kopi Hitam - Rp6000 (stok: 15)
5. Roti Bakar - Rp10000 (stok: 5)

===== KASIR SEDERHANA =====
1. Daftar Menu & Harga
2. Input Pesanan
3. Hitung Total
4. Cetak Struk & Kembalian
0. Keluar
Pilih: 0
Terima kasih!
```

### Contoh 2: Validasi dan error

Keranjang kosong, ID salah, jumlah salah, stok kurang, menu 4 sebelum menu 3, dan pilihan menu tidak valid.

```text

===== KASIR SEDERHANA =====
1. Daftar Menu & Harga
2. Input Pesanan
3. Hitung Total
4. Cetak Struk & Kembalian
0. Keluar
Pilih: 3
Keranjang masih kosong! Isi pesanan dulu (menu 2).

===== KASIR SEDERHANA =====
1. Daftar Menu & Harga
2. Input Pesanan
3. Hitung Total
4. Cetak Struk & Kembalian
0. Keluar
Pilih: 4
Keranjang kosong, tidak ada yang dicetak!

===== KASIR SEDERHANA =====
1. Daftar Menu & Harga
2. Input Pesanan
3. Hitung Total
4. Cetak Struk & Kembalian
0. Keluar
Pilih: 2

Masukkan ID menu (0 = selesai): 9
ID tidak ditemukan!

Masukkan ID menu (0 = selesai): 1
Jumlah: 0
Jumlah harus lebih dari 0!

Masukkan ID menu (0 = selesai): 1
Jumlah: 15
Stok tidak cukup! Sisa stok: 10

Masukkan ID menu (0 = selesai): 3
Jumlah: 2
2 x Es Teh masuk keranjang.

Masukkan ID menu (0 = selesai): 0

===== KASIR SEDERHANA =====
1. Daftar Menu & Harga
2. Input Pesanan
3. Hitung Total
4. Cetak Struk & Kembalian
0. Keluar
Pilih: 4
Total belum dihitung! Jalankan menu 3 dulu.

===== KASIR SEDERHANA =====
1. Daftar Menu & Harga
2. Input Pesanan
3. Hitung Total
4. Cetak Struk & Kembalian
0. Keluar
Pilih: 3

=== RINCIAN TOTAL ===
Subtotal    : Rp8000
Diskon 0%   : Rp0
Pajak 10%   : Rp800
TOTAL BAYAR : Rp8800

===== KASIR SEDERHANA =====
1. Daftar Menu & Harga
2. Input Pesanan
3. Hitung Total
4. Cetak Struk & Kembalian
0. Keluar
Pilih: 4

Total yang harus dibayar: Rp8800
Uang bayar: Rp10000

==============================
        STRUK BELANJA
==============================
Es Teh: 2 x 4000 = 8000
------------------------------
Subtotal  : Rp8000
Diskon    : Rp0
Pajak     : Rp800
TOTAL     : Rp8800
Bayar     : Rp10000
Kembalian : Rp1200
==============================
   Terima kasih, datang lagi!

===== KASIR SEDERHANA =====
1. Daftar Menu & Harga
2. Input Pesanan
3. Hitung Total
4. Cetak Struk & Kembalian
0. Keluar
Pilih: 7
Pilihan tidak valid!

===== KASIR SEDERHANA =====
1. Daftar Menu & Harga
2. Input Pesanan
3. Hitung Total
4. Cetak Struk & Kembalian
0. Keluar
Pilih: 0
Terima kasih!
```

---

## 6. Pembagian Tugas dan Pertanyaan Presentasi

| Anggota | Fitur                              | Bagian    |
|:-------:|------------------------------------|-----------|
| 1       | Daftar Menu & Harga                | `case 1`  |
| 2       | Input Pesanan                      | `case 2`  |
| 3       | Hitung Total + Diskon + Pajak      | `case 3`  |
| 4       | Cetak Struk & Kembalian            | `case 4`  |

Setiap anggota wajib menjelaskan:

1. Alur logika fitur yang dikerjakan langkah demi langkah.
2. Alasan memakai jenis perulangan dan percabangan tersebut.
3. Kasus error apa saja yang ditangani.
4. Apa yang terjadi jika fiturnya dijalankan pada urutan yang "salah" (misalnya menu 4 sebelum menu 3).
