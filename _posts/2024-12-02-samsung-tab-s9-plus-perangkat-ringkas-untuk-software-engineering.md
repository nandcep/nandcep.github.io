---
title: Samsung Tab S9+, Perangkat Ringkas untuk Software Engineering
date: 2024-12-02 00:00:01
categories: []
tags: [teknologi,galaxy-tab,pemrograman]     # TAG names should always be lowercase
---

Awalnya saya ingin berbagi experience penggunaan Tab S9+ yang sudah hampir setahun intensif digunakan untuk bekerja. Berhubung sudah banyak review positifnya yang bertebaran di Youtube mengenai performa harian, saya memutuskan untuk mengulas apakah dapat menunjang produktivitas di bidang software engineering.

Dan seperti yang telah disampaikan oleh para reviewer, benar tablet ini direkomendasikan.

![](https://lh3.googleusercontent.com/pw/AP1GczNcN9KfVqgcn0ewDQ8i3epx0nlf-IDA_OuK4xFOIvQPIaFXiHAV1M7Xd_07tj_tNZikWBCZhnY8mPFrV-3-h_-nM8jUst6ww-3hpR8Rxw1sOREIiPaDnW198qYwXfSL7ZiUKlRqF37i6fz8Drpz86Xeqg=w2626-h1478-s-no?authuser=0)
Samsung Tab S9+ dengan Ubuntu in Action

> Ringan, ringkas, taktis adalah hal-hal yang ada di benak saya ketika membayangkan bekerja dengan sebuah tablet

Terinspirasi dari VP saya ketika mengikuti acara workshop AI di Microsoft Desember 2023 lalu. Tampak simple. Dengan tablet beliau mencatat materi, ambil foto, dan yang terpenting terlihat profesional. Makin mantap meminang tablet ini. Maklum masih ada stigma bekerja hanya dengan smartphone dianggap tidak serius apalagi ketika sedang rapat offline.

Berikut adalah ulasan singkat dari saya.

# Perangkat Handal Urusan Kantor
Dimulai dari paling mendasar seperti browsing, rapat online dengan berbagi layar, akses-reply email, dan manajemen dokumen perusahaan semua dapat dieksekusi. Akses VPN untuk akses internal aplikasi tidak menjadi masalah karena ketentuan secara resmi ada pada panduan perusahaan. Secara teknis semua aplikasi untuk keperluan harian resmi tersedia di Playstore.

Yang paling saya suka adalah File Manager bawaan tablet ini yang dapat mengelola cloud storage serta SFTP tanpa perlu tambahan aplikasi. Beberapa perihal yang saya harapkan untuk diperbaiki oleh Microsoft adalah Office365 untuk Android sebaiknya dapat mengedit master slide dan ekspor PDF. Untuk keperluan tersebut saya masih harus melakukannya di versi desktop.

# Termux sang Swiss Army Knife

Terminal adalah kebutuhan mendasar sebagai seorang developer. Terminal yang saya butuhkan digunakan untuk olah perintah Linux seperti SSH, Git, Vim, manajemen direktori, edit file, dsb. Setelah menelusuri artikel dan tutorial saya memutuskan untuk menggunakan Termux yang secara profile serta fungsionalitas cocok.

![](https://lh3.googleusercontent.com/pw/AP1GczPzux5RVZCrYUHozV9WYGBlx3Bmbv48zbGW8K5R6YhoP80lMbY7HlWJBkRVpJC-fFFdoxkiCIgZv10VJSlmuZQkYLD6uwQjLtYkN2wHsryyp4jkCIqDJJ5OAsV-AuZoyzkhwXOcue80AB165TleWMGRnA=w1400-h876-s-no?authuser=0)
Termux di Tab S9+

Termux dinfungsikan sebagai package manager dari Android untuk membuatnya memiliki kemampuan seperti pada distro Linux lainnya. Saya dapat menginstall package seperti multiplexer, neofetch, zsh dan ohmyzsh, dsb sesuai dengan kebutuhan. Ultimatenya juga dapat dimanfaatkan untuk virtualisasi seperti halnya di Windows biasa menggunakan WSL atau juga Docker.

# Ubuntu untuk Development

Menginstall Ubuntu untuk keperluan software engineering di atas tablet Android merupakan pengalaman baru yang menyenangkan bagi saya. Tanpa root dengan Termux, seamless dan aktivitas development saya tidak membuat registry pada host menjadi “kotor”. Ubuntu yang saya gunakan adalah versi 22.04 LTS untuk ARM dengan menyesuaikan arsitektur prosesor dari hardware melalui panduan [berikut](https://github.com/tuanpham-dev/termux-ubuntu).

![](https://lh3.googleusercontent.com/pw/AP1GczMTnI5KWKSEkX6E2eU0Qgr9fCLHzc13kvmn_50bLqf1lCYLM_qq6wlESMojDq-xqjjZOULajP9ElA96pluS5WWrntggyxaR1_okx-J9QpbbLmAbsYbvbnT24IDMl4qouZXCpT60YCY7V6cjIgZaRucGkQ=w2362-h1478-s-no?authuser=0)
Ubuntu via Termux

Secara virtualisasi ternyata Ubuntu yang saya install melalui Termux menggunakan shared resource secara fisik dengan host, terasa mirip dengan WSL. Prosesor dan memori di Ubuntu terintegrasi dengan Android bahkan mampu mengakses filesystem host meski terbatas secara permission. Dengan shared resource tersebut, development dengan Go hingga yang membutuhkan resource besar seperti Java pun tidak menjadi masalah.

![](https://lh3.googleusercontent.com/pw/AP1GczPCNCop4xZL66LKVSJ6rF1WBtJuRZhcaiVFBdmhtTiSn520RBMBb3dZvU5GVMfLQI_xE9ZQ9uM4qQzm5ousHJtEWJ_x2OzQql9JTuaDdqV0gW2R-SyqAjlEqHWKsoJNFVD8W1hfCMiRQDHdMqrYRLrTCA=w2362-h1478-s-no?authuser=0)
Remote Ubuntu dari Termux via AVNC

Proses multi-task semisal membuka DBeaver yang berbasis Eclipse, Visual Code dengan banyak plugin, beberapa tab terbuka di browser Chromium, serta parallel ketika kompilasi source code terasa enteng. Snapdragon 8 Gen 2 dan RAM 12GB benar-benar digunakan dengan optimal. Saya menjadi penasaran apakah iPad dapat melakukan hal yang sama.

# QEMU sang Docker Saver

Kehidupan virtualisasi serta development environment di era sekarang tidak akan jauh dari yang namanya Container. Sehingga Docker turut menjadi kebutuhan utama saya. Maka dari itu saya memutuskan untuk menginstall QEMU di atas Termux. Pada QEMU tersebut saya membuat sebuah mini VM dengan sistem operasi Alpine.

![](https://lh3.googleusercontent.com/pw/AP1GczNP9FXc4qDmoT-lgDzRVYMB9GYRTJUi7ZzcHX9j4pNeTYbbO_UqaF_-XPe13e_9q3kCV0wGLyAzDPqAK75ovvORpXiPjEfGXQC77WrYZGe8r3-76Ggf8LOjIDN7EnGsCAAwXa5hu0cBSYyOpdD_g9qa2w=w2362-h1478-s-no?authuser=0)
Qemu di Termux dengan Alpine untuk Docker

Pada sistem operasi tersebut saya melakukan setup untuk Docker dengan panduan [berikut](https://gist.github.com/oofnikj/e79aef095cd08756f7f26ed244355d62). Namun perlu dicatat bahwa untuk melakukan build image serta menjalankan kontainerisasi tentunya tidak terlalu cepat dan lancar karena prosesnya berada di dalam VM, namun setidaknya dapat digunakan dan berfungsi normal. Untuk arsitektur QEMU yang kebetulan saya gunakan adalah berbasis Intel karena menyesuaikan kebutuhan.

# Aktivitas Coding

Di tablet ini saya menginstall Go, NodeJS, JDK, dan beberapa SDK lainnya yang saya butuhkan di dalam Ubuntu 22.04 LTS. Tentunya spesifikasinya perlu disesuaikan dengan arsitektur prosesor dari hardware yaitu berbasis ARM. Untuk editor source code saya selalu percaya dengan Visual Studio Code yang juga untuk Ubuntu berbasis ARM.

![](https://lh3.googleusercontent.com/pw/AP1GczOuXD4tSOfDxVuS1hShELR3yaQtISOMW-Qk3Y0ideVns73Wij6LSA2KvSR1LMxCiLRqr1zHrGSNwe0OGhc5Q9UNaR33JnPmG776Z6FI-4IqZVFCeN6rZCs7lTOz3Zhzbu97hk5nGeP9bRuAFCfQKYU15Q=w2362-h1478-s-no?authuser=0)
VSCode di Tab S9+ menggunakan PWA serta Remote Development

Berbekal akun GitHub dan Visual Studio Code saya menggunakan fleksibilitas dari fitur remote development. Dengan fitur remote development memungkinkan Visual Studio Code di dalam Ubuntu dapat diremote melalui browser Chrome Android dengan PWA (Progressive Web Application). Alasan menggunakan remote development di Android yaitu agar terasa terlihat dan terasa lebih native.

Untuk akses database seperti yang sebelumnya telah disebutkan menggunakan dBeaver. Sedangkan untuk mengakses Redis saya menggunakan Tiny RDM. Browser utama yang saya andalkan adalah menggunakan Chromium. Perkara pemasangan piranti lunak hampir semuanya dapat diakomodir meskipun tanpa menggunakan Snap.

Multi-task DBeaver dan TinyRDM di Ubuntu pada Tab S9+
Ada beberapa kekurangan dari Ubuntu untuk Android lewat Termux. Karena banyak permission yang terbatas secara filesystem meski sudah akses Ubuntu sebagai root, saya tidak dapat menginstall beberapa aplikasi yang penting seperti Postman atau Insomnia API Client. Sehingga sebagai alternatifnya, saya menggunakan Thunder Client pada Visual Studio Code.

# Aplikasi Pendukung Lainnya

Sebagai seorang arsitek maka pekerjaan saya tidak hanya perkara membuat baris kode namun juga melakukan desain perangkat lunak. Untungnya aplikasi yang biasa saya gunakan untuk mendesain sistem adalah berbasis web. Dengan menggunakan PWA, semua aplikasi yang dibutuhkan dapat diinstall pada tablet, seolah native dengan limitasi harus online.

![](https://lh3.googleusercontent.com/pw/AP1GczOFv7FSPjpsj161Ubix8XNBaxM10dS-niUtDUHlFvm9Nk-igfU0wt--HaxFH5R24RkiDcAvz1r5w_KOlsdux0e_BGXa1uMJLXAvu7yt4BczGIi-4y67DKEztcr44vMTp__lDzVRbU3TmQWqeju5Y3l8QA=w2362-h1478-s-no?authuser=0)
DrawIO Progressive Web App di Tab S9+

Berikut adalah beberapa aplikasi yang berhasil diinstall dengan menggunakan PWA. MermaidJS untuk melakukan pembuatan UML serta diagram yang dapat diembed ke dalam format markdown. DrawIO untuk membuat desain infrastruktur serta bagan yang lebih fleksible.

![](https://lh3.googleusercontent.com/pw/AP1GczOVThXUy5tERk0006a3ffO9x2L7s8BLNOYueK45dkxEOl2KEMBKDQym8N9ypy9vE9F1_dBAu51vm9TzC0J6P0dTehDa39p2QKS410skc9bK-GzCLHRNa6yOjVkrtq67BCIT7yv_r8cJyNpAVTEravAsFg=w2362-h1478-s-no?authuser=0)
MermaidJS Progressive Web App di Tab S9+

Sebagai alternatif dari Thunder Client apabila malas membuka Ubuntu, saya menggunakan API Tester yang berbasis Android native. API Tester juga melakukan import cURL dan Postman Collection. Selain itu tidak lupa Termius apabila membutuhkan SSH serta SCP, berhubung banyak sekali server serta akun yang tidak mungkin saya hapal semua.

# Kesimpulan

Sejauh ini keterbatasan dari tablet ini adalah bukan untuk pengembangan aplikasi berbasis mobile misal Android dan apalagi iOS. Limitasi emulasi serta akses filesystem juga masih menjadi penghalang utama, jadi jangan berharap dapat menggunakan Kubernetes juga.

> Apabila benar-benar kita membutuhkan hal-hal tersebut, remote development menjadi pilihan yang tepat, semisal dengan menyewa VPS atau EC2 instance di AWS.

Namun dari semua kebutuhan yang telah saya jabarkan secara keseluruhan, hingga saat ini tablet ini masih membantu saya untuk bekerja dan menjadi pilihan utama karena ukurannya yang sangat kecil, ringkas, serta ringan untuk dibawa kemana pun.

By the way saya juga membuat postingan ini menggunakan VSCode pada tablet lewat GitHub tunnel. Remote development memang membantu dan sangat memungkinkan untuk blogging dengan menggunakan tablet Samsung S9+. Senang dapat berbagi!