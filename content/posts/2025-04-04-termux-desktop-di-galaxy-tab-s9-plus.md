---
title: Termux Desktop di Galaxy Tab S9+
date: 2025-04-04 00:00:01
categories: []
tags: [teknologi,galaxy-tab,termux,android]     # TAG names should always be lowercase
---

![](https://lh3.googleusercontent.com/pw/AP1GczMY1kBoyQHOyDb4zFY3NGKS_31OydXlRNmGlYzMygML-47Oa7VJ3hB5IcyWR44KVkKTZvIuyqrmY4YlUxYOW42o65XABJzQB1YMf0jQ-bSBy2w4_uCCCud9Bx0MkeTTAPl3iRauqeLxKcU7WasWbpeztg=w2340-h1464-s-no-gm?authuser=0)

Kejadian yang kurang menyenangkan ketika berlebaran dan ingin melanjutkan pekerjaan dari kampung halaman. Ubuntu yang terinstall via Termux tidak dapat mengakses menu Settings. Beberapa error juga terjadi pada Terminal sehingga sangat mengganggu pikiran serta aktivitas saya.

Setelah kembali mengingat ada 2 kemungkinan, yang pertama corrupt dan yang kedua adalah karena package update serta upgrade yang tempo lalu saya lakukan. Maka solusinya hanya satu, install ulang dan itu membuat semakin kesal karena harus mengeluarkan kuota lebih.

Karena belajar dari pengalaman buruk bersama Ubuntu, maka saya mencari alternatif dengan Termux. Dari hasil pencarian, saya menemukan repositori yang menarik setelah membaca dokumentasinya. Berikut ringkasan dari pembelajaran saya.

[Termux Desktop](https://github.com/sabamdarif/termux-desktop/tree/main) adalah sebuah solusi untuk mengakses Termux dalam bentuk Desktop OS. Desktop tersebut sendiri adalah alternatif user interface dari sistem operasi Android yang di mana dapat menggunakan style dari GTK. Contohnya XFCE, OpenBox, GNOME, dan lainnya.

Untuk menambah fleksibilitas terhadap environmentnya pun dapat menggunakan package manager dari Debian atau Arch. Hanya dapat pilih salah satu, serta mengakses UInya tanpa menggunakan VNC yaitu menggunakan Termux X11. Ini dapat menjadi kelebihan serta juga kekurangan.

![](https://lh3.googleusercontent.com/pw/AP1GczOOkjExuD1kwHzBGC-J9C20e-aUKLR6zSpJIymS-ltQ69Rfa_U4xPlgPzIG2MpJuZ9ti36Zkdy0MFkBFYWWVcE2XZCqj-kAkdwvrRsXRg9kgfHGjnkcjwYKqGrlTpiyc3oPD9HQ4BHCk3OExoewtzGrTA=w2340-h1464-s-no-gm?authuser=0)

Apa kekurangannya? Tanpa VNC Berarti saya tidak dapat remote melalui jaringan yang sama pada perangkat lain. _And absolutely, that is suck_. Kelebihannya dengan Termux X11 berarti ringan dan ringkas tanpa memakan bandwith. _So fortunately, that is amazing_.

Perbedan lainnya, jika menggunakan OS lain di atas Termux (yaitu lewat proot seperti yang saya lakukan sebelumnya bersama Ubuntu), di mana berbagi resource. Ternyata saya baru paham yang dibagi adalah kemampuan computing CPU dan sisanya adalah emulasi.

> Pantas saja ketika melakukan remote tunnel sering putus apalagi ketika sedang proses yang membutuhkan resource besar seperti Java, langsung diterminate oleh Phantom

Yang artinya GPU tidak diutilisasi secara optimal, alhasil ada glitch yang terlihat tidak smooth karena tidak native hardware. Dengan Termux Desktop hal ini disolusikan melalui [Hardware Acceleration](https://github.com/LinuxDroidMaster/Termux-Desktops/blob/main/Documentation/terminology.md). Yang artinya driver yang digunakan disesuaikan dengan GPU.

Metode tersebut built-in di dalam Termux Desktop, sehingga ketika install drivernya sudah auto inject. Sangat mempermudah tidak perlu effort paska instalasi untuk konfigurasi. Dampaknya berbeda karena komputasi grafis tidak lagi menggunakan CPU namun sudah menggunakan GPU.

![](![](https://lh3.googleusercontent.com/pw/AP1GczPmaDeIBOueg2LdSGNwqheZ8-6eub2HP9Dixx731TCGgQPWLjIAg6K_QN_ExqsQFXSRyJ0A80xJYKXlfhbBoBlpwHA0qzQ_MqXe9bXb-XzaIKpeb6LBDtTmQGv5dTaf2Plg0ZG6ksJRcpSY1TLDXn52IQ=w2340-h1464-s-no-gm?authuser=0)
_Run Visual Code, DataGrip, and HTTPie_)

Signifikan? Jelas yang pertama tidak lagi glitch dan sangat smooth, yang kedua adalah tablet saya tidak panas karena GPU juga dapat bekerja membantu CPU. Kelebihan lainnya adalah karena juga dilengkapi dengan package manager Debian maka dapat menginstall aplikasi berbasis Debian / Ubuntu.

Untuk proses instalasi dapat mengikuti petunjuk pada repositori tersebut dan juga join group Telegramnya. Terimakasih MDArief untuk proyek open sourcenya!


