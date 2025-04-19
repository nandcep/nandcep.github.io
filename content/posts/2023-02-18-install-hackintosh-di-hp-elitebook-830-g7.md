---
title: Install Hackintosh di HP Elitebook 830 G7
date: 2023-02-18 00:00:01
categories: []
tags: [hackintosh,teknologi]     # TAG names should always be lowercase
---

![](https://lh3.googleusercontent.com/pw/AP1GczM56AejlyYgRVtsOXtbGwwbF7OTj3miUd0zdgb7BdTGcSsdqAgF8o9pyn_TMhRZHj2QNuYClVu_DH42KpZRjJUoVOuFkoeWK4lyagPt6absScvj3vGhM1E7mAtzCGU0GVku7hko9WgpPQ6ZzqJ14UaX4Q=w2628-h1478-s-no?authuser=0)

> Berselang 14 tahun tidak menyangka akan kembali menekuni hobi “mengoprek” komputer.

Teringat di tahun 2008 saya ikut bergabung di salah satu forum lokal di dunia maya terkait teknologi informasi yaitu Oprek PC. Tidak sengaja membaca sebuah topik di mana membahas tentang Hackintosh, metode instalasi Mac OS milik Apple di komputer berbasis Intel X86 dan X64.

Tertarik mendalami, memasuki semester 2 tahun 2009 akhirnya membulatkan tekad membeli DVD installernya dengan versi Mac OS X 10.5.2 Leopard di Kaskus yang di mana penjualnya adalah juga salah satu pegiat di forum Oprek PC.

Distro atau pengembangnya dari installernya cukup bervarian. Kalyway, iDeneb, JaS, dan iAtkos. Namun saat itu menurut mayoritas Kalyway adalah distro yang menyediakan Kext (Kernel Extension) paling lengkap. Sedangkan JaS seingat saya maksimal di versi Mac OS X 10.4.8 Tiger.

Kalau tidak salah saya membeli DVD tersebut dengan harga Rp 75.000,- dan transfer via BRI. Seminggu kemudian paket pos sudah sampai. Karena kebetulan kakak saya telah selesai magang dan sibuk menyusun skripsi menggunakan ASP.NET di laptopnya, maka PC di rumah dengan leluasa saya gunakan sebagai tempat percobaan.

![](https://lh3.googleusercontent.com/pw/AP1GczO3FhKjPG1xZ9y2ROB6hCn2hQyFiFccDMfn4IOUk15SXp4Yg_5-_reZ95saIFjRPB_GYnR1ksciEUpDiZdzrnkB3AQPW0bfqrCJ4UM-MbYHz5DEW-p6c9QCCp_qUWH05OBBCzxADjgDyJv1Ipv_3SPVBQ=w720-h540-s-no?authuser=0)
Kalyway 10.5.2 Hackintosh Pertama

PC yang saya gunakan saat itu dibeli dan dirakit sendiri oleh kakak pada tahun 2007, karena kebetulan kuliah di bidang elektronika maka lebih sering menggunakan sistem berbasis Windows dengan spesifikasi yang dapat dibilang mid-end pada jamannya. Berikut spesifikasi dari PC tersebut :

- Motherboard : Abit LG-95
- Processor : Intel Core 2 Duo 2.2GHz
- Memory : DDR2 2GB (2x1GB)
- Harddisk : SATA 80 GB 5400RPM
- GPU : GeForce 8400GS 256 MB
- Ethernet : Realtek 8169
- Soundcard : Realtek AC97
- Dual Boot : No (clean install)

Singkat cerita akhirnya berhasil menginstal Mac OS X berbekal panduan dengan distro Kalyway 10.5.2 dan dapat diupdate secara official Apple menggunakan combo update hingga versi 10.5.7. Setelah bosan juga berhasil update ke OS X versi selanjutnya yaitu 10.6.2 dengan distro Hazard dan secara official Apple berhasil update hingga versi 10.6.7.

PC Hackintosh Core 2 Duo berserta Mac PPC G4 sangat berjasa menghantarkan saya untuk meraih gelar sarjana dan dipensiunkan tepat ketika memasuki dunia kerja. Singkat cerita setelah bekerja saya memutuskan mengambil paket dari kantor yaitu MacBook Pro MD101 dan iPhone 5 di awal tahun 2013 sehingga semenjak itu tidak lagi menyentuh Hackintosh.

![](https://lh3.googleusercontent.com/pw/AP1GczMYDmmJHKMkf64SIdSc0pi0IpYwqz92pAkePfKn-4UhvkDSy4VVytTMzEicK3i0yIAx7BdNnwQH7pS7Qx8eefY8w2RxKOYhXGYOS6W_EcbkQ8XqbJMWpP483WZias71lsmV60RkHuUuL7W29k-8LbVDGg=w720-h540-s-no?authuser=0)
Hazard 10.6.2 dengan Basillisk VM Running System 7.5

Apple sangat identik dengan presisi dan optimal. Entah kenapa selalu terasa pas dan “ngacir”. Begitu juga ketika saya menggunakan Hackintosh, meskipun penuh dengan keterbatasan pada perangkat keras tetapi entah kenapa terasa sangat halus performanya ketika menggunakan Mac OS X ketimbang Windows.

Hardwarenya sendiri sudah pasti sangat tahan badan seperti misalnya MD101 saya dapat bertahan hampir 9 tahun dan rusaknya akibat salah menyimpan di laci yang lembab (layar berjamur). Sedangkan MacBook Air M1 saya meskipun hanya memiliki memori 8GB namun tidak terdapat kendala ketika membangun aplikasi berbasis Java dan Go.

Tidak mau terlarut dengan nostalgia, saya sangat beruntung bekerja di Telkomsel karena selalu berhati baik kepada karyawannya. Saya kelimpahan hibah notebook HP Elitebook 830 G7 dari tahun lalu lengkap dengan sistem operasi Windows 10.

Setelah bergumul dengan Windows 10 yang juga sebetulnya sudah update ke Windows 11, saya merasa UNIX tetap adalah sistem operasi terbaik. Awalnya saya justru ingin install Pop!_OS karena cukup ringan, namun teringat Hackintosh jadi berubah pikiran.

Rasanya seperti kembali ke dasar, ternyata tidak ada lagi cara menginstall dengan distro ala Hazard, Kalyway, dan kawan-kawannya yang tinggal klik-klik dan kustom di belakang. Ada 2 model bootloader untuk instalasi dan operasi macOS jaman now yaitu Clover dan OpenCore dengan vanilla installer macOS.

Berhubung di Kaskus sudah jarang ada pegiat akhirnya saya pindah ke Reddit, group Telegram Hackintosh Lover (Indonesia), dan thread GitHub OpenCore. Di ketiga tempat tersebut saya mendapatkan banyak ilham dan pencerahan, senang sekali bertemu dengan para pegiat Hackintosh.

> Untuk instalasi saya diarahkan untuk mengikuti [panduan](https://dortania.github.io/OpenCore-Install-Guide/prerequisites.html) dari Dortania dan ternyata tidak terlalu sulit.

Berbekal internet Orbit sekaligus menghabiskan kuota Halo yang sudah mepet ke akhir bulan, saya bersiap untuk awal mencoba. Saya menganggap notebook HP ini adalah setara dengan MacBook Pro keluaran 2020 yang jika tidak salah defaultnya adalah Big Sur sehingga saya memutuskan nantinya saya akan menginstallnya dengan sistem operasi tersebut.

Dengan mengikuti panduan yang berbasis genre notebook beserta generasi prosesornya yaitu Comet Lake akhirnya saya berhasil menginstall macOS Big Sur meskipun dengan teknik tambal sulam via OpenCore. Untuk spesifikasi notebook:
```
HP Elitebook 830 G7
Processor : Intel i5-10210U 1.6GHz
Memory : DDR4 16GB (2x8GB), upgrade dari awalnya hanya 1x8GB
Storage : SSD 512GB nVME
GPU : Intel UHD Graphics 620
Jaringan : Intel AX201 untuk Wifi dan Bluetooth
Sound : Intel SST dan Realtek ALC285
Dual Boot : No (clean install)
Metode : Dortania
Bootloader : OpenCore 0.8.9
```

Setelah saya pertimbangkan dengan matang akhirnya memutuskan untuk hijrah tetap bersama Big Sur dan belum tertarik update ke Monterey ataupun Ventura.

Menurut pengamatan saya pada beberapa sistem operasi macOS terakhir secara berturut-turut Apple support secara penuh yaitu 3 tahun semenjak pertama kali diluncurkan. Sehingga prediksi di akhir tahun 2023 maka adalah masa akhir dari Big Sur.

Semua hal yang berfungsi dan tidak berfungsi telah saya lampirkan pada repositori, namun sebagai catatan utama turut kembali dicantumkan pada tulisan ini.
Fitur Berfungsi:

| **#**  | **Status** | **Part**                               |
|--------|------------|----------------------------------------|
| **1**  | 🆗         | Intel UHD QE/CI dengan Graphic Control |
| **2**  | 🆗         | Speaker Suara dengan Sound Control     |
| **3**  | 🆗         | Wi-Fi                                  |
| **4**  | 🆗         | Bluetooth                              |
| **5**  | 🆗         | Webcam                                 |
| **6**  | 🆗         | Backlight Brightness and Nightshift    |
| **7**  | 🆗         | Keyboard, Trackpad, and Gestures       |
| **8**  | 🆗         | 2 USB 3.0                              |
| **9**  | 🆗         | 2 USB-C Thunderbolt                    |
| **10** | 🆗         | HDMI dan Audio                         |
| **11** | 🆗         | External display dan audio with USB-C  |
| **12** | 🆗         | Status Baterai                         |
| **13** | 🆗         | Touchscreen                            |
| **14** | 🆗         | Shutdown, Restart, dan Sleep           |

Fitur Belum Berfungsi
```
⛔️ Internal Microphone (Permasalahan Intel SST)
⛔️ Airdrop (aktif namun tidak dapat mendeteksi serta terdeteksi perangkat lain untuk mengirimkan dan juga menerima berkas)
⛔️ Wake dari Sleep
```

Sejauh ini masih merasa nyaman karena kebutuhan pokok dapat diakomodir oleh Hackintosh serta belum ada kendala. Internal mic tidak menjadi masalah karena biasa menggunakan AirPod Pro dan Airdrop tidak digunakan karena menggunakan aplikasi SendAnywhere yang jauh lebih fleksibel.

Tetapi agak bingung juga ketika meeting dengan keluarga ataupun conference call dengan kolega di luar kantor karena internal mic tidak berfungsi, tidak mungkin jika harus sharing AirPod bersama isteri atau rekan kantor. Tampaknya apabila untuk keperluan pribadi atau conference call saya merasa lebih baik menggunakan MacBook Air M1.

> Untuk repositori dapat mengunjungi tautan [berikut](https://github.com/nandcep/hackintosh-opencore-efi-hp-elitebook-830-g7).

Apabila memiliki perangkat yang sama maka dapat mengunduh EFI dari repositori saya dan copy folder EFInya ke dalam partisi EFI, jangan lupa untuk generate serial number di SMBIOS pada config.plist sebelum memulai. Bootloader telah saya hias sedemikan rupa sehingga tidak terlalu terasa console di mata.

![](https://raw.githubusercontent.com/nandcep/hackintosh-opencore-efi-hp-elitebook-830-g7/refs/heads/master/banner.jpg)
Sukses Install Big Sur di HP Elitebook 830 G7