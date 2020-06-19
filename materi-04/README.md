Docker Orchestration Hands-on Lab
Section 1: What is Orchestration
Jadi, apa itu orkestrasi? Yah, orkestrasi mungkin paling baik digambarkan menggunakan contoh. Katakanlah Anda memiliki aplikasi yang memiliki lalu lintas tinggi bersama dengan persyaratan ketersediaan tinggi. Karena persyaratan ini, Anda biasanya ingin menggunakan setidaknya 3+ mesin, sehingga jika tuan rumah gagal, aplikasi Anda masih dapat diakses dari setidaknya dua lainnya. Jelas, ini hanya sebuah contoh dan kasus penggunaan Anda kemungkinan akan memiliki persyaratan sendiri, tetapi Anda mendapatkan idenya.

Menyebarkan aplikasi Anda tanpa Orkestrasi biasanya sangat memakan waktu dan rawan kesalahan, karena Anda harus secara manual SSH ke setiap mesin, mulai aplikasi Anda, dan kemudian terus mengawasi hal-hal untuk memastikan itu berjalan seperti yang Anda harapkan.

Section 2: Configure Swarm Mode
Aplikasi dunia nyata biasanya digunakan di beberapa host seperti yang dibahas sebelumnya. Ini meningkatkan kinerja aplikasi dan ketersediaan, serta memungkinkan komponen aplikasi individu untuk skala secara mandiri. Docker memiliki alat asli yang tangguh untuk membantu Anda melakukan ini.

Step 2.1 - Create a Manager node
Step 2.2 - Join Worker nodes to the Swarm
Anda akan melakukan prosedur berikut pada node2 dan node3. Menjelang akhir prosedur, Anda akan beralih kembali ke node1.

Sekarang, ambil seluruh buruh pelabuhan bergabung ... perintah kami salin sebelumnya dari node1 di mana ia ditampilkan sebagai terminal output. Kita perlu menempelkan perintah yang disalin ke terminal node2 dan node3.

Section 3: Deploy applications across multiple hosts
Step 3.1 - Deploy the application components as Docker services
Aplikasi tidur kami menjadi sangat populer di internet (karena mengenai Reddit dan HN). Orang suka itu. Jadi, Anda harus mengukur aplikasi Anda untuk memenuhi permintaan puncak. Anda harus melakukan ini di beberapa host untuk ketersediaan tinggi juga. Kami akan menggunakan konsep Layanan untuk mengukur aplikasi kami dengan mudah dan mengelola banyak wadah sebagai satu kesatuan.

Section 4: Scale the application
Permintaannya gila! Semua orang menyukai aplikasi tidur Anda! Sudah waktunya untuk meningkatkan skala.

Salah satu hal hebat tentang layanan adalah Anda dapat menaikkan dan menurunkannya untuk memenuhi permintaan. Pada langkah ini Anda akan meningkatkan layanan dan kemudian kembali turun.

Section 5: Drain a node and reschedule the containers