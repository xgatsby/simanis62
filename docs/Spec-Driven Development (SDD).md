# Spec-Driven Development (SDD): Sistem Manajemen Aset Terintegrasi SMAN 62 Jakarta ("SIMANIS 62")

**Dokumen Kontrol:**
*   **Target Institusi:** SMAN 62 Jakarta (Kramat Jati)
*   **Basis Platform:** Snipe-IT (Open Source Asset Management)
*   **Regulasi Acuan:** Permendagri No. 19 Tahun 2016 (Pedoman Pengelolaan Barang Milik Daerah) & Permendagri 108/2016 (Kodefikasi Aset).
*   **Status Dokumen:** DRAFT 1.0 (Architecture Baseline)

---

## 1. Executive Summary
Proyek ini bertujuan memodernisasi tata kelola aset SMAN 62 Jakarta dengan mengadopsi platform **Snipe-IT** yang dikustomisasi (*Tailored Adaptation*). Sistem harus mampu menjembatani operasional modern (QR Code, Mobile Opname) dengan kepatuhan administrasi negara (Laporan KIB A-E, Penyusutan Pemerintah). Dokumen ini menjadi acuan tunggal teknis (*Single Source of Truth*) bagi pengembang.

## 2. Arsitektur Sistem (Hybrid Approach)
Sistem menggunakan pendekatan **"Core-Extension Pattern"** untuk menjaga *upgradeability* Snipe-IT sambil memenuhi kebutuhan unik sekolah.

*   **Core Layer:** Snipe-IT v7.x (Laravel 10+) menangani autentikasi, manajemen database CRUD dasar, log aktivitas, dan API. **DILARANG** mengubah struktur tabel inti (`assets`, `users`, `locations`).
*   **Extension Layer:**
    *   **Custom Fieldsets:** Memetakan atribut KIB (Kartu Inventaris Barang) ke dalam JSON column Snipe-IT tanpa *alter table*.
    *   **Report Engine:** Modul terpisah (Controller & View baru) untuk men-generate laporan PDF format Dinas Pendidikan DKI.
    *   **Logic Hooks:** Event listener untuk validasi aturan bisnis spesifik (contoh: validasi nilai kapitalisasi).

## 3. Spesifikasi Data (Data Schema Mapping)
Mengacu pada `@data_skema.md`, berikut adalah spesifikasi pemetaan data ke dalam fitur **Custom Fieldsets** Snipe-IT.

### Strategi: Table-per-Type via Fieldsets
Setiap Kategori Aset di Snipe-IT akan diikat dengan satu *Fieldset* spesifik.

#### 3.1. KIB A: Tanah (Land)
*   **Snipe-IT Category:** `Tanah & Bangunan` -> `Tanah`
*   **Fieldset Name:** `SPEC_KIB_A`
*   **Atribut Spesifik:**
    | Field Name | Tipe Data | Wajib? | Helper Text / Note |
    | :--- | :--- | :--- | :--- |
    | `Luas (M2)` | Numeric | YES | Luas total tanah |
    | `Hak Tanah` | Dropdown | YES | Opsi: Hak Pakai, Hak Pengelolaan |
    | `Nomor Sertifikat` | Text | YES | |
    | `Tanggal Sertifikat`| Date | YES | |
    | `Penggunaan` | Text | YES | Contoh: Area Sekolah, Lapangan Olahraga |

#### 3.2. KIB B: Peralatan & Mesin (Equipment)
*   **Snipe-IT Category:** `Elektronik`, `Kendaraan`, `Meubelair`
*   **Fieldset Name:** `SPEC_KIB_B`
*   **Atribut Spesifik:**
    | Field Name | Tipe Data | Wajib? | Helper Text / Note |
    | :--- | :--- | :--- | :--- |
    | `Merk/Type` | Text | YES | Otomatis ambil dari Model, tapi perlu field eksplisit untuk laporan |
    | `Ukuran/CC` | Text | NO | Untuk kendaraan atau dimensi alat |
    | `Bahan` | Text | NO | Contoh: Besi, Kayu, Plastik |
    | `Nomor Pabrik` | Text | NO | Serial Number (Mapping ke field default Serial) |
    | `Nomor Rangka` | Text | NO | Khusus Kendaraan |
    | `Nomor Polisi` | Text | NO | Khusus Kendaraan |

#### 3.3. KIB C: Gedung & Bangunan
*   **Snipe-IT Category:** `Gedung`
*   **Fieldset Name:** `SPEC_KIB_C`
*   **Atribut Spesifik:**
    | Field Name | Tipe Data | Wajib? | Helper Text / Note |
    | :--- | :--- | :--- | :--- |
    | `Bertingkat` | Radio | YES | Bertingkat / Tidak |
    | `Beton` | Radio | YES | Beton / Tidak |
    | `Luas Lantai (M2)` | Numeric | YES | |
    | `Lokasi Fisik` | Text | YES | Alamat lengkap (Jl. Raya Bogor...) |
    | `Nomor Dokumen` | Text | NO | IMB atau surat aset |

#### 3.4. KIB D: Jalan, Irigasi & Jaringan
*   **Snipe-IT Category:** `Infrastruktur`
*   **Fieldset Name:** `SPEC_KIB_D`
*   **Atribut:** `Konstruksi`, `Panjang (m)`, `Lebar (m)`, `Status Tanah`.

#### 3.5. KIB E: Aset Tetap Lainnya
*   **Snipe-IT Category:** `Buku`, `Kesenian`, `Hewan/Ternak`
*   **Fieldset Name:** `SPEC_KIB_E`
*   **Atribut:** `Judul Buku`, `Spesifikasi`, `Asal Daerah` (untuk seni).

## 4. Spesifikasi Fungsional (FRS)

### 4.1. Manajemen Identitas Aset (Digital Tagging)
*   **Requirement:** Setiap aset harus memiliki ID Unik yang bisa discan.
*   **Solusi:** Gunakan fitur *Asset Tag* Snipe-IT.
*   **Format:** `SMAN62-{TAHUN}-{URUT}` (Contoh: `SMAN62-2025-0001`).
*   **QRCode:** Snipe-IT *auto-generate* QR. Saat discan via HP, URL membuka halaman detail aset.

### 4.2. Logika Kapitalisasi (Capitalization Logic)
*   **Constraint:** Barang < Rp 750.000 dianggap "Persediaan" (Ekstrakomptabel), >= Rp 750.000 adalah "Aset Tetap" (Intrakomptabel).
*   **Implementasi:**
    *   Buat *Custom Status Label*: `Aset Tetap (Audit)` dan `Persediaan (Non-Audit)`.
    *   *Event Logic:* Saat Operator input harga beli, sistem memberikan *warning* visual jika salah pilih kategori status.

### 4.3. Opname & Mutasi (Audit & Movement)
*   **Skenario:** Guru meminjam Laptop dari Lab ke Kelas.
*   **Fitur:** `Checkout` (Peminjaman) ke User (Guru) atau Location (Kelas).
*   **Audit Log:** Sistem otomatis mencatat "Laptop X dipindah dari Lab -> Kelas oleh Admin pada Jam Y". Ini memenuhi syarat mutasi KIR.

## 5. Spesifikasi Teknis & Integrasi

### 5.1. Tech Stack
*   **Backend:** PHP 8.1+ (Laravel Framework)
*   **Database:** MySQL 8.0 / MariaDB 10.x
*   **Frontend:** Blade Templates + Bootstrap (Default Snipe-IT) + Custom JS untuk validasi form KIB.
*   **Server:** On-Premise SMAN 62 (Ubuntu Server) atau VPS (Jaringan ID/Biznet).

### 5.2. Poin Kustomisasi (Dev Required)
1.  **Laporan KIB (PDF):**
    *   Buat Controller baru: `App\Http\Controllers\Reports\KibReportController.php`.
    *   Query database bergabung antara tabel `assets` dan table `custom_fields`.
    *   Output: PDF Landscape sesuai format tabel Permendagri.
2.  **Dashboard Widget:**
    *   Menambahkan Widget "Total Aset per KIB" di dashboard utama admin agar Kepala Sekolah (Bu Rina Astuti) bisa langsung lihat rekap.

## 6. Rencana Migrasi & Deployment
1.  **Fase 1: Instalasi & Konfigurasi Dasar (Minggu 1)**
    *   Instal Snipe-IT.
    *   Setup Company, Location (SMAN 62), Categories.
2.  **Fase 2: Pembuatan Fieldsets (Minggu 2)**
    *   Create Fieldsets KIB A-E sesuai Section 3.
    *   Testing input data dummy.
3.  **Fase 3: Import Data Lama (Minggu 3)**
    *   Konversi Excel sekolah lama ke CSV format Import Snipe-IT.
    *   Mapping kolom Excel -> Custom Fields.
4.  **Fase 4: UAT & Pelatihan (Minggu 4)**
    *   Training Operator (Pak Wijo) dan Koordinator Lab.
    *   Simulasi Opname QR Code.

## 7. Kriteria Keberhasilan (Acceptance Criteria)
*   [ ] Sistem mampu mencetak Laporan KIB B (Peralatan) yang valid secara kolom (Merk, Ukuran, No Pabrik).
*   [ ] QR Code dapat dipindai oleh HP Android standar dan menampilkan data aset.
*   [ ] Kepala Sekolah dapat melihat Total Nilai Aset secara *real-time* di Dashboard.
*   [ ] Log mutasi barang terekam akurat (Siapa, Kapan, Kemana).

---
**Pengesahan:**
Disusun untuk kebutuhan pengembangan internal SMAN 62 Jakarta.
Dokumen ini bersifat dinamis dan akan diperbarui seiring *feedback* User Acceptance Test (UAT).
