# 1. Docker 

Pertama saya membuat vm menggunakan idcloudhost

(gambar idcloudhost)

Lalu saya mengupdate package dan membuat file Dockerfile untuk di build dan di run, beserta default.conf yang akan dimasukkan kedalam docker container untuk web server nginx

Karna Docker belum terinstall di vm ini, saya sudah menyiapkan script ansible untuk menginstall docker dari lokal ke vm target di idcloudhost

(gambar install docker)
(gambar docker version)

Lalu saya membuild docker dengan perintah dibawah, ada sekali kesalahan karna saya menaruh file default.conf yang berbeda. Walaupun saya tidak tahu default.conf sudah ada atau belum, saya pastikan ada dengan mengcopy script default.conf yang saya buat agar dimasukkan kedalam docker container yang akan saya build dan saya run.
```
sudo docker build -t nginx-reverse-proxy .
```
(gambar build docker)

Lalu saya run dockernya. 
(run docker)

dan ini hasilnya, dia mendirect ke 2 website

(gambar web 1)
(gambar web 2)


# 3.1 Jenkins

Didalam jenkins, ada beberapa tahap (stages) seperti Checkout, Build, Test, dan Deploy. 

## 3.1.1 Persiapan
* Pastikan Jenkins terinstal di sistem Anda.
* Install plugin Jenkins yang diperlukan seperti Git, Pipeline, dan Docker (jika menggunakan Docker untuk deploy).
* Pastikan proyek (Java atau Go) sudah tersedia di repository Github.

Jenkins file ada di file berikut (file Jenkinsfile)


A. environment:

Mendeklarasikan variabel-variabel lingkungan seperti URL repository, branch yang digunakan, dan direktori build (BUILD_DIR).

B. Stage Checkout:

Menarik kode sumber dari repository Git yang ditentukan pada REPO_URL dan branch yang ditentukan (BRANCH).

C. Stage Build:

Mengidentifikasi apakah proyek menggunakan Java atau Go berdasarkan ada tidaknya file pom.xml (untuk Java) atau go.mod (untuk Go).
Jika proyek adalah Java, pipeline akan menjalankan perintah mvn clean package untuk melakukan build menggunakan Maven.
Jika proyek adalah Go, pipeline akan menjalankan perintah go build untuk membangun aplikasi Go.

D. Stage Test:

Setelah build selesai, pipeline akan menjalankan pengujian. Untuk proyek Java, menggunakan Maven (mvn test), dan untuk Go menggunakan go test.

E. Stage Deploy:

Jika aplikasi Java, Jenkins akan membangun Docker image dengan perintah docker build dan menjalankan aplikasi menggunakan Docker (docker run).
Jika aplikasi Go, langkah yang sama akan dilakukan untuk aplikasi Go.

F. post:

Bagian post memastikan bahwa workspace Jenkins dibersihkan setelah pipeline selesai berjalan, terlepas dari apakah sukses atau gagal.



### Mengaktifkan Pipeline di Jenkins
A. Buat Job Baru:

Masuk ke dashboard Jenkins.
Pilih New Item dan pilih tipe Pipeline.
Beri nama job, misalnya Java-Go-Deploy.
Pilih Pipeline script from SCM untuk menggunakan Jenkinsfile yang ada di repository Git.
Konfigurasikan URL repository Git dan branch yang sesuai.


B. Trigger Pipeline:

Pipeline akan berjalan otomatis ketika Anda melakukan commit atau push ke repository yang terhubung dengan Jenkins.


# 3.3 Upgrade OS

Berikut untuk proses upgrade OS menggunakan ansible playbook dan scriptnya

(gambar ansible upgrade os)
(script upgradeOS.yml)
