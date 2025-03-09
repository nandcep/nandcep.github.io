---
title: IBM Maximo Export Object to XML Automation Script
date: 2022-03-05 00:00:01
categories: []
tags: [pemrograman,ibm-maximo]     # TAG names should always be lowercase
---

![](https://images.unsplash.com/photo-1475770230762-6409e81d7589?q=80&w=2832&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D)
Credit to Unsplash

Pada posting berikut saya ingin share bagaimana cara ekspor data dari IBM Maximo menjadi file XML dengan format bebas (user defined). Hal ini didasarkan oleh ketika terdapat kebutuhan semisal ingin melakukan data loading dari Maximo ke aplikasi lain.
 
Loading data tidak dalam bentuk flat file, melainkan dalam bentuk XML yang formatnya harus ada kesepakatan antara 2 belah pihak. Kita dapat melakukannya menggunakan Automation Script.

> Sebagai contoh jika ingin share data item maka cukup memanfaatkan Automation Script dengan launch point action pada object item.

Beberapa hal yang cukup bermanfaat serta dapat kita mainkan adalah pustaka dari Java untuk parsing ke XML yaitu W3C DOM dan Java XML Parser. Pada Gist di bawah ini saya share coding Automation Script yang berisikan perintah untuk ekspor data ke file XML.

Skeleton script berikut adalah catatan silam saya ketika implementasi aplikasi EAM IBM Maximo di project kantor lama sehingga jika ada yang membutuhkan dapat disesuaikan saja dengan kebutuhan yang bersangkutan.

Penjelasan terdapat pada bagian hashtag atau komentar.

{{< gist nandcep 414530a86d7ab38e54906fe01e051c1b >}}

Ekspektasi output dari eksekusi script di atas adalah sebagai berikut:

```xml
<?xml version="1.0" encoding="UTF-8">
<Item itemnum="XXXXXXXXX">
 <Description>YYYYYYYYYY</Description>
</Item>
```

Dari sini kita ambil kesimpulan bahwa XML Parser dari pustaka Java dapat digunakan secara out of the box di IBM Maximo. Sebetulnya tidak hanya pustaka XML Parser namun semua kapabilitas pustaka pada JDK yang bertugas sebagai runtime juga dapat dimanfaatkan serta dieksekusi oleh IBM Maximo.

Mungkin akan sangat menarik serta mempermudah kita jika reactive streaming yang ada di Java 8 ke atas dapat digunakan pada scripting Automation Script. Untuk inovasi dan pengembangannya sendiri tergantung kreativitas dari masing-masing pengembangnya.

Semoga bermanfaat, salam.