# Ubiquitous Language (Bahasa Seragam) - SIMANIS 62

Dokumen ini adalah **Kamus Istilah Resmi** yang wajib digunakan oleh Pengembang, Pengguna (Guru/Staf SMAN 62), dan Manajemen dalam berkomunikasi selama proyek SIMANIS 62. Tujuannya adalah menghilangkan ambiguitas antara istilah teknis IT dan istilah birokrasi pemerintahan (Permendagri/BMD).

---

## 1. Konsep Inti Aset (Core Asset Concepts)

*   **BMD (Barang Milik Daerah):** Istilah payung hukum untuk semua kekayaan yang dibeli atas beban APBD atau perolehan lain yang sah. Dalam sistem ini, BMD = Semua data yang ada di database.
*   **Aset Tetap (Intrakomptabel):** Barang dengan nilai perolehan **≥ Batas Kapitalisasi** (misal: Rp 1.000.000,00 sesuai kebijakan berjalan), masa manfaat > 12 bulan. Wajib disusutkan (depresiasi) dan masuk Neraca Daerah.
*   **Aset Persediaan (Ekstrakomptabel):** Barang dengan nilai perolehan **< Batas Kapitalisasi**. Dianggap beban operasional, tidak disusutkan, namun tetap harus dicatat keberadaannya (Inventarisasi).
*   **Kapitalisasi (Capitalization):** Proses sistem menetapkan apakah suatu barang masuk kategori "Aset Tetap" atau "Persediaan" berdasarkan harga input.

---

## 2. Dokumen Inventaris (The "Cards")

*   **KIB (Kartu Inventaris Barang):** Dokumen induk pencatatan aset.
    *   **KIB A:** Tanah.
    *   **KIB B:** Peralatan & Mesin (Laptop, AC, Mobil).
    *   **KIB C:** Gedung & Bangunan.
    *   **KIB D:** Jalan, Irigasi & Jaringan.
    *   **KIB E:** Aset Tetap Lainnya (Buku Perpustakaan, Barang Bercorak Kesenian).
    *   **KIB F:** Konstruksi Dalam Pengerjaan (KDP) - *Note: Jarang dipakai di level sekolah.*
*   **KIR (Kartu Inventaris Ruangan):** Daftar turunan yang menampilkan subset barang yang **saat ini berlokasi** di satu ruangan spesifik (Snapshot Lokasi). KIR ditempel di dinding ruangan.

---

## 3. Aktivitas & Transaksi (Actions)

*   **Registrasi (Register):** Proses input data aset baru ke dalam sistem saat barang diterima (BAST).
*   **Peminjaman (Checkout):** Perpindahan penguasaan barang secara **SEMENTARA** kepada **Personil** (Guru/Siswa), tanpa mengubah penempatan lokasi permanennya.
    *   *Contoh:* Guru meminjam Infocus dari TU untuk mengajar 2 jam.
*   **Mutasi (Mutation):** Perpindahan lokasi barang secara **PERMANEN** antar ruangan yang mengubah tanggung jawab pemeliharaan.
    *   *Contoh:* Memindahkan Lemari dari Ruang Guru ke Ruang TU selamanya.
*   **Opname (Physical Audit):** Kegiatan memverifikasi fisik barang di lapangan dengan cara memindai QR Code untuk mencocokkan data sistem dengan fakta (Ada/Hilang/Rusak).
*   **Penghapusan (Disposal):** Proses administratif mengeluarkan aset dari Neraca karena rusak berat, hilang, atau dihibahkan. Status sistem menjadi "Deleted/Archived", bukan dihapus dari database log.

---

## 4. Status & Kondisi (States)

*   **Ready to Deploy (Siap Pakai):** Barang dalam kondisi baik, ada di gudang/penyimpanan, siap digunakan/dipinjam.
*   **Deployed (Terpakai):** Barang sedang digunakan (Status: Checked Out) oleh User atau terpasang di Ruangan.
*   **Maintenance (Dalam Perbaikan):** Barang sedang diservis dan tidak bisa dipinjam.
*   **Rusak Ringan (RR):** Barang masih bisa dipakai tapi butuh perbaikan kecil.
*   **Rusak Berat (RB):** Barang tidak bisa dipakai sama sekali, kandidat untuk Penghapusan.
*   **Pending Disposal:** Barang Intrakomptabel yang sudah diajukan hapus oleh Admin tapi belum di-Approve Kepala Sekolah.

---

## 5. Peran & Struktur (Roles & Structures)

*   **Sarpras (Sarana Prasarana):** Unit kerja di sekolah (dikepalai Wakil Kepala Sekolah Bid. Sarpras) yang menjadi *Product Owner* sistem ini.
*   **Koordinator Ruang (Room Custodian):** Guru/Staf yang ditunjuk bertanggung jawab atas KIR di ruangan tertentu (misal: Kepala Lab Fisika).
*   **Operator Aset:** Staff TU yang memegang akun Admin sistem untuk input data harian.

---

## 6. Istilah Teknis vs Bisnis (Technical Mapping)

| Istilah Sistem (Snipe-IT Native) | Bahasa Seragam (SMAN 62) | Catatan Penting |
| :--- | :--- | :--- |
| **Asset Tag** | **Kode Barang / ID Aset** | Format: `SMAN62-{Tahun}-{Urut}` |
| **Serial Number** | **No. Pabrik / No. Rangka** | Nomor bawaan produsen. |
| **Model** | **Merk & Tipe** | Digabung, misal "Epson EB-X500". |
| **Category** | **Jenis KIB** | Mapping Category ke KIB (A,B,C,D,E). |
| **Status Label** | **Kondisi Fisik** | Baik, RR, RB. |
| **Custom Fields** | **Atribut KIB** | Kolom tambahan (Luas, Sertifikat). |

---

**Aturan Penggunaan:**
Dalam setiap diskusi, rapat, atau penulisan kode (variabel/komentar), **WAJIB** menggunakan istilah di kolom "Bahasa Seragam" atau "Konsep Inti" untuk menghindari kesalahpahaman. Jangan gunakan istilah "Inventory" jika yang dimaksud adalah "Aset Tetap".
