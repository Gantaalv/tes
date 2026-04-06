# FLAG [10] 
### Exploit by Command Injection

### Description 
Apa kode akses vault yang ditemukan di dalam file /opt/nac/final_directive.txt setelah mengakses halaman diagnostik admin?

### Hint
💡 Hint Flag 10: Command Injection, Url

### Analysis
Langkah awal kami lakukan dengan mengidentifikasi halaman administrasi pada direktori /admin/. Karena akses awal ditolak dengan pesan 403 Forbidden, kami melakukan analisis pada parameter URL dan manajemen sesi.

### Solution
1. Akses Admin Dashboard
Karena akses ke admin dashboard dibatasi kami menggunakan user dengan hak akses tinggi yang telah didapatkan sebelumnya.

![accsdenied](<Screenshot 2026-04-06 194837.png>)

user dengan hak akses tinggi (Alexander Magnus).

![highuserauth](image.png)

2. Identifikasi Kerentanan Command Injection
Setelah masuk ke panel admin, kami mengakses menu Diagnostics. Fitur ini menyediakan alat pengujian jaringan (Ping) yang menerima input dari pengguna. Melalui pengujian input, ditemukan bahwa sistem tidak melakukan sanitasi terhadap karakter khusus. Kami memanfaatkan celah OS Command Injection dengan menggunakan karakter pemisah perintah ; (semicolon) untuk menjalankan perintah sistem operasi Linux di luar fungsi aslinya.

![diag](<Screenshot 2026-04-06 203556.png>)


3. Eksplorasi Direktori dan Pengambilan Data
Kami mencoba membaca file /opt/nac/final_directive.txt menggunakan perintah cat, namun file tersebut hanya berisi dokumen arsip. Untuk mencari data yang lebih relevan, kami melakukan inspeksi direktori lebih dalam menggunakan perintah ls -la /opt/nac/.

![cat](<Screenshot 2026-04-06 204143.png>)

![ls](<Screenshot 2026-04-06 205015.png>)

![catflagstore](<Screenshot 2026-04-06 205226.png>)

4. Pengambilan Kode Akses (Flag)
Hasil inspeksi menunjukkan adanya symbolic link tersembunyi bernama .runtime_cache yang mengarah ke file asli di /opt/nac/vault/.flag_store. Kami mengeksekusi perintah cat /opt/nac/vault/.flag_store melalui celah injeksi tersebut. Langkah ini berhasil menampilkan isi file yang berisi kode flag yang di cari (d3c0d3_th3_v4ult_th1s)

![flag](<Screenshot 2026-04-06 213303.png>)