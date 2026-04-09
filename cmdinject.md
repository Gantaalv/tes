# FLAG [10] 
### Exploit by Command Injection

### Description 
Apa kode akses vault yang ditemukan di dalam file /opt/nac/final_directive.txt setelah mengakses halaman diagnostik admin?

### Hint
💡 Hint Flag 10: Command Injection, Url

### Analysis
Langkah awal sesuai dengan salah satu hint yang diberikan yaitu Url kami mencoba mengidentifikasi url page administrasi yaitu http://160.25.222.15:8070/admin dengan pola Url yang berhubungan dengan soal. Ditemukan lah /admin/diagnostics.php? yang menuju ke error code 403 dengan keterangan Usage: diagnostics.php?token=YOUR_TOKEN yang berarti kita memerlukan token spesial untuk mengakses halaman diagnostik tersebut. 

### Solution
1. Akses halaman diagnostik
Karena akses ke halaman diagnostik memerlukan token, kita kembali ke /var/log/nac/admin_actions.log pada flag sebelumnya yang berisi log aktifitas admin serta token yang kita butuhkan untuk mengakses halaman diagnostik yaitu (7f3a9b2c). Maka didapatlah Url http://160.25.222.15:8070/admin/diagnostics.php?token=7f3a9b2c sebagai url untuk mengakses halaman diagnostik.

![403forbiden](image-1.png)

2. Identifikasi Kerentanan Command Injection
Setelah masuk ke halaman diagnostik, fitur ini menyediakan alat pengujian jaringan (Ping) yang menerima input dari pengguna. Melalui pengujian input, ditemukan bahwa sistem tidak melakukan sanitasi terhadap karakter khusus. Kami memanfaatkan celah OS Command Injection dengan menggunakan karakter pemisah perintah ; (semicolon) untuk menjalankan perintah sistem operasi Linux di luar fungsi aslinya.

![diag](<Screenshot 2026-04-06 203556.png>)


3. Eksplorasi Direktori dan Pengambilan Data
Kami mencoba membaca file /opt/nac/final_directive.txt menggunakan perintah cat, namun file tersebut hanya berisi dokumen arsip. Untuk mencari data yang lebih relevan, kami melakukan inspeksi direktori lebih dalam menggunakan perintah ls -la /opt/nac/.

![cat](<Screenshot 2026-04-06 204143.png>)

![ls](<Screenshot 2026-04-06 205015.png>)

![catflagstore](<Screenshot 2026-04-06 205226.png>)

4. Pengambilan Kode Akses (Flag)
Hasil inspeksi menunjukkan adanya symbolic link tersembunyi bernama .runtime_cache yang mengarah ke file asli di /opt/nac/vault/.flag_store. Kami mengeksekusi perintah cat /opt/nac/vault/.flag_store melalui celah injeksi tersebut. Langkah ini berhasil menampilkan isi file yang berisi kode flag yang di cari 

![flag](<Screenshot 2026-04-06 213303.png>)

5. Submit Flag
Submit flag yang sudah ditemukan (d3c0d3_th3_v4ult_th1s) ke bot. 

![submitflag](<WhatsApp Image 2026-04-05 at 10.16.43.jpeg>)
