Kubernetes for Beginners

Apa aplikasi ini?
Ini adalah penambang DockerCoin!

Tidak, Anda tidak dapat membeli kopi dengan DockerCoins

Cara kerja DockerCoins:

pekerja meminta rng untuk menghasilkan beberapa byte acak

pekerja memasukkan byte ini ke dalam hasher

dan ulangi selamanya!

setiap detik, pembaruan pekerja redis untuk menunjukkan berapa banyak loop dilakukan

webui meminta redis, dan menghitung dan mengekspos "kecepatan hashing" di browser Anda

Mendapatkan kode sumber aplikasi
Kami telah membuat contoh aplikasi untuk dijalankan di sebagian bengkel. Aplikasi ini ada dalam repositori dockercoins.

Mari kita lihat tata letak umum dari kode sumber:

ada file Tulis docker-compose.yml…

… Dan 4 layanan lainnya, masing-masing dalam direktori sendiri:

rng = layanan web menghasilkan byte acak

hasher = hash komputasi layanan web dari data POSTed

pekerja = proses latar belakang menggunakan rng dan hasher

webui = antarmuka web untuk melihat kemajuan

Kami akan mengkloning repositori GitHub

Repositori juga berisi skrip dan alat yang akan kami gunakan melalui workshop

Running the application
Buka direktori dockercoins, di repo hasil kloning:

cd ~ / dockercoins
Gunakan Tulisan untuk membuat dan menjalankan semua wadah:
docker-compose up

Banyak log
Aplikasi terus menerus menghasilkan log

Kita dapat melihat layanan pekerja membuat permintaan ke rng dan hasher

Mari kita letakkan itu di latar belakang

Menghubungkan ke UI web
Wadah webui memperlihatkan dasbor web; mari kita melihatnya

Dengan browser web, sambungkan ke node1 di sini pada port 8000 (dibuat saat Anda menjalankan aplikasi)

Clean up
Sebelum melanjutkan, mari matikan semuanya dengan mengetikkan Ctrl-C.

Kubernetes concepts
-Kubernetes adalah sistem manajemen wadah
-Ini berjalan dan mengelola aplikasi kemas pada sebuah cluster
-Apa maksudnya itu?

Hal-hal dasar yang dapat kita minta dari Kubernet
-Mulai 5 kontainer menggunakan image atseashop / api: v1.3
-Tempatkan penyeimbang beban internal di depan wadah ini
-Mulai 10 wadah menggunakan gambar atseashop / webfront: v1.3
-Tempatkan penyeimbang beban publik di depan wadah ini
-Ini Black Friday (atau Natal), lonjakan lalu lintas, tumbuhkan cluster kami dan tambahkan kontainer
-Rilis baru! Ganti wadah saya dengan gambar baru atseashop / webfront: v1.4
-Pertahankan permintaan pemrosesan selama peningkatan; perbarui wadah saya satu per satu

Hal-hal lain yang bisa dilakukan Kubernet untuk kita
-Autoscaling dasar
-Biru / hijau, penyebaran kenari
-Layanan berjalan lama, tetapi juga pekerjaan batch (sekali saja)
-Komitmen berlebihan pada kluster kami dan usir pekerjaan prioritas rendah
-Jalankan layanan dengan data stateful (database dll.)
-Kontrol akses berbutir halus menentukan apa yang dapat dilakukan oleh siapa pada sumber daya mana
-Mengintegrasikan layanan pihak ketiga (katalog layanan)
-Mengotomatiskan tugas-tugas kompleks (operator)

Kubernetes architecture
Kubernetes architecture: the master
Logika Kubernetes ("otak" -nya) adalah kumpulan layanan:
-server API (titik masuk kami ke semuanya!)
-layanan inti seperti scheduler dan manajer pengontrol
-etcd (toko kunci / nilai yang sangat tersedia; "database" Kubernetes)

Kubernetes architecture: the nodes
-Node yang mengeksekusi kontainer kami menjalankan koleksi layanan lain:
 ^Engine wadah (biasanya Docker)
 ^kubelet ("agen simpul")
 ^kube-proxy (komponen jaringan yang diperlukan tetapi tidak cukup)
-Node sebelumnya disebut "pelayan"
-Merupakan kebiasaan untuk tidak menjalankan aplikasi pada node yang menjalankan komponen master (Kecuali ketika menggunakan cluster pengembangan kecil)

Kubernetes resources
-API Kubernetes mendefinisikan banyak objek yang disebut sumber daya
-Sumber daya ini disusun berdasarkan jenis, atau Jenis (dalam API)
-Beberapa jenis sumber daya umum adalah:
 ^simpul (mesin - fisik atau virtual - di cluster kami)
 ^pod (sekelompok kontainer yang berjalan bersama pada sebuah node)
 ^layanan (titik akhir jaringan yang stabil untuk terhubung ke satu atau beberapa kontainer)
 ^namespace (kelompok hal yang kurang lebih terisolasi)
 ^rahasia (bundel data sensitif untuk diteruskan ke wadah)
-Dan banyak lagi! (Kita dapat melihat daftar lengkap dengan menjalankan get kubectl)

Declarative vs imperative
-Orkestra kontainer kami sangat menekankan deklaratif
-Deklaratif:
 Saya mau secangkir teh.
-Imperatif:
 Rebus air. Tuangkan dalam teko. Tambahkan daun teh. Curam sebentar. Sajikan dalam cangkir.
-Deklaratif tampaknya lebih sederhana pada awalnya ...
-... Selama kamu tahu cara menyeduh teh

Ringkasan deklaratif vs imperatif
Sistem imperatif:

lebih sederhana

jika suatu tugas terganggu, kita harus memulai kembali dari awal

Sistem deklaratif:

jika tugas terganggu (atau jika kami muncul di pesta setengah jalan), kami dapat mencari tahu apa yang hilang dan hanya melakukan apa yang perlu

kita harus bisa mengamati sistem

... dan menghitung "perbedaan" antara apa yang kita miliki dan apa yang kita inginkan

Deklaratif vs imperatif di Kubernetes
Hampir semua yang kita buat di Kubernetes dibuat dari spec

Tonton bidang spesifikasi di file YAML nanti!

Spesifikasi tersebut menjelaskan bagaimana kita menginginkannya

Kubernetes akan merekonsiliasi kondisi saat ini dengan spek (secara teknis, ini dilakukan oleh sejumlah pengendali)

Ketika kami ingin mengubah beberapa sumber daya, kami memperbarui spesifikasi

Kubernetes kemudian akan menyatukan sumber daya itu

Model jaringan Kubernetes
TL, DR:

Cluster kami (node ​​dan pod) adalah satu jaringan IP flat besar.

Secara terperinci:

semua node harus dapat mencapai satu sama lain, tanpa NAT

semua pod harus dapat saling menjangkau, tanpa NAT

pod dan node harus dapat mencapai satu sama lain, tanpa NAT

setiap pod mengetahui alamat IP-nya (tidak ada NAT)

Kubernetes tidak mengamanatkan implementasi khusus apa pun

Model jaringan Kubernetes: yang baik
Semuanya bisa mencapai segalanya

Tidak ada terjemahan alamat

Tidak ada terjemahan port

Tidak ada protokol baru

Pod tidak dapat berpindah dari satu node ke node lain dan menjaga alamat IP-nya

Alamat IP tidak harus “portabel” dari satu simpul ke simpul lainnya (Kita dapat menggunakan misalnya subnet per simpul dan menggunakan topologi yang dialihkan sederhana)

Spesifikasi ini cukup sederhana untuk memungkinkan berbagai implementasi

Model jaringan Kubernetes: semakin kurang bagus
Semuanya bisa mencapai segalanya

jika Anda menginginkan keamanan, Anda perlu menambahkan kebijakan jaringan

implementasi jaringan yang Anda gunakan perlu mendukung mereka

Ada lusinan implementasi di luar sana (15 tercantum dalam dokumentasi Kubernetes)

Sepertinya Anda memiliki jaringan level 3, tetapi ini hanya level 4 (Spesifikasi ini membutuhkan UDP dan TCP, tetapi tidak rentang port atau paket IP sewenang-wenang)

kube-proxy berada di jalur data saat menyambungkan ke pod atau wadah, dan itu tidak terlalu cepat (bergantung pada proxy atau iptables userland)

Model jaringan Kubernetes: dalam praktiknya
Node yang kami gunakan telah diatur untuk menggunakan Weave

Kami tidak menganjurkan Weave dengan cara tertentu, itu hanya Berfungsi Untuk Kami

Jangan khawatir tentang peringatan tentang kinerja proksi kubus

Kecuali kamu:

secara rutin menjenuhkan antarmuka jaringan 10G

hitung tarif paket dalam jutaan per detik

menjalankan VOIP lalu lintas tinggi atau platform game

lakukan hal-hal aneh yang melibatkan jutaan koneksi simultan (dalam hal ini Anda sudah terbiasa dengan penyetelan kernel)

Kontak pertama dengan kubectl
kubectl adalah (hampir) satu-satunya alat yang kita perlukan untuk berbicara dengan Kubernetes

Ini adalah alat CLI yang kaya di sekitar API Kubernetes (Semua yang dapat Anda lakukan dengan kubectl, Anda dapat melakukannya langsung dengan API)

Anda juga dapat menggunakan flag --kubeconfig untuk meneruskan file konfigurasi

Atau langsung --server, --user, dll.

kubectl dapat diucapkan "Cube C T L", "Cube cuttle", "Cube cuddle" ...

-kubectl get
-Obtaining machine-readable output
-(Ab) menggunakan kubectl dan jq

Apa yang tersedia?
kubectl memiliki fasilitas introspeksi yang cukup bagus

Kami dapat mendaftar semua jenis sumber daya yang tersedia dengan menjalankan get kubectl
Kami dapat melihat detail tentang sumber daya dengan:
kubectl describe type/name
kubectl describe type name
Kami dapat melihat definisi untuk jenis sumber daya dengan:
kubectl jelaskan tipe

Jasa
Suatu layanan adalah titik akhir yang stabil untuk terhubung ke "sesuatu" (Dalam proposal awal, mereka disebut "portal")

Daftarkan layanan di kluster kami:
kubectl get services

Layanan ClusterIP
Layanan ClusterIP bersifat internal, hanya tersedia dari cluster

Ini berguna untuk introspeksi dari dalam wadah

Coba sambungkan ke API.

-k digunakan untuk melewati verifikasi sertifikat
Pastikan untuk mengganti 10.96.0.1 dengan CLUSTER-IP yang ditunjukkan oleh $ kubectl get svc

Mendaftar kontainer yang berjalan
Kontainer dimanipulasi melalui pod

Pod adalah sekelompok wadah:

berjalan bersama (pada node yang sama)

berbagi sumber daya (RAM, CPU; tetapi juga jaringan, volume)

Daftarkan pod di kluster kami:

kubectl mendapatkan polong
Ini bukan pod yang Anda cari. Tapi di mana mereka?!?

Ruang nama
Ruang nama memungkinkan kami untuk memisahkan sumber daya

Daftar ruang nama pada kluster kami dengan salah satu dari perintah ini:

kubectl mendapatkan namespace
salah satu dari ini akan bekerja juga:

kubectl mendapatkan namespace
kubectl dapatkan ns
Anda tahu apa ... Sistem sistem kubus ini terlihat mencurigakan.

Mengakses ruang nama
Secara default, kubectl menggunakan namespace default

Kita dapat beralih ke namespace yang berbeda dengan opsi -n

Daftar pod di namespace sistem kubus:

kubectl -n kube-system mendapatkan pod
Ding ding ding ding ding!

Apa semua polong ini?
etcd adalah server etcd kami

kube-apiserver adalah server API

kube-controller-manager dan kube-scheduler adalah komponen master lainnya

kube-dns adalah komponen tambahan (tidak wajib tetapi sangat berguna, jadi ada di sana)

kube-proxy adalah komponen (per-simpul) yang mengelola pemetaan port dan semacamnya

weave adalah komponen (per-simpul) yang mengelola overlay jaringan
kolom READY menunjukkan jumlah kontainer di setiap pod

pod dengan nama yang diakhiri dengan -node1 adalah komponen master (mereka secara khusus "disematkan" ke node master)

Menjalankan kontainer pertama kami di Kubernetes
Hal pertama yang pertama: kita tidak bisa menjalankan wadah

Kami akan menjalankan pod, dan di pod itu akan ada satu wadah

Dalam wadah itu di pod, kita akan menjalankan perintah ping sederhana

Kemudian kita akan memulai salinan pod tambahan

Memulai pod sederhana dengan menjalankan kubectl
Kita harus menentukan setidaknya nama dan gambar yang ingin kita gunakan

Mari kita ping 8.8.8.8, DNS publik Google

kubectl run pingpong --image alpine ping 8.8.8.8
Oke, apa yang baru saja terjadi?

Di belakang layar lari kubectl
Mari kita lihat sumber daya yang dibuat oleh kubectl run

Daftarkan sebagian besar jenis sumber daya:

kubectl dapatkan semuanya
Kita harus melihat hal-hal berikut:

deploy / pingpong (penyebaran yang baru saja kita buat)
rs / pingpong-xxxx (set replika yang dibuat oleh penyebaran)
po / pingpong-yyyy (pod dibuat oleh set replika)
Apa saja hal-hal yang berbeda ini?
Penempatan adalah konstruksi tingkat tinggi

memungkinkan penskalaan, pembaruan bergulir, rollback

beberapa penyebaran dapat digunakan bersama untuk mengimplementasikan penyebaran kenari

mendelegasikan manajemen pod untuk set replika

Set replika adalah konstruksi tingkat rendah

memastikan bahwa sejumlah pod yang sama dijalankan

memungkinkan penskalaan

jarang digunakan secara langsung

Kontroler replikasi adalah pendahulu (usang) dari set replika

Penyebaran pingpong kami
kubectl run membuat penyebaran, deploy / pingpong

Penempatan itu menciptakan set replika, rs / pingpong-xxxx

Set replika itu membuat pod, po / pingpong-yyyy

Kita akan lihat nanti bagaimana orang-orang ini bermain bersama untuk:

scaling

ketersediaan tinggi

bergulir pembaruan

Melihat hasil wadah
Mari kita gunakan perintah log kubectl

Kami akan melewati salah satu nama pod, atau tipe / nama (E.g. jika kami menentukan set penyebaran atau replika, ia akan mendapatkan pod pertama di dalamnya)

Kecuali ditentukan lain, itu hanya akan menampilkan log dari wadah pertama di pod (Untung hanya ada satu di kita!)

Lihat hasil dari perintah ping kami:

log kubectl deploy / pingpong
Log streaming secara real time
Sama seperti log buruh pelabuhan, log kubectl mendukung opsi yang nyaman:

-f / - ikuti untuk melakukan streaming log secara real time (à la tail -f)

--tail untuk menunjukkan berapa banyak garis yang ingin Anda lihat (dari akhir)

--Karena mendapatkan log hanya setelah cap waktu yang diberikan

Lihat log terbaru dari perintah ping kami:

log kubectl deploy / pingpong --tail 1 --follow

Penskalaan aplikasi kita
Kami dapat membuat salinan tambahan dari wadah kami (atau lebih tepatnya pod kami) dengan skala kubectl

Skala penyebaran pingpong kami:

menyebarkan skala kubectl / pingpong --replicas 8
Catatan: bagaimana jika kita mencoba skala rs / pingpong-xxxx? Kita bisa! Tetapi penyebaran akan segera menyadarinya, dan mengurangi ke tingkat awal.

Ketangguhan
Pingpong deployment menyaksikan set replika-nya

Set replika memastikan jumlah pod yang tepat berjalan

Apa yang terjadi jika pod hilang?

Di jendela terpisah, daftarkan pod, dan terus tonton:
kubectl dapatkan pod -w
Ctrl-C untuk menghentikan menonton.

Jika Anda ingin menghancurkan pod, Anda akan menggunakan pola ini di mana yyyy adalah pengidentifikasi pod tertentu:
kubectl hapus pod pingpong-yyyy
Bagaimana jika kita menginginkan sesuatu yang berbeda?
Bagaimana jika kita ingin memulai wadah "satu tembakan" yang tidak bisa dimulai kembali?

Kita bisa menggunakan kubectl run --restart = OnFailure atau kubectl run --restart = Never

Perintah-perintah ini akan menciptakan pekerjaan atau pod bukannya penyebaran

Di bawah tenda, kubectl run memanggil "generator" untuk membuat deskripsi sumber daya

Kita juga bisa menulis sendiri deskripsi sumber daya ini (biasanya dalam YAML), dan membuatnya di cluster dengan kubectl apply -f (dibahas nanti)

Dengan kubectl run --schedule = ..., kita juga dapat membuat cronjobs

Melihat log beberapa pod
Saat kami menentukan nama penggunaan, hanya satu log pod tunggal yang ditampilkan

Kami dapat melihat log beberapa pod dengan menentukan pemilih

Selector adalah ekspresi logika menggunakan label

Mudahnya, ketika Anda kubectl menjalankan nama samaran, objek terkait memiliki label run = nama samaran

Lihat baris log terakhir dari semua pod dengan label run = pingpong:

kubectl logs -l run = pingpong --tail 1
Sayangnya, --follow tidak dapat (belum) digunakan untuk melakukan streaming log dari banyak wadah.

Membersihkan
Bersihkan penyebaran Anda dengan menghapus pingpong

kubectl delete deploy / pingpong

Wadah terbuka
kubectl expose menciptakan layanan untuk pod yang ada

Suatu layanan adalah alamat yang stabil untuk sebuah pod (atau sekelompok pod)

Jika kami ingin terhubung ke pod kami, kami perlu membuat layanan

Setelah layanan dibuat, kube-dns akan memungkinkan kami untuk menyelesaikannya dengan nama (mis. Setelah membuat layanan halo, nama halo akan menyelesaikan sesuatu)

Ada berbagai jenis layanan, dirinci pada slide berikut:

ClusterIP, NodePort, LoadBalancer, ExternalName

Jenis layanan dasar
ClusterIP (tipe default)

alamat IP virtual dialokasikan untuk layanan (dalam rentang privat, internal) alamat IP ini hanya dapat dijangkau dari dalam cluster (node ​​dan pod) kode kami dapat terhubung ke layanan menggunakan nomor port asli

NodePort

port dialokasikan untuk layanan (secara default, dalam kisaran 30000-32768) bahwa port tersedia pada semua node kami dan siapa pun dapat terhubung dengannya kode kami harus diubah untuk menyambung ke nomor port baru tersebut

Jenis layanan ini selalu tersedia.

Di bawah tenda: kube-proxy menggunakan proxy userland dan banyak aturan iptables.

Jenis layanan lainnya
LoadBalancer
penyeimbang beban eksternal dialokasikan untuk layanan ini
penyeimbang beban dikonfigurasikan sesuai (mis .: .: layanan NodePort dibuat, dan penyeimbang beban mengirimkan lalu lintas ke port itu)
Nama Eksternal

entri DNS yang dikelola oleh kube-dns hanya akan menjadi CNAME untuk catatan yang disediakan
tidak ada port, tidak ada alamat IP, tidak ada yang dialokasikan

Menjalankan kontainer dengan port terbuka
Karena ping tidak memiliki apa pun untuk dihubungkan, kami harus menjalankan sesuatu yang lain

Mulai banyak wadah ElasticSearch:
kubectl run elastic --image = elasticsearch: 2 --replicas = 4
Lihat mereka dimulai:

kubectl dapatkan pod -w
Opsi -w "menonton" peristiwa yang terjadi pada sumber daya yang ditentukan.

Catatan: mohon JANGAN panggil pencarian layanan. Itu akan bertabrakan dengan TLD.

Mengungkap penyebaran kami
Kami akan membuat layanan ClusterIP default

Buka port ElasticSearch HTTP API:

kubectl expose deploy / elastic --port 9200
Cari alamat IP yang dialokasikan:

kubectl dapatkan svc
Layanan adalah konstruksi lapisan 4
Anda dapat menetapkan alamat IP untuk layanan, tetapi mereka masih lapisan 4 (yaitu layanan bukan alamat IP; itu adalah alamat IP + protokol + port)

Ini disebabkan oleh implementasi kube-proxy saat ini (bergantung pada mekanisme yang tidak mendukung layer 3)

Akibatnya: Anda harus menunjukkan nomor port untuk layanan Anda

Menjalankan layanan dengan port sewenang-wenang (atau rentang port) memerlukan peretasan (mis. Mode jaringan host)

Menguji layanan kami
Kami sekarang akan mengirim beberapa permintaan HTTP ke pod ElasticSearch kami

Mari kita dapatkan alamat IP yang dialokasikan untuk layanan kami, secara terprogram:

IP = $ (kubectl dapatkan svc elastic -o-templat --template '{{.spec.clusterIP}}')
Kirim beberapa permintaan:

ikal http: // $ IP: 9200 /
Permintaan kami dimuat seimbang di beberapa pod.

Membersihkan
Kami sudah selesai dengan penyebaran elastis, jadi mari kita bersihkan

hapus kubectl deploy / elastic
Aplikasi kami di Kube
Apa yang ada di menu?
Pada bagian ini, kita akan:

membangun gambar untuk aplikasi kita,

kirimkan gambar-gambar ini dengan registri,

jalankan penyebaran menggunakan gambar-gambar ini,

mengekspos penyebaran ini sehingga mereka dapat berkomunikasi satu sama lain,

mengekspos UI web sehingga kami dapat mengaksesnya dari luar.

Rencana
Bangun di simpul kontrol kami (node1)

Beri tag pada gambar sehingga diberi nama $ USERNAME / servicename

Unggah mereka ke Hub Docker

Buat penyebaran menggunakan gambar

Ekspos (dengan ClusterIP) layanan yang perlu berkomunikasi

Paparkan (dengan NodePort) WebUI

Mempersiapkan
Di terminal pertama, setel variabel lingkungan untuk nama pengguna Docker Hub Anda. Ini bisa menjadi nama pengguna Docker Hub yang sama dengan yang Anda gunakan untuk masuk ke terminal di situs ini.

export USERNAME = YourUserName
Pastikan Anda masih berada di direktori dockercoins.
pwd

Catatan tentang pendaftar
Untuk bengkel ini, kami akan menggunakan Docker Hub. Ada sejumlah opsi lain, termasuk dua yang disediakan oleh Docker.

Docker juga menyediakan:

Docker Trusted Registry yang menambahkan banyak fitur keamanan dan penyebaran termasuk pemindaian keamanan, dan kontrol akses berbasis peran.
Open Source Registry Docker.
Hub Docker
Docker Hub adalah registri default untuk Docker.

Nama gambar di Hub hanya $ USERNAME / $ IMAGENAME atau $ ORGANISATIONNAME / $ IMAGENAME.

Gambar resmi dapat disebut sebagai hanya $ IMAGENAME.

Untuk menggunakan Hub, pastikan Anda memiliki akun. Kemudian ketik login buruh pelabuhan di terminal dan login dengan nama pengguna dan kata sandi Anda.

Menggunakan Docker Trusted Registry, Docker Open Source Registry sangat mirip.

Nama gambar pada pendaftar lain adalah $ REGISTRYPATH / $ USERNAME / $ IMAGENAME atau $ REGISTRYPATH / $ ORGANISATIONNAME / $ IMAGENAME.

Login menggunakan docker login $ REGISTRYPATH.

Langkah selanjutnya
Baiklah, bagaimana saya memulai dan membuat kontainer aplikasi saya?

Daftar centang kontainerisasi yang disarankan:

menulis Dockerfile untuk satu layanan dalam satu aplikasi
tulis Dockerfiles untuk layanan (buildable) lainnya
menulis file Menulis untuk seluruh aplikasi itu
pastikan devs diberdayakan untuk menjalankan aplikasi dalam wadah
mengatur pembuatan otomatis gambar wadah dari repo kode
mengatur pipa CI menggunakan gambar wadah ini
mengatur jalur pipa CD (untuk staging / QA) menggunakan gambar-gambar ini
Dan sekarang saatnya untuk melihat orkestrasi!

Ruang nama
Ruang nama memungkinkan Anda menjalankan banyak tumpukan yang identik berdampingan

Dua ruang nama (mis. Biru dan hijau) masing-masing dapat memiliki layanan redis sendiri

Masing-masing dari dua layanan redis memiliki ClusterIP sendiri

kube-dns membuat dua entri, memetakan ke dua alamat ClusterIP ini:

redis.blue.svc.cluster.local dan redis.green.svc.cluster.local

Pod di namespace biru mendapatkan akhiran pencarian blue.svc.cluster.local

Akibatnya, menyelesaikan redis dari pod di namespace biru menghasilkan redis "lokal"

Ini tidak menyediakan isolasi! Itu akan menjadi tugas kebijakan jaringan.

Layanan stateful (basis data dll.)
Sebagai langkah pertama, lebih bijaksana untuk menjaga layanan stateful di luar cluster

Mengekspos mereka ke polong dapat dilakukan dengan beberapa solusi:

Layanan ExternalName (redis.blue.svc.cluster.local akan menjadi catatan CNAME)

Layanan ClusterIP dengan Titik Akhir eksplisit (alih-alih membiarkan Kubernet menghasilkan titik akhir dari pemilih)

Layanan duta besar (proksi tingkat aplikasi yang dapat memberikan suntikan kredensial dan lainnya)

Layanan stateful (pengambilan kedua)
Jika Anda benar-benar ingin meng-host layanan stateful di Kubernetes, Anda dapat melihat:

volume (untuk membawa data persisten)
plugin penyimpanan
klaim volume persisten (untuk meminta karakteristik volume spesifik)
set stateful (polong yang tidak fana)

Penanganan lalu lintas HTTP
Layanan adalah konstruksi lapisan 4

HTTP adalah protokol lapisan 7

Ini ditangani oleh ingresses (jenis sumber daya yang berbeda)

Ingress memungkinkan:

perutean host virtual
lengket sesi
Pemetaan URI
dan banyak lagi!
Logging dan metrik
Penebangan didelegasikan ke mesin kontainer

Metrik biasanya ditangani dengan Prometheus

Mengelola konfigurasi aplikasi kita
Dua konstruksi sangat berguna: rahasia dan peta konfigurasi

Mereka memungkinkan untuk mengekspos informasi sewenang-wenang ke wadah kami

Hindari menyimpan konfigurasi dalam gambar wadah (Ada beberapa pengecualian untuk aturan itu, tetapi itu umumnya Ide Buruk)

Jangan pernah menyimpan informasi sensitif dalam gambar wadah (Ini adalah wadah yang setara dengan kata sandi pada catatan tempel di layar Anda)

Federasi cluster
Operasi induk Kubernet bergantung pada etcd

etcd menggunakan protokol Raft

Raft merekomendasikan latensi rendah antar node

Bagaimana jika cluster kami menyebar ke beberapa wilayah?

Hancurkan dalam kelompok lokal

Kelompokkan mereka dalam federasi klaster

Sinkronisasi sumber daya di seluruh cluster

Temukan sumber daya di seluruh cluster