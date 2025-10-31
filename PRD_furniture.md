# DOKUMEN PERSYARATAN PRODUK (PRD)

## Nama Produk: Aplikasi Furniture [tripel f]

**Versi:** 1.1 (Final)
**Tanggal Dibuat:** 31 Oktober 2025
**Pembuat:** [-Muhammad Fandi Kusuma(221240001295)]
            [-Muhammad Fahcri Husaini(221240001286)]

---

## 1. Pendahuluan

### 1.1 Tujuan dan Visi Produk
Dokumen ini mendefinisikan persyaratan fungsional dan non-fungsional untuk pengembangan aplikasi mobile e-commerce furniture berbasis **Flutter**.

* **Visi:** Menyediakan platform digital yang efisien dan transparan bagi pelanggan untuk membeli furniture siap pakai, memesan kustom, dan memonitor proses pengerjaan secara real-time.
* **Lingkup:** Aplikasi mobile (iOS & Android) dengan dua peran pengguna utama (Pelanggan dan Admin).

---

## 2. Sasaran (Goals)

| Kategori | Tujuan (Goal) | Metrik Keberhasilan (Success Metric) |
| :--- | :--- | :--- |
| Pelanggan | Memudahkan pemesanan dan pembelian furniture. | Minimal 80% transaksi berhasil diselesaikan melalui aplikasi. |
| Pelanggan | Meningkatkan transparansi proses pengerjaan. | Pelanggan mengakses fitur monitoring minimal 2 kali per pesanan. |
| Admin | Mempercepat pengelolaan inventaris dan pesanan. | Waktu pemrosesan pesanan baru berkurang 50% dibandingkan metode manual. |

---

## 3. Fitur dan Persyaratan Fungsional

### 3.1 Halaman/Modul Pelanggan

| ID Fitur | Nama Fitur | Deskripsi Persyaratan | Kebutuhan Teknis |
| :--- | :--- | :--- | :--- |
| P-1 | **Autentikasi User** | Login, Register, Lupa Password. | Koneksi ke DB User. |
| P-2 | **Katalog Produk** | Menampilkan produk dengan filter dan pencarian. | Data diambil dari DB Produk. |
| P-3 | **Pemesanan Barang** | Memilih produk, menentukan kuantitas. | Logika pengecekan stok. |
| P-4 | **Keranjang Belanja** | Daftar item, ubah kuantitas, hitung subtotal, checkout. | Kalkulasi dinamis harga total. |
| **P-7** | **Kalkulasi Biaya Admin** | **Menerapkan biaya admin sebesar 5% dari subtotal pesanan (sebelum ongkos kirim) pada proses checkout.** | Logika kalkulasi harga total: `Total = Subtotal Produk + Biaya Admin (5% dari Subtotal) + Ongkos Kirim`. |
| P-5 | **Monitoring Pengerjaan** | Menampilkan detail pesanan dan status proses pengerjaan (Timeline/Stepper). | Data status diambil real-time dari DB Pesanan. |
| P-6 | **Rating & Ulasan** | Memberikan bintang (1-5) dan komentar untuk produk yang telah selesai/diterima. | Menyimpan data rating ke DB Ulasan. |

### 3.2 Halaman/Modul Admin

| ID Fitur | Nama Fitur | Deskripsi Persyaratan | Kebutuhan Teknis |
| :--- | :--- | :--- | :--- |
| A-1 | **Autentikasi Admin** | Login khusus untuk user dengan role Admin. | Pengecekan role user saat login. |
| A-2 | **CRUD Produk** | Create, Read, Update, Delete produk furniture. | Form input validasi, fitur Image Upload. |
| A-3 | **Pesanan Masuk** | Melihat daftar semua pesanan, detail pesanan, filter status. | Akses ke DB Pesanan. |
| A-4 | **Update Status Pengerjaan** | Mengubah status pesanan secara manual/bertingkat. | Logika update status harus memicu P-5 di sisi Pelanggan. |
| A-5 | **Lihat Ulasan Pelanggan** | Menampilkan semua ulasan (P-6) yang masuk, dilengkapi rata-rata rating. | Tampilan ringkas, bisa filter per produk. |

---

## 4. Persyaratan Non-Fungsional

### 4.1 Persyaratan Kinerja (Performance)
* Waktu loading halaman utama: $\le 3$ detik.
* Waktu respons API untuk CRUD: $\le 1$ detik.
* Pencarian produk: $\le 2$ detik.

### 4.2 Persyaratan Keamanan (Security)
* Semua password user harus di-hash 
* Komunikasi client-server menggunakan HTTPS/SSL.
* Implementasi Role-Based Access Control (RBAC) untuk pemisahan akses Pelanggan dan Admin.

### 4.3 Persyaratan Teknologi
* **Framework:** Flutter.
* **Bahasa:** Dart.
* **Backend & DB:** [Appwrite].
* **State Management:** [Provider.]

---

## 5. Model Data (Skema Minimum)

| Entitas | Atribut Penting (Contoh) | Keterangan |
| :--- | :--- | :--- |
| **User** | `id`, `nama`, `email`, `password_hash`, `role`. | Autentikasi. |
| **Produk** | `id`, `nama_produk`, `harga`, `stok`, `gambar_url`. | Katalog. |
| **Pesanan** | `id_pesanan`, `id_user`, `tanggal`, **`subtotal_produk`**, **`biaya_admin`**, `total_bayar`, `status_proses`. | Riwayat, Monitoring, dan detail Biaya. |
| **Ulasan** | `id`, `id_produk`, `id_user`, `rating_bintang`, `komentar`. | Rating Penjualan. |

---