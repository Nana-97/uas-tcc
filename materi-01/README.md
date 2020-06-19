Docker for Beginners - Linux

Tugas 0: Prasyarat
Clone the GitHub Repo Lab
Gunakan perintah berikut untuk mengkloning repo lab dari GitHub (Anda dapat mengklik perintah atau mengetiknya secara manual). Ini akan membuat salinan repo laboratorium di sub-direktori baru yang disebut linux_tweet_app.

Tugas 1: Jalankan beberapa Docker container sederhana
 -Untuk menjalankan satu tugas: Ini bisa berupa skrip shell atau aplikasi khusus.
 -Interaktif: Ini menghubungkan Anda ke container yang mirip dengan cara Anda SSH ke server jauh.
 -Di latar belakang: Untuk layanan jangka panjang seperti situs web dan basis data.
Jalankan satu tugas dalam wadah Alpine Linux
Pada langkah ini kami akan memulai sebuah container baru dan memintanya untuk menjalankan perintah nama host. Kontainer akan mulai, jalankan perintah hostname, lalu keluar.
1. Jalankan perintah berikut di konsol Linux Anda.
Output ini menunjukkan bahwa alpine: gambar terbaru tidak dapat ditemukan secara lokal. Ketika ini terjadi, Docker secara otomatis menariknya dari Docker Hub.
2. Docker menjaga wadah berjalan selama proses itu dimulai di dalam wadah masih berjalan. Dalam hal ini proses nama host keluar segera setelah output ditulis. Ini artinya wadah berhenti. Namun, Docker tidak menghapus sumber daya secara default, sehingga wadah masih ada dalam status Keluar.
Jalankan wadah Ubuntu interaktif
1. Jalankan wadah Docker dan akses cangkangnya.
Dua parameter pertama memungkinkan Anda untuk berinteraksi dengan wadah Docker.

Kami juga memberi tahu kontainer untuk menjalankan bash sebagai proses utamanya (PID 1).

Ketika wadah mulai Anda akan jatuh ke shell pesta dengan root prompt default @ <id wadah>: / #. Docker telah menempel pada shell di wadah, menyampaikan input dan output antara sesi lokal Anda dan sesi shell di wadah.
2. Jalankan perintah berikut dalam wadah.
ls / akan mencantumkan isi dari root director di dalam container, ps aux akan menunjukkan proses yang sedang berjalan di dalam container, cat / etc / issue akan menunjukkan distro Linux yang dijalankan oleh container
3. Ketik exit untuk keluar dari sesi shell. Ini akan menghentikan proses bash, menyebabkan wadah untuk keluar.
4. Untuk bersenang-senang, mari kita periksa versi VM host kami.
Perhatikan bahwa host VM kami menjalankan Alpine Linux, namun kami dapat menjalankan wadah Ubuntu. Seperti disebutkan sebelumnya, distribusi Linux di dalam wadah tidak perlu cocok dengan distribusi Linux yang berjalan di host Docker.

Namun, wadah Linux mengharuskan host Docker menjalankan kernel Linux. Misalnya, wadah Linux tidak dapat berjalan langsung di host Windows Docker. Hal yang sama berlaku untuk kontainer Windows - mereka harus dijalankan pada host Docker dengan kernel Windows.
Jalankan latar belakang wadah MySQL
1. Jalankan wadah MySQL baru dengan perintah berikut.
--detach akan menjalankan kontainer di latar belakang.
--name akan menamainya mydb.
-e akan menggunakan variabel lingkungan untuk menentukan kata sandi root (CATATAN: Ini seharusnya tidak pernah dilakukan dalam produksi).
Selama proses MySQL berjalan, Docker akan menjaga kontainer tetap berjalan di latar belakang.
2. Daftar wadah yang sedang berjalan.
3. Anda dapat memeriksa apa yang terjadi di wadah Anda dengan menggunakan beberapa perintah Docker bawaan: docker container logs dan docker container top.
4. Daftar versi MySQL menggunakan docker container exec.
5. Anda juga dapat menggunakan docker container exec untuk terhubung ke proses shell baru di dalam wadah yang sudah berjalan. Menjalankan perintah di bawah ini akan memberi Anda shell interaktif (sh) di dalam wadah MySQL Anda.
6. Mari kita periksa nomor versi dengan menjalankan perintah yang sama lagi, hanya kali ini dari dalam sesi shell baru di wadah.
7. Ketik exit untuk keluar dari sesi shell interaktif.

Tugas 2: Paket dan menjalankan aplikasi khusus menggunakan Docker
Pada langkah ini Anda akan belajar cara mengemas aplikasi Anda sendiri sebagai gambar Docker menggunakan Dockerfile.
Sintaks Dockerfile sangat mudah. Dalam tugas ini, kami akan membuat situs web NGINX sederhana dari Dockerfile.

Bangun gambar situs web sederhana
1. Pastikan Anda berada di direktori linux_tweet_app.
2. Tampilkan konten Dockerfile.
  -FROM menentukan gambar dasar yang akan digunakan sebagai titik awal untuk gambar baru yang Anda buat ini. Untuk contoh ini, kami mulai dari nginx: terbaru.
  -SALIN menyalin file dari host Docker ke dalam gambar, di lokasi yang diketahui. Dalam contoh ini, COPY digunakan untuk menyalin dua file ke dalam gambar: index.html. dan grafik yang akan digunakan di halaman web kami.
  -TAMPILKAN dokumen yang port aplikasi gunakan.
  -CMD menentukan perintah apa yang harus dijalankan ketika sebuah wadah dimulai dari gambar. Perhatikan bahwa kita dapat menentukan perintah, serta argumen run-time.
3. Untuk membuat perintah berikut ini lebih ramah salin / tempel, ekspor variabel lingkungan yang berisi DockerID Anda (jika Anda tidak memiliki DockerID, Anda bisa mendapatkannya secara gratis melalui Docker Hub).

Anda harus mengetikkan perintah ini secara manual karena membutuhkan DockerID unik Anda.
4. Gema nilai variabel kembali ke terminal untuk memastikan itu disimpan dengan benar.
5. Gunakan perintah build image docker untuk membuat image Docker baru  menggunakan instruksi di Dockerfile.
  --tag memungkinkan kita memberi gambar nama khusus. Dalam hal ini terdiri dari DockerID kami, nama aplikasi, dan versi. Memiliki ID Docker yang terlampir pada nama akan memungkinkan kami untuk menyimpannya di Docker Hub di langkah selanjutnya
  . memberitahu Docker untuk menggunakan direktori saat ini sebagai konteks pembangunan
Pastikan untuk memasukkan titik (.) Di akhir perintah.

6. Gunakan perintah menjalankan wadah buruh pelabuhan untuk memulai wadah baru dari gambar yang Anda buat.

Karena kontainer ini akan menjalankan server web NGINX, kami akan menggunakan flag --publish untuk menerbitkan port 80 di dalam container ke port 80 pada host. Ini akan memungkinkan lalu lintas masuk ke host Docker pada port 80 untuk diarahkan ke port 80 dalam wadah. Format flag --publish adalah host_port: container_port.

7. Klik di sini untuk memuat situs web yang seharusnya berjalan.
8. Setelah Anda mengakses situs web Anda, matikan dan hapus.

Tugas 3: Memodifikasi situs web yang sedang berjalan
Mulai aplikasi web kami dengan bind mount
1. Mari mulai aplikasi web dan pasang direktori saat ini ke dalam wadah.
2. Situs web harus berjalan.

Ubah situs web yang berjalan
1. Salin index.html baru ke dalam wadah.
2. Buka situs web yang sedang berjalan dan segarkan halaman. Perhatikan bahwa situs tersebut telah berubah.

Untuk menunjukkan ini, hentikan wadah saat ini dan jalankan kembali gambar 1.0 tanpa ikatan bind.
1. Hentikan dan hapus wadah yang sedang berjalan.
2. Jalankan kembali versi saat ini tanpa bind mount.
3. Perhatikan situs web kembali ke versi aslinya.
4. Berhenti dan lepaskan wadah saat ini.

Update the image
1. Buat gambar baru dan beri tag sebagai 2.0
2. Mari kita lihat gambar pada sistem.

Test the new version
1. Jalankan wadah baru dari versi gambar yang baru.
2. Periksa versi baru situs web (Anda mungkin perlu menyegarkan peramban untuk mendapatkan versi baru untuk dimuat).
3. Jalankan wadah baru lainnya, kali ini dari versi lama gambar.
4. Lihat versi lama situs web.

Push your images to Docker Hub
1. Daftar gambar pada host Docker Anda.
2. Sebelum Anda dapat mendorong gambar Anda, Anda harus masuk ke Docker Hub.
3. Push versi 1.0 aplikasi web Anda menggunakan docker image push.
4. Sekarang push versi 2.0.
