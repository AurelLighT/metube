# MeTube

![Build Status](https://github.com/alexta69/metube/actions/workflows/main.yml/badge.svg)
![Docker Pulls](https://img.shields.io/docker/pulls/alexta69/metube.svg)

Web GUI for youtube-dl (using the [yt-dlp](https://github.com/yt-dlp/yt-dlp) fork) with playlist support. Allows you to download videos from YouTube and [dozens of other sites](https://github.com/yt-dlp/yt-dlp/blob/master/supportedsites.md).

![screenshot1](https://github.com/alexta69/metube/raw/master/screenshot.gif)

# TUGAS KOMDAT KELOMPOK 2 (Hosting VPS)

## Anggota Kelompok

 - Irfan Alamsyah (G6401211029)
 - Steven Hesang (G6401211044)
 - Khansa Nabila Alfiyani (G6401211115)
 - Viragita Athaya Haura (G6401211116)

# Gunakan Sistem Operasi Linux

# Set up Nginx dan Docker

## 1. Install Nginx
```
sudo apt update
sudo apt install nginx
```

## 2. Allow firewall untuk Nginx
```
sudo ufw allow 'Nginx HTTP'
sudo ufw enable
```

## 3. Install Docker
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

# Set up reverse proxy menggunakan nginx

## 1. Jalankan
```
sudo nano /etc/nginx/sites-enabled/metube
```

##  2. Paste config berikut
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

## 3. Cek file config apakah sudah ok
```
sudo nginx -t
```

## 4. Reload config Nginx
```
sudo service nginx reload
```

## 5. Cek dan pastikan
cek di browser pada [IP Address]:8080, apakah sudah muncul nginx atau tidak

# Jalankan aplikasi web

## 1. Jalankan
```
nano docker-compose.yml
```

## 2. Paste config berikut
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

## 3. Jalankan Docker composenya
```
sudo docker compose up -d --build
```

## 4. Cek dan pastikan
Cek di browser pada [IP Address]:8080, apakah sudah muncul aplikasi web nya atau tidak

