---
title: Membuat Plugin WordPress Sederhana
date: 2017-04-08 00:00:01
categories: []
tags: [wordpress,pemrograman]     # TAG names should always be lowercase
---

![](https://images.unsplash.com/photo-1599305445671-ac291c95aaa9?q=80&w=2938&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D)
Credit to Unsplash

Ini adalah tulisan perdana saya tentang kustom di WordPress, hanya saja kustom yang saya bahas bukanlah kustom secara frontal yang melakukan ubah kode pada base. Namun dengan cara yang lebih halus yaitu menggunakan plugin. Kelebihannya adalah ketika ingin upgrade versi WordPress pun pastinya jadi lebih aman.

Tanpa perlu khawatir kustom kode terkena rollback atau tertimpa update. Yang kita perlukan adalah code editor, saya sendiri menggunakan Smultron atau Visual Code di MacBook jadul karena cukup ringan dan tidak perlu autocomplete. Yang penting sintaks berwarna warni di layar.

Untuk use case adalah semisal kita ingin menyisipkan sebuah link kustom CSS kita pada bagian header di WordPress. Contohnya header HTML misal seperti baris kode di bawah ini:

```html
<head>
    <!--wp original header [start]-->
    .... 
    <!--wp original header [end]-->
    <!--kustom CS link yang ingin disisipkan [start]-->
    <link rel="stylesheet" type="text/css" href="ourcssfile" />
    <!--kustom CSS link yang ingin disisipkan [end]-->
</head>
```

Dengan pendekatan kasar maka kita akan menambahkan pada header file WordPress atau mungkin pada theme yang kita gunakan. Untuk kustom pada theme menurut saya tidak efisien karena berarti harus mengubah koding pada setiap theme. Namun dengan plugin akan menjadi lebih ringan, berikut adalah bagaimana cara kita membuat dan menempatkan kustom CSS kita.

1. Buat sebuah file PHP
2. Hook action untuk link CSS seperti pada koding di bawah ini:

```php
<?php
// komentar-komentar di bawah ini nanti ditranslasi menjadi deskripsi di daftar plugin secara otomatis ketika diupload ke dalam WordPress
/**
    * @package nama_plugin 
    * @version 1.x.x
**/

/*
Plugin Name: nama_plugin
Plugin URI: url_pengembang
Description: deskripsi_plugin
Author: nama_pengembang
Version: 1.x.x
Author URI: uri_pengembang
*/
add_action( 'wp_head', 'inject_css', 1);
function inject_css(){
    echo '<link rel="stylesheet" type="text/css" href="ourcssfile" />';
}
?>
```

Beri nama file PHP tersebut misal dengan nama inject_custom_css.php. Lalu kompresi file PHPnya dengan format zip. Langkah berikutnya adalah login ke dashboard WordPress administrator dan unggah file zip yang telah dibuat pada menu Plugins. Jangan lupa aktivasi plugin dan selesai sudah.

Hanya dengan membuat sebuah fungsi dengan nama `inject_css()` dan hook ke dalam fungsi bawaan WordPress yang bernama `wp_head` dengan perintah `add_action`. Apa sebenarnya fungsi `wp_head` itu? Adalah sebuah action function di mana digunakan untuk menambahkan elemen ke dalam header HTML.

Jika butuh penjelasan lengkapnya dapat merujuk ke [docs](https://codex.wordpress.org/Function_Reference/wp_head) milik WordPress.org. Tanpa perlu ada kompilasi dan langsung diinterpreter oleh engine PHP. _On the fly_, tentu saja karena ini bukan Java. Setelah aktivasi seharusnya link CSS akan muncul pada halaman WordPress ketika diakses.