# SADB --- Project Specification

**Nama project:** SADB\
**Kepanjangan:** SA02 + Database\
**Jenis aplikasi:** Personal Academic Management System\
**Status:** Perencanaan / MVP\
**Pemilik awal:** Satu pengguna (personal)\
**Bahasa antarmuka awal:** Bahasa Indonesia\
**Gaya desain:** Clean, terang, modern, minimalis, responsif

------------------------------------------------------------------------

## 1. Visi dan tujuan

SADB adalah aplikasi web pribadi untuk mengelola aktivitas perkuliahan
dalam satu tempat. Tujuan utamanya adalah mengurangi kebutuhan mencari
tugas, jadwal, materi, catatan, dan tautan referensi di banyak chat
WhatsApp.

Aplikasi dikembangkan secara bertahap, dengan fondasi sederhana yang
mudah dirawat dan bisa diperluas apabila suatu hari diperlukan dukungan
pengguna lain.

## 2. Prinsip pengembangan

1.  Bangun MVP terlebih dahulu; jangan membuat fitur yang belum
    disetujui.
2.  Utamakan kemudahan penggunaan, aksesibilitas, responsivitas,
    keamanan, dan kode yang mudah dipahami.
3.  Gunakan GitHub sebagai sumber kode utama.
4.  Semua AI dan kontributor wajib membaca dokumen ini sebelum mengubah
    arsitektur atau implementasi.
5.  Jangan menimpa desain, file, atau perilaku yang sudah ada tanpa
    memahami dampaknya.
6.  Jangan mengklaim pengujian berhasil jika pengujian tidak benar-benar
    dijalankan.
7.  Bila ada konflik spesifikasi atau keputusan yang berdampak besar,
    jelaskan masalahnya dan minta persetujuan sebelum melanjutkan.
8.  Jangan memasukkan rahasia, kredensial, token, atau data pribadi ke
    repository.

## 3. Pengguna dan akses

### MVP

-   SADB digunakan oleh satu pemilik akun.
-   Data akademik bersifat privat.
-   Pendaftaran publik tidak diperlukan untuk MVP.
-   UI yang menyembunyikan halaman bukanlah mekanisme keamanan. Akses
    data harus dilindungi di backend/database.
-   Jika kelak aplikasi dibuka untuk pengguna lain, desain autentikasi,
    kepemilikan data, dan kebijakan akses harus ditinjau kembali.

## 4. Teknologi dan arsitektur

### Teknologi yang direncanakan

-   **Frontend:** HTML, CSS, dan JavaScript.
-   **Hosting frontend:** Vercel.
-   **Version control:** Git dan GitHub.
-   **Backend terkelola:** Supabase.
-   **Autentikasi:** Supabase Auth.
-   **Database:** Supabase PostgreSQL.
-   **Keamanan data:** PostgreSQL Row Level Security (RLS).
-   **Integrasi file tahap lanjutan:** Google Drive API melalui alur
    backend tepercaya, misalnya Supabase Edge Functions, bila sesuai
    dengan kebutuhan dan batasan API.

### Alur arsitektur

1.  Browser memuat frontend dari Vercel.
2.  Pengguna melakukan autentikasi melalui Supabase Auth.
3.  Frontend mengakses data aplikasi melalui Supabase dengan kebijakan
    RLS yang membatasi akses.
4.  Database menyimpan data terstruktur dan metadata file, bukan isi
    dokumen/video yang besar.
5.  Jika integrasi Google Drive diimplementasikan, operasi OAuth dan
    tindakan sensitif dilakukan melalui alur backend yang sesuai.
    Rahasia OAuth dan token sensitif tidak boleh ditanam di frontend.
6.  Google Drive menyimpan file sebenarnya; database SADB menyimpan
    metadata dan referensi yang diperlukan.

**Catatan:** Detail OAuth, metode upload, dan izin Drive harus
diverifikasi terhadap dokumentasi Google terbaru sebelum implementasi.
Jangan mengasumsikan endpoint atau scope tanpa memeriksa dokumentasi
resmi.

## 5. Fitur MVP

### 5.1 Dashboard

-   Ringkasan tugas yang belum selesai.
-   Tugas yang mendekati deadline.
-   Jadwal kuliah terdekat.
-   Ringkasan mata kuliah.
-   Aksi cepat untuk menambahkan tugas atau catatan.

### 5.2 Mata kuliah

-   Tambah, lihat, ubah, dan hapus mata kuliah.
-   Data minimum: nama mata kuliah, kode opsional, dosen opsional,
    deskripsi opsional, dan warna/penanda opsional.
-   Data mata kuliah dapat dihubungkan dengan tugas, catatan, dan
    materi.

### 5.3 Tugas dan deadline

-   Tambah, lihat, ubah, dan hapus tugas.
-   Data minimum: judul, deskripsi opsional, mata kuliah opsional,
    deadline opsional, status, prioritas, dan waktu pembuatan.
-   Status awal: `todo`, `in_progress`, `done`.
-   Prioritas awal: `low`, `medium`, `high`.
-   Sediakan pencarian dan filter berdasarkan status, mata kuliah, serta
    deadline.
-   Zona waktu tampilan mengikuti waktu lokal pengguna; penyimpanan
    tanggal/waktu harus konsisten.

### 5.4 Jadwal kuliah

-   Tambah, lihat, ubah, dan hapus jadwal.
-   Data minimum: nama kegiatan/mata kuliah, hari atau tanggal, waktu
    mulai, waktu selesai, lokasi opsional, dan catatan opsional.
-   Tampilan jadwal harus nyaman di layar ponsel dan laptop.
-   Hindari benturan jadwal yang tidak sengaja; jika ada benturan,
    tampilkan peringatan, bukan menghapus data otomatis.

### 5.5 Catatan

-   Buat, lihat, ubah, dan hapus catatan.
-   Data minimum: judul, isi, mata kuliah opsional, dan waktu pembaruan.
-   Untuk MVP, editor teks sederhana sudah cukup.

### 5.6 Materi dan referensi

-   Simpan judul, deskripsi, mata kuliah terkait, tipe sumber, dan
    referensi.
-   Dukung tautan eksternal, termasuk tautan pesan atau file WhatsApp
    yang diberikan pengguna.
-   Jangan menganggap tautan WhatsApp sebagai file yang tersimpan di
    SADB.
-   Integrasi unggah dan pengelolaan file Google Drive merupakan tahap
    lanjutan.

### 5.7 Pencarian dan tampilan

-   Pencarian materi, tugas, catatan, dan mata kuliah.
-   Filter yang relevan pada setiap halaman.
-   Tema terang sebagai tema default.
-   Struktur CSS memungkinkan tema gelap ditambahkan tanpa menulis ulang
    semua komponen.
-   Desain responsif untuk ponsel dan desktop.
-   Gunakan state kosong, indikator loading, pesan error yang ramah, dan
    konfirmasi untuk tindakan destruktif.

## 6. Fitur di luar MVP

Jangan mengimplementasikan hal-hal berikut sebelum diminta atau
disetujui:

-   Kolaborasi multi-pengguna dan berbagi data antar-akun.
-   Notifikasi push atau integrasi kalender eksternal.
-   AI untuk merangkum materi.
-   Aplikasi native Android/iOS.
-   Pembayaran atau fitur berlangganan.
-   Sistem role/permission yang kompleks.
-   Sinkronisasi otomatis dengan chat WhatsApp.
-   Penyimpanan file besar di database PostgreSQL.

## 7. Rancangan data awal

Skema berikut adalah rancangan konseptual. Buat migration SQL yang
eksplisit dan tinjau sebelum diterapkan ke project Supabase.

### `profiles`

-   `id`: UUID, primary key, terhubung ke `auth.users.id`.
-   `display_name`: teks opsional.
-   `created_at`: timestamp.
-   `updated_at`: timestamp.

### `courses`

-   `id`: UUID, primary key.
-   `user_id`: UUID, pemilik data.
-   `name`: teks wajib.
-   `code`: teks opsional.
-   `lecturer`: teks opsional.
-   `description`: teks opsional.
-   `color`: teks opsional.
-   `created_at`, `updated_at`: timestamp.

### `tasks`

-   `id`: UUID, primary key.
-   `user_id`: UUID, pemilik data.
-   `course_id`: UUID opsional, foreign key ke `courses.id`.
-   `title`: teks wajib.
-   `description`: teks opsional.
-   `due_at`: timestamp opsional.
-   `status`: enum atau teks tervalidasi (`todo`, `in_progress`,
    `done`).
-   `priority`: enum atau teks tervalidasi (`low`, `medium`, `high`).
-   `created_at`, `updated_at`: timestamp.

### `schedule_events`

-   `id`: UUID, primary key.
-   `user_id`: UUID, pemilik data.
-   `course_id`: UUID opsional, foreign key ke `courses.id`.
-   `title`: teks wajib.
-   `day_of_week`: integer opsional (gunakan konvensi yang
    didokumentasikan) atau tanggal untuk acara satu kali.
-   `start_time`, `end_time`: waktu opsional untuk jadwal mingguan.
-   `starts_at`, `ends_at`: timestamp opsional untuk acara bertanggal.
-   `location`, `notes`: teks opsional.
-   `created_at`, `updated_at`: timestamp.

Pilih model jadwal mingguan atau acara bertanggal secara konsisten saat
membuat migration; jangan membuat kolom yang saling tumpang tindih tanpa
kebutuhan jelas.

### `notes`

-   `id`: UUID, primary key.
-   `user_id`: UUID, pemilik data.
-   `course_id`: UUID opsional, foreign key ke `courses.id`.
-   `title`: teks wajib.
-   `content`: teks opsional.
-   `created_at`, `updated_at`: timestamp.

### `materials`

-   `id`: UUID, primary key.
-   `user_id`: UUID, pemilik data.
-   `course_id`: UUID opsional, foreign key ke `courses.id`.
-   `title`: teks wajib.
-   `description`: teks opsional.
-   `source_type`: nilai tervalidasi, misalnya `external_link` atau
    `google_drive`.
-   `external_url`: URL opsional.
-   `created_at`, `updated_at`: timestamp.

### `files` (untuk tahap Google Drive)

-   `id`: UUID, primary key.
-   `user_id`: UUID, pemilik data.
-   `material_id`: UUID opsional, foreign key ke `materials.id`.
-   `drive_file_id`: ID file Google Drive, bila tersedia.
-   `file_name`: teks.
-   `mime_type`: teks opsional.
-   `size_bytes`: integer opsional.
-   `created_at`, `updated_at`: timestamp.

Jangan menyimpan access token, refresh token, OAuth client secret, atau
URL sesi upload yang sensitif dalam tabel metadata file. Rancangan
penyimpanan token harus ditinjau terpisah sebelum integrasi Drive.

### Aturan data

-   Gunakan UUID untuk ID entitas utama.
-   Tambahkan foreign key, validasi, dan indeks yang diperlukan.
-   Kolom kepemilikan harus ditetapkan dari sesi pengguna yang
    terautentikasi, bukan dipercaya begitu saja dari input browser.
-   Hapus data terkait secara terencana; jangan menggunakan cascade
    delete tanpa memahami konsekuensinya.
-   Skema akhir harus disertai migration SQL yang bisa ditinjau dan
    diuji.

## 8. Aturan keamanan wajib

1.  Aktifkan RLS pada setiap tabel yang menyimpan data pengguna.
2.  Buat policy agar pengguna hanya dapat membaca dan mengubah data
    miliknya.
3.  Terapkan validasi input di frontend dan backend/database; validasi
    frontend saja tidak cukup.
4.  Jangan pernah memasukkan Supabase service-role key, OAuth client
    secret, access token, atau refresh token ke kode frontend.
5.  Environment variable frontend yang menggunakan prefiks publik tetap
    dapat terlihat oleh browser. Hanya masukkan nilai yang memang aman
    untuk publik.
6.  Jangan menonaktifkan RLS untuk mengatasi error.
7.  Jangan mempercayai `user_id`, ID file, atau metadata dari browser
    tanpa memverifikasi kepemilikan dan izin.
8.  File Google Drive bersifat privat secara default. Jangan membuat
    tautan publik otomatis.
9.  Jika fitur berbagi file ditambahkan, buat tindakan eksplisit,
    tampilkan konsekuensi izin, dan sediakan pencabutan akses jika
    didukung.
10. Jangan meminta pengguna menuliskan password, token, atau rahasia
    akun di chat AI.
11. Jangan commit `.env`, file kredensial, dump data pribadi, atau
    secret ke GitHub.
12. Gunakan pesan error yang aman; jangan tampilkan token atau detail
    internal yang sensitif.
13. Perubahan database atau policy keamanan harus dijelaskan sebelum
    diterapkan.

## 9. Aturan desain dan aksesibilitas

-   Tema awal terang dengan tampilan bersih, modern, dan minim dekorasi.
-   Gunakan hierarki visual yang konsisten, ruang kosong yang cukup,
    tipografi yang mudah dibaca, dan komponen yang konsisten.
-   Pastikan kontras teks memadai dan kontrol dapat digunakan dengan
    keyboard.
-   Tombol dan input memiliki label yang jelas.
-   Jangan menyampaikan status hanya melalui warna.
-   Hindari animasi berlebihan.
-   Semua halaman utama harus responsif dan tetap dapat digunakan pada
    layar ponsel.
-   Jangan mengubah desain yang telah disetujui secara tidak terkait
    saat mengerjakan fitur lain.

## 10. Struktur repository awal

Struktur ini merupakan usulan awal, bukan alasan untuk memindahkan atau
mengganti file yang sudah ada tanpa pemeriksaan.

``` text
SADB/
├── index.html
├── assets/
│   ├── css/
│   │   └── styles.css
│   ├── js/
│   │   ├── app.js
│   │   └── config.example.js
│   └── images/
├── pages/
│   ├── login.html
│   ├── dashboard.html
│   ├── courses.html
│   ├── tasks.html
│   ├── schedule.html
│   ├── notes.html
│   └── materials.html
├── supabase/
│   └── migrations/
├── PROJECT_SPEC.md
├── README.md
└── .gitignore
```

Struktur dapat disederhanakan atau disesuaikan berdasarkan keputusan
implementasi pertama. Jangan menambahkan framework atau dependency besar
tanpa alasan dan persetujuan.

## 11. Environment dan repository

-   Gunakan `.env` lokal bila tooling memerlukannya, dan pastikan `.env`
    masuk `.gitignore`.
-   Sediakan `.env.example` hanya dengan nama variabel dan placeholder,
    tanpa nilai rahasia.
-   Jangan pernah menyalin kredensial nyata ke dokumentasi.
-   Simpan konfigurasi Supabase publik hanya pada lokasi yang sesuai
    untuk frontend; tetap lindungi data dengan RLS.
-   Hubungkan repository GitHub ke Vercel setelah build dan konfigurasi
    dasar siap.
-   Gunakan branch `main` untuk versi stabil dan branch fitur seperti
    `feature/ui-foundation`, `feature/auth`, serta `feature/tasks` untuk
    perubahan terpisah.

## 12. Pembagian kerja AI

### ChatGPT --- Project Lead

-   Menjaga spesifikasi dan arsitektur.
-   Memecah pekerjaan menjadi task kecil.
-   Meninjau hasil, integrasi, dan risiko.
-   Membantu debugging berdasarkan kode dan error yang benar-benar
    diberikan.
-   Tidak mengklaim telah mengakses repository atau menjalankan tes jika
    belum dilakukan.

### Claude --- Implementation Engineer

-   Mengerjakan task implementasi yang spesifik.
-   Membaca file dan spesifikasi sebelum mengubah kode.
-   Mengembalikan daftar file yang berubah, kode lengkap yang relevan,
    keputusan, dan instruksi pengujian.
-   Tidak mengubah arsitektur atau menambahkan dependency di luar scope
    tanpa persetujuan.

### Gemini --- Reviewer / Test Engineer

-   Meninjau hasil implementasi secara independen.
-   Mencari bug, edge case, masalah aksesibilitas, keamanan, dan
    ketidaksesuaian spesifikasi.
-   Memberi temuan dengan tingkat prioritas dan bukti yang jelas.
-   Memisahkan bug yang terkonfirmasi dari risiko yang belum
    diverifikasi.

AI yang berbeda tidak boleh mengedit file yang sama secara bersamaan
tanpa koordinasi. GitHub adalah sumber kebenaran untuk versi kode.

## 13. Alur kerja setiap task

1.  Project Lead membuat ID task, tujuan, scope, file terkait, dan
    acceptance criteria.
2.  Satu AI mengimplementasikan task tersebut.
3.  Implementasi ditinjau oleh Project Lead dan, jika perlu, AI
    reviewer.
4.  Perubahan diuji secara lokal.
5.  Perubahan digabungkan ke branch stabil setelah hasil ditinjau.
6.  Catat keputusan arsitektur atau perubahan skema yang penting dalam
    dokumentasi.

### Format laporan hasil

-   Task ID dan status.
-   Ringkasan implementasi.
-   File yang dibuat/diubah.
-   Keputusan teknis.
-   Perubahan API, dependency, atau database.
-   Tes yang benar-benar dijalankan dan hasilnya.
-   Tes yang belum dijalankan.
-   Risiko keamanan atau integrasi.
-   Cara menjalankan dan memverifikasi hasil.
-   Hal yang memerlukan keputusan Project Lead.

## 14. Acceptance criteria MVP

MVP dianggap siap untuk penggunaan pribadi awal jika:

-   Pengguna dapat masuk dan keluar dengan aman.
-   Pengguna dapat mengelola mata kuliah, tugas, jadwal, catatan, dan
    referensi.
-   Data tetap tersedia setelah halaman dimuat ulang.
-   Pengguna dapat mencari dan memfilter data yang relevan.
-   Halaman utama berfungsi di ponsel dan desktop.
-   Status loading, kosong, sukses, dan error ditangani dengan jelas.
-   RLS diuji untuk memastikan akses tanpa izin ditolak.
-   Tidak ada secret di repository atau bundle frontend.
-   Instruksi setup dan deployment tersedia di `README.md`.
-   Fitur Google Drive boleh tetap belum tersedia pada MVP pertama,
    selama keterbatasannya dijelaskan dengan jelas.

## 15. Tahap implementasi

### Tahap 1 --- Repository dan UI foundation

-   Buat repository GitHub.
-   Tambahkan dokumen ini dan README.
-   Implementasikan layout responsif, navigasi, dashboard awal, komponen
    dasar, dan tema terang.
-   Gunakan data contoh yang jelas diberi label sebagai data dummy.
-   Jangan membuat asumsi bahwa backend sudah tersedia.

### Tahap 2 --- Supabase dan autentikasi

-   Buat project Supabase.
-   Tetapkan skema dan migration.
-   Terapkan RLS dan uji akses.
-   Implementasikan login, logout, session handling, serta alur
    pemulihan akun jika diperlukan.
-   Untuk penggunaan personal, nonaktifkan pendaftaran publik setelah
    akun pemilik dibuat dan diverifikasi.

### Tahap 3 --- Fitur akademik

-   Implementasikan mata kuliah, tugas, jadwal, catatan, materi,
    pencarian, dan filter.
-   Hubungkan semua data dengan pemilik akun.
-   Uji operasi tambah, baca, ubah, dan hapus.

### Tahap 4 --- Google Drive

-   Verifikasi dokumentasi resmi dan kebutuhan OAuth.
-   Tentukan scope minimum yang dibutuhkan.
-   Implementasikan alur backend tepercaya untuk operasi sensitif.
-   Pastikan file privat secara default.
-   Uji upload, unduh/akses, kegagalan jaringan, validasi tipe/ukuran
    file, serta pencabutan izin berbagi jika fitur berbagi disediakan.

### Tahap 5 --- Hardening dan deployment

-   Uji responsivitas, validasi, sesi, policy RLS, dan error jaringan.
-   Uji backup dan pemulihan data yang relevan.
-   Deploy ke Vercel.
-   Dokumentasikan batasan layanan gratis dan prosedur pemeliharaan.

## 16. Keputusan yang belum final

Hal berikut harus diputuskan saat diperlukan, bukan diasumsikan oleh AI:

-   Apakah jadwal disimpan sebagai jadwal mingguan, acara satu kali,
    atau keduanya.
-   Apakah Google Drive menggunakan OAuth per pengguna atau pendekatan
    lain yang sesuai dan aman.
-   Bagaimana batas ukuran file dan tipe file yang diizinkan.
-   Apakah diperlukan ekspor data dalam format JSON/CSV.
-   Apakah tema gelap masuk MVP atau ditambahkan setelah tema terang
    stabil.

------------------------------------------------------------------------

**Status dokumen:** Draft awal untuk menjadi sumber kebenaran bersama.\
**Aturan perubahan:** Perubahan arsitektur, keamanan, atau skema data
harus disertai alasan dan persetujuan Project Lead.
