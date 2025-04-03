---
title: Install AppImage Debian Termux
date: 2025-04-03 00:00:01
categories: []
tags: [teknologi,android,termux]     # TAG names should always be lowercase
---

Di bulan April ini saya mengalami hal yang kurang mengenakkan terkait Termux dan Ubuntu di mana harus menginstall ulang Termux. Hal tersebut akan saya ceritakan pada postingan berikutnya terkait [Termux Desktop](https://github.com/sabamdarif/termux-desktop). Namun dari hal yang buruk tersebut, turut mendapatkan pengalaman yang bermanfaat.

Pada postingan berikut ini saya berbagi mengenai bagaimana cara menginstall aplikasi berbasis AppImage untuk package manager Debian di Termux. Tentunya aplikasi yang diunduh harus dikhususkan untuk prosesor berbasis ARM64. Bagaimana cara memulainya?

Seluruh proses ini dilakukan di dalam Termux Desktop dan menggunakan profile Debian. Untuk casenya saya mengunduh aplikasi yang diinginkan dengan contoh [HTTPie](https://httpie.io/) yang berekstensi AppImage. Mengunduhnya melalui peramban dan ditempatkan pada direktori Downloads.

**Proses Ekstraksi**
```
cd ~/Downloads
chmod +x HTTPie-2025.2.0-arm64.AppImage
./HTTPie-2025.2.0-arm64.AppImage --appimage=extract
```

Dari perintah-perintah di atas tersebut maka aplikasi HTTPPie akan diektrak ke dalam direktori squashfs-root. Berikutnya adalah proses untuk merapihkan aplikasi agar mudah diakses serta dieksekusi. Hal tersebut dapat dilakukan dengan langkah berikut pada direktori Downloads.

**Proses Paska Instalasi**
```
mv squashfs-root/ HTTPie
mv HTTPie ~/
cd ~/HTTPie
```

Dari perintah-perintah diatas untuk mempermudah akses file executable ada pada direktori ~/HTTPie. Pastikan terdapat file binary httpie. Kita dapat menjalankan HTTPie dengan mengeksekusi perintah `./httpie --no-sandbox`. 

![](https://lh3.googleusercontent.com/pw/AP1GczN6bLMKNCEcSvHww-pvayFCZ2GE8iOaU-oQPn9vZ2v3C5wsF5VT49yh9Xx9iy7Jhig7Ttk2XiitDKbBGAH7AAhFRp2yeqB4gXmy8NPA4zSHooJY5-GaOaB4E7nZgLdDHU48W05OIYW62UIi1huE00e20Q=w2340-h1464-s-no-gm?authuser=0)

Langkah-langkah tersebut dapat berlaku juga bagi aplikasi lain semisal [Another Redis Desktop Manager](https://github.com/qishibo/AnotherRedisDesktopManager) yang berbasis NodeJS. Terimakasih.


  
