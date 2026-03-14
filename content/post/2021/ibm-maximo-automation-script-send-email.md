---
date: 2021-01-07T00:01:00+07:00
lastmod: 2021-01-07T00:01:30+07:00
authors: ["nandra"]
title: IBM Maximo Automation Script Send Email
slug: ibm-maximo-automation-script-send-email
tags: [programming]
---

Automation Script merupakan fitur custom development yang disediakan oleh platform IBM Maximo sejak versi 7.5 untuk mengakomodir proses yang membutuhkan scripting. Kenapa menggunakan pendekatan scripting? Tentu karena lebih fleksibel dibandingkan dengan menggunakan fitur workflow, apalagi jika orientasi kita lebih ke arah programming.

Kebetulan saya pernah diminta oleh senior consultant untuk menyediakan fitur komunikasi untuk mengirimkan pesan email dari aplikasi work order. Tulisan ini tidak membahas secara detail desain yang telah saya kerjakan karena rahasia kantor, namun saya ingin mengulas bagaimana inti teknis scripting mengirimkan pesan ke email melalui platform IBM Maximo.

Jika ada yang membutuhkan maka harapannya tidak sulit untuk melakukan pengubahan sesuai dengan kebuthan masing-masing. Seharusnya tidak terlalu ribet karena cukup dasar dengan menggunakan syntax Jython. Berikut ini adalah Gist dari GitHub saya untuk send email via Automation Script.

{{< gist nandcep 8be6d74ba348ccbb64ba86f1091e1366 >}}

Pada snippet code di atas saya memberikan 2 contoh pengiriman email, yaitu untuk single destination email address dan multi destination email address. Single email cukup menggunakan variable berupa string sedangkan multi email dengan menggunakan array of string. Untuk mengirimkan cukup menggunakan method sendEMail.

Untuk parameter diperlukan adalah variabel:

- Email tujuan
- Email pengirim
- Judul email
- Body atau pesan.

Silakan dicustom sesuai kebutuhan apabila ada yang memerlukannya, mari berbagi.