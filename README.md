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

# Commit 4 Reflection notes
- Server yang ditulis berjalan secara synchronous pada satu thread. Ia hanya bisa melayani satu pelanggan pada satu waktu. Saat server sedang melayani /sleep, ia benar-benar sibuk dan membiarkan antrean request lain menumpuk. Ini mendemonstrasikan secara langsung mengapa aplikasi dunia nyata membutuhkan arsitektur konkuren (seperti Thread Pool) untuk melayani banyak request secara bersamaan tanpa saling memblokir.

# Commit 5 Reflection notes
Cara kerja Thread Pool:
- Listener: Main Thread bertugas mendengarkan koneksi jaringan yang masuk.
- Queue: Setiap tugas dimasukkan ke dalam saluran komunikasi (seperti pipa mpsc di Rust).
- Workers: Sejumlah thread siaga di latar belakang. Mereka secara terus-menerus memantau antrean.
- Eksekusi Konkuren: Pekerja yang kosong mengambil tugas, memprosesnya, mengirimkan hasil berupa response ke browser, lalu kembali menganggur untuk mengambil tugas berikutnya.

# Commit Bonus Reflection notes
Beberapa perbandingan fungsi new dan fungsi build:
1. Fungsi new sebelumnya menggunakan makro assert!(size > 0). Dalam konteks aplikasi server-side, penggunaan assert! yang memicu panic! memiliki beberapa kelemahan:
- Jika user memberikan input 0, aplikasi akan langsung berhenti secara paksa (crash).
- Library user tidak diberikan kesempatan untuk menangani kegagalan tersebut dengan anggun (graceful handling).
2. Dengan mengubah fungsi menjadi build(size: usize) -> Result<ThreadPool, PoolCreationError>, logika inisialisasi kini mengikuti prinsip Error Handling yang lebih baik di Rust:
- Kegagalan inisialisasi kini diperlakukan sebagai nilai balik, bukan sebagai error yang menghentikan program.
- Fungsi secara eksplisit memberi tahu developer bahwa fungsi ini bisa gagal dan mereka wajib menangani kemungkinan tersebut.
3. Beberapa prinsip yang diterapkan setelah menggunakan build:
- Robustness: Meningkatkan ketahanan program terhadap input yang tidak valid
- Separation of Concerns: Memisahkan logika pengecekan validitas input dari logikan pembuatan struktur data
- Custom Error Type: Penggunaan PoolCreationError memungkinkan spesifikasi error yang lebih jelas di masa depan jika terdapat parameter lain yang divalidasi.

