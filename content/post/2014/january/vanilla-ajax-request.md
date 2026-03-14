---
date: 2014-01-04T00:01:00+07:00
lastmod: 2014-01-04T00:01:30+07:00
authors: ["nandra"]
title: Vanilla AJAX Request
slug: vanilla-ajax-request
tags: [programming]
---

Biasanya agar praktis dalam melakukan AJAX request ke controller saya selalu memanfaatkan jQuery lewat fungsi `$.ajax`. Sebetulnya tanpa jQuery saya dapat memanfaatkan fungsi dari engine browser yaitu `XMLHttpRequest`.

> `$.ajax` sebetulnya adalah wrapper dari `XMLHttpRequest` untuk melakukan request secara background ke server dengan HTTP method tertentu untuk mendapatkan response
> 

Berikut contoh cara request ke suatu file untuk mendapatkan responsenya. Kita anggap file tersebut sebagai response dari controller. File yang digunakan adalah text file yang berisikan string “_ini dari file…_” dengan nama test.txt yang sejajar dengan file html.

Selanjutnya adalah baris kode yang memanggil file tersebut. Namun karena menggunakan fungsi bawaan maka terlihat lebih panjang baris kodenya:
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

Untuk mendapatkan data, saya perlu mendefinisikan HTTP method serta URL yang dituju. Pada contoh saya menggunakan POST dan test.txt, dan dapat disesuaikan dengan spesifikasi. 

Untuk responsenya karena data yang direquest berisikan text atau string bukan dalam bentuk JSON, maka dapat menggunakan function `responseText` secara langsung sebagai return value.

Di jQuery saya biasa memanfaatkan event on “success” dan “error” sedangkan dengan cara *oldschool* harus dengan fungsi lambda terhadap `onreadystatechange` sebagai callback. Jika response statenya bukan bernilai 4 biasanya adalah gagal. 

Untuk kustom terkait set header yang biasanya adalah standar untuk memanggil secara RESTful juga dapat dilakukan dengan fungsi berikut

```jsx
XMLHttpRequest.setRequestHehader(header, value)
```

Memang penggunaan vanila jarang terlihat karena sudah banyak framework Javscript yang bertebaran. Namun biasanya website dan browser menjadi lebih berat padahal hanya untuk task yang simple karena ukurannya yang lebih berat dari vanilla.

Referensi lengkap dapat dilihat pada dokumentasi [berikut](https://docs.w3cub.com/dom/xmlhttprequest). Terimakasih semoga bermanfaat.

Disadur dari blog lama saya di *WordPress.com*