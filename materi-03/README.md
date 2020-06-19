Application Containerization and Microservice Orchestration

Stage Setup
Mari kita mulai dengan mengkloning repositori kode demo, mengubah direktori kerja, dan memeriksa cabang demo terlebih dahulu.
Step 0: Basic Link Extractor Script
Checkout the step0 branch and list files in it.
Step 1: Containerized Link Extractor Script
Dengan menggunakan Dockerfile ini, kita dapat menyiapkan gambar Docker untuk skrip ini. Kami mulai dari gambar Python Docker resmi yang berisi lingkungan run-time Python serta alat yang diperlukan untuk menginstal paket dan dependensi Python. Kami kemudian menambahkan beberapa metadata sebagai label (langkah ini tidak penting, tetapi merupakan praktik yang baik). Berikutnya dua instruksi jalankan perintah install pip untuk menginstal dua paket pihak ketiga yang diperlukan agar script berfungsi dengan baik. Kami kemudian membuat direktori / aplikasi yang berfungsi, menyalin file linkextractor.py di dalamnya, dan mengubah izinnya untuk membuatnya menjadi skrip yang dapat dieksekusi. Akhirnya, kami menetapkan skrip sebagai titik masuk untuk gambar.

Step 2: Link Extractor Module with Full URI and Anchor Text
Pada langkah ini skrip linkextractor.py diperbarui dengan perubahan fungsional berikut:
-Jalur dinormalisasi ke URL lengkap
-Melaporkan tautan dan teks jangkar
-Dapat digunakan sebagai modul di skrip lain

Step 3: Link Extractor API Service
Perubahan berikut telah dilakukan pada langkah ini:
-Menambahkan skrip server main.py yang menggunakan modul ekstraksi tautan yang ditulis pada langkah terakhir
-Dockerfile diperbarui untuk merujuk ke file main.py
-Server dapat diakses sebagai API WEB di http: // <hostname> [: <prt>] / api / <url>
-Ketergantungan dipindahkan ke file requirement.txt
-Membutuhkan pemetaan port untuk membuat layanan dapat diakses di luar wadah (server Flask yang digunakan di sini mendengarkan pada port 5000 secara default)

Step 4: Link Extractor API and Web Front End Services
Pada langkah ini perubahan berikut telah dilakukan sejak langkah terakhir:
-Layanan tautan API JSON extractor (ditulis dengan Python) dipindahkan dalam folder ./api terpisah yang memiliki kode yang sama persis seperti pada langkah sebelumnya
-Aplikasi front-end web ditulis dalam PHP di bawah folder ./www yang berbicara dengan API JSON
-Aplikasi PHP dipasang di dalam php resmi: gambar Docker 7-apache untuk modifikasi yang lebih mudah selama pengembangan
-Aplikasi web dapat diakses di http: // <hostname> [: <prt>] /? Url = <url-encoded-url>
-Variabel lingkungan API_ENDPOINT digunakan di dalam aplikasi PHP untuk mengkonfigurasinya untuk berbicara dengan server JSON API
-File docker-compose.yml ditulis untuk membangun berbagai komponen dan merekatkannya.

Step 5: Redis Service for Caching
Beberapa perubahan nyata dari langkah sebelumnya adalah sebagai berikut:
-Dockerfile lain ditambahkan di folder ./www untuk aplikasi web PHP untuk membangun gambar yang lengkap dan menghindari pemasangan file langsung
-Wadah Redis ditambahkan untuk caching menggunakan gambar Redis Docker resmi
-Layanan API berbicara dengan layanan Redis untuk menghindari mengunduh dan mem-parsing halaman yang sudah dikerik sebelumnya
-Variabel lingkungan REDIS_URL ditambahkan ke layanan API untuk memungkinkannya terhubung ke cache Redis.

Step 6: Swap Python API Service with Ruby
Beberapa perubahan signifikan dari langkah sebelumnya meliputi:

-Layanan API yang ditulis dengan Python diganti dengan implementasi Ruby yang serupa
-Variabel lingkungan API_ENDPOINT diperbarui untuk menunjuk ke layanan Ruby API baru
-Kejadian cache ekstraksi tautan (HIT / MISS) dicatat dan dipertahankan menggunakan volume


