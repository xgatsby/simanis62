# Skema Data SIMANIS62 (Adaptasi Snipe-IT)

Skema data untuk sistem manajemen aset di **SMAN 62 Jakarta** dirancang menggunakan platform **Snipe-IT v8.3.7** sebagai basis. Dokumen ini menjelaskan bagaimana kebutuhan pengelolaan aset sekolah dipetakan ke fitur-fitur yang tersedia di Snipe-IT.

> **Catatan:** Dokumen ini telah direvisi untuk fokus pada implementasi yang realistis dan dapat dicapai dengan konfigurasi standar Snipe-IT, tanpa memerlukan custom development.

---

## 1. Strategi Pengelompokan Aset

### Kategori Aset (Categories)
Aset dikelompokkan berdasarkan klasifikasi KIB menggunakan fitur **Categories** di Snipe-IT:

| Category | Deskripsi | Contoh Aset |
|----------|-----------|-------------|
| KIB A - Tanah | Tanah milik sekolah | Lahan sekolah |
| KIB B - Peralatan dan Mesin | Peralatan kantor, IT, laboratorium | Komputer, printer, proyektor, AC |
| KIB C - Gedung dan Bangunan | Bangunan dan konstruksi | Gedung utama, lab, perpustakaan |
| KIB D - Jalan, Irigasi, Jaringan | Infrastruktur | Pagar, saluran air, jaringan listrik |
| KIB E - Aset Tetap Lainnya | Buku, koleksi, alat olahraga | Buku perpustakaan, alat musik |

### Spesialisasi via Custom Fieldsets
Setiap kategori aset memiliki **Fieldset** khusus dengan kolom tambahan sesuai kebutuhan format laporan pemerintah:

#### Fieldset 1: Peralatan Umum (untuk KIB B)
| Field Name | Tipe | Opsi | Keterangan |
|------------|------|------|------------|
| Bahan | Text | - | Material pembuatan (kayu, besi, plastik) |
| Ukuran/Spesifikasi | Text | - | Dimensi atau spesifikasi teknis |
| Sumber Dana | Dropdown | BOS, APBD, Hibah, Komite, Lainnya | Asal perolehan aset |

#### Fieldset 2: Bangunan (untuk KIB C)
| Field Name | Tipe | Opsi | Keterangan |
|------------|------|------|------------|
| Konstruksi | Dropdown | Permanen, Semi-Permanen, Darurat | Jenis konstruksi bangunan |
| Luas Lantai (M²) | Number | - | Luas bangunan dalam meter persegi |
| Tahun Dibangun | Number | - | Tahun pembangunan |
| Bertingkat | Checkbox | Ya/Tidak | Apakah bangunan bertingkat |
| Beton | Checkbox | Ya/Tidak | Apakah menggunakan konstruksi beton |

#### Fieldset 3: Buku Perpustakaan (untuk KIB E)
| Field Name | Tipe | Opsi | Keterangan |
|------------|------|------|------------|
| Penulis/Pencipta | Text | - | Nama penulis atau pencipta |
| Penerbit | Text | - | Nama penerbit |
| Bahasa | Dropdown | Indonesia, Inggris, Sunda, Arab, Lainnya | Bahasa buku |
| ISBN | Text | - | Nomor ISBN jika ada |

### Hirarki Lokasi (Locations)
Lokasi menggunakan struktur **parent-child** untuk memodelkan realitas fisik sekolah dan mendukung pembuatan **Kartu Inventaris Ruangan (KIR)**:

```
SMAN 62 Jakarta (Root)
├── Gedung Utama
│   ├── Ruang Kepala Sekolah
│   ├── Ruang Guru
│   ├── Ruang TU
│   └── Kelas X-1 s/d X-10
├── Gedung Lab
│   ├── Lab Komputer 1
│   ├── Lab Komputer 2
│   └── Lab IPA
├── Gedung Olahraga
│   └── Sasana Krida
└── Perpustakaan
```

---

## 2. Siklus Hidup Aset

### Perpindahan Aset (Checkout/Checkin)
Snipe-IT mencatat setiap perpindahan aset melalui fitur **Checkout** dan **Checkin**:
- Checkout ke **User** - Aset dipinjamkan ke guru/staf
- Checkout ke **Location** - Aset dipindahkan ke ruangan tertentu
- Semua perpindahan tercatat di **Activity Log** untuk audit trail

### Pemeliharaan (Maintenance)
Riwayat perbaikan dan perawatan dicatat menggunakan fitur **Asset Maintenance**:
- Jenis pemeliharaan (perbaikan, upgrade, kalibrasi)
- Tanggal mulai dan selesai
- Biaya pemeliharaan
- Vendor/teknisi yang menangani
- Catatan pekerjaan

### Status Kondisi Aset
Kondisi fisik aset dilacak menggunakan **Status Labels** sesuai standar inventaris pemerintah:

| Status | Kode | Tipe Snipe-IT | Warna | Keterangan |
|--------|------|---------------|-------|------------|
| Baik | B | Deployable | Hijau | Aset dalam kondisi baik, siap digunakan |
| Kurang Baik | KB | Pending | Kuning | Aset perlu perbaikan minor (rusak ringan) |
| Rusak Berat | RB | Undeployable | Merah | Aset tidak dapat digunakan |
| Dalam Perbaikan | - | Pending | Biru | Aset sedang dalam proses perbaikan |
| Dihapuskan | - | Archived | Abu-abu | Aset sudah dihapus dari inventaris |

---

## 3. Penyusutan Aset (Depreciation)

Snipe-IT mendukung perhitungan penyusutan otomatis dengan metode **garis lurus (straight-line)**:

| Jenis Aset | Masa Manfaat | Contoh |
|------------|--------------|--------|
| Komputer & Laptop | 5 tahun | iMac, PC, Notebook |
| Printer & Scanner | 5 tahun | Printer laser, scanner |
| Proyektor & CCTV | 5 tahun | LCD Projector, kamera CCTV |
| AC & Elektronik | 8 tahun | AC Split, Kipas Angin |
| Furniture | 10 tahun | Meja, Kursi, Lemari |
| Gedung Permanen | 30 tahun | Gedung utama, lab |
| Gedung Semi-Permanen | 20 tahun | Bangunan sementara |

**Rumus Penyusutan:**
```
Penyusutan Tahunan = Harga Perolehan / Masa Manfaat
Nilai Buku = Harga Perolehan - Akumulasi Penyusutan
```

---

## 4. Hak Akses Pengguna (RBAC)

Snipe-IT menyediakan sistem **Permission Groups** untuk mengatur hak akses:

| Role | Hak Akses | Pengguna |
|------|-----------|----------|
| Super Admin | Akses penuh ke seluruh sistem | Administrator IT |
| Kepala Sekolah | View semua, approval penghapusan | Rina Astuti |
| Operator BMD | CRUD aset, checkout/checkin, maintenance | Wijo Sunarko |
| Koordinator Lab | View & update kondisi aset di lab | Achmad Taufiq |
| Guru/Staf | View aset yang dipinjam, request aset | Semua guru |

---

## 5. Pelaporan dan Kompatibilitas Format

### Kompatibilitas dengan Format Dokumen Pemerintah

| Format Dokumen | Kompatibilitas | Metode Implementasi |
|----------------|----------------|---------------------|
| KIR (Kartu Inventaris Ruangan) | 95% | Custom Report filter by Location + Excel Template |
| KIB B (Peralatan dan Mesin) | 95% | Native fields + Fieldset "Peralatan Umum" |
| KIB C (Gedung dan Bangunan) | 90% | Native fields + Fieldset "Bangunan" |
| KIB E (Aset Tetap Lainnya) | 85% | Native fields + Fieldset "Buku Perpustakaan" |
| Rekapitulasi Inventaris Tahunan | 70% | Depreciation Report + Excel formatting |

### Workflow Export Laporan
Untuk menghasilkan laporan format resmi pemerintah:

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  Snipe-IT   │ --> │ Export CSV  │ --> │   Excel     │ --> │   Print     │
│  Database   │     │ (Custom     │     │  Template   │     │  Laporan    │
│             │     │  Report)    │     │  (KIR/KIB)  │     │  Resmi      │
└─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘
```

### Excel Templates yang Diperlukan
1. **Template KIR** - Per ruangan, dengan header kop dan tanda tangan
2. **Template KIB B** - Daftar peralatan dan mesin
3. **Template KIB C** - Daftar gedung dan bangunan
4. **Template KIB E** - Daftar buku dan aset tetap lainnya
5. **Template Rekapitulasi** - Ringkasan tahunan dengan penyusutan

### Laporan Native Snipe-IT
- **Custom Asset Report** - Pilih kolom dan filter sesuai kebutuhan
- **Depreciation Report** - Nilai buku aset per periode
- **Activity Report** - Log semua transaksi aset
- **Maintenance Report** - Riwayat perbaikan dan biaya
- **Audit Log** - Jejak perubahan data

---

## 6. Identifikasi Aset

### Asset Tag
Setiap aset memiliki **Asset Tag** unik sebagai identitas utama. Format yang disarankan:

```
[KATEGORI]-[TAHUN]-[NOMOR URUT]
Contoh: 
- KOM-2024-0001 (Komputer pertama tahun 2024)
- PRJ-2024-0001 (Proyektor pertama tahun 2024)
- MEJ-2024-0001 (Meja pertama tahun 2024)
```

### Label & QR Code
Snipe-IT dapat generate:
- Label aset dengan barcode
- QR Code untuk scan cepat
- Label bisa dicetak untuk ditempel di aset fisik

---

## 7. Pemetaan Kolom Format Pemerintah ke Snipe-IT

### KIR (Kartu Inventaris Ruangan)
| Kolom KIR | Field Snipe-IT |
|-----------|----------------|
| No | Auto-generated |
| Nama Barang | Asset Name / Model Name |
| Merk/Model | Manufacturer + Model |
| No. Seri Pabrik | Serial Number |
| Ukuran | Custom Field (Ukuran/Spesifikasi) |
| Bahan | Custom Field (Bahan) |
| Tahun Pembuatan | Purchase Date |
| Kode Barang | Asset Tag |
| Jumlah | Qty |
| Harga Satuan | Purchase Cost |
| Keadaan (B/KB/RB) | Status Label |
| Keterangan | Notes |

### KIB B (Peralatan dan Mesin)
| Kolom KIB B | Field Snipe-IT |
|-------------|----------------|
| Kode Barang | Asset Tag |
| Nama Barang | Asset Name / Model |
| No. Register | Serial Number |
| Merk/Type | Manufacturer + Model |
| Bahan | Custom Field (Bahan) |
| Tahun Pembelian | Purchase Date |
| Asal Usul | Custom Field (Sumber Dana) |
| Harga | Purchase Cost |
| Akumulasi Penyusutan | Depreciation (calculated) |
| Nilai Buku | Current Value (calculated) |

---

## Catatan Implementasi

Dokumen ini merupakan **adaptasi realistis** dari kebutuhan pengelolaan aset sekolah ke platform Snipe-IT. 

**Yang TIDAK diimplementasikan (by design):**
- Kode Barang 12 digit Permendagri
- Kode Lokasi 24 digit hierarkis
- Validasi otomatis kode Permendagri
- Integrasi SIMDA/SIPKD
- Ambang kapitalisasi Rp 750.000

**Alasan:** Fitur-fitur tersebut memerlukan custom development yang tidak realistis untuk scope proyek ini. Data yang dihasilkan Snipe-IT dapat digunakan sebagai **sumber data** untuk pelaporan manual jika diperlukan.

---

*Terakhir diperbarui: 30 Desember 2025*
