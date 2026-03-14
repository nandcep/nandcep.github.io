---
date: 2026-01-10T00:01:00+07:00
lastmod: 2026-01-10T00:01:30+07:00
authors: ["nandra"]
title: Go Pointers, Stack, and Heap Memory
slug: go-pointers-stack-and-heap-memory
tags: [arsitektur]
---

![](https://evansnicholas.github.io/assets/images/pointers.png)

Credit to https://evansnicholas.github.io/2016/11/20/experiments-in-c-pointers

Saya pernah melakukan kesalahan fatal yang umum ketika melakukan pembuatan aplikasi berbasis C dan Go, yaitu early optimization tentang penggunaan memori. Dari awal langsung terpikirkan “ini aplikasi harus efisien dalam penggunaan memorinya, gas pointers from the start”. Dan untuk C, sangat relevan karena aplikasi benar-benar hidup tanpa Garbage Collector (GC).

Saya menganggap semua data dibuat sekali dan benar-benar mutasinya dari awal cukup lewat referensi alamat memori, serba hemat alokasi. Karena device yang diberikan komputasi tidak sederhana, yaitu instrumen PLC dan SCADA dan juga EDC. Memang spesifikasinya sangat terbatas, apalagi di tahun 2012 microcontroller itu culun tidak secanggih Raspberry Pi.

Namun pemikiran itu terbawa hingga saya menjadi pengembang aplikasi berbasis Go, kalau Java jujur saya tidak pernah overthinking karena yang pertama berbasis Java Virtual Machine alias memori sudah dialokasikan sejak awal dalam satu kotak dan sudah ada exception seperti Null Pointer Exception. Sejak bertemu Go, saya kembali bertemu raw pointers. Shit.

# Salah Mindset

Memang kalau di C memang saya sering menggunakan fungsi `malloc` dan `free` atau sekedar membuatnya menjadi NULL. Dengan device yang sangat sederhana dan terbatas, membuat saya selalu berpikir bagaimana bijak dalam mengatur memori juga tidak memberatkan komputasi ketika look-up data di dalam storage.

Kesalahan saya di Go yang pertama mungkin tidak relevan bagi sebagian orang, yaitu salah mindset. Saya membuat aplikasi di atas komputer yang canggih. Server yang berlimpah memori, memiliki arsitektur terbaik dari Intel atau ARM, dan sistem operasi juga bahasa yang kekinian. Terfatal adalah saya mengabaikan bahwa Go memiliki GC, saya adalah rajanya memori.

Saya baru sadar tidak lagi banyak melakukan pengolahan data yang bentuknya mutasi dari struct inputan hingga menjadi output. Banyak juga yang sifatnya hanya numpang lewat atau hanya sebagai informasi dan justru tidak perlu mutation untuk biarlah dikloning. **Semakin banyak pointer bertebaran dan deep-in ke function lain akan semakin lambat GC membersihkannya**.

# Bikin Kabur Memory

Kesalahan saya di Go yang kedua sering dibahas pada video Youtube dan juga komunitas pemrograman. Baik di dalam bahasa apapun entah itu C atau Go, memori untuk aplikasi ada 2 tempat yaitu Heap dan Stack. Variable dan parameter itu unik dan punya karakteristik yang dapat diprediksi.

Kalau pointers itu dianalisis selesai dalam satu fungsi maka akan masuk ke Stack, namun jika pointers itu dilempar-lempar ke variable lain dan sifatnya global atau di luar fungsi maka ditempatkan di Heap. Alokasi ke stack akan lebih cepat secara penggunaan dan berdampak ke performa. Sedangkan alokasi ke Heap tidak akan secepat Stack karena membutuhkan peran GC.

Variable yang dipassing oleh saya dari controller ke business logic lalu menyebrang ke storage selalu pointers. Membuat alokasi kabur ke Heap dan membebani GC. Ditambah di arsitektur komputer 64-bit, 1 pointer berukuran 8 byte untuk data apapun. Jadi passing pointers untuk tipe data sederhana yang isinya kecil namun masif malah membuat boros dan berat.

# Hobi Insecure

NULL atau Nil adalah teman, namun tidak bagi saya. Saya selalu paranoid dengan data yang bersifat NULL atau Nil karena dapat membuat crash akibat unhandled data behavior. Sehingga saya terlalu obsesif semua harus dipointer-kan agar dapat dicek keberadaannya. Sehingga membuat kode menjadi rumit.

Intinya saya menjadi suka insecure apabila ada variable berseliweran namun tidak dicek keberadaannya. Ditambah developer dari vendor tidak menambahkan pointers yang akhirnya membuat aplikasi sering crash. Namun saya menjadi sadar saya terlalu polos melihat Go sebagai bahasa primitif, ternyata memang pengalaman saya kurang. Go sangat lengkap.

Di Go sudah menyediakan beberapa Struct yang dapat digunakan untuk hal sepele mengecek variable kosong atau tidak. Katakanlah sql.NullString, sql.NullInt64, atau sql.NullFloat64. Dengan memanfaatkan beberapa beberapa pustaka lain untuk value check ternyata membuat efektivitas memori menjadi lebih baik dan level paranoid saya menurun.

# Kesimpulan

Dari ketiga kesalahan tersebut saya belajar bahwa pengalaman Go saya masih kurang dan harus mengulang dari fundamental agar lebih memahami Go dengan benar. Semua kesalahan tersebut terjadi di awal-awal saya develop aplikasi, memang sebagai proses perbaikannya perlu waktu dan bertahap bahkan iteratif untuk melakukan fine tuning.

Solusi yang akhirnya saya lakukan adalah merubah kebiasaan dan mindset serta upgrade skill:

| **#** | **Solusi** |
| --- | --- |
| 1 | Mengikuti panduan dari Effective Go, sebuah dokumentasi lengkap bagaimana belajar Go yang selalu saya baca ulang khususnya apabila ada update terbaru. |
| 2 | By default selalu menggunakan direct value alias tidak lagi pointer-pointeran karena *early optimization is the root cause of evil*. |
| 3 | Saya menggunakan pointer dengan mindful, apabila memang butuh mutasi dari data ukuran besar lebih dari *n* bytes maka saya akan menggunakan pointers. |
| 4 | Penggunaan pointers sebisa mungkin untuk obyek yang global dan/atau bentuknya Singleton, namun tidak semuanya tergantung dari analisis race condition. |

Jadi tidak instan, banyak belajar dari kesalahan dengan berkali-kali deploy dengan alasan spike memori juga memberi teror ke user kalau aplikasi siap crash tiap beberapa jam hehe. Beberapa hal yang saya lakukan untuk meng-observe apakah masih ada yang terlewat:

| **#** | Observasi |
| --- | --- |
| 1 | Menggunakan Go Tools pprof (Program Profiler) untuk analisis atau memprofile CPU, memory, goroutine, dsb. Singkatnya seperti stetoskop sebelum saya build aplikasinya dan deploy harus checklist apakah semua di atas minimum threshold standar performa. |
| 2 | Menggunakan flags seperti -gcflags dan GODEBUG untuk cek escape analysis ketika source code dicompile dan cek performa Garbage Collector ketika aplikasi berjalan. |
| 3 | Membuat Benchmark dan dikombinasi dengan benchmark memory agar lebih firm secara statisik alokasi memori dengan B/op (bytes per operation) dan alloc/ops untuk cek seberapa banyak yang berjalan di Stack. |
| 4 | 3 poin di atas adalah preventive. Last but not least perlu juga langkah corrective yaitu pasang agent atau tracing di aplikasi dan juga server non-functional testing lalu hajar stress tests. |