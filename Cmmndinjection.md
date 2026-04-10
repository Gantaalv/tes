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

<img width="1317" height="611" alt="Screenshot 2026-04-06 200340" src="https://github.com/user-attachments/assets/471f757c-2c90-4d77-924c-e07d13d7be66" />


user dengan hak akses tinggi (Alexander Magnus).

<img width="1331" height="672" alt="Screenshot 2026-04-06 213303" src="https://github.com/user-attachments/assets/d950f8a5-c768-439f-8325-73eed04104fa" />


2. Identifikasi Kerentanan Command Injection
Setelah masuk ke panel admin, kami mengakses menu Diagnostics. Fitur ini menyediakan alat pengujian jaringan (Ping) yang menerima input dari pengguna. Melalui pengujian input, ditemukan bahwa sistem tidak melakukan sanitasi terhadap karakter khusus. Kami memanfaatkan celah OS Command Injection dengan menggunakan karakter pemisah perintah ; (semicolon) untuk menjalankan perintah sistem operasi Linux di luar fungsi aslinya.

<img width="1359" height="669" alt="Screenshot 2026-04-06 203556" src="https://github.com/user-attachments/assets/91db2ce2-fab7-4a41-b499-3efb4e224bb6" />



3. Eksplorasi Direktori dan Pengambilan Data
Kami mencoba membaca file /opt/nac/final_directive.txt menggunakan perintah cat, namun file tersebut hanya berisi dokumen arsip. Untuk mencari data yang lebih relevan, kami melakukan inspeksi direktori lebih dalam menggunakan perintah ls -la /opt/nac/.

<img width="1357" height="725" alt="Screenshot 2026-04-06 204143" src="https://github.com/user-attachments/assets/69a4d357-0380-4465-98ca-2496dfca5bfe" />


<img width="1322" height="659" alt="Screenshot 2026-04-06 205015" src="https://github.com/user-attachments/assets/5bb0c77b-f190-477b-a03f-7b5942b07e9e" />

4. Pengambilan Kode Akses (Flag)
Hasil inspeksi menunjukkan adanya symbolic link tersembunyi bernama .runtime_cache yang mengarah ke file asli di /opt/nac/vault/.flag_store. Kami mengeksekusi perintah cat /opt/nac/vault/.flag_store melalui celah injeksi tersebut. Langkah ini berhasil menampilkan isi file yang berisi kode flag yang di cari (d3c0d3_th3_v4ult_th1s)

<img width="1320" height="681" alt="Screenshot 2026-04-06 205226" src="https://github.com/user-attachments/assets/a8c14523-9072-4044-8b05-d8a5e50d9b29" />

5. Submit Flag Submit flag yang sudah ditemukan (d3c0d3_th3_v4ult_th1s) ke bot.
   
![WhatsApp Image 2026-04-05 at 10 16 43](https://github.com/user-attachments/assets/a3df403a-00cc-4aaa-9f8b-755f5c7ef4f4)
