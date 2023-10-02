# Aplikasi Web MeTube

|Name|NIM|
|--|--|
|[Irfan Alamsyah](https://github.com/irfanalmsyah)|G6401211029|
|[Steven Hesang](https://github.com/AurelLighT)|G6401211044|
|[Khansa Nabila Alfiyani](https://github.com/acrimsonmoon)|G6401211115|
|[Viragita Athaya Haura](https://http://github.com/viragitaa)|G6401211116|

# Sekilas tentang MeTube
Metube adalah web GUI untuk mendownload video youtube. Aplikasi ini memudahkan user untuk mendownload video dari youtube dan [banyak website yang lain](https://github.com/yt-dlp/yt-dlp/blob/master/supportedsites.md) dan menyimpannya untuk diakses di waktu lain.

Demo diakses melalui link berikut: [http://128.199.83.122:8080/](http://128.199.83.122:8080/).

# Instalasi
Tahap ini merupakan instalasi NGINX dan Docker pada OS Ubuntu 20.04. Prosedur instalasi dapat bervariasi pada OS yang berbeda.

## NGINX
[NGINX](https://nginx.org/en/docs/) adalah web server yang dapat digunakan sebagai reverse proxy. Reverse proxy adalah server yang berada di antara client dan server yang bertugas untuk menerima request dari client dan meneruskannya ke server. Reverse proxy ini dapat digunakan untuk meningkatkan performa dan keamanan server.

### 1. Update package manager
```bash
sudo apt update
```
### 2. Install NGINX
```bash
sudo apt install nginx
```

### 3. Allow firewall untuk NGINX agar dapat diakses dari luar
``` bash
sudo ufw allow 'Nginx HTTP'
sudo ufw enable
```

### 4. Cek status NGINX
```bash
sudo systemctl status nginx
```

## Docker
[Docker](https://docs.docker.com/) adalah platform untuk mengembangkan, mengirimkan, dan menjalankan aplikasi. Docker memungkinkan untuk mengisolasi aplikasi ke dalam container sehingga dapat dijalankan di berbagai platform.

### 1. Update package manager
```bash
sudo apt-get update
```

### 2. Install ca-certificates, curl, dan gnupg agar dapat mengakses repository melalui HTTPS
```bash
sudo apt-get install ca-certificates curl gnupg
```

### 3. Tambahkan Docker GPG key
```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 4. Update package manager setelah menambahkan repository
```bash
sudo apt-get update
```

### 5. Install Docker Engine
```bash 
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

# Konfigurasi
## Reverse Proxy
Reverse proxy digunakan untuk mengakses aplikasi web yang berjalan di dalam container Docker. Reverse proxy yang digunakan adalah NGINX. Pada kasus ini, MeTube berjalan pada port 8081, kemudian dilakukan reverse proxy pada port 8080.

### 1. Buat file config NGINX
```bash
echo 'server {
  listen 8080;
  
  location / {
    proxy_pass http://localhost:8081;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_set_header Host $host;
  }
}' | sudo tee /etc/nginx/sites-enabled/metube

```
Perintah di atas akan membuat file config NGINX pada direktori `/etc/nginx/sites-enabled/` dengan nama `metube`. File config tersebut akan mengarahkan request yang masuk pada port 8080 ke port 8081.

## Docker Compose

### 1. Buat file config docker compose
```bash
echo 'version: "3"
services:
  metube:
    image: ghcr.io/alexta69/metube
    container_name: metube
    restart: unless-stopped
    ports:
      - "8081:8081"
    volumes:
      - /path/to/downloads:/downloads' > docker-compose.yml
```
Perintah di atas akan membuat file config docker compose pada direktori yang sama dengan nama `docker-compose.yml`. File config tersebut akan membuat container dengan nama `metube` yang akan berjalan pada port 8081. Selain itu, file config tersebut akan membuat volume `/path/to/downloads` yang akan digunakan untuk menyimpan file yang didownload oleh MeTube.

### 3. Jalankan Docker composenya
``` bash
sudo docker compose up 
```

Web MeTube dapat diakses pada [IP Address]:8080. 


# Cara pemakaian
1. Salin tautan video atau playlist YouTube yang ingin Anda download.
2. Tempelkan tautan tersebut ke dalam kolom yang telah disediakan.
3. Pilih format file yang Anda inginkan untuk didownload.
4. Klik tombol "add".
5. Tunggu hingga proses di server selesai.
6. Setelah selesai, klik tombol untuk download file tersebut.

![screenshot1](https://github.com/alexta69/metube/raw/master/screenshot.gif)

# Pembahasan

## Kelebihan
- Mampu keep track semua file yang telah didownload.
- Dapat mendownload banyak file dalam waktu yang bersamaan.
- Tersedia berbagai pilihan format file yang dapat dipilih sesuai kebutuhan.
- Bisa mendownload video dari berbagai sumber, termasuk YouTube dan [berbagai website lainnya](https://github.com/yt-dlp/yt-dlp/blob/master/supportedsites.md)
- Antarmuka responsif yang memastikan kenyamanan pengguna saat menggunakannya di berbagai perangkat
- Mode gelap yang tersedia untuk mengurangi kelelahan mata.
- Dapat mengimport link playlist, yang memungkinkan untuk mendownload banyak video sekaligus.
## Kekurangan 
- Tidak bisa mendownload tipe video yang dienkripsi
- Memerlukan penyimpanan dan bandwidth yang cukup besar
- Kecepatan download video dapat terpengaruh jika server tidak dioptimalkan dengan baik, yang dapat mengganggu pengalaman pengguna.
- Terdapat risiko melanggar hak cipta dan masalah legalitas.

## Perbandingan dengan web lain
Website yang digunakan untuk perbandingan adalah [y2mate](https://www.y2mate.com/). Y2mate adalah salah satu platform yang digunakan untuk mengunduh video dari YouTube. Namun, terdapat perbedaan yang cukup signifikan antara Y2mate dan MeTube.

Salah satu perbedaan utama antara keduanya adalah dalam hal kemampuan mengunduh beberapa video sekaligus. MeTube memungkinkan pengguna untuk mengunduh beberapa video yang ada dalam satu playlist, sementara Y2mate tidak memiliki fitur ini. Hal ini membuat MeTube lebih praktis jika Anda ingin mengunduh beberapa video sekaligus.

Selain itu, ada perbedaan lain dalam hal penyimpanan riwayat pengunduhan. Video yang diunduh menggunakan MeTube akan memiliki riwayat pengunduhan yang tersimpan, sehingga Anda dapat dengan mudah melihat daftar video yang telah Anda unduh sebelumnya. Di sisi lain, Y2mate tidak menyimpan riwayat pengunduhan, yang berarti riwayat video yang telah Anda download akan hilang jika.

Terdapat perbedaan penting lainnya antara MeTube dan Y2mate yang harus diperhatikan. MeTube adalah platform open source, yang berarti source code tersedia untuk dilihat dan dimodifikasi oleh siapa saja yang tertarik. Hal ini memberikan fleksibilitas kepada developer untuk mengadaptasi atau memperbaiki platform sesuai dengan kebutuhan mereka, serta memberikan tingkat transparansi yang lebih tinggi terkait keamanan dan privasi data pengguna.

Di sisi lain, Y2mate adalah platform closed source, yang berarti source code tidak tersedia untuk umum. Ini berarti pengguna tidak dapat melihat atau memodifikasi kode sumbernya, dan mereka harus mengandalkan developer platform untuk memelihara dan mengelola semua aspek teknisnya.

Dengan adanya perbedaan ini, MeTube dapat lebih menarik bagi pengguna yang peduli tentang kontrol, privasi, dan potensi kontribusi ke dalam pengembangan platform. Di sisi lain, y2mate mungkin lebih cocok bagi mereka yang hanya memerlukan layanan unduhan video sederhana tanpa perlu terlibat dalam sisi teknisnya.


# Referensi
- [alexta69/metube](https://github.com/alexta69/metube)
- [Nginx Documentation](https://nginx.org/en/docs/)
- [Docker Documentation](https://docs.docker.com/)
