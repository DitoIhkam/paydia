![alt text](?raw=true)
# 1. Docker 

Pertama saya membuat vm menggunakan idcloudhost

![alt text](https://github.com/DitoIhkam/paydia/blob/main/img/1.%20IDCLOUDHOST.png?raw=true)


Lalu saya mengupdate package dan membuat file Dockerfile untuk di build dan di run, beserta default.conf yang akan dimasukkan kedalam docker container untuk web server nginx
![alt text](https://github.com/DitoIhkam/paydia/blob/main/img/2.%20APT%20UPDATE%2C%20buat%20Dockerfile%2C%20dan%20reverseproxy.png?raw=true)
![alt text](https://github.com/DitoIhkam/paydia/blob/main/img/2.%20folder2%20nya.png?raw=true)

Karna Docker belum terinstall di vm ini, saya sudah menyiapkan script ansible untuk menginstall docker dari lokal ke vm target di idcloudhost


(gambar install docker)
![alt text](https://github.com/DitoIhkam/paydia/blob/main/img/3.%20Install%20Docker.png?raw=true)
(gambar docker version)
![alt text](https://github.com/DitoIhkam/paydia/blob/main/img/3.%20Dockerversion.png?raw=true)

Lalu saya membuild docker dengan perintah dibawah, ada sekali kesalahan karna saya menaruh file default.conf yang berbeda. Walaupun saya tidak tahu default.conf sudah ada atau belum, saya pastikan ada dengan mengcopy script default.conf yang saya buat agar dimasukkan kedalam docker container yang akan saya build dan saya run.
```
sudo docker build -t nginx-reverse-proxy .
```

![alt text](https://github.com/DitoIhkam/paydia/blob/main/img/4.%20Build%20Docker.png?raw=true)

Lalu saya run dockernya. 
![alt text](https://github.com/DitoIhkam/paydia/blob/main/img/5.%20Lalu%20saya%20run%20Docker%20nya.png?raw=true)

dan ini hasilnya, dia mendirect ke 2 website

![alt text](https://github.com/DitoIhkam/paydia/blob/main/img/6.%20gambar%20hasil%20(1).png?raw=true)
(gambar web 1)
![alt text](https://github.com/DitoIhkam/paydia/blob/main/img/6.%20gambar%20hasil%20(2).png?raw=true)
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


# 3.2 Skrip Bash

1. Generate Data first_name, last_name, pekerjaan, join_date, deskripsi. Berikut adalah hasil data nya
(gambar data2)
2. Saya perlu membuat bash script terlebih dahulu dengan cara mengoding isi bash script dan membuat file itu menjadi file yang bisa dijalankan menggunakan bash script dengan menggunakan command `chmod +x joindate2.sh`
3. Skrip bash itu digunakan untuk mengambil data untuk menampilkan first_name dan last_name berdasarkan join_date, serta mengambil data untuk menampilkan deskripsi untuk pekerjaan tertentu.
Berikut adalah hasilnya 
(gambar hasil)

# 3.3 Upgrade OS

Berikut untuk proses upgrade OS menggunakan ansible playbook dan scriptnya

(gambar ansible upgrade os)
(script upgradeOS.yml)
