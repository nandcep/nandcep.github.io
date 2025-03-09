---
title: BMC Remedy Data Import Tool di UNIX
date: 2017-06-20 00:00:01
categories: []
tags: [bmc-remedy,unix]     # TAG names should always be lowercase
---

![](https://images.unsplash.com/photo-1488229297570-58520851e868?q=80&w=2938&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D)
Credit to Unsplash

Dikarenakan di site ada kebutuhan untuk custom development pada BMC Remedy, klien saya memerlukan bulk upload dari CSV dan harus diproses oleh AR Server secara real-time. Sebetulnya di BMC Remedy secara _out of the box_ memiliki fitur data import menggunakan Pentaho Spoon. Dengan membuat job dan dieksekusi melalui script atau run manual di Pentaho.

> Sebagai alternatif lain adalah dengan menggunakan command line di Sistem Operasi lewat executable dataimport.

Sesuai kebutuhan di site mengharuskan saya untuk mengembangkan executable dataimport agar dapat tereksekusi melalui filter dengan event ketika user selesai upload file CSVnya. Jika menggunakan sistem operasi Microsoft Windows maka dapat memanfaatkan file `dataimport.bat` yang berada di `[AR System directory]\DataImportTools`.

Sedangkan jika menggunakan sistem operasi berbasis UNIX maka harus membuat Shellscript terlebih dahulu. Dikarenakan DataImportTools tidak tersedia di instalasi ARSystem untuk sistem operasi UNIX. Saya mengikuti tutorial dari link [berikut](https://docs.bmc.com/docs/display/public/ars81/Enabling+the+Data+Import+utility%22). Langkah-langkah prasyarat yang dibutuhkan terlampir:

1. Di direktori instalasi AR System buat direktori baru dengan nama DataImport.
2. Buat sebuah shellscript pada direktori baru tersebut dengan nama semisal `dataimport.sh`. 
3. Perlu diperhatikan juga hak akses eksekusi shellscriptnya, better diberikan akses "777" hanya untuk testing. 
4. Yang perlu dipastikan adalah full path dari ARSystem API Library. Kebetulan di lokal saya pathnya terdapat pada `/arapp/remedy/ARSystem/api/lib`. 
5. Pastikan di dalamnya sudah ada pustaka arapi[version]_build[xxx].jar, arapiext[version]_build[xxx].jar, dan log4j-[version].jar. Ketiga file ini wajib diketahui keberadaannya.
6. Setelah itu cek juga JDK JAVA_HOME yang digunakan oleh BMC Remedy. Di lokal saya, JDK yang digunakan di `/arapp/java/jdk1.7.0_50/`

Pada Shellscript `dataimport.sh`, berdasarkan parameter yang telah disesuaikan dengan env variables maka konfigurasi perintah adalah sebagai berikut.

```shell
#!/bin/sh
APIDROP=/arapp/remedy/ARSystem/api/lib

export APIDROP
echo apidrop=$APIDROP

JAVA_HOME=/arapp/java/jdk1.7.0_50/
export JAVA_HOME
echo javahome=$JAVA_HOME

PATH=$JAVA_HOME/bin:$PATH:$APIDROP
export PATH
echo path=$PATH

exec java -cp $APIDROP/arapi81_build001.jar:$APIDROP/log4j-1.2.14.jar:$APIDROP/arapiext81_build001.jar com.bmc.arsys.apiext.data.DataImport ${1+"$@"}
```

Dengan langkah yang singkat di atas maka sudah dapat menggunakan Data Import Remedy di UNIX. Untuk melakukan testing apakah sudah ketika mengeksekusi import data maka dibutuhkan :

1. File mapping .armx. File mapping untuk sample saya buat pada direktori `/arapp/contoh/datamap.armx`
2. Data CSV saya letakkan pada direktori `/arapp/contoh/data.csv`

Untuk menjalankan test data import adalah dengan menjalankan perintah seperti di bawah ini pada terminal atau console:

```shell
sh /arapp/remedy/ARSystem/DataImport/dataimport.sh \ 
    -x "[alamat server]" \ 
    -u "[username]" \
    -p "[password user]" \
    -a [nomor port] \
    -M "/arapp/contoh/datamap.armx" \
    -o "/arapp/contoh/data.csv"
```

Parameter alamat server, username, dan password dapat disesuaikan dengan environment BMC Remedy yang telah terinstall.

Untuk eksekusi dataimport.sh sebaiknya menggunakan full path dibandingkan relative path agar shellscript dapat diresolve dengan pasti. Saya sudah berhasil mencobanya untuk BMC Remedy versi 8.1. Untuk versi 7.6 ke atas kurang lebih sama seharusnya.

Untuk _end-to-end_ pengembangan upload CSV dan diimport otomatis ke dalam Remedy, dapat menggunakan proses attachment yang eventnya dibaca oleh filter. Result dari proses attachment dipassing sebagai argumen di shellscript.

1. Buat filter yang membaca event pada sebuah form dan field attachment
2. Result yang berupa file beserta full path pada attachment dimasukkan ke dalam variabel argumen
3. Buat aksi untuk eksekusi shellscript sebagai berikut, argumen saya lambangkan dengan `$namaVariabel$`.

```shell
sh /arapp/remedy/ARSystem/DataImport/dataimport.sh \ 
    -x "$AREnvironmentIpAddress$" \
    -u "$ARCurrentUsername$" \
    -p "{ARCurrentCredential}" \
    -a $AREnvironmentPort$ \
    -M "/arapp/contoh/datamap.armx" \
    -o "$ResultFileFullPath$"
```

Variabel seperti sesi, informasi kredensial, AR IP Address, dan AR Port dapat dibaca pada dokumentasi produk BMC. Semoga bermanfaat, terimakasih.