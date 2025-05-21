# Penilaian sumatif akhir tahun 2425 - Aplikasi CRUD PHP
Aplikasi ini adalah sistem login dan manajemen data sederhana berbasis PHP dan MySQL yang dapat digunakan untuk belajar dasar-dasar pengembangan web dengan PHP.

#
## 🚀 Fitur Utama

* Sistem Login dan Logout
* Operasi CRUD (Create, Read, Update, Delete) untuk manajemen data
* Antarmuka mode gelap (dark mode)
* Deployment otomatis via UserData di AWS EC2
* Koneksi ke database MySQL/RDS

#
## 🛠️ Cara Deploy ke AWS EC2 dengan UserData

### 1. Persiapan Database RDS

Sebelum men-deploy aplikasi, pastikan Anda telah membuat database RDS:

1. Masuk ke AWS Console
2. Buka layanan RDS
3. Klik "Create database"
4. Pilih "MySQL"
5. Atur konfigurasi sesuai kebutuhan:
   * DB instance identifier: `(nama pilihan Anda)` 
   * Master username: `admin`
   * Master password: `(gunakan password yang lebih kuat untuk produksi)` 
6. Atur pengaturan jaringan agar dapat diakses dari EC2 instance
7. Catat endpoint RDS yang akan digunakan pada konfigurasi .env

### 2. Membuat Security Group

Security Group diperlukan untuk mengatur akses ke instance EC2:

1. Buka layanan EC2 di AWS Console
2. Klik "Security Groups" di panel navigasi kiri
3. Klik "Create security group"
4. Beri nama dan deskripsi yang sesuai
5. Tambahkan aturan inbound:
   * HTTP (Port 80) - Source: 0.0.0.0/0
   * HTTPS (Port 443) - Source: 0.0.0.0/0
   * SSH (Port 22) - Source: 0.0.0.0/0 
6. Klik "Create security group"

### 3. Membuat EC2 Instance dengan UserData

1. Masuk ke AWS Console
2. Pilih layanan EC2
3. Klik "Launch Instance"
4. Konfigurasi instance:
   * Name: `PSAT2425-Server` (atau nama pilihan Anda)
   * Application and OS Images: Pilih Ubuntu Server (misalnya Ubuntu Server 22.04 LTS)
   * Instance type: `t2.micro` (Eligible for free tier)
   * Key pair: Pilih key pair yang sudah ada atau buat baru
   * Network settings: Pilih security group yang telah dibuat sebelumnya
5. Scroll ke bawah sampai menemukan "Advanced details"
6. Di bagian "User data", masukkan script berikut:

```bash
#!/bin/bash
sudo apt update -y
            sudo apt install -y apache2 php php-mysql libapache2-mod-php mysql-client
            sudo rm -rf /var/www/html/{*,.*}
            sudo git clone (link repository anda) /var/www/html
            sudo chmod -R 777 /var/www/html
            echo DB_USER=(username) > /var/www/html/.env
            echo DB_PASS=(password)  >> /var/www/html/.env
            echo DB_NAME=(nama database)  >> /var/www/html/.env
            echo DB_HOST=(endpoint rds) >> /var/www/html/.env
```
#
### 5. Verifikasi Instalasi

1. Setelah inisialisasi selesai (biasanya 3-5 menit), akses aplikasi melalui browser
2. Buka Public IPv4 address atau Public IPv4 DNS dari instance EC2 Anda
3. Contoh: `http://ec2-12-34-56-78.compute-1.amazonaws.com` atau `http://12.34.56.78`

### 6. Menjalankan Aplikasi

🔑 Login menggunakan kredensial default:
* Username: admin
* Password: 123

📥 Setelah login, Anda dapat menggunakan fitur CRUD untuk mengelola data sesuai kebutuhan.

