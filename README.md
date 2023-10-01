
# Aplikasi Web MeTube

## Anggota Kelompok

 - Irfan Alamsyah (G6401211029)
 - Steven Hesang (G6401211044)
 - Khansa Nabila Alfiyani (G6401211115)
 - Viragita Athaya Haura (G6401211116)

# Sekilas tentang MeTube
Metube adalah web GUI untuk mendownload video youtube. Aplikasi ini memudahkan user untuk mendownload video dari youtube dan [banyak website yang lain](https://github.com/yt-dlp/yt-dlp/blob/master/supportedsites.md).


## [Link Hostingan dapat diklik disini](http://128.199.83.122:8080/)



# Konfigurasi

## Set up Nginx dan Docker

### 1. Install Nginx
```
sudo apt update
sudo apt install nginx
```

### 2. Allow firewall untuk Nginx
```
sudo ufw allow 'Nginx HTTP'
sudo ufw enable
```

### 3. Install Docker
```
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## Set up reverse proxy menggunakan nginx

### 1. Jalankan
```
sudo nano /etc/nginx/sites-enabled/metube
```

###  2. Paste config berikut
```
server {
        listen 8080;

        location / {
            proxy_pass http://localhost:8081;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            proxy_set_header Host $host;
        }
}
```

### 3. Cek file config apakah sudah ok
```
sudo nginx -t
```

### 4. Reload config Nginx
```
sudo service nginx reload
```

### 5. Cek dan pastikan
cek di browser pada [IP Address]:8080, apakah sudah muncul nginx atau tidak

## Jalankan aplikasi web

### 1. Jalankan
```
nano docker-compose.yml
```

### 2. Paste config berikut
```
version: "3"
services:
  metube:
    image: ghcr.io/alexta69/metube
    container_name: metube
    restart: unless-stopped
    ports:
      - "8081:8081"
    volumes:
      - /path/to/downloads:/downloads
```

### 3. Jalankan Docker composenya
```
sudo docker compose up -d --build
```

### 4. Cek dan pastikan
Cek di browser pada [IP Address]:8080, apakah sudah muncul aplikasi web nya atau tidak


# Cara pemakaian
1. Copy link video youtube yang ingin digunakan
2. Tunggu hingga video di download oleh server
3. Download video

![screenshot1](https://github.com/alexta69/metube/raw/master/screenshot.gif)

# Pembahasan

## Kelebihan
- Bisa keep track apa saja yang sudah di download
- Bisa mendownload banyak file sekaligus
- Banyak pilihan format file apa yang diinginkan
- Tidak hanya video youtube saja, bisa juga dari sumber [website yang lain](https://github.com/yt-dlp/yt-dlp/blob/master/supportedsites.md)

## Kekurangan 
- Tidak bisa mendownload tipe video yang dienkripsi
- Membutuhkan penyimpanan dan bandwidth yang cukup besar
- Pengelolaan dan pemeliharaan server yang cukup rumit 
- Keterbatasan kecepatan video apabila server tidak dioptimalkan dengan baik sehingga dapat mengakibatkan pengalaman pengguna yang buruk
- Terdapat kemungkinan dalam melanggar hak cipta dan legalitas
- Dapat menjadi sasaran dalam kurangnya keamanan

## Perbandingan dengan web lain
Contoh website yang dibandingkan adalah dengan website [y2mate](https://www.y2mate.com/id740/youtube) <br />
y2mate merupakan salah satu website untuk youtube downloader. Salah satu kekurang dari website y2mate adalah hanya bisa mendownload video dari youtube saja, semetnara MeTube bisa mendownload dari beberapa website yang lain. MeTube juga bisa melakukan download beberapa video secara sekaligus dengan mudah akan tetapi y2mate jika sangat ingin mendownload banyak video akan sedikit lebih ribet. Tidak hanya itu, Video yang telah didownload oleh MeTube akan tersimpan historynya
# Referensi
[alexta69/metube](https://github.com/alexta69/metube)
