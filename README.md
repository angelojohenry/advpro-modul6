# Commit 1 Reflection notes
- Di dalam fungsi handle_connection, terdapat instance BufReader yang membungkus referensi dari stream.
- Variabel http_request dibuat untuk menampung baris-baris teks yang dikirim oleh browser.
- Data akan disimpan dalam bentuk Vector, yang ditandai dengan anotasi Vec<_>
- Metode .lines() membaca aliran data dan membaginya menjadi baris terpisah setiap mendeteksi adanya karakter newline.
- Untuk mengambil nilai String, kode menggunakan fungsi .map() dan .unwrap()
- Setelah semua baris berhasil dikumppulkan menjadi satu di dalam vector http_request, program akan mencetaknya ke layar konsol.

# Commit 2 Reflection notes
- Penambahan fs ke dalam pernyataan use tujuannya adalah mengimpor module file system dari standard library Rust. Tanpa modul ini, program tidak akan memiliki izin untuk berinteraksi dengan file yang ada di dalam hard drive server.
- Dengan menggunakan modul fs, prgram membaca seluruh isi dari sebuah file.
- fs::read_to_string akan mengambil kode HTML di dalam file dan menyimpannya ke dalam memori aplikasi sebagai tipe data String.
- Makro format! berfungsi menggabungkan teks header HTTP standar dengan string berisi kode HTML ke dalam satu kesatuan teks yang disebut response body.
- Header Content-Length di dalam response berfungsi agar pesan HTTP dianggap valid secara standar dan mencegah browser mengalami loading terus-menerus karena menunggu data yang sebenarnya sudah selesai dikirim.
  ![Commit 2 screen capture](/assets/images/commit2.png)

# Commit 3 Reflection notes
- Setelah baris pertama (request_line) berhasil diambil, program akan melakukan pengecekan:
Blok if: Program mengecek apakah baris tersebut persis sama dengan GET / HTTP/1.1 . Jika cocok, server akan mengirimkan isi dari file hello.html.
Blok else: Jika requestnya berbeda (misalnya browser meminta 127.0.0.1:7878/bad atau URL lainnya), program akan masuk ke blok else yang akan mengirimkan status error.
- Selain mengirimkan status error, server juga akan mengirimkan sebuah file HTML khusus (halaman error 404) agar pengguna melihat tampilan peringatan yang rapi di browser, bukan sekadar layar kosong atau pesan connection error.
  ![Commit 3 screen capture](/assets/images/commit3.png)
- Refactor diperlukan karena terdapat banyak repetisi di dalam blok if dan else. Mereka sama-sama membaca file dan menulis konten dari file ke dalam stream.