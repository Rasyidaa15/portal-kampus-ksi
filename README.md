# Portal Kampus Fiktif (Latihan KSI)

Portal kampus fiktif untuk latihan keamanan sistem informasi. Semua akun dan data bersifat sintetis. Aplikasi ini **sengaja dibuat rentan**, jadi jangan dipakai untuk data asli.

Website: https://rasyidaa15.github.io/portal-kampus-ksi/

## Fitur

- Mahasiswa: masuk, melihat nilai, mengunggah tugas, berdiskusi.
- Dosen: memberi nilai.
- Admin: mengelola akun (ubah peran, nonaktifkan).

Aplikasi berupa satu file `index.html` yang berjalan sepenuhnya di browser. Data disimpan di localStorage, tanpa server.

## Akun latihan (sintetis)

| Peran | Username | Sandi |
|---|---|---|
| Admin | admin | Admin#2026 |
| Dosen | dosen1 | Dosen#2026 |
| Mahasiswa | mhs1 | Mhs#0001 |
| Mahasiswa | mhs2 | Mhs#0002 |

Tombol "Reset data latihan" di halaman masuk mengembalikan semua data ke kondisi awal.

## Kerentanan yang sengaja dibuat

### 1. IDOR / kontrol akses lemah
- Lokasi: halaman Nilai (`#/nilai/ID`), fungsi `vNilai` di `index.html` (ditandai `VULN-1`).
- Penyebab: ID pemilik nilai diambil dari URL dan tidak dicocokkan dengan pengguna yang sedang login.
- Cara membuktikan: masuk sebagai `mhs1`, buka menu Nilai (`#/nilai/3`), lalu ubah angka di URL menjadi `4`. Nilai milik `mhs2` ikut tampil.

### 2. Stored XSS
- Lokasi: halaman Diskusi, fungsi `drawForum` di `index.html` (ditandai `VULN-2`).
- Penyebab: isi komentar disimpan apa adanya dan ditampilkan dengan `innerHTML` tanpa encoding.
- Cara membuktikan: masuk sebagai mahasiswa, kirim komentar `<img src=x onerror=alert('XSS')>`. Alert muncul setiap halaman Diskusi dibuka.

### 3. Eskalasi hak akses
- Lokasi: rute `#/dosen` dan `#/admin`, fungsi `render` dan `vAdmin` di `index.html` (ditandai `VULN-3`).
- Penyebab: peran hanya dipakai untuk menyembunyikan menu, rute tidak memeriksa peran pengguna.
- Cara membuktikan: masuk sebagai `mhs1`, lalu ketik `#/admin` di address bar. Halaman admin terbuka dan peran akun dapat diubah.

## Catatan

Pengujian hanya dilakukan pada portal ini dengan akun dan data sintetis, untuk keperluan pembelajaran.
