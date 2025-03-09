---
title: Koneksi ASP Classic dengan Database MySQL (ODBC)
date: 2014-03-02 00:00:01
categories: []
tags: [asp,pemrograman]     # TAG names should always be lowercase
---

![](https://images.unsplash.com/photo-1475770230762-6409e81d7589?q=80&w=2832&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D)
Credit to Unsplash

ASP Classic merupakan bahasa scripting yang menjadi trend pada tahun 1998–2000an. Aplikasi dan pengembangnya sudah langka untuk ditemukan di era sekarang. Bahasanya cukup unik yaitu VBS dan JScript dan tampak seperti OOP. Saya sendiri menggunakannya ketika masih kuliah karena sempat diminta untuk memaintain serta tuning website universitas yang berbasis ASP Classic.

Sejak tahun 2010 sudah full migrasi ke PHP CodeIgniter akibat dihack dengan SQL Injection hingga mengakibatkan data loss. Sedikit defensive, sebelumnya sudah ada wacana untuk menggunakan ORM atau minimal prepared statement tetapi karena keterbatasan waktu serta tenaga akhirnya dibiarkan. Dan justru berakhir fatal.

Pada tulisan ini saya ingin menuliskan tentang bagaimana cara melakukan koneksi ke sebuah database dengan ASP Classic. Umumnya bahasa pemrograman VBS didukung koneksi native oleh MS SQL Server sedangkan untuk ke database lain seingat saya harus menggunakan protokol ODBC (Open Database Connectivity).

Saya sendiri hampir tidak pernah menggunakan protokol tersebut, tetapi berhubung tertarik untuk membuat scripting service berbasis Classic ASP dengan database yang universal maka membutuhkan ODBC. Untuk enable ODBC dapat dikonfigurasi terlebih dahulu berdasarkan sistem operasi. Berikut adalah script koneksi menggunakan protokol ODBC agar dapat terhubung dengan database MySQL.

```c#
<% 
    Dim odbcStr
    odbcStr = "DRIVER={MySQL ODBC 3.51 Driver}; SERVER=$your_host; DATABASE=$your_schema; UID=$your_username;PASSWORD=$your_password; OPTION=3" 
    
    Dim dbOdbc 
    Set dbOdbc = Server.CreateObject("ADODB.Connection") 
    dbOdbc.Open(odbcStr) 
%>
```

Beberapa parameter yang harus disesuaikan adalah sebagai berikut:

1. Host database, silakan ganti variabel `$your_host` dengan alamat host
2. Skema database, silakan ganti variabel `$your_schema` dengan nama skema
3. User database, silakan ganti variabel `$your_username` dengan username
4. Password database, silakan ganti variabel `$your_password` dengan kata sandi

Bagaimana dengan portnya? Karena menggunakan ODBC maka slot port sudah dibook berdasarkan DSN pada server sehingga tidak perlu didefinisikan. Untuk penggunaannya dapat simpan script di atas sebagai VBS dan include pada file VBS yang dipakai untuk aktivitas CRUD. 

Lebih baik dilakukan open koneksi sekali saja dengan pola singleton dan sisanya bermain pool agar meminimalkan aktivitas open close yang kurang bijak. Meski saya sendiri tidak yakin apakah teknologi ADO di VBS menggunakan versi yang sama dengan ADO versi .NET di mana telah menerapkan pooling. Khawatirnya berbeda COM.

Semoga bermanfaat dan terimakasih.

Disadur dari blog lama saya di _WordPress.com_