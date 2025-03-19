<details>
<summary>Commit 1</summary>

Fungsi `handle_connection` bertugas memproses koneksi TCP dari klien ke server HTTP sederhana. Fungsi ini menerima `TcpStream` yang merepresentasikan koneksi aktif, lalu menggunakan `BufReader` untuk membaca data dari stream secara efisien. Kode di dalamnya membaca baris-baris teks dari permintaan HTTP yang dikirim klien (menggunakan `.lines()`), mengabaikan kesalahan dengan `.unwrap()` (sederhana, tidak ideal untuk produksi), dan mengumpulkan header HTTP hingga menemukan baris kosong (dihentikan oleh `.take_while(|line| !line.is_empty())`). Hasilnya disimpan dalam vektor `http_request` yang kemudian dicetak ke konsol untuk logging, meski belum mengirim respons balik ke klien. Fungsi ini hanya menangani pembacaan dan logging permintaan, belum menghasilkan respons HTTP yang valid.

</details>

<details>
<summary>Commit 2</summary>

![Commit 2 screen capture](commit2.png)

Perbedaan utama pada versi terbaru `handle_connection` ini adalah **penambahan logika untuk mengirim respons HTTP kembali ke klien**, sedangkan sebelumnya hanya membaca dan menampilkan permintaan. Pada kode baru ini, setelah membaca header request, fungsi ini:  
1. **Membaca file HTML** (`hello.html`) menggunakan `fs::read_to_string`,  
2. **Membentuk respons HTTP lengkap** dengan status `200 OK`, header `Content-Length`, dan konten HTML dari file,  
3. **Mengirim respons** ke klien melalui `stream.write_all()`.  

Sebelumnya, fungsi hanya berhenti di logging request tanpa respons, sedangkan versi ini membuat server menjadi **fungsional** (bisa menampilkan halaman web). Perubahan ini juga memperkenalkan potensi error handling yang kurang robust (masih pakai `unwrap()`) dan ketergantungan pada file eksternal `hello.html`.

</details>


