# Definisi Aktor & User Stories (SIMANIS 62)

Dokumen ini mendefinisikan secara spesifik siapa saja yang terlibat dalam Sistem Manajemen Aset SMAN 62 Jakarta dan apa yang mereka lakukan, sesuai standar operasional sekolah dan regulasi BMD (Barang Milik Daerah).

---

## 1. Definisi Aktor (Actors)

Kami membagi aktor menjadi 4 kategori spesifik berdasarkan wewenang dan tanggung jawab di SMAN 62 Jakarta.

### 1.1. Administrator Aset (Operator Sekolah)
*   **Peran:** Pengelola utama sistem sehari-hari.
*   **Contoh Personil:** Bapak Wijo Sunarko (TU/Sarpras).
*   **Wewenang:**
    *   Akses Penuh (Create, Read, Update, Delete) ke seluruh database aset (KIB A-E).
    *   Mencetak Label QR Code aset.
    *   Menginput Transaksi Mutasi (Perpindahan barang antar ruang).
    *   Men-generate Laporan Resmi untuk Dinas (KIB/KIR).
    *   Mengatur konfigurasi sistem (User, Lokasi, Kategori).

### 1.2. Executive Viewer (Kepala Sekolah)
*   **Peran:** Pengambil keputusan dan pengawas aset strategis.
*   **Contoh Personil:** Ibu Rina Astuti, S.Pd.
*   **Wewenang:**
    *   **View-Only Dashboard:** Melihat ringkasan total nilai aset (Intrakomptabel vs Ekstrakomptabel).
    *   **Approval Authority:** Memberikan persetujuan digital untuk penghapusan aset (Disposal) atau pengadaan baru yang bernilai tinggi.
    *   Mengunduh Ringkasan Eksekutif Mutasi Aset Tahunan.

### 1.3. Custodian (Koordinator Ruangan/Lab)
*   **Peran:** Penanggung jawab aset di lokasi spesifik.
*   **Contoh Personil:** Bapak Achmad Taufiq (Koordinator Lab Komputer), Kepala Perpustakaan, atau Wali Kelas.
*   **Wewenang:**
    *   Melihat daftar barang *hanya* di ruangan yang mereka pimpin (Scope-limited view).
    *   Melaporkan kerusakan barang (Maintenance Request).
    *   Mengonfirmasi penerimaan barang baru di ruangannya.

### 1.4. Tim Opname (Audit Field Officer)
*   **Peran:** Petugas lapangan yang melakukan sensus fisik berkala (Stock Opname).
*   **Contoh Personil:** Staf TU yang ditunjuk atau Siswa Magang.
*   **Wewenang:**
    *   Menggunakan Mobile App/Scanner untuk memindai QR Code.
    *   Mengupdate status fisik aset (Baik -> Rusak Ringan).
    *   Tidak memiliki hak menghapus data, hanya update kondisi & lokasi terkini.

---

## 2. User Stories (Skenario Fungsional)

Berikut adalah daftar kebutuhan fungsional berdasarkan sudut pandang masing-masing aktor.

### A. Untuk Administrator Aset (Operasional Harian)
1.  **[Input Aset Baru]**
    *   Sebagai **Administrator Aset**, saya ingin **memilih Template KIB (A/B/C/D)** saat menambah barang, agar **field data yang muncul relevan** (contoh: Input Tanah tidak minta Merk Laptop).
2.  **[Cetak Label]**
    *   Sebagai **Administrator Aset**, saya ingin **mencetak QR Code secara massal** untuk 44 unit iMac baru, agar **tim lapangan bisa langsung menempelkannya**.
3.  **[Mutasi Barang]**
    *   Sebagai **Administrator Aset**, saya ingin **memindahkan lokasi aset** dari "Gudang" ke "Lab Komputer 1", agar **Kartu Inventaris Ruangan (KIR) otomatis terupdate**.
4.  **[Laporan Pemerintah]**
    *   Sebagai **Administrator Aset**, saya ingin **mengunduh PDF Laporan KIB B** per Semester, agar **sesuai dengan format pelaporan Dinas Pendidikan DKI**.

### B. Untuk Executive Viewer (Kepala Sekolah)
5.  **[Monitoring Aset]**
    *   Sebagai **Kepala Sekolah**, saya ingin **melihat Dashboard Grafik** yang menampilkan Total Nilai Aset Tetap vs Aset Rusak Berat, agar **saya bisa merencanakan anggaran perbaikan**.
6.  **[Audit Log]**
    *   Sebagai **Kepala Sekolah**, saya ingin **melihat siapa yang membawa Laptop Dinas** saat ini, agar **terjamin akuntabilitas penggunaan aset negara**.

### C. Untuk Custodian (Koordinator Lab)
7.  **[Lapor Kerusakan]**
    *   Sebagai **Koordinator Lab**, saya ingin **melaporkan "AC Rusak" via sistem** dengan menyertakan foto, agar **TU segera memanggil teknisi**.
8.  **[Validasi Ruangan]**
    *   Sebagai **Koordinator Lab**, saya ingin **melihat daftar inventaris di ruangan saya** via HP, agar **saya bisa mencocokkan fisik barang dengan data sistem**.

### D. Untuk Tim Opname (Sensus)
9.  **[Scan Cepat]**
    *   Sebagai **Tim Opname**, saya ingin **memindai QR Code di meja**, agar **sistem langsung membuka detail aset tersebut tanpa mengetik kode manual**.
10. **[Update Kondisi]**
    *   Sebagai **Tim Opname**, saya ingin **mengubah status aset** dari "Ready to Deploy" menjadi "Broken" saat audit, agar **data kondisi fisik di database akurat**.

---

## 3. Hubungan Use Case & Logika Bisnis (UML Relationships)

Bagian ini memetakan ketergantungan antar aktivitas sistem menggunakan notasi UML (`<<include>>` dan `<<extend>>`) untuk memastikan alur logika yang tepat.

### 3.1. Diagram Matriks Relasi

| Use Case Utama | Tipe Relasi | Use Case Terkait | Keterangan Logika Bisnis |
| :--- | :--- | :--- | :--- |
| **Semua Transaksi** | `<<include>>` | **Login Sistem** | User wajib terautentikasi (AuthMiddleware) sebelum akses. |
| **Input Aset Baru** | `<<include>>` | **Validasi Kapitalisasi** | Sistem otomatis cek harga. <br>• Jika < Rp 750rb: Set status "Persediaan" (Ekstrakomptabel).<br>• Jika ≥ Rp 750rb: Set status "Aset Tetap" (Intrakomptabel). |
| **Input Aset Baru** | `<<extend>>` | **Cetak Label QR** | Setelah simpan data, user *bisa memilih* untuk langsung cetak label atau menundanya (Opsional). |
| **Mutasi Aset** | `<<include>>` | **Cek Status Awal** | Sistem menolak mutasi jika aset sedang dalam status "Checkout" (Dipinjam Personil) atau "Rusak Berat". |
| **Mutasi Aset** | `<<include>>` | **Catat Log Mutasi** | Setiap perpindahan lokasi *wajib* membuat record di tabel `action_logs` untuk audit trail. |
| **Lapor Kerusakan** | `<<extend>>` | **Upload Foto Bukti** | User disarankan mengunggah foto kerusakan. Sifatnya opsional tapi direkomendasikan jika Rusak Berat. |
| **Stock Opname** | `<<include>>` | **Scan QR Code** | Identifikasi aset valid hanya via scan QR untuk menghindari kesalahan input ID manual. |
| **Stock Opname** | `<<extend>>` | **Update Kondisi Aset** | *Hanya terjadi jika* kondisi riil berbeda dengan data (misal: Data "Baik", Fisik "Rusak"). |
| **Penghapusan Aset**| `<<include>>` | **Approval Kepsek** | Aset Intrakomptabel tidak bisa langsung dihapus; hanya berubah status "Pending Disposal" menunggu persetujuan digital Kepala Sekolah. |

### 3.2. Penjelasan Logika Bisnis Kunci

#### A. Logika "Checkout" (Peminjaman) vs "Mutasi" (Perpindahan)
Sering terjadi kebingungan antara keduanya, berikut aturannya:
1.  **Checkout (Peminjaman Sementara):**
    *   **Subjek:** Barang berpindah tangan ke **Personil** (Guru/Siswa/Staf).
    *   **Implikasi:** Lokasi fisik mungkin tetap (misal Laptop Lab dipakai Guru di Lab), tapi tanggung jawab berpindah.
    *   **Syarat:** Satu barang hanya bisa di-checkout ke satu user pada satu waktu.
2.  **Mutasi (Perpindahan Permanen):**
    *   **Subjek:** Barang berpindah **Lokasi Ruangan** (Gudang -> Ruang Guru).
    *   **Implikasi:** Aset masuk ke dalam Kartu Inventaris Ruangan (KIR) lokasi tujuan.
    *   **Syarat:** Status kepemilikan ruangan berubah.

#### B. Logika Kapitalisasi Otomatis
Untuk mencegah kesalahan akuntansi operator:
1.  Saat User input `Harga Beli`.
2.  Sistem membaca Config `CAPITALIZATION_THRESHOLD` (misal: 750.000).
3.  **Action:**
    *   Jika `Harga < Threshold`: Auto-assign ke Model/Kategori "Persediaan". Depresiasi = 0.
    *   Jika `Harga ≥ Threshold`: Auto-assign ke Model/Kategori "Aset Tetap". Depresiasi = *Straight Line* sesuai masa manfaat barang.

#### C. Logika Validasi Penghapusan (Disposal Workflow)
Penghapusan aset negara melibatkan proses legal.
1.  Admin menekan tombol `Request Delete`.
2.  Sistem mengecek flag `is_intracomptable`:
    *   **FALSE (Barang Habis Pakai):** Langsung Soft-delete.
    *   **TRUE (Aset Tetap):** Block delete. Ubah status jadi `Pending Disposal`. Kirim notifikasi email/dashboard ke Kepala Sekolah.
3.  Kepala Sekolah login -> Klik `Approve`.
4.  Sistem melakukan `Soft-delete` dan mencatat `deleted_at`.
