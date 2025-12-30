Skema data untuk sistem manajemen aset di **SMAN 62 Jakarta** dirancang sebagai basis data relasional yang terstruktur untuk memenuhi regulasi **Barang Milik Daerah (BMD)** Provinsi DKI Jakarta. Arsitektur data ini mengintegrasikan profil institusi di Kramat Jati dengan standar teknis akuntansi pemerintah.

Berikut adalah komponen utama dari skema data tersebut:

### 1. Strategi Arsitektur Basis Data
*   **Asset Master & Inheritance:** Skema ini menggunakan strategi **"Table-per-Type"** atau *Class Table Inheritance*, di mana tabel pusat (**Asset Master**) menyimpan atribut umum seluruh BMD. Tabel ini terhubung secara *one-to-one* ke tabel rincian spesifik berdasarkan kategori **Kartu Inventaris Barang (KIB)**.
*   **Spesialisasi KIB:** Data dibagi menjadi rincian untuk **KIB A (Tanah)**, **KIB B (Peralatan & Mesin)** seperti 44 unit iMac, **KIB C (Gedung)** seperti Sasana Krida, hingga **KIB E (Aset Tetap Lainnya)** untuk koleksi perpustakaan.
*   **Hirarki Spasial:** Entitas **Locations** memodelkan realitas fisik sekolah, di mana aset dikaitkan dengan **Ruangan** yang berada di dalam **Gedung** tertentu di area kampus SMAN 62


### 2. Standar Pengkodean Hirarkis
Skema data wajib mampu menyimpan dan memvalidasi dua format kode identifikasi ganda:
*   **Kode Barang (12 Digit):** Mengikuti Permendagri 108/2016 yang merinci klasifikasi aset dari tingkat Akun (seperti Aset Tetap) hingga Sub-sub Detail Objek (seperti laptop atau PC unit).
*   **Kode Lokasi (24 Digit):** Menentukan "rumah" administratif aset, yang mencakup kode Provinsi DKI (31), Jakarta Timur (05), ID unik SMAN 62 Jakarta, hingga tahun perolehan aset.

### 3. Entitas Transaksional dan Siklus Hidup
Basis data tidak hanya mencatat inventaris statis, tetapi juga siklus hidup aset melalui tabel transaksi:
*   **Mutations:** Mencatat setiap pergerakan aset antar ruang (misalnya pemindahan proyektor ke ruang kelas) untuk menghasilkan **Kartu Inventaris Ruangan (KIR)** yang akurat.
*   **Maintenance:** Menyimpan riwayat perbaikan, biaya, dan vendor yang menangani fasilitas sekolah.
*   **Condition History:** Melacak status fisik barang dalam kategori **Baik (B)**, **Rusak Ringan (RR)**, atau **Rusak Berat (RB)** berdasarkan hasil opname fisik rutin.

### 4. Logika Keuangan dan Tata Kelola
*   **Ambang Kapitalisasi:** Sistem menerapkan logika kondisional di mana hanya barang dengan nilai perolehan minimal **Rp 750.000,00** (untuk peralatan kantor) yang dicatat sebagai aset tetap, sementara di bawah itu masuk ke tabel persediaan.
*   **Penyusutan (Depreciation):** Menggunakan metode **garis lurus (*straight-line*)** untuk menghitung nilai buku aset setiap tahun berdasarkan masa manfaat (contoh: 5 tahun untuk komputer, 20-40 tahun untuk gedung permanen).
*   **Role-Based Access Control (RBAC):** Hak akses didefinisikan secara ketat sesuai peran organisasi, mulai dari **Kepala Sekolah** (Nurita Siregar/Rina Astuti) untuk persetujuan, **Operator** (Wijo Sunarko) untuk input data, hingga **Koordinator Lab** (Achmad Taufiq) untuk pengecekan kondisi.

***

**Analogi:**
Skema data ini ibarat **sistem rekam medis digital bagi setiap benda di sekolah**. Asset Master adalah **nomor induk kependudukan** benda tersebut, tabel KIB adalah **riwayat kesehatan spesifik** (apakah ia tanah atau komputer), sedangkan tabel mutasi dan kondisi adalah **log perjalanan dan pemeriksaan fisik** harian yang memastikan setiap "pasien" (barang) selalu terlacak posisinya dan diketahui tingkat kesehatannya.