---
title: Vanilla AJAX Request
date: 2014-01-04 00:00:01
categories: []
tags: [javascript,pemrograman]     # TAG names should always be lowercase
---

![](https://images.unsplash.com/photo-1483546416237-76fd26bbcdd1?q=80&w=2670&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D)
Credit to Unsplash

Biasanya agar praktis dan mudah dalam melakukan AJAX request ke controller saya selalu memanfaatkan jQuery dengan fungsi andalan yaitu $.ajax. Sebetulnya tanpa jQuery pun saya dapat memanfaatkan class dan fungsi sakti dari engine browser itu sendiri untuk melakukannya, yaitu dengan XMLHttpRequest.

> $.ajax sebetulnya juga melakukan wrapper terhadap XMLHttpRequest yaitu melakukan request secara background ke server dengan HTTP method tertentu untuk mendapatkan response

Berikut adalah contoh bagaimana cara melakukan request ke suatu file lalu mendapatkan responsenya. Kita dapat anggap file tersebut adalah sebagai response dari controller. File yang digunakan adalah text file yang berisikan text string “_ini dari file…_”. Filenya saya simpan dengan nama test.txt sejajar dengan file .html yang akan discripting.

Selanjutnya adalah baris kode yang memanggil file tersebut. Namun karena menggunakan engine bawaan maka akan terlihat sedikit lebih panjang baris kodenya:

```html
<html>
    <head>
        <title>AJAX Pure Basic</title>
    </head>
    <body>
    <script type="text/javascript">
        function fnAjaxCall() { 
            var req = false;
            if(window.XMLHttpRequest) 
                req = new XMLHttpRequest(); 
            else 
                req = new ActiveXObject("Microsoft.XMLHTTP"); 
            req.open("POST", "test.txt", true); 
            req.onreadystatechange = function(){ 
                if(req.readyState == 4) 
                    document.getElementById("load").innerHTML = hasil.responseText; 
            } 
            req.send(null); 
  	}
    </script>
	<input type="button" value"Call AJAX" onclick="javascript:fnAjaxCall();">
	<span id="load"></span>
    </body>
</html>
```

Untuk mendapatkan data, saya perlu mendefinisikan HTTP method serta URL yang dituju. Pada contoh saya menggunakan POST dan test.txt, namun dapat disesuaikan dengan keadaan lapangan. Untuk responsenya karena data yang direquest berisikan text atau string bukan dalam bentuk JSON maka dapat menggunakan function responseText secara langsung sebagai return value.

Di jQuery saya biasa memanfaatkan event on “success” dan “error” sedangkan dengan cara _oldschool_ lebih suka menggunakan fungsi lambda terhadap onreadystatechange sebagai callback. Jika response statenya bukan bernilai 4 biasanya adalah gagal dan 4 adalah berhasil.

Untuk kustom terkait set header yang biasanya adalah standar untuk memanggil secara RESTful juga dapat dilakukan dengan fungsi berikut

```javascript
XMLHttpRequest.setRequestHehader(header, value)
```

Memang sebetulnya penggunaan vanila seperti ini sudah jarang terlihat karena sekarang sudah banyak framework Javscript yang bertebaran. Namun tidak ada salahnya menggunakan vanila apabila tidak ingin dibebani oleh pustaka yang membuat website dan browser menjadi lebih berat padahal hanya untuk task yang simple.

Referensi lengkap dapat dilihat pada dokumentasi [berikut](https://docs.w3cub.com/dom/xmlhttprequest). Terimakasih semoga bermanfaat.

Disadur dari blog lama saya di _WordPress.com_