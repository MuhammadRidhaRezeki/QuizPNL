# SRS (Software Requirement Specification) — QuiziPNL

## 1. Pendahuluan
### 1.1 Latar Belakang
Di Politeknik Negeri Lhokseumawe, evaluasi pembelajaran biasanya dilakukan lewat kuis atau ujian.  
Cara manual menggunakan kertas sering menimbulkan kendala seperti lama diperiksa, rawan salah penilaian, dan kurang fleksibel.  
QuiziPNL dibuat untuk membantu dosen membuat kuis secara online, mahasiswa bisa mengerjakan kuis dengan timer, dan nilai tersimpan otomatis di sistem.

### 1.2 Tujuan
Dokumen ini menjelaskan kebutuhan perangkat lunak **QuiziPNL**, agar jelas fungsi yang harus dibuat sebelum masuk ke tahap desain (HLD/LLD) dan implementasi dengan Laravel 12.

### 1.3 Ruang Lingkup
- Sistem berbasis web.  
- Digunakan oleh **Admin/Dosen** dan **Mahasiswa**.  
- Mendukung soal **pilihan ganda (auto-scoring)** dan **essay (manual-scoring)**.  
- Ada fitur **timer kuis**, **randomisasi soal**, **riwayat nilai**, dan **dashboard nilai sederhana**.  

---

## 2. Gambaran Umum
### 2.1 Aktor Utama
- **Admin/Dosen**: membuat kuis, menambahkan soal, memeriksa jawaban essay, melihat rekap nilai.  
- **Mahasiswa**: mengikuti kuis sesuai jadwal, mengerjakan soal, melihat nilai dan riwayat.  

### 2.2 Asumsi
- Akses internet stabil saat mengerjakan kuis.  
- Browser modern (Chrome/Firefox/Edge).  
- Server mendukung PHP 8.3 dan MySQL/MariaDB.  

---

## 3. Kebutuhan Fungsional
- **FR-01 Autentikasi & Role**: login/register sebagai dosen/admin atau mahasiswa.  
- **FR-02 Manajemen Kuis**: CRUD kuis (judul, deskripsi, durasi, jadwal).  
- **FR-03 Bank Soal**: tambah soal pilihan ganda dan essay.  
- **FR-04 Randomisasi Soal**: urutan soal diacak untuk tiap mahasiswa.  
- **FR-05 Pengerjaan Kuis**: mahasiswa menjawab soal dalam durasi yang sudah ditentukan.  
- **FR-06 Timer**: hitung mundur otomatis saat kuis berlangsung.  
- **FR-07 Penilaian**: soal pilihan ganda dinilai otomatis, essay dinilai manual oleh dosen.  
- **FR-08 Riwayat Nilai**: mahasiswa bisa melihat riwayat kuis dan nilai.  
- **FR-09 Dashboard Nilai**: dosen bisa melihat rekap nilai peserta kuis.  
- **FR-10 Ekspor Nilai (opsional)**: rekap nilai bisa diunduh dalam format CSV/Excel.  

---

## 4. Kebutuhan Non-Fungsional
- **Performa**: halaman kuis cepat diakses, respon < 1 detik di server lokal.  
- **Keamanan**: password di-hash, input divalidasi, ada proteksi CSRF.  
- **Ketersediaan**: data nilai tersimpan otomatis, ada peringatan jika koneksi terputus.  
- **UI/UX**: desain responsif dengan TailwindCSS, mudah digunakan.  
- **Aksesibilitas**: form jelas, navigasi mudah, kontras warna cukup.  

---

## 5. Use Case Utama
- **UC-01**: Login/Logout pengguna.  
- **UC-02**: Dosen membuat dan mem-publish kuis.  
- **UC-03**: Dosen menambahkan soal MCQ/Essay.  
- **UC-04**: Mahasiswa mengerjakan kuis dengan timer.  
- **UC-05**: Sistem menilai otomatis soal MCQ, dosen memberi skor essay.  
- **UC-06**: Mahasiswa melihat nilai dan riwayat.  
- **UC-07**: Dosen melihat rekap nilai kuis.  

---

## 6. Model Data (Draft)
- **users** (id, name, email, password, role, kelas, timestamps)  
- **quizzes** (id, title, description, duration_minutes, start_time, end_time, status, created_by, timestamps)  
- **questions** (id, quiz_id, type[mcq/essay], text, points, image_path, timestamps)  
- **options** (id, question_id, text, is_correct, timestamps)  
- **quiz_attempts** (id, quiz_id, user_id, started_at, submitted_at, score, status, timestamps)  
- **answers** (id, attempt_id, question_id, option_id, essay_text, is_correct, score, timestamps)  

---

## 7. Kriteria Penerimaan
- Dosen bisa membuat kuis dengan minimal 5 soal.  
- Mahasiswa bisa mengerjakan kuis sesuai durasi yang sudah diatur.  
- Nilai otomatis keluar untuk soal pilihan ganda.  
- Essay bisa dinilai manual oleh dosen.  
- Mahasiswa bisa melihat riwayat nilai, dosen bisa melihat rekap nilai.  

---

## 8. Risiko
- **Koneksi internet terputus** → sistem menyimpan jawaban per soal agar tidak hilang.  
- **Kesalahan jadwal kuis** → validasi saat dosen membuat kuis.  
- **Beban server tinggi** → gunakan pagination & indexing database.  

---

## 9. Rencana Uji
- **Unit Test**: relasi antar model (Quiz ↔ Question, Attempt ↔ Answer).  
- **Feature Test**: alur pengerjaan kuis dari mulai sampai submit.  
- **UI Test**: timer berjalan, form validasi jawaban.  
- **Performance Test**: load daftar nilai dengan pagination.  

---
