# Commit 1 Reflection notes
- Di dalam fungsi handle_connection, terdapat instance BufReader yang membungkus referensi dari stream.
- Variabel http_request dibuat untuk menampung baris-baris teks yang dikirim oleh browser.
- Data akan disimpan dalam bentuk Vector, yang ditandai dengan anotasi Vec<_>
- Metode .lines() membaca aliran data dan membaginya menjadi baris terpisah setiap mendeteksi adanya karakter newline.
- Untuk mengambil nilai String, kode menggunakan fungsi .map() dan .unwrap()
- Setelah semua baris berhasil dikumppulkan menjadi satu di dalam vector http_request, program akan mencetaknya ke layar konsol.