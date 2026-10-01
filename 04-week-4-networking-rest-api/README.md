# Laporan Praktikum Modul 04: Future & REST API Dasar

- **Nama**: Nurin Albait
- **NIM**: 362558302005
- **Kelas / Prodi**: 2E / Sarjana Terapan TRPL
- **Mata Kuliah**: Pemrograman Perangkat Bergerak (Semester 5)

---

## 1. Ringkasan Implementasi
Praktikum ini mengimplementasikan integrasi REST API menggunakan library `http` dan pola manajemen state lokal `StatefulWidget` dengan `FutureBuilder`. Fitur utama yang dikembangkan meliputi:
- Pemanggilan HTTP GET ke endpoint API `jsonplaceholder.typicode.com/posts` dengan timeout 10 detik.
- Penanganan 4 keadaan antarmuka secara presisi: *Loading*, *Error*, *Empty*, dan *Success*.
- Penyaringan lokal berdasarkan kategori tanpa memicu *request* baru ke server.
- Fitur *Pull-to-Refresh* dan tombol segarkan data pada AppBar.
- Layar detail pengumuman melalui navigasi `Navigator.push`.

---

## 2. Bukti Keadaan Antarmuka (4-State UI)

### A. Keadaan Memuat (Loading)
Menampilkan indikator berputar saat data Future sedang dalam perjalanan.
![Keadaan Memuat](./gambar%201.png)

### B. Keadaan Berhasil (Success)
Menampilkan daftar kartu pengumuman yang berhasil dipetakan dari JSON.
![Keadaan Berhasil](./gambar%202.png)

### C. Keadaan Gagal (Error)
Menampilkan pesan kesalahan koneksi/timeout dan menyediakan tombol coba lagi.
![Keadaan Gagal](./gambar%203.png)

---

## 3. Pertanyaan Refleksi

1. **Penyaringan di build() vs di Repository:**
   - *Lebih mudah:* Penyaringan lokal di `build()` sangat cepat dan tidak membebani server/jaringan.
   - *Lebih sulit:* Jika data di server bertambah hingga ribuan baris, penyaringan lokal tidak lagi efisien sehingga kategori harus diteruskan sebagai parameter query ke API/Repository.

2. **Dampak menyembunyikan error dengan `catch (e) { return []; }`:**
   Menyembunyikan error membuat kegagalan jaringan tertukar dengan keadaan data kosong (empty). Pengguna mengira tidak ada pengumuman baru, padahal server sedang down atau internet terputus. Seharusnya error dilempar ulang agar `FutureBuilder` dapat menampilkan pesan kegagalan dan tombol retry.

   