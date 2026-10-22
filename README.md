# PRODUCT REQUIREMENTS DOCUMENT
## SACR — Sistem Absensi Club Renang
### Aplikasi Desktop berbasis WPF, C#, dan SQL Server
#### link google drive : 
https://drive.google.com/drive/folders/1Rqgo_ulFSGE3e-xq2cQGIVt7zt_jFRYy

| Informasi Tim | Keterangan |
|---|---|
| Kelompok | 1 |
| Anggota | Antonius Andy Martono (5025241191) |
| | Sinurat Federico (5025241244) |
| Tugas | Minggu ke-7 — PBKK |

| Informasi Dokumen | Deskripsi |
|---|---|
| Nama Produk | SACR — Sistem Absensi Club Renang |
| Jenis Dokumen | Product Requirements Document (PRD) |
| Versi | 1.0 — As-built (mengikuti hasil implementasi) |
| Platform | Desktop Windows — WPF |
| Teknologi Utama | C#, XAML, .NET, SQL Server (LocalDB), ADO.NET (Microsoft.Data.SqlClient) |
| Pola Arsitektur | MVVM (ICommand, INotifyPropertyChanged, ObservableCollection) |
| Referensi Desain | Aplikasi SISMA (sidebar, header, kartu statistik, DataGrid modern) |

Dokumen kebutuhan produk untuk acuan desain, implementasi, dan pengujian.

**JAWABAN ETS :**
[JAWABAN ETS](ETS/5025241191_JAWABAN_ETS.md)

---

## 1. Ringkasan Eksekutif

SACR (Sistem Absensi Club Renang) adalah aplikasi desktop untuk membantu admin/pelatih club renang mengelola data atlet, kelompok latihan, jadwal latihan, serta pencatatan absensi per sesi latihan melalui antarmuka modern. Produk dirancang dengan pendekatan Rich UI: navigasi sidebar tetap, header, kartu statistik dashboard, tabel data (DataGrid), formulir terstruktur, dan pesan notifikasi yang konsisten. PRD ini menerjemahkan kebutuhan operasional club renang menjadi kebutuhan produk yang dapat direncanakan, dibangun, dan diuji.

### 1.1 Tujuan Produk
- Menyediakan satu aplikasi desktop untuk mengelola data atlet, kelompok, dan jadwal latihan.
- Menggantikan absensi manual (kertas) dengan pencatatan digital per sesi latihan.
- Menyajikan ringkasan kehadiran harian (dashboard) dan bulanan (rekap) yang dihitung dari database.
- Memberikan riwayat kehadiran per atlet yang dapat difilter per bulan dan tahun.
- Menerapkan pemisahan kode (MVVM + Repository) agar aplikasi mudah diuji dan dikembangkan.

### 1.2 Pernyataan Produk
Untuk admin dan pelatih club renang yang membutuhkan pencatatan kehadiran atlet yang cepat dan akurat, SACR menyediakan aplikasi desktop terintegrasi yang menggabungkan manajemen data master, absensi per sesi, rekap bulanan, dan UI modern dalam satu lingkungan kerja.

---

## 2. Latar Belakang dan Permasalahan
- Absensi atlet selama ini tercatat manual sehingga rawan hilang, sulit direkap, dan sulit diaudit.
- Pelatih membutuhkan gambaran cepat: siapa hadir, izin, sakit, atau alpha pada latihan hari ini.
- Perhitungan persentase kehadiran per atlet/bulan memakan waktu bila dilakukan manual.
- Data atlet, kelompok, dan jadwal tersebar tidak terstruktur sehingga pembaruan data lambat.
- Perlu jaminan integritas: satu atlet tidak boleh memiliki dua catatan absensi pada sesi yang sama.

---

## 3. Sasaran, Indikator, dan Batasan

| Sasaran | Indikator keberhasilan awal |
|---|---|
| Pengelolaan master data | Pengguna dapat menambah, melihat, mengubah, dan menghapus atlet, kelompok, dan jadwal. |
| Pencarian & filter atlet | Pencarian berdasarkan nama serta filter kelompok/status menghasilkan data yang relevan. |
| Absensi per sesi | Pengguna dapat memilih jadwal + tanggal, mengisi status Hadir/Izin/Sakit/Alpha + catatan, dan menyimpan. |
| Dashboard | Empat kartu statistik dan daftar jadwal hari ini menggunakan data database, bukan angka statis. |
| Rekap & riwayat | Persentase kehadiran dihitung dari query agregat database. |
| Keandalan | Kesalahan validasi atau koneksi ditangani dengan pesan yang dapat dipahami; aplikasi tidak crash. |
| Keamanan dasar | Query berparameter dan password disimpan sebagai hash SHA-256. |

Angka pada kartu statistik (mis. total atlet aktif) merupakan hasil query saat aplikasi berjalan, bukan nilai statis.

---

## 4. Pengguna dan Hak Akses

| Peran | Kebutuhan utama | Hak akses |
|---|---|---|
| Admin / Pelatih | Mengelola master data, mengisi absensi, melihat rekap & riwayat. | Akses penuh ke seluruh menu aplikasi. |
| Pengguna belum login | Masuk ke aplikasi. | Hanya dapat melihat halaman login. |

Catatan: versi pertama hanya memiliki satu peran (Admin/Pelatih). Portal atlet/orang tua tidak termasuk scope.

---

## 5. Ruang Lingkup Produk

### 5.1 In Scope — Rilis Pertama
- Login dan logout (password hash SHA-256, akun default `admin`).
- Dashboard: kartu Total Atlet Aktif, Latihan Hari Ini, Hadir Hari Ini, Tidak Hadir Hari Ini; daftar Jadwal Hari Ini dengan tombol **Isi Absensi**.
- Absensi: pilih jadwal + tanggal, sesi latihan dibuat otomatis bila belum ada, daftar atlet aktif per kelompok, status Hadir/Izin/Sakit/Alpha, catatan opsional, simpan (transaksi), dan edit absensi tersimpan.
- Atlet: DataGrid, pencarian nama, filter kelompok, filter status, tambah/edit/hapus dengan validasi dan konfirmasi.
- Kelompok: tambah/edit/hapus (nama, deskripsi opsional, status aktif).
- Jadwal: tambah/edit/hapus (kelompok, hari, jam mulai, jam selesai) dengan validasi jam.
- Rekap: filter bulan/tahun/kelompok; tabel Hadir/Izin/Sakit/Alpha + persentase per atlet.
- Riwayat: filter atlet/bulan/tahun; ringkasan kehadiran + tabel riwayat per tanggal.
- Validasi form, konfirmasi penghapusan, notifikasi hasil operasi, empty state, dan penanganan error koneksi.
- Penyimpanan data terpusat pada SQL Server.

### 5.2 Out of Scope — Rilis Berikutnya
- Ekspor Excel/PDF, pengiriman laporan via email/WhatsApp.
- Pembayaran/SPP, QR code, face recognition, fingerprint, GPS.
- Portal web/mobile untuk atlet atau orang tua.
- Notifikasi otomatis, sistem kompetisi, catatan waktu renang, program latihan.
- Multi-cabang dan sinkronisasi cloud.
- Tema dark mode/light mode (direncanakan setelah seluruh fitur utama stabil).

---

## 6. Struktur Navigasi dan Halaman

| Halaman | Elemen UI utama | Tujuan |
|---|---|---|
| Login | Judul aplikasi, username, password, tombol Login, pesan error, hint akun default. | Memverifikasi identitas pengguna. |
| Dashboard | 4 kartu statistik, daftar Jadwal Hari Ini, tombol Isi Absensi per jadwal. | Ringkasan kondisi hari ini dan pintasan absensi. |
| Absensi | Dropdown jadwal, DatePicker tanggal, tombol Muat Atlet, banner sesi aktif, DataGrid (nama, status, catatan), tombol Simpan Absensi. | Mencatat dan memperbaiki absensi per sesi. |
| Atlet | Kotak pencarian, filter kelompok, filter status, DataGrid, form tambah/edit, tombol Simpan/Hapus/Tambah Baru. | Mengelola data atlet. |
| Kelompok | DataGrid, form nama/deskripsi/checkbox aktif, tombol Simpan/Hapus/Tambah Baru. | Mengelola kelompok latihan. |
| Jadwal | DataGrid, form kelompok/hari/jam mulai/jam selesai/aktif, tombol Simpan/Hapus/Tambah Baru. | Mengelola jadwal latihan mingguan. |
| Rekap | Filter kelompok/bulan/tahun, DataGrid rekap + persentase. | Laporan kehadiran bulanan per kelompok. |
| Riwayat | Filter atlet/bulan/tahun, kartu ringkasan, DataGrid riwayat. | Menelusuri kehadiran per atlet. |
| Sidebar | Menu Dashboard, Absensi, Atlet, Kelompok, Jadwal, Rekap, Riwayat, dan Logout. | Navigasi utama aplikasi. |

---

## 7. Kebutuhan Fungsional

| ID | Fitur | Persyaratan | Prioritas |
|---|---|---|---|
| FR-01 | Autentikasi | Sistem menyediakan login dengan kredensial valid dan menolak kredensial tidak valid dengan pesan yang jelas. | Must |
| FR-02 | Logout | Pengguna dapat keluar; aplikasi kembali ke halaman login. | Must |
| FR-03 | Dashboard statistik | Sistem menghitung total atlet aktif, jumlah latihan hari ini, hadir hari ini, dan tidak hadir hari ini dari database. | Must |
| FR-04 | Jadwal hari ini & pintasan | Sistem menampilkan jadwal yang harinya sama dengan hari berjalan beserta tombol Isi Absensi yang membuka halaman absensi terisi otomatis. | Must |
| FR-05 | Daftar atlet | Sistem menampilkan atlet dalam DataGrid dengan kolom Nama, Tanggal Lahir, Kelompok, Nomor HP, Tanggal Bergabung, Status. | Must |
| FR-06 | Pencarian atlet | Pengguna dapat mencari atlet berdasarkan nama (parsial). | Must |
| FR-07 | Filter atlet | Pengguna dapat memfilter berdasarkan kelompok dan status aktif/nonaktif. | Must |
| FR-08 | CRUD atlet | Tambah/ubah/hapus atlet dengan validasi (nama wajib, kelompok wajib) dan konfirmasi hapus. | Must |
| FR-09 | CRUD kelompok | Tambah/ubah/hapus kelompok (nama, deskripsi opsional, status). | Must |
| FR-10 | CRUD jadwal | Tambah/ubah/hapus jadwal (kelompok, hari, jam mulai, jam selesai) dengan validasi jam selesai > jam mulai dan format jam. | Must |
| FR-11 | Pemilihan sesi absensi | Pengguna memilih jadwal dan tanggal; sistem membuat SesiLatihan otomatis bila belum ada (unik per jadwal+tanggal). | Must |
| FR-12 | Pengisian absensi | Sistem menampilkan seluruh atlet aktif pada kelompok jadwal tersebut; pengguna memilih status Hadir/Izin/Sakit/Alpha dan catatan opsional per atlet. | Must |
| FR-13 | Penyimpanan absensi | Sistem menyimpan seluruh baris dalam satu transaksi; satu atlet maksimal satu catatan per sesi (unique constraint); simpan ulang berfungsi sebagai pembaruan. | Must |
| FR-14 | Edit absensi tersimpan | Absensi yang sudah tersimpan dimuat kembali saat sesi dibuka sehingga dapat diperbaiki. | Must |
| FR-15 | Rekap bulanan | Sistem menampilkan per atlet: jumlah Hadir, Izin, Sakit, Alpha, dan persentase kehadiran untuk bulan/tahun/kelompok terpilih. | Must |
| FR-16 | Riwayat atlet | Sistem menampilkan ringkasan (nama, kelompok, jumlah kehadiran, persentase) dan daftar riwayat (tanggal, kelompok, jam, status, catatan) sesuai filter. | Must |
| FR-17 | Validasi form | Kolom wajib divalidasi sebelum operasi database; pesan error ditampilkan pada halaman login dan form. | Must |
| FR-18 | Notifikasi | Sistem menampilkan pesan berhasil/gagal setelah operasi tambah, ubah, hapus, dan simpan absensi. | Must |
| FR-19 | Empty state | Setiap daftar menampilkan pesan informatif ketika tidak ada data. | Must |
| FR-20 | Penanganan error koneksi | Kegagalan koneksi database ditampilkan sebagai pesan ramah tanpa menutup aplikasi secara tidak terkendali. | Must |
| FR-21 | Tema dark/light | Pengguna dapat mengganti tema tampilan. | Should (rilis berikutnya) |

---

## 8. User Stories dan Kriteria Penerimaan

**US-01 — Login sebagai pelatih**
Sebagai pelatih, saya ingin masuk menggunakan username dan password agar data club terlindungi.
- Kredensial benar membuka MainWindow dengan header nama pengguna.
- Kredensial salah menampilkan "Username atau password salah."
- Database tidak terjangkau menampilkan pesan koneksi, aplikasi tetap hidup.

**US-02 — Melihat dashboard**
Sebagai pelatih, saya ingin melihat ringkasan hari ini agar tahu kondisi latihan secara cepat.
- Kartu menampilkan nilai hasil query database.
- Daftar Jadwal Hari Ini hanya berisi jadwal dengan hari = hari berjalan.
- Bila tidak ada jadwal, tampil empty state "Tidak ada jadwal untuk hari ini."

**US-03 — Mengisi absensi sesi latihan**
Sebagai pelatih, saya ingin mencatat kehadiran atlet per sesi agar data tidak hilang.
- Memilih jadwal + tanggal lalu Muat Atlet menampilkan seluruh atlet aktif kelompok tersebut.
- Status dapat dipilih per atlet (Hadir/Izin/Sakit/Alpha) dan catatan dapat diketik langsung.
- Simpan menampilkan "Absensi berhasil disimpan." dan data masuk database dalam satu transaksi.

**US-04 — Memperbaiki absensi yang salah input**
Sebagai pelatih, saya ingin mengubah absensi yang sudah tersimpan bila terjadi kesalahan.
- Membuka sesi yang sama kembali memuat status dan catatan sebelumnya.
- Perubahan tersimpan sebagai UPDATE (tidak membuat baris duplikat).

**US-05 — Mengelola data atlet**
Sebagai admin, saya ingin menambah, mengubah, dan menghapus atlet agar data selalu mutakhir.
- Simpan tanpa nama atau tanpa kelompok ditolak dengan pesan yang jelas.
- Hapus memunculkan dialog konfirmasi; pembatalan tidak mengubah data.
- Pencarian dan filter memperbarui tabel secara langsung.

**US-06 — Melihat rekap bulanan**
Sebagai pelatih, saya ingin melihat persentase kehadiran per atlet per bulan untuk evaluasi.
- Memilih kelompok/bulan/tahun menampilkan tabel hitungan dan persentase.
- Angka konsisten dengan data absensi yang tersimpan.

**US-07 — Menelusuri riwayat atlet**
Sebagai pelatih, saya ingin melihat riwayat kehadiran satu atlet beserta catatannya.
- Memilih atlet menampilkan kartu ringkasan dan tabel riwayat terurut tanggal terbaru.
- Filter bulan/tahun mempersempit hasil; "Semua" menampilkan seluruh periode.

**US-08 — Logout**
Sebagai pengguna, saya ingin keluar agar sesi tidak terbuka saat komputer ditinggalkan.
- Klik Logout menutup MainWindow dan menampilkan kembali halaman login.

---

## 9. Kebutuhan UI/UX dan Design System

| Aspek | Ketentuan desain |
|---|---|
| Gaya visual | Desktop modern, profesional, bersih, berorientasi produktivitas pelatih. |
| Warna | Navy/dark blue (`#0F172A`, `#1E3A8A`) sebagai dasar sidebar dan aksen; biru (`#3B82F6`) untuk aksi utama; hijau/merah/ungu sebagai pembeda kartu statistik; merah (`#DC2626`) untuk aksi berbahaya. |
| Layout | Sidebar tetap di kiri (250px), header di atas, area konten di tengah; kartu statistik berjajar; form berada di bawah DataGrid dalam panel kartu. |
| Tipografi | Font sans-serif bawaan sistem dengan hierarki judul halaman (22pt), label form (12pt), dan isi (13–14pt). |
| Komponen | Tombol Primary/Secondary/Danger, TextBox, ComboBox, DatePicker, CheckBox, DataGrid bergaya modern (header navy, zebra row, hover), dialog konfirmasi, message box notifikasi. |
| Status UI | State loading (tombol "Memproses..." saat login), sukses, error, dan data kosong tersedia pada seluruh halaman. |
| Konsistensi | Seluruh warna, style tombol, style form, dan style DataGrid didefinisikan sekali pada ResourceDictionary (`AppStyles.xaml`) dan dipakai bersama. |
| Aksesibilitas | Kontras teks memadai, label wajib ditandai asterisk (*), pesan error tidak hanya mengandalkan warna. |
| Responsivitas | Layout menggunakan Grid/DockPanel sehingga menyesuaikan ukuran window minimum 1100×700. |

---

## 10. Kebutuhan Nonfungsional

| ID | Kategori | Persyaratan |
|---|---|---|
| NFR-01 | Performa | Navigasi antar halaman dan operasi CRUD terasa responsif pada perangkat laboratorium. |
| NFR-02 | Performa data | Operasi database dijalankan singkat (query terfilter/berparameter); pencarian atlet langsung memperbarui tabel tanpa langkah tambahan. |
| NFR-03 | Keamanan | Seluruh query menggunakan parameter; password disimpan sebagai hash SHA-256 (VARBINARY(64)), tidak pernah plaintext. |
| NFR-04 | Keandalan | Kegagalan koneksi ditampilkan sebagai pesan "Tidak dapat terhubung ke database..." saat startup dan saat operasi; aplikasi tidak crash. |
| NFR-05 | Pemeliharaan | Kode dipisah menjadi Views, ViewModels, Models, Repositories, Services, Commands, Helpers; logika tidak ditulis di code-behind (kecuali jembatan PasswordBox dan pergantian window). |
| NFR-06 | Integritas data | Primary key, foreign key, unique constraint (jadwal+tanggal pada sesi; sesi+atlet pada absensi), check constraint (status, nama hari), dan transaksi untuk penyimpanan absensi massal. |
| NFR-07 | Kompatibilitas | Windows 10/11 dengan .NET Desktop; SQL Server LocalDB untuk pengembangan, kompatibel instance SQL Server penuh melalui penggantian connection string. |
| NFR-08 | Pesan error | Pesan teknis (detail SqlException) hanya ditampilkan sebagai informasi pendukung pada dialog, tidak menyembunyikan pesan utama yang dapat ditindaklanjuti. |

---

## 11. Rancangan Data

### 11.1 Entitas

| Entitas | Atribut inti | Catatan |
|---|---|---|
| Users | UserId, Username, PasswordHash, Nama, Role, IsActive | PasswordHash VARBINARY(64) hasil SHA-256; seed default `admin`. |
| Kelompok | KelompokId, NamaKelompok, Deskripsi, IsActive | Master kelompok latihan (Beginner/Junior/Senior). |
| Atlet | AtletId, Nama, TanggalLahir, KelompokId, NomorHP, TanggalBergabung, IsActive | KelompokId foreign key ke Kelompok. |
| Jadwal | JadwalId, KelompokId, Hari, JamMulai, JamSelesai, IsActive | Hari dibatasi check constraint Senin–Minggu; jam bertipe TIME. |
| SesiLatihan | SesiId, JadwalId, Tanggal | UNIQUE (JadwalId, Tanggal); dibuat otomatis saat absensi pertama dibuka. |
| Absensi | AbsensiId, SesiId, AtletId, Status, Catatan | UNIQUE (SesiId, AtletId); Status dibatasi Hadir/Izin/Sakit/Alpha. |

### 11.2 Relasi utama
- Kelompok 1—* Atlet
- Kelompok 1—* Jadwal
- Jadwal 1—* SesiLatihan
- SesiLatihan 1—* Absensi
- Atlet 1—* Absensi
- Users berdiri sendiri (autentikasi)

Constraint diterapkan pada skrip `Database/schema.sql` sebelum aplikasi digunakan.

---

## 12. Arsitektur Teknis

| Lapisan | Tanggung jawab | Contoh file implementasi |
|---|---|---|
| View | Presentasi dan interaksi visual. | `MainWindow.xaml`, `LoginView.xaml`, `DashboardView.xaml`, `AbsensiView.xaml`, `AtletView.xaml`, `KelompokView.xaml`, `JadwalView.xaml`, `RekapView.xaml`, `RiwayatView.xaml` |
| ViewModel | State UI, command, validasi tampilan, pemanggilan repository. | `MainViewModel`, `LoginViewModel`, `DashboardViewModel`, `AbsensiViewModel`, `AtletViewModel`, `KelompokViewModel`, `JadwalViewModel`, `RekapViewModel`, `RiwayatViewModel`, `ViewModelBase` |
| Model | Representasi entitas/domain. | `User`, `Atlet`, `Kelompok`, `Jadwal`, `SesiLatihan`, `Absensi`, `AbsensiItem`, `RekapItem`, `RiwayatAbsensi` |
| Repository | Query database berparameter dan pemetaan hasil. | `AtletRepository`, `KelompokRepository`, `JadwalRepository`, `AbsensiRepository`, `DashboardRepository`, `RekapRepository`, `RiwayatRepository` |
| Service | Aturan aplikasi lintas repository. | `AuthenticationService` (verifikasi hash), `NavigationService` (perpindahan halaman + konfigurasi ViewModel) |
| Helper | Utilitas bersama. | `DatabaseHelper` (connection string tunggal), `MessageBoxHelper` (notifikasi/konfirmasi), `TanggalHelper` (nama hari/bulan) |
| Command | Implementasi ICommand. | `RelayCommand` |
| Database | Persistensi, constraint, seed. | SQL Server `SwimmingAttendanceDB` (LocalDB) |

Teknologi: WPF dengan C# dan XAML, .NET Desktop, SQL Server, Microsoft.Data.SqlClient, MVVM, ICommand, INotifyPropertyChanged, ObservableCollection. Navigasi menggunakan pola ContentControl + DataTemplate pemetaan ViewModel→View.

---

## 13. Alur Pengguna Utama
1. Pengguna membuka aplikasi; sistem memeriksa koneksi database (pesan peringatan bila gagal, aplikasi tetap berjalan).
2. Pengguna memasukkan username dan password; sistem memverifikasi hash SHA-256 terhadap tabel Users.
3. Dashboard ditampilkan dengan statistik dan jadwal hari ini hasil query database.
4. Pelatih melihat jadwal hari ini dan menekan **Isi Absensi** (atau membuka menu Absensi manual).
5. Sistem memuat/membuat SesiLatihan untuk jadwal+tanggal terpilih dan menampilkan atlet aktif kelompok tersebut.
6. Pelatih memilih status dan mengisi catatan, lalu menekan **Simpan Absensi**; sistem menyimpan dalam satu transaksi (INSERT baru / UPDATE yang sudah ada).
7. Sistem menampilkan notifikasi berhasil; dashboard, rekap, dan riwayat otomatis memakai data terbaru.
8. Pengguna logout; aplikasi kembali ke halaman login.

---

## 14. Rencana Implementasi Bertahap (Realisasi)

| Fase | Aktivitas | Deliverable | Status |
|---|---|---|---|
| Fase 1 — Fondasi | Project WPF, struktur folder MVVM, design tokens (`AppStyles.xaml`), shell sidebar + header + navigasi. | Shell aplikasi dan navigasi dasar. | Selesai |
| Fase 2 — Database | Skrip `schema.sql`: 6 tabel, PK/FK/unique/check constraint, seed admin (hash) + 3 kelompok; `DatabaseHelper`; cek koneksi startup. | `SwimmingAttendanceDB` dan koneksi teruji. | Selesai |
| Fase 3 — Login | `LoginView`, `LoginViewModel`, `AuthenticationService`, verifikasi SHA-256, logout, header dinamis. | Autentikasi berfungsi end-to-end. | Selesai |
| Fase 4 — Master data | Model + repository + ViewModel + View untuk Kelompok, Atlet (search/filter), Jadwal (validasi jam); konfirmasi hapus; empty state. | Modul master data berfungsi. | Selesai |
| Fase 5 — Absensi | `AbsensiRepository` (auto-create sesi, upsert transaksional), `AbsensiView` (dropdown jadwal, tanggal, grid status+catatan), simpan & edit absensi. | Fitur inti absensi berfungsi. | Selesai |
| Fase 6 — Dashboard, Rekap, Riwayat | Query agregat statistik, kartu dashboard, tombol Isi Absensi dari dashboard, rekap bulanan berpersentase, riwayat per atlet berfilter. | Pelaporan dinamis berfungsi. | Selesai |
| Fase 7 — Finishing & QA | Validasi menyeluruh, konsistensi UI, pengujian akhir, dokumentasi (PRD ini), panduan penggunaan. | Build kandidat rilis + dokumen. | Berjalan |

---

## 15. Strategi Pengujian

| Area | Skenario minimum | Hasil yang diharapkan |
|---|---|---|
| Login | Kredensial benar; salah; database mati. | Masuk berhasil; pesan "Username atau password salah."; pesan koneksi tanpa crash. |
| CRUD | Tambah/ubah/hapus kelompok, atlet, jadwal; hapus data yang masih dipakai relasi. | Perubahan tersimpan; hapus terblokir FK menampilkan pesan ramah. |
| Validasi | Nama/kelompok kosong; format jam salah (`17.00`); jam selesai ≤ jam mulai. | Input ditolak dengan pesan spesifik. |
| Pencarian & filter | Cari nama ada/tidak ada; filter kelompok & status. | Tabel menyesuaikan; empty state tampil bila kosong. |
| Absensi | Simpan sesi baru; buka ulang sesi; ubah status lalu simpan lagi. | Satu baris per atlet per sesi; data lama dimuat; update berhasil. |
| Dashboard | Database berisi data dan kosong (hari tanpa jadwal). | Angka sesuai query; empty state jadwal tampil. |
| Rekap & riwayat | Pilih kelompok/bulan/tahun; pilih atlet. | Hitungan dan persentase konsisten dengan absensi tersimpan. |
| Koneksi | Connection string diarahkan ke database tidak ada. | Dialog peringatan muncul; aplikasi tetap dapat digunakan/ditutup normal. |
| Logout | Klik Logout dari sidebar. | Kembali ke login; window utama tertutup. |

---

## 16. Risiko dan Mitigasi

| Risiko | Dampak | Mitigasi |
|---|---|---|
| Connection string tidak sesuai lingkungan | Aplikasi tidak dapat mengakses data. | Connection string terpusat di `DatabaseHelper`; dokumentasi penggantian instance (LocalDB ↔ SQL Server penuh); penambahan `Encrypt=False` untuk kompatibilitas LocalDB dengan Microsoft.Data.SqlClient 5+. |
| Query tidak berparameter | Risiko SQL injection. | Seluruh repository wajib memakai `SqlParameter`; code review. |
| Logika bisnis di code-behind | Kode sulit diuji dan dipelihara. | State dan command berada di ViewModel; code-behind hanya `InitializeComponent` dan jembatan kontrol (PasswordBox, pergantian window). |
| Data dashboard statis | Informasi menyesatkan. | Seluruh kartu memakai query agregat (COUNT/SUM CASE); diuji dengan perubahan data. |
| Absensi ganda per sesi | Data kehadiran tidak valid. | Unique constraint (SesiId, AtletId) + pola upsert pada repository. |
| Kehilangan data saat simpan massal | Sebagian baris tersimpan, sebagian gagal. | Transaksi database dengan rollback pada kegagalan. |
| Duplikasi class antar file (kesalahan pengembangan) | Build gagal (CS0101). | Satu class per file; verifikasi pencarian global sebelum build. |
| File build terkunci dua editor | Build gagal (MSB3061). | Standar kerja: satu editor aktif untuk build; bersihkan bin/obj bila diperlukan. |

---

## 17. Asumsi dan Keputusan Produk
- Versi pertama hanya satu peran (Admin/Pelatih); matriks multi-peran tidak diperlukan.
- Database pengembangan: SQL Server LocalDB `(localdb)\MSSQLLocalDB`; deployment produksi dapat mengganti connection string.
- Akun default sistem: `admin` / `admin123` (hash SHA-256); wajib diganti pada pemakaian nyata.
- SesiLatihan dibuat otomatis saat halaman absensi pertama kali memuat jadwal+tanggal (bukan jadwal terpisah manual).
- Persentase kehadiran = Hadir ÷ total catatan absensi pada periode terpilih.
- Ekspor laporan, notifikasi otomatis, dan tema dark/light ditunda ke rilis berikutnya sesuai kesepakatan scope.
- Backup dan retensi data mengikuti kebijakan pengelola club (di luar aplikasi).

---

## 18. Definisi Selesai (Definition of Done)
- Seluruh persyaratan berprioritas Must (FR-01 s.d. FR-20) telah diimplementasikan dan lulus skenario uji.
- Statistik dashboard, rekap, dan riwayat bersumber dari query database.
- Validasi, konfirmasi, notifikasi, dan empty state tersedia pada seluruh halaman.
- Seluruh query menggunakan parameter; password tidak disimpan sebagai teks biasa.
- Penyimpanan absensi bersifat transaksional dan terlindung constraint unik.
- UI konsisten dengan design system (sidebar navy, tombol primary/secondary/danger, DataGrid modern).
- Kode, skrip database (`Database/schema.sql`), cara instalasi, dan PRD ini terdokumentasi.
