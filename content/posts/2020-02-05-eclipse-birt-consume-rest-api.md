---
title: Eclipse BIRT Consume REST API
date: 2020-02-05 00:00:01
categories: []
tags: [pemrograman,business-intelligence]     # TAG names should always be lowercase
---

Pada artikel ini saya ingin membahas tentang bagaimana BIRT (Business Intelligence Reporting Tools) sebuah platform open source untuk reporting dari Eclipse dapat memanfaatkan JSON dari REST API sebagai sumber data. Untuk lebih mengenal BIRT dapat mengunjungi halaman [ini](https://eclipse-birt.github.io/birt-website/) dan [ini](https://projects.eclipse.org/projects/technology.birt).

Sebenarnya kenapa memilih BIRT adalah suatu pertimbangan yang relatif dan berikut adalah beberapa dari alasan tersebut:

1. BIRT merupakan platform yang dapat berdiri sendiri alias stand-alone.
2. Memiliki viewer yang web-native dengan teknologi Java dan dapat membuka file report tanpa perlu compile seperti Jasper.
3. Web viewer ini dapat menjadi solusi untuk memiliki feature atau komponen yang loose coupling sehingga setidaknya memiliki rasa “Microservice”
4. Secara koding tidak terlalu banyak menulis kode di sisi backend dan tidak dari nol karena dapat menggunakan berbagai macam data source yang sudah tersedia secara out of the box.
5. The most thing that I love about the report design is XML based.

Karena saat ini banyak aplikasi yang sedang hype untuk penggunaan REST API, muncul rasa penasaran

> Apakah BIRT dapat menggunakan JSON dari REST API sebagai data source seperti yang saya sampaikan di awal artikel?

Secara eksplisit pada website Eclipse BIRT mungkin tidak dijelaskan mendetail tentang kapabilitas ini, namun ternyata BIRT memiliki kemampuan untuk mengakomodirnya yaitu dengan komponen Scripted Data Source.

# Development

Untuk studi kasusnya akan menghasilkan report yang menampilkan data dari sebuah REST API dengan URL (akses menggunakan metode POST dengan security token tentunya) :

```
[POST] http://localhost:1081/api/sample/v1/get-revenue
```

Data yang diperoleh dari REST URL tersebut berbentuk JSON seperti di bawah ini:

```json
{
 data : [
  {id : 1, customer: 'PT Brantas', revenue : '18500000000.00'},
  {id : 2, customer: 'PT Ardhaya Bima', revenue : '22305000000.00'},
  {id : 3, customer: 'CV Satelit Inovasi', revenue : '1530000105.00'},
  {id : 4, customer: 'PT Indo Persada', revenue : '71200002400.00'},
  {id : 5, customer: 'PT Malika Sari', revenue : '25100300.00'},
  {id : 6, customer: 'CV Rempoa Kholis', revenue : '1400502.00'}
 ]
}
```

# Report Parameter

Untuk memulainya kita dapat mengunduh designer Eclipse BIRT dengan versi terbaru, lalu silakan membuat project report dan sebuah blank report. Mari ikuti langkah dan penjelasannya sebagai berikut :

- Buka Report Design Perspective.
- Demi keamanan API yang akan diakses tentu membutuhkan token autentikasi.
- Pada Bagian Report Parameters klik kanan pilih New Parameter dan set dengan tipe data String beri nama token.
- Value parameter ini nanti akan diberikan melalui query string.

![](https://lh3.googleusercontent.com/pw/AP1GczNnNe-NmogU1p-G83WU0Bqf5Wr73epbHPAdUmUqh0FimJ7amTa54GX8EPy24MCIjkJuuP2hwaY9LAhwJAvofhylE-FaFVQAtczr0kq4sX426nrqGze8v1C0yhBL84S5QU_VXwytI96xLFfD8lY7hXiBDA=w700-h369-s-no-gm?authuser=0)

# Report Data Query

- Pada bagian Data Explorer pilih Data Sources klik kanan pilih New Data Source.
- Pilih Scripted Data Source lalu silakan diberi nama serta tekan tombol Finish.

![](https://lh3.googleusercontent.com/pw/AP1GczOjFe5Zxl4OPC7WtRQnLGEjMOrkr9b5EoG7YzTCBIes887OqBbnwzmk4timx26lYaFy-q9IDFLxS3e4AxAPPxh-0gg-VY39NBehBwu0L_5oVkw347dMz3yWPCI0QzhQlo29euG1KTdeL-JW_9ZVyEbUsQ=w700-h422-s-no-gm?authuser=0)

Sebelum masuk ke tahap selanjutnya, apa yang sebetulnya dimaksud dengan Data Sources? Dan apa bedanya dengan Data Sets? Data Sources adalah sumber data dari sebuah report, dapat berupa JDBC Data Source, MongoDB, POJO, Web Services, dan lain sebagainya.

Sedangkan Data Sets adalah tabel-tabel atau entitas object yang ada di setiap Data Sources. Kita pun dapat melakukan join antar Data Sets yang berbeda Data Sources.

- Pada bagian Data Explorer pilih Data Sets klik kanan pilih New Data Set. Pastikan value dari field Data Set Type adalah Scripted Data Set.
- Silakan diberi nama serta tekan tombol Finish.

![](https://lh3.googleusercontent.com/pw/AP1GczNanpm9t7aXTsatR_8SJYxDcYTskGVWyv1HHT-KzbOtKeODSg1uIkjxGXC1PBIWnF3yUV6c0LEIkEFXP3a3Y_OR5G1M9deuK8dp61uqjKP6vYEijxaqOge713U3lh1dIs61HlY0Rygv3NdAknyhBd0EYw=w700-h382-s-no-gm?authuser=0)

- Selanjutnya kita membutuhkan variable dari entitas JSON yang akan diterima sebagai kolom. Buka dialog dari Data Set yang baru saja dibuat pada bagian Output Columns tambahkan seperti berikut.

![](https://lh3.googleusercontent.com/pw/AP1GczPQuPRtcZiSt38NpRHB13UJaGBUJLh9KOI8_q4cWCowFqZCWlSoa8DmEZR_SG9wriSyF03LqY4MsxQYeBF8PGTqJMitU0q9WYhlYh2YEJsG7Dw39bgOjf0vS5NnzQl8-7jCbo7_USENNYB-8ekkKOMrSA=w700-h341-s-no-gm?authuser=0)

Sampai dengan tahap ini, kita sudah selesai untuk menyiapkan object yang akan digunakan untuk scripting. Selanjutnya silakan pilih Data Set yang baru saja dibuat.

- Pada bagian editor pilih tab Script maka akan ada beberapa opsi tahapan siklus Data Set yang dapat kita manipulasi dengan scripting seperti _open, describe, fetch, close_, dan lain sebagainya.

# Report RestAPI Call
Sekedar informasi bahasa scripting yang kita ketik adalah Rhino, sebuah engine Javascript yang dibangun menggunakan Java. Jadi bagi yang sudah terbiasa dengan Javascript seharusnya tidak terlalu asing.

- Untuk mengeksekusi URL REST API maka silakan pilih opsi open dan silakan tambahkan baris kode sebagai berikut.

{{< gist nandcep 45de643c2a278ecbc9ceeb608193fffa >}}

- Selanjutnya adalah untuk mapping data dari JSON yang diterima dari opsi open ke bagian Output Columns, silakan pilih opsi fetch dan tambahkan baris kode sebagai berikut.

{{< gist nandcep 07f4c2a14386c6efd723c117e89299bc >}}

- Semisal kita memiliki security token sebagai berikut, kita dapat menginputkan value dari access_token tersebut sebagai default value pada parameter token.

![](https://lh3.googleusercontent.com/pw/AP1GczOGs_ZqwUq4ImrYHBuY0QLMa2_XgOqxBuOTdrhFMyWgN5xbiN0Uh6Lqp36Oor56pxGpClzNArvDO9aKduGwyH02N22xZhtb-f0ZqsKbXHWasbeVqGjvkgQUKESP501_UWIJtMojS0zXJXRYcYYf2X5opw=w700-h195-s-no-gm?authuser=0)

- Selanjutnya kita dapat mencoba melihat hasil dari script kita atau Preview Results.
- Jika benar maka akan menghasilkan result sebagai berikut dan muncul print log pada terminal Eclipse (harus eksekusi Eclipse melalui command prompt di Windows atau terminal di UNIX).

![](https://lh3.googleusercontent.com/pw/AP1GczPBSlSKBHNmWgXDM2a4uFnaaP5-izdVQ5_X8tE66byvYheQCtxftfNH4w6uXgfktKqZxG2wvG2A5I8YWpp3djcxTJHYacqXhAQ6nJpSrPcCcj_u954qQfloe9iRi83igo9PfvjsguUuVFe_3rOtYv6q5Q=w700-h340-s-no-gm?authuser=0)

![](https://lh3.googleusercontent.com/pw/AP1GczOpK6iuws-AS1l2CSCC55evg5n7ZW0DJ_RMInH2jMzS4bfed0VkkCRfNYljmsj0nguRCyGGblGdxJCEHPwbJ29678l2u6g2bKuD2FDX_fhVM6xUNQiIRQEmiwb_ZQRUcEY6F9ysQKuD2hps7tMw2YiceA=w700-h394-s-no-gm?authuser=0)

- Jika sudah tampil, maka kita dapat melanjutkan ke bagian Report Designer untuk menghasilkan laporan dalam bentuk web yang nanti dapat dieksport ke dalam beberapa format sesuai fitur BIRT.

# Report Design

Untuk design report kita dapat membuatnya dalam bentuk seperti di bawah ini cukup dengan drag and drop Data Sets yang telah dibuat jika ingin generate table, ringkas bukan?

![](https://lh3.googleusercontent.com/pw/AP1GczPMJGoNX-B7_mjKURuoUuwWq2MjqnPxrcn8opGgRnnvukmZtPY7kjsZ6vz0NjWN3a30Dp02IKe3NJwDZJQEXoonq7InsYMKZHiSxnOISPx43kZxZ-e1ebGfiztlkgyoVAClLlfHw9lt71ql0SGBCOO_yQ=w700-h348-s-no-gm?authuser=0)

# Hasil Akhir

Setelah selesai dengan design tampilan, tahap terakhir adalah untuk preview hasilnya pada browser dengan menginputkan token security. Silakan tekan tombol “View Report in Web Viewer”.

![](https://lh3.googleusercontent.com/pw/AP1GczMiA9d30bliQ8d_JRoqJIbj-kKkcl9cyXorTKOOv2o5wPppNPsL0DZBtrpKlirYn6Rb4Z0wvAY2_POXUWeGeLAe7Q83MDV7WArF6rH6dnDP_l9DeUDwJhtjhND4ueRyMdwgZkpMSKfVnJoWpKgBdahc-Q=w700-h452-s-no-gm?authuser=0)

![](https://lh3.googleusercontent.com/pw/AP1GczPdGWC0WD66XkrUK4XdiYaTZW7BPObdTgqnzDNnQtMbZgZmdH1vSDmQei04UTASQHcwMWco-tauFFv93-FrwnmKETNrQQIA9NRSc3J9D1teN8vO4dX_K1hNwPZR9Vw3DldypwjxvgM1t5UZOOKnJZkJ4A=w700-h248-s-no-gm?authuser=0)

By the way security token tersebut nantinya dapat dipassing input valuenya melalui query parameter URL seperti berikut:

```
[GET] http://birt-viewer/nama_report.rptdesign?token=[security_token_anda]
```

Untuk template report beserta source code secara keseluruhan ada pada Gist [ini](https://gist.github.com/nandcep/b5c72988c2c5e4eb87f6cbf46dd38005#file-birt_rest_api_sample-rptdesign).

Jika seandainya menginginkan improvisasi seperti format money atau fungsi-fungsi lainnya, kita dapat memanfaatkan built-in function dari BIRT tanpa perlu merubah koding dari backend atau sisi API. Cukup lengkap, praktis, dan siap digunakan.

![](https://lh3.googleusercontent.com/pw/AP1GczMb020trAydmws4TT5G_U98NsmOpwu9Dcmg8OriC1bqKqOUrh14ODguBGZgiXlrR_rWd8paNROZUaxwMcMcCjCDqS5DeVzRmfqakck1nBKLkACGAuOsXb0zIsoS0xIYUgk0g0upq-dhWOtAQl82AqzNjg=w700-h221-s-no-gm?authuser=0)

Oke sekian dari saya jika ada komentar atau diskusi mari kita berbagi ilmu. Terimakasih 😀