---
date: 2014-03-02T00:01:00+07:00
lastmod: 2014-03-02T00:01:30+07:00
authors: ["nandra"]
title: Koneksi ASP Classic dengan Database MySQL (ODBC)
slug: koneksi-asp-classic-dengan-database-mysql-odbc
tags: [programming]
---

ASP Classic merupakan bahasa scripting yang menjadi trend pada tahun 1998–2000an. Pengembangnya sudah langka di era sekarang. Bahasanya cukup unik yaitu VBS dan JScript, tampak seperti OOP. Saya menggunakannya saat kuliah karena diminta maintain serta tuning website kampus yang berbasis ASP Classic.

Sejak tahun 2010 sudah full migrasi ke PHP CodeIgniter akibat dihack dengan SQL Injection hingga mengakibatkan data loss. Sedikit defensive, sebelumnya sudah ada wacana untuk menggunakan ORM atau minimal prepared statement tetapi karena keterbatasan waktu serta tenaga akhirnya dibiarkan. Dan justru berakhir fatal.

Pada tulisan ini saya ingin menuliskan tentang bagaimana cara melakukan koneksi ke sebuah database dengan ASP Classic. Umumnya bahasa pemrograman VBS didukung koneksi native oleh MS SQL Server sedangkan untuk ke database lain seingat saya harus menggunakan protokol ODBC (Open Database Connectivity).

Saya sebelumnya tidak pernah menggunakan ODBC, tetapi karena tertarik untuk scripting service berbasis Classic ASP dengan database MySQL maka membutuhkan ODBC. Untuk enable protokol dapat dikonfigurasi berdasarkan sistem operasi. Berikut adalah script koneksi dengan protokol ODBC agar terhubung dengan MySQL.

```ASP
<%
    Dim odbcStr
    odbcStr = "DRIVER={MySQL ODBC 3.51 Driver}; SERVER=$your_host; DATABASE=$your_schema; UID=$your_username;PASSWORD=$your_password; OPTION=3"

    Dim dbOdbc
    Set dbOdbc = Server.CreateObject("ADODB.Connection")
    dbOdbc.Open(odbcStr)
%>
```

Beberapa parameter yang harus disesuaikan adalah sebagai berikut:

1. Host database, silakan ganti variabel `$your_host` dengan alamat host
2. Skema database, silakan ganti variabel `$your_schema` dengan nama skema
3. User database, silakan ganti variabel `$your_username` dengan username
4. Password database, silakan ganti variabel `$your_password` dengan kata sandi

Bagaimana dengan portnya? Karena menggunakan ODBC maka slot port sudah dibook berdasarkan DSN pada server sehingga tidak perlu didefinisikan. Untuk penggunaannya dapat simpan script di atas sebagai VBS dan include pada file VBS yang dipakai untuk aktivitas CRUD.

Lebih baik dilakukan open koneksi sekali saja dengan pola singleton dan sisanya bermain pool agar meminimalkan aktivitas open close yang kurang bijak. Meski saya sendiri tidak yakin apakah teknologi ADO di VBS menggunakan versi yang sama dengan ADO versi .NET di mana telah menerapkan pooling. Khawatirnya berbeda COM.

Semoga bermanfaat dan terimakasih.

Disadur dari blog lama saya di *WordPress.com*