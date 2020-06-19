Docker Networking Hands-on Lab

Section #1 - Networking Basics
Step 1: The Docker Network Command
Output perintah menunjukkan bagaimana menggunakan perintah dan juga semua sub-perintah jaringan buruh pelabuhan. Seperti yang Anda lihat dari output, perintah jaringan buruh pelabuhan memungkinkan Anda membuat jaringan baru, mendaftar jaringan yang ada, memeriksa jaringan, dan menghapus jaringan. Ini juga memungkinkan Anda untuk menghubungkan dan memutuskan wadah dari jaringan.
Step 2: List networks
Jalankan jaringan buruh pelabuhan ls perintah untuk melihat jaringan kontainer yang ada pada host Docker saat ini.
Output di atas menunjukkan jaringan kontainer yang dibuat sebagai bagian dari instalasi standar Docker.

Jaringan baru yang Anda buat juga akan muncul di output dari perintah docker network ls.

Step 3: Inspect a network
Perintah docker network inspect digunakan untuk melihat detail konfigurasi jaringan. Rincian ini meliputi; nama, ID, driver, driver IPAM, info subnet, wadah terhubung, dan banyak lagi.

Gunakan docker network inspect <network> untuk melihat detail konfigurasi jaringan wadah pada host Docker Anda. Perintah di bawah ini menunjukkan rincian jaringan yang disebut jembatan.

Step 4: List network driver plugins
Perintah docker info menunjukkan banyak informasi menarik tentang pemasangan Docker.
Jalankan perintah docker info dan temukan daftar plugin jaringan.

Section #2 - Bridge Networking
Step 1: The Basics
Output di atas menunjukkan bahwa jaringan bridge dikaitkan dengan driver bridge. Penting untuk dicatat bahwa jaringan dan driver terhubung, tetapi mereka tidak sama. Dalam contoh ini jaringan dan driver memiliki nama yang sama - tetapi mereka bukan hal yang sama!

Output di atas juga menunjukkan bahwa jaringan bridge dicakup secara lokal. Ini berarti bahwa jaringan hanya ada pada host Docker ini. Ini berlaku untuk semua jaringan yang menggunakan driver bridge - driver jembatan menyediakan jaringan host tunggal.

Step 2: Connect a container
Step 3: Test network connectivity
Step 4: Configure NAT for external connectivity

Section #3 - Overlay Networking
Step 1: The Basics
Step 2: Create an overlay network
Step 3: Create a service
Step 4: Test the network
Step 5: Test service discovery
Cleaning Up