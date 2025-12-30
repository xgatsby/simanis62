# System Boundary (Batasan Sistem) - SIMANIS 62

Dokumen ini menetapkan garis tegas antara apa yang **dikelola** oleh aplikasi SIMANIS 62 (Sistem Manajemen Aset SMAN 62 Jakarta) dan apa yang berada di **luar kendali** sistem. Definisi ini krusial untuk mencegah *scope creep* (perluasan fitur tanpa kendali) dan memastikan fokus pengembangan pada manajemen inventaris sekolah.

---

## 1. Diagram Konteks (High-Level Scope)

**SIMANIS 62** berfokus secara eksklusif pada **Siklus Hidup Fisik & Administratif Aset**; mulai dari barang diterima sekolah, dicatat, digunakan, dipelihara, hingga dihapuskan. Sistem ini **BUKAN** sistem keuangan (Accounting) penuh dan **BUKAN** sistem pengadaan (Procurement/e-Katalog).

---

## 2. In-Scope (Dalam Batasan Sistem)

Fitur dan proses berikut adalah tanggung jawab utama SIMANIS 62:

### A. Manajemen Inventaris Inti (Core Inventory)
1.  **Pencatatan Aset (Receiving):** Input data detail aset baru sesuai field KIB A (Tanah), B (Peralatan), C (Gedung), D (Jalan/Jaringan), dan E (Lainnya).
2.  **Digital Tagging:** Pembuatan otomatis ID unik (`SMAN62-YYYY-XXXX`) dan generasi QR Code untuk setiap unit barang.
3.  **Manajemen Lokasi:** Pemetaan hierarkis lokasi sekolah (Gedung -> Lantai -> Ruangan) untuk mengetahui posisi eksak aset.
4.  **Kondisi Fisik:** Pelacakan status kondisi (Baik, Rusak Ringan, Rusak Berat) secara *real-time*.

### B. Sirkulasi & Pelacakan (Circulation)
5.  **Check-out / Check-in:** Peminjaman aset kepada personil (Guru/Staf) atau penempatan tetap di ruangan.
6.  **Mutasi Ruangan:** Perpindahan permanen aset antar ruangan yang mengubah Kartu Inventaris Ruangan (KIR).
7.  **Audit / Stock Opname:** Modul untuk mencocokkan data sistem dengan fisik lapangan menggunakan scan QR code.

### C. Kepatuhan & Laporan (Compliance)
8.  **Depresiasi Pemerintah:** Perhitungan penyusutan nilai aset menggunakan metode Garis Lurus (*Straight Line*) sesuai parameter umur ekonomis Permendagri.
9.  **Laporan KIB & KIR:** Generasi dokumen PDF yang diformat persis sesuai standar pelaporan Dinas Pendidikan DKI Jakarta.
10. **Kapitalisasi Otomatis:** Logika pemisahan otomatis antara "Aset Tetap" (Intrakomptabel) dan "Barang Persediaan" (Ekstrakomptabel) berdasarkan ambang batas harga.

### D. Manajemen Pengguna Terbatas
11. **User Directory:** Data Guru dan Tendik (Tenaga Kependidikan) hanya disimpan sebatas Nama, NIP, dan Email untuk keperluan peminjaman barang.

---

## 3. Out-of-Scope (Di Luar Batasan Sistem)

Proses berikut secara eksplisit **TIDAK AKAN** ditangani oleh SIMANIS 62:

### A. Keuangan & Penganggaran
*   **Sistem Penggajian (Payroll):** Pembayaran gaji guru/karyawan.
*   **Proses Lelang/Pengadaan:** Sistem tidak menangani proses bidding vendor atau e-Katalog. SIMANIS 62 baru bekerja **setelah** barang dibeli dan tiba di sekolah (BAST - Berita Acara Serah Terima).
*   **SPP Sekolah:** Pembayaran keuangan siswa.

### B. Akademik & Siswa
*   **Dapodik Sync Real-time:** Tidak ada sinkronisasi *two-way* otomatis dengan Dapodik pusat (risiko keamanan tinggi).
*   **Nilai Siswa (e-Rapor):** Tidak menyimpan data nilai atau presensi siswa.
*   **Detail Siswa:** Data siswa (Peminjam) hanya bersifat sementara/minimal (Nama & Kelas) jika siswa meminjam alat lab, bukan database kesiswaan lengkap.

### C. Manajemen Konstruksi
*   **Detail Gambar Teknik Gedung:** Sistem mencatat "Ada Gedung A seluas 500m2", tapi tidak menyimpan cetak biru arsitektur (CAD Files) atau manajemen proyek konstruksi renovasi.

---

## 4. Antarmuka Sistem Eksternal (External Interfaces)

SIMANIS 62 berinteraksi dengan entitas luar melalui titik-titik berikut:

### A. Perangkat Keras (Hardware Interfaces)
*   **Barcode/Label Printer:** Sistem harus mampu mengirim perintah cetak ke printer label standar (via Browser Print dialog).
*   **Mobile Scanner (Android/iOS):** Sistem diakses melalui *Mobile Browser* yang memanfaatkan kamera HP untuk memindai QR Code.

### B. Format Data Eksternal (Software Interfaces)
*   **Dinas Pendidikan (Export):** Sistem menyediakan menu "Export Excel" dengan format kolom yang **persis** dengan template pelaporan Aset Daerah (SIPKD/e-Aset Provinsi), untuk memudahkan upload manual oleh Operator. (Bukan Direct API Integrasi karena alasan birokrasi/security Pemprov).
*   **Import Data Awal:** Sistem menerima file CSV/Excel untuk migrasi data inventaris lama (Excel Spreadsheet sekolah).

---

## 5. Batasan Teknis & Operasional

*   **Konektivitas:** Sistem berjalan di Jaringan Lokal (LAN/Intranet) SMAN 62 untuk keamanan, atau Private Cloud dengan VPN jika dibutuhkan akses luar.
*   **Bahasa Pengantar:** Antarmuka sistem menggunakan **Bahasa Indonesia** baku untuk istilah aset, namun basis kode tetap menggunakan Bahasa Inggris.
*   **Kapasitas:** Sistem dirancang untuk menampung hingga ~50.000 record aset (Skala menengah) tanpa degradasi performa signifikan.

