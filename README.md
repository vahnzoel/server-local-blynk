# Apa itu Blynk?
Blynk adalah platform dengan aplikasi iOS dan Android untuk mengontrol Arduino, ESP8266, Raspberry Pi dan sejenisnya melalui Internet. Anda dapat dengan mudah membangun antarmuka grafis untuk semua proyek Anda hanya dengan menyeret dan menjatuhkan widget.
Jika Anda memerlukan informasi lebih lanjut, ikuti tautan ini:
* [Blynk site](https://www.blynk.io)
* [Blynk docs](http://docs.blynk.cc)
* [Blynk community](https://community.blynk.cc)
* [Blynk Examples generator](https://examples.blynk.cc)
* [Facebook](http://www.fb.com/blynkapp)
* [Twitter](http://twitter.com/blynk_app)
* [App Store](https://itunes.apple.com/us/app/blynk-control-arduino-raspberry/id808760481?ls=1&mt=8)
* [Google Play](https://play.google.com/store/apps/details?id=cc.blynk)
* [Blynk library](https://github.com/blynkkk/blynk-library)
* [Kickstarter](https://www.kickstarter.com/projects/167134865/blynk-build-an-app-for-your-arduino-project-in-5-m/description)

![Dashboard settings](https://github.com/blynkkk/blynk-server/blob/master/docs/overview/dash_settings.png)
![Widgets Box](https://github.com/blynkkk/blynk-server/blob/master/docs/overview/widgets_box.png)
![Dashboard](https://github.com/blynkkk/blynk-server/blob/master/docs/overview/dash.png)
![Dashboard2](https://github.com/blynkkk/blynk-server/blob/master/docs/overview/dash2.png)

# Content 

- [Download](#blynk-server)
- [Requirements](#requirements)
- [Quick Local Server setup](#quick-local-server-setup)
- [Enabling mail on Local server](#enabling-mail-on-local-server)
- [Quick local server setup on Raspberry PI](#quick-local-server-setup-on-raspberry-pi)
- [Docker container setup](#docker-container-setup)
- [Enabling server auto restart on unix-like systems](#enabling-server-auto-restart-on-unix-like-systems)
- [Enabling server auto restart on Windows](#enabling-server-auto-restart-on-windows)
- [Update instruction for unix-like systems](#update-instruction-for-unix-like-systems)
- [Update instruction for Windows](#update-instruction-for-windows)
- [App and sketch changes for Local Server](#app-and-sketch-changes)
- [Advanced local server setup](#advanced-local-server-setup)
- [Administration UI](#administration-ui)
- [HTTP/S RESTful API](#https-restful)
- [Enabling sms on local server](#enabling-sms-on-local-server)
- [Enabling raw data storage](#enabling-raw-data-storage)
- [Automatic Let's Encrypt Certificates](#automatic-lets-encrypt-certificates-generation)
- [Manual Let's Encrypt SSL/TLS Certificates](#manual-lets-encrypt-ssltls-certificates)
- [Generate own SSL certificates](#generate-own-ssl-certificates)
- [Install java for Ubuntu](#install-java-for-ubuntu)
- [How Blynk Works?](#how-blynk-works)
- [Blynk Protocol](#blynk-protocol)

# MULAI

## Blynk server
Blynk Server adalah server Java berbasis Open-Source [Netty] (https://github.com/netty/netty), yang bertanggung jawab untuk meneruskan pesan antara aplikasi Blynk dan berbagai papan mikrokontroler dan SBC (i.e. Arduino, Raspberry Pi, dll.).

**Unduh build server terbaru [di sini](https://github.com/blynkkk/blynk-server/releases).**

[![GitHub version](https://img.shields.io/github/release/blynkkk/blynk-server.svg)](https://github.com/blynkkk/blynk-server/releases/latest)
[![GitHub download](https://img.shields.io/github/downloads/blynkkk/blynk-server/total.svg)](https://github.com/blynkkk/blynk-server/releases/latest)
[ ![Build Status](https://travis-ci.org/blynkkk/blynk-server.svg?branch=master)](https://travis-ci.org/blynkkk/blynk-server)

## Persyaratan
- Java 8/11 required (OpenJDK, Oracle) 
- OS apa pun yang dapat menjalankan java 
- Setidaknya 30 MB RAM (bisa lebih sedikit dengan penyetelan)
- Buka ports 9443 (untuk aplikasi dan hardware dengan ssl), 8080 (untuk hardware tanpa ssl)

[Instruksi instalasi java pada Ubuntu](#install-java-for-ubuntu).

Untuk Windows unduh Java [di sini](https://www.oracle.com/technetwork/java/javase/downloads/jdk11-downloads-5066655.html) dan instal. 

## Pengaturan server lokal cepat

+ Pastikan Anda menggunakan Java 11

        java -version
        Output: java version "11"

+ Jalankan server secara default 'hardware port 8080' dan default 'application port 9443' (SSL port)

        java -jar server-0.41.13.jar -dataFolder /path
        
Itu dia! 

**NOTE: ```/path``` harus ada jalur nyata ke folder tempat Anda ingin menyimpan semua data Anda.**

+ Sebagai output, Anda akan melihat sesuatu seperti itu:

        Blynk Server successfully started.
        All server output is stored in current folder in 'logs/blynk.log' file.
        
### Mengaktifkan email di server lokal
Untuk mengaktifkan pemberitahuan email di server lokal Anda harus memberikan kredensial email Anda sendiri. Buat file `mail.properties` dalam folder yang sama di mana `server.jar` adalah.
Mail properties:

        mail.smtp.auth=true
        mail.smtp.starttls.enable=true
        mail.smtp.host=smtp.gmail.com
        mail.smtp.port=587
        mail.smtp.username=YOUR_EMAIL_HERE
        mail.smtp.password=YOUR_EMAIL_PASS_HERE
        
Temukan Contoh [di sini](https://github.com/blynkkk/blynk-server/blob/master/server/notifications/email/src/main/resources/mail.properties).

PERINGATAN: hanya akun gmail yang diizinkan.

NOTE : Anda harus menyiapkan Gmail untuk memungkinkan aplikasi yang kurang aman.
Pergi [kesini](https://www.google.com/settings/security/lesssecureapps) dan lalu klik "Allow less secure apps".

## Pengaturan server lokal cepat pada Raspberry PI

+ Login ke Raspberry Pi via ssh;
+ Instal java 8: 
        
        sudo apt install openjdk-8-jdk openjdk-8-jre
        
+ Pastikan Anda menggunakan Java 8

        java -version
        Output: java version "1.8"
        
+ Unduh file jar Blynk server (atau copy secara manual ke Raspberry Pi melalui ssh dan scp command): 
   
        wget "https://github.com/vahnzoel/server-local-blynk/blob/master/server-0.41.16-java8.jar"

+ Jalankan server secara default 'hardware port 8080' dan default 'application port 9443' (SSL port)

        java -jar server-0.41.13-java8.jar -dataFolder /home/pi/Blynk
        
Itu dia! 

+ Sebagai output, Anda akan melihat sesuatu seperti itu:

        Blynk Server successfully started.
        All server output is stored in current folder in 'logs/blynk.log' file.

## Docker container setup

### Peluncuran cepat

+ Instal [Docker](https://docs.docker.com/install/)
+ Jalankan Docker container

        docker run -p 8080:8080 -p 9443:9443 mpherg/blynk-server

### Peluncuran cepat pada Raspberry Pi

+ Instal [Docker](https://docs.docker.com/engine/install/debian/)
+ Jalankan Docker container

        docker run -p 8080:8080 -p 9443:9443 linuxkonsult/rasbian-blynk

### Kustomisasi penuh

+ Cek [README](server/Docker) di docker folder




## Mengaktifkan restart otomatis server pada sistem seperti unix
        
+ Untuk mengaktifkan auto restart pada server temukan /etc/rc.local file dan tambahkan:

        java -jar /home/pi/server-0.41.13-java8.jar -dataFolder /home/pi/Blynk &
        
+ Atau jika cara di atas tidak berhasil, jalankan 
       
        crontab -e

tambahkan baris berikut

        @reboot java -jar /home/pi/server-0.41.13-java8.jar -dataFolder /home/pi/Blynk &
        
save dan exit.

## Mengaktifkan restart otomatis server pada Windows

+ Buat bat file:

        start-blynk.bat

+ Masukkan satu baris: 

        java -jar server-0.41.13.jar -dataFolder /home/pi/Blynk
        
+ Masukkan file bat ke folder startup windows

Anda juga dapat menggunakan [ini](https://github.com/blynkkk/blynk-server/tree/master/scripts/win) script to run server.

## Perbarui instruksi untuk sistem seperti unix

**PENTING**
Server harus selalu diperbarui sebelum Anda memperbarui Aplikasi Blynk. Untuk memperbarui server Anda ke versi yang lebih baru, Anda harus mematikan proses lama dan memulai yang baru.

+ Temukan id proses Blynk server

        ps -aux | grep java
        
+ Anda harus melihat sesuatu seperti itu
 
        username   10539  1.0 12.1 3325808 428948 pts/76 Sl   Jan22   9:11 java -jar server-0.41.13.jar   
        
+ Kill proses lama

        kill 10539
        
10539 - blynk proses server id dari output perintah di atas.
 
+ Mulai server baru [seperti biasa](#quick-local-server-setup)

Setelah langkah ini, Anda dapat memperbarui aplikasi Blynk. Downgrade versi server tidak didukung. 

**PERINGATAN!**
Tolong ** jangan ** kembalikan server Anda ke versi yang lebih rendah. Anda dapat kehilangan semua data Anda.

## Perbarui instruksi untuk Windows

+ Buka Task Manager;

+ Temukan Java process;

+ Hentikan process;

+ Mulai server baru [seperti biasa](#quick-local-server-setup)
                
## Perubahan aplikasi dan sketsa

+ Tentukan jalur server khusus di aplikasi Anda

![Custom server icon](https://github.com/blynkkk/blynk-server/blob/master/docs/login.png)
![Server properties menu](https://github.com/blynkkk/blynk-server/blob/master/docs/custom.png)

+ Ubah sketsa ethernet Anda dari

    ```
    Blynk.begin(auth);
    ```
    
    menjadi
    
    ```
    Blynk.begin(auth, "your_host", 8080);
    ```
    
    atau menjadi
    
    ```
    Blynk.begin(auth, IPAddress(xxx,xxx,xxx,xxx), 8080);
    ```
        
+ Ubah sketsa WIFI Anda dari
        
    ```
    Blynk.begin(auth, SSID, pass));
    ```
   
    menjadi
    
    ```
    Blynk.begin(auth, SSID, pass, "your_host", 8080);
    ```
    
    atau menjadi
    
    ```
    Blynk.begin(auth, SSID, pass, IPAddress(XXX,XXX,XXX,XXX), 8080);
    ```
        
+ Ubah javascript rasp PI Anda dari

    ```
    var blynk = new Blynk.Blynk(AUTH, options = {connector : new Blynk.TcpClient()});
    ```
    
    menjadi
    
    ```
    var blynk = new Blynk.Blynk(AUTH, options= {addr:"xxx.xxx.xxx.xxx", port:8080});
    ```
        
+ atau dalam kasus USB ketika menjalankan blynk-ser.sh, berikan opsi '-s' dengan alamat server lokal Anda

        ./blynk-ser.sh -s you_host_or_IP
        
        
**PENTING** 
Blynk terus dikembangkan. Aplikasi dan server seluler sering diperbarui. Untuk menghindari masalah selama pembaruan, matikan pembaruan otomatis untuk aplikasi Blynk, atau perbarui server lokal dan aplikasi blynk secara bersamaan untuk menghindari kemungkinan masalah migrasi.

**PENTING**
Server lokal Blynk berbeda dari server Blynk Cloud. Mereka tidak berhubungan sama sekali. Anda harus membuat akun baru saat menggunakan server lokal Blynk.

## Penyiapan server lokal tingkat lanjut
Untuk lebih fleksibel Anda dapat memperluas server dengan lebih banyak opsi dengan membuat file ```server.properties``` dalam folder yang sama dengan ```server.jar```. 
Contoh dapat ditemukan [di sini](https://github.com/blynkkk/blynk-server/blob/master/server/core/src/main/resources/server.properties).
Anda juga bisa menentukan jalur apa saja ke ```server.properties``` file melalui argumen command line ```-serverConfig```. Kamu bisa lakukan hal yang sama dengan ```mail.properties``` melalui ```-mailConfig``` dan ```sms.properties``` melalui ```-smsConfig```.
 
Untuk contoh:

    java -jar server-0.41.13-java8.jar -dataFolder /home/pi/Blynk -serverConfig /home/pi/someFolder/server.properties

Opsi server yang tersedia:

+ Blynk app, https, web sockets, admin port
        
        https.port=9443


+ Http, hardware and web sockets port

        http.port=8080
        
        
+ Untuk kesederhanaan Blynk sudah menyediakan server jar dengan sertifikat SSL bawaan, jadi Anda memiliki server yang berfungsi di luar kotak melalui soket SSL / TLS. Tetapi karena sertifikat dan kunci privatnya ada di publik, ini sama sekali tidak aman. Jadi untuk memperbaikinya Anda perlu memberikan sertifikat sendiri. Dan ubah properti di bawah ini dengan jalur ke sertifikat Anda. dan kunci pribadi dan kata sandi itu. Lihat cara membuat sertifikat yang ditandatangani sendiri [di sini](#generate-ssl-certificates)

        #points to cert and key that placed in same folder as running jar.
        
        server.ssl.cert=./server_embedded.crt
        server.ssl.key=./server_embedded.pem
        server.ssl.key.pass=pupkin123
        
        
+ Folder profil pengguna. Folder tempat semua profil pengguna akan disimpan. Secara default System.getProperty ("java.io.tmpdir") / blynk digunakan. Akan dibuat jika tidak ada

        data.folder=/tmp/blynk
        

+ Folder untuk semua log aplikasi. Akan dibuat jika tidak ada. "." dir dari mana Anda menjalankan skrip.

        logs.folder=./logs
        

+ Tingkat debug debug. Nilai yang mungkin: trace | debug | info | error. Menentukan seberapa akurat pencatatan. Dari kiri ke kanan -> maksimum logging ke minimum

        log.level=trace
        

+ Jumlah maksimum dasbor pengguna yang diizinkan.

        user.dashboard.max.limit=100
        

+ 100 Req / detik batas tarif per pengguna. Anda juga mungkin ingin memperpanjang batas ini pada [sisi hardware](https://github.com/blynkkk/blynk-library/blob/f4e132652906d63d683abeed89f5d6ebe369e37a/Blynk/BlynkConfig.h#L42).

        user.message.quota.limit=100
        

+ pengaturan ini menentukan seberapa sering Anda dapat mengirim email / tweet / push atau pemberitahuan lainnya. Ditentukan dalam hitungan detik
        
        notifications.frequency.user.quota.limit=60
        

+ Ukuran profil pengguna maksimum yang diizinkan. Di Kb.

        user.profile.max.size=128
        
        
+ Jumlah string yang akan disimpan di widget terminal (data riwayat terminal)

        terminal.strings.pool.size=25
        

+ Jumlah maksimum antrian pemberitahuan yang dibolehkan. Antrian bertanggung jawab untuk memproses email, push, pengiriman twit. Karena masalah kinerja - antrian tersebut diproses dalam utas terpisah, ini diperlukan karena sifat pemblokiran semua operasi di atas. Biasanya batas tidak boleh tercapai
        
        notifications.queue.limit=5000
        
        
+ Jumlah utas untuk melakukan operasi pemblokiran - push, twits, email, permintaan db. Dianjurkan untuk mempertahankan nilai ini rendah kecuali Anda harus melakukan banyak operasi pemblokiran.

        blocking.processor.thread.pool.limit=6
        

+ Periode untuk membuang semua DB pengguna ke disk. Dalam milis

        profile.save.worker.period=60000

+ Menentukan periode waktu maksimum saat soket perangkat keras tidak dapat digunakan. Setelah itu soket akan ditutup karena tidak aktif. Dalam hitungan detik. Biarkan kosong untuk batas waktu tak terbatas

        hard.socket.idle.timeout=15
        
+ Paling diperlukan untuk pengaturan server lokal jika pengguna ingin mencatat data mentah dalam format CSV. Lihat [data mentah] (#raw-data-storage) bagian untuk info lebih lanjut.
        
        enable.raw.data.store=true
        
+ Url untuk membuka halaman admin. Harus dimulai dari "/". Untuk jalur "/ admin" akan terlihat seperti itu "https://127.0.0.1:9443/admin". 

        admin.rootPath=/admin
        
+ Daftar IP administrator yang dipisahkan koma. Izinkan akses ke UI admin hanya untuk IP tersebut. Anda dapat mengaturnya untuk 0.0.0.0/0 untuk memungkinkan akses untuk semua. Anda dapat menggunakan notasi CIDR. Misalnya, 192.168.0.53/24.
        
        allowed.administrator.ips=0.0.0.0/0
        
+ Nama dan kata sandi admin default. Akan dibuat pada awal server awal
        
        admin.email=admin@blynk.cc
        admin.pass=admin

+ Host untuk mengatur ulang pengalihan kata sandi dan pembuatan sertifikat. Secara default IP server saat ini diambil dari antarmuka jaringan "eth". Dapat diganti dengan nama host yang lebih ramah. Disarankan untuk mengganti properti ini dengan IP server Anda untuk menghindari kemungkinan masalah penyelesaian host.
        
        server.host=blynk-cloud.com
        
+ Email yang digunakan untuk pendaftaran sertifikat, bisa dihilangkan jika Anda sudah menentukannya di mail.properties.
        
        contact.email=pupkin@gmail.com
        
## Administration UI

Server Blynk menyediakan panel administrasi tempat Anda dapat memonitor server Anda. Itu dapat diakses di URL ini:

        https://your_ip:9443/admin
        
![Administration UI](https://github.com/blynkkk/blynk-server/blob/master/docs/admin_panel.png)
              
**PERINGATAN**
Silakan ubah kata sandi dan nama admin default tepat setelah login ke halaman admin. **INI ADALAH UKURAN KEAMANAN**.
        
**PERINGATAN**
```allowed.administrator.ips``` default memungkinkan akses untuk semua orang. Dengan kata lain,
halaman administrasi tersedia dari komputer lain. Harap batasi akses ke sana melalui properti ```allowed.administrator.ips```.

### Matikan peringatan https chrome di localhost

- Paste di chrome 

        chrome://flags/#allow-insecure-localhost

- Anda akan melihat teks yang disoroti mengatakan: "Allow invalid certificates for resources loaded from localhost". Klik enable.
        
## HTTP/S RESTful
Blynk HTTP / S RESTful API memungkinkan untuk dengan mudah membaca dan menulis nilai ke / dari Pin di aplikasi dan Perangkat Keras Blynk. Deskripsi Http API dapat ditemukan [di sini](http://docs.blynkapi.apiary.io).

### Mengaktifkan sms di server lokal
Untuk mengaktifkan notifikasi SMS pada Server Lokal Anda perlu memberikan kredensial untuk gateway SMS (saat ini server Blynk hanya mendukung 1 penyedia - [Nexmo](https://www.nexmo.com/). Anda perlu membuat file ```sms.properties``` 
dalam folder yang sama dengan server.jar.

        nexmo.api.key=
        nexmo.api.secret=
        
Dan isi properti di atas dengan kredensial yang akan Anda dapatkan dari Nexmo. (Akun -> Pengaturan -> Pengaturan API).
Anda juga dapat mengirim SMS melalui email jika penyedia seluler Anda mendukungnya. Lihat [diskusi](http://community.blynk.cc/t/sms-notification-for-important-alert/2542) untuk lebih jelasnya.
 

## Mengaktifkan penyimpanan raw data
Secara default, penyimpanan raw data dinonaktifkan (karena terlalu banyak menghabiskan ruang disk).
Saat Anda mengaktifkannya, setiap perintah ```Blynk.virtualWrite``` akan disimpan ke DB.
Anda perlu menginstal Database PostgreSQL (**versi minimum yang diwajibkan adalah 9,5**) untuk mengaktifkan fungsi ini:

#### 1. Mengaktifkan raw data di server

Enable raw data in ```server.properties``` : 

        enable.db=true
        enable.raw.db.data.store=true

#### 2. Instal PostgreSQL. Opsi A

        sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
        wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -
        
        sudo apt-get update
        sudo apt-get install postgresql postgresql-contrib
        
#### 2. Instal PostgreSQL.  Opsi B 

        sudo apt-get update
        apt-get --no-install-recommends install postgresql-9.6 postgresql-contrib-9.6

#### 3. Unduh script Blynk DB

        wget https://raw.githubusercontent.com/blynkkk/blynk-server/master/server/core/src/main/resources/create_schema.sql
        wget https://raw.githubusercontent.com/blynkkk/blynk-server/master/server/core/src/main/resources/reporting_schema.sql

#### 4. Pindahkan create_schema.sql dan reporting_schema.sql ke temp folder (untuk menghindari masalah izin)

        mv create_schema.sql /tmp
        mv reporting_schema.sql /tmp
        
Hasil:  

        /tmp/create_schema.sql
        /tmp/reporting_schema.sql
        
Salin ke clipboard dari konsol Anda.

#### 5. Connect ke PostgreSQL

        sudo su - postgres
        psql

#### 6. Buat Blynk DB and Reporting DB, test user dan tables

        \i /tmp/create_schema.sql
        \i /tmp/reporting_schema.sql
        
```/tmp/create_schema.sql``` - adalah jalan dari langkah 4.
        
Anda akan melihat output berikutnya:

        postgres=# \i /tmp/create_schema.sql
        CREATE DATABASE
        You are now connected to database "blynk" as user "postgres".
        CREATE TABLE
        CREATE TABLE
        CREATE TABLE
        CREATE TABLE
        CREATE TABLE
        CREATE TABLE
        CREATE TABLE
        CREATE TABLE
        CREATE TABLE
        CREATE TABLE
        CREATE TABLE
        CREATE ROLE
        GRANT
        GRANT

#### Quit

        \q
               
Sekarang mulai server Anda dan Anda akan melihat teks berikutnya dalam file ```postgres.log```: 

        2017-03-02 16:17:18.367 - DB url : jdbc:postgresql://localhost:5432/blynk?tcpKeepAlive=true&socketTimeout=150
        2017-03-02 16:17:18.367 - DB user : test
        2017-03-02 16:17:18.367 - Connecting to DB...
        2017-03-02 16:17:18.455 - Connected to database successfully.
        
PERINGATAN:
Raw data dapat menggunakan ruang disk Anda dengan sangat cepat!

### CSV data format

Format data adalah:

        value,timestamp,deviceId
        
Contoh:

        10,1438022081332,0
        
Dimana ```10``` - adalah nilai pin.
```1438022081332``` - perbedaannya, diukur dalam milidetik, antara waktu saat ini dan tengah malam, 1 Januari 1970 UTC.
Untuk menampilkan tanggal / waktu dalam excel Anda dapat menggunakan rumus:

        =((COLUMN/(60*60*24)/1000+25569))
        
```0``` - device id
        
### Otomatis Mari Enkripsi pembuatan sertifikat

Server Blynk terbaru memiliki fitur yang sangat keren - otomatis membuat generasi Enkripsi Enkripsi.
Namun, ia memiliki beberapa persyaratan:
 
+ Tambah ```server.host``` properti di ```server.properties``` file. 
Contoh : 
 
        server.host=myhost.com

IP tidak didukung, ini adalah batasan dari Let's Encrypt. Perlu diingat juga bahwa ```myhost.com```
harus diselesaikan dengan DNS severs publik.
        
+ Tambah ```contact.email``` properti di ```server.properties```. contoh : 
 
        contact.email=test@gmail.com
        
+ Anda perlu memulai server pada port 80 (memerlukan hak root atau admin) atau
buat [port forwarding](#port-forwarding-for-https-api) menjadi default port HTTP Blynk - 8080.

Itu dia! Jalankan server seperti biasa dan sertifikat akan dihasilkan secara otomatis.

![](https://gifyu.com/images/certs.gif)

### Manual Mari Mengenkripsi Sertifikat SSL / TLS

+ Pertama instal [certbot](https://github.com/certbot/certbot) di server Anda (mesin tempat Anda menjalankan Blynk Server)

        wget https://dl.eff.org/certbot-auto
        chmod a+x certbot-auto
        
+ Buat dan verifikasi sertifikat (server Anda harus terhubung ke internet dan memiliki port 80/443 terbuka)

        ./certbot-auto certonly --agree-tos --email YOUR_EMAIL --standalone -d YOUR_HOST

Contoh 

        ./certbot-auto certonly --agree-tos --email pupkin@blynk.cc --standalone -d blynk.cc

+ Kemudian tambahkan ke file ```server.properties``` Anda (dalam folder dengan server.jar)

        server.ssl.cert=/etc/letsencrypt/live/YOUR_HOST/fullchain.pem
        server.ssl.key=/etc/letsencrypt/live/YOUR_HOST/privkey.pem
        server.ssl.key.pass=
        
### Hasilkan sertifikat SSL sendiri

+ Hasilkan sertifikat dan key yang ditandatangani sendiri

        openssl req -x509 -nodes -days 1825 -newkey rsa:2048 -keyout server.key -out server.crt
        
+ Konversikan server.key ke file kunci pribadi PKCS#8 dalam format PEM

        openssl pkcs8 -topk8 -inform PEM -outform PEM -in server.key -out server.pem
        
Jika Anda menghubungkan perangkat keras dengan [skrip USB](https://github.com/blynkkk/blynk-library/tree/master/scripts) Anda harus memberikan opsi '-s' yang menunjuk ke "nama umum" (nama host) Anda tidak ditentukan selama pembuatan sertifikat.
        
Sebagai output, Anda akan mengambil file server.crt dan server.pem yang perlu Anda berikan untuk properti server.ssl.

### Instal java untuk Ubuntu

        sudo add-apt-repository ppa:openjdk-r/ppa \
        && sudo apt-get update -q \
        && sudo apt install -y openjdk-11-jdk
        
atau jika di atas tidak berfungsi:

        sudo apt-add-repository ppa:webupd8team/java
        sudo apt-get update
        sudo apt-get install oracle-java8-installer
        
### Port forwarding for HTTP/S API

        sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080
        sudo iptables -t nat -A PREROUTING -p tcp --dport 443 -j REDIRECT --to-port 9443

### Mengaktifkan QR generation di server
        
        sudo apt-get install libxrender1

### Di belakang router wifi
Jika Anda ingin menjalankan server Blynk di belakang WiFi-router dan ingin diakses dari Internet, Anda harus menambahkan aturan port-forwarding pada router Anda. Ini diperlukan untuk meneruskan semua permintaan yang datang ke router dalam jaringan lokal ke server Blynk.

### Cara membangun
Blynk memiliki banyak tes integrasi yang membutuhkan DB, jadi Anda harus melewati tes saat membangun.

        mvn clean install -Dmaven.test.skip=true
        
### Bagaimana Blynk Bekerja?
Ketika perangkat keras terhubung ke Blynk cloud, ia membuka koneksi ssl / tls tetap-hidup pada port 443 (9443 untuk server lokal) atau tetap-hidup polos
koneksi tcp / ip pada port 8080. Aplikasi Blynk membuka koneksi mutual ssl / tls ke Blynk Cloud pada port 443 (9443 untuk server lokal).
Blynk Cloud bertanggung jawab untuk meneruskan pesan antara perangkat keras dan aplikasi. Dalam koneksi (aplikasi dan perangkat keras) yang digunakan Blynk
protokol biner sendiri dijelaskan di bawah ini.

### Protokol Blynk


#### Protokol sisi perangkat keras

Blynk mentransfer pesan biner antara server dan perangkat keras dengan struktur berikut:

| Command       | Message Id    | Length/Status   | Body     |
|:-------------:|:-------------:|:---------------:|:--------:|
| 1 byte        | 2 bytes       | 2 bytes         | Variable |

Definisi Perintah dan Status: [BlynkProtocolDefs.h](https://github.com/blynkkk/blynk-library/blob/7e942d661bc54ded310bf5d00edee737d0ca44d7/src/Blynk/BlynkProtocolDefs.h)


#### Protokol sisi aplikasi seluler

Blynk mentransfer pesan biner antara server dan aplikasi seluler dengan struktur berikut:

| Command       | Message Id    | Length/Status   | Body     |
|:-------------:|:-------------:|:---------------:|:--------:|
| 1 byte        | 2 bytes       | 4 bytes         | Variable |


#### Protokol sisi websockets

Blynk mentransfer pesan biner antara server dan soket web (untuk web) dengan struktur berikut:

| Websocket header   | Command       | Message Id    | Body     |
|:------------------:|:-------------:|:-------------:|:--------:|
|                    | 1 byte        | 2 bytes       | Variable |


Ketika kode perintah == 0, selanjutnya struktur pesan:

| Websocket header   | Command       | Message Id    | Response code |
|:------------------:|:-------------:|:-------------:|:-------------:|
|                    | 1 byte        | 2 bytes       | 4 bytes       |

[Possible response codes](https://github.com/blynkkk/blynk-server/blob/master/server/core/src/main/java/cc/blynk/server/core/protocol/enums/Response.java#L12).
[Possible command codes](https://github.com/blynkkk/blynk-server/blob/master/server/core/src/main/java/cc/blynk/server/core/protocol/enums/Command.java#L12)

Id dan Panjang Pesan adalah [big endian](http://en.wikipedia.org/wiki/Endianness#Big-endian).
Body memiliki format khusus-perintah.

## Perizinan
[GNU GPL license](https://github.com/blynkkk/blynk-server/blob/master/license.txt)
