---
title: Install BMC AR Developer Studio 9.1 di Mac
date: 2016-09-27 00:00:01
categories: []
tags: [mac,bmc-remedy]     # TAG names should always be lowercase
---

![](https://lh3.googleusercontent.com/pw/AP1GczMNgFlVxt1V8RMVGXBFzNXU6HJTdsVVtxABvHfHIdeFFXbc9UA6tDYbsEDWxPHy2G3Xs1xZAa_BiT2tOhK89Q0mPYn2mf0eD2JMChhB6kkfSFx9H4pXiA5sCys4q8dkYfoL-w54jf0a1_2s17_wnFl9nw=w1280-h800-s-no?authuser=0)

Akhirnya saya dapat install BMC AR Developer Studio di Mac OS dan menjalankannya secara native. MacBook saya terinstall OS X El-Capitan. Pada dasarnya BMC AR Developer Studio adalah sebuah IDE yang berbasiskan Eclipse. Jadi untuk menyeusaikan versi 9.1 setidaknya saya membutuhkan Eclipse dengan versi 4.5 atau dengan kode “Mars”

Ide orisinil saya mendapatkannya dari forum BMC di [sini](https://communities.bmc.com/ideas/2032). Lalu bagaimana cara saya melakukan instalasinya? Posting ini juga berfungsi sebagai backup seandainya artikel pada forum tersebut dihapus atau hilang:

1. Memiliki BMC AR Developer Studio yang telah terinstall di mesin Windows
2. Unduh Eclipse versi 4.5 untuk Mac (64 bit cocoa)
3. Extract dan rename app dari Eclipse.app misal menjadi ARDeveloperStudio.app, jangan ada spasi karena dapat membuat Eclipsenya tidak tereksekusi nantinya
4. Klik kanan pada ARDeveloperStudio.app dan akses sub-menu "Show Package Contents"
5. Masuk ke folder Contents ▶︎ Eclipse ▶︎ dropins
6. Pada BMC AR Developer Studio di mesin Windows, copy folder plugin dan features lalu paste di folder dropins pada step nomor 5
7. Pindah ke folder Contents ▶︎ Eclipse ▶︎ configuration. Buka config.ini menggunakan text editor dan modifikasi section `osgi.splashPath` seperti berikut `osgi.splashPath=platform\:/base/dropins/plugins/com.bmc.arsys.studio.ui`
8. Selesai sudah, untuk pemanis kita dapat mengganti icon launcher Contents ▶︎ Resources ▶︎ Eclipse.icns dengan icon yang kita inginkan.

Berikut adalah screenshot BMC AR Developer Studio di MacBook saya

![](https://lh3.googleusercontent.com/pw/AP1GczPBjWWkx3WrpFwQoDkqYGOiilUUI4j7DUEuDR1nHoadIRNoBEEJnADOWkAK10srQCMq3yIGwp_sFbUYKI_1B60R-8AC9jCQyoQ3TkRjNtfSdxQ2H0nojq6HFQ21TWCDE5OZbTMlJYRUtGtu-T15SsRs4Q=w1280-h800-s-no?authuser=0)

Credit to Andres, thanks for your feedback

> You would need to run the eclipse executable with the -clean option on the first run. it will cleanup and reload all plugins. For Mac, go inside the eclipse package and run the eclipse executable (at the `Contents/MacOS` directory) from a Terminal window

_Disadur dari blog lama saya di WordPress.com_