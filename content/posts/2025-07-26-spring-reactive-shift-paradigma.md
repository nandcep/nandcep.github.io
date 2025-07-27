---
title: Spring Reactive, Shift Paradigma
date: 2025-07-26 00:00:01
categories: []
tags: [spring,java,pemrograman]
---

Sudah lama tidak mengisi post akhirnya menemukan topik yang ingin saya tulis di sini. Kurang lebih 3 bulan belakangan berkutat dengan teknologi yang menurut saya powerful dan seru yaitu Spring Reactive. Powerful karena meningkatkan produktivitas pengembangan aplikasi, seru karena hal baru yang merubah paradigma serta pola pikir. Kebetulan memang saya adalah tipikal yang cepat bosan sehingga mencoba ekplorasi sekalian upgrade pengalaman. Intro tentang reaktif dan non-blocking dapat dibaca pada post [berikut](https://nandcep.github.io/posts/2022-12-23-reactive-system-in-a-nutshell/).

Spring dikenal sebagai framework yang sangat lengkap namun syarat dengan konsumsi thread yang cukup tinggi. Khususnya saat berurusan dengan peak request terhadap koneksi protocol seperti request HTTP, koneksi database, koneksi redis, dan filesystem. Dikarenakan hal tersebut maka memiliki potensi resiko bottleneck terhadap thread yang dapat mengakibatkan menurunnya resiliensi dari fungsi aplikasi. 
Sebagai solusinya, non-blocking I/O pada perangkat keras dan sistem operasi modern akan meningkatkan resiliensi. 

//kasih gambar thread bottleneck

Namun pada tulisan berikut saya tidak akan membahas serta membuktikan peningkatan performa terhadap aplikasi. Saya akan membahas pengalaman bagaimana penulisan secara reaktif merubah pola pikir dari yang sebelumnya imperatif menjadi reaktif sehingga mendapatkan benefit penulisan kode yang lebih efisien. Biarlah peningkatan performa serta resiliensi menjadi bonusnya karena perlu benchmark lebih lanjut dengan load serta performance testing.

Dengan pembuatan aplikasi secara reaktif memungkinkan logika kode diatur oleh urutan yang memiliki prioritas dan akan diatur oleh I/O sistem operasi. Dengan mengandalkan Spring Reactive, urutan tersebut akan diatur oleh CPU dan eksekusinya akan diserahkan oleh I/O sistem operasi dan dicek secara berkala oleh event loop sehingga pengembang harus memahami terkait data flow dengan concurrency. Dari sini dapat terdapat kata kunci yaitu reactive adalah asynchronous sedangan imperatif adalah synchronous. 

Perintah imperatif semua fungsi akan diolah CPU dan menunggu I/O untuk menyelesaikan tugasnya secara tegak lurus sehingga ketika melebihi thresholdnya akan membuat thread baru untuk lanjut eksekusi, berbeda dengan reaktif semua fungsi akan dibagikan oleh CPU secara independen kepada I/O sehingga memaksimalkan single thread dan meminimalkan pembuatan thread baru. Dengan memahami sifat dasarnya maka akan mempermudah ketika mentransformasi pola pikir imperatif ke reaktif. 

Hal yang menjadi tantangan dari imperatif menuju reaktif adalah tentang kebiasaan dalam pengetikan kode, jadi untuk membantu membentuk kebiasaan yang baru harus dimulai dengan perkenalan.

# Spring Reactive 

Spring Reactive adalah pendekatan agar pengembang dapat membangun aplikasi secara non-blocking I/O, Standar Spring Reactive selaras dengan JSR-365 untuk spesifikasi async event dan reactive RestAPI. Untuk menerapkan Spring Reactive sangat berbeda dengan Spring konvensional dikarenakan semua komponen Spring konvensional tidak memiliki interopabilitas di Spring Reactive. Dengan demikian pengembang harus memilih komponen yang by default adalah reaktif. 

Komponen utama untuk membangun solusi yang reaktif adalah dengan Project Reactor. Di dalam Project Reactor terdapat Spring WebFlux yang menjadi pustaka utama untuk membangun aplikasi Springboot, Spring Data Redis Reactive sebagai pengganti Spring Data Redis, Spring Data Reactive sebagai pengganti Spring Data, dan juga pustaka lainnya yang berubah menjadi memiliki sifat reaktif. Sehingga Spring Reactive adalah sebuah set pustaka yang berbeda dari Spring konvensional.

Dikarenakan terdapat pustaka khusus dan berbasis JSR-365 maka di dalam penggunaan Spring Reactive memiliki perbedaan signifikan secara sintaksis dibandingkan Spring konvensional. Deklarasi kode pada Spring Reactive berparadigma fungsional, berbeda dengan sebelumnya yaitu imperatif. Pengembang dituntut memahami data flow pada Project Reactor dengan prioritas sehingga tidak terjadi perubahan pada alur proses bisnis dan ada celah pada error handling.

Spring Reactive adalah versi lengkap dari pustaka RxJava karena meliputi dari web framework, data, dan security bahkan komponen lainnya. Dikarenakan RxJava sendiri adalah pustaka terpisah dari ekosistem Spring sehingga untuk melengkapi menjadi solusi yang benar-benar reaktif masih memerlukan pustaka lainnya yang mendukung. Hal saya jabarkan dikarenakan memang untuk memaksimalkan pemahaman tentang Spring Reactive adalah dengan mengoptimalkan penggunaan Spring Reactive itu sendiri yang sudah lengkap.

# Usecase 

Ketika membangun aplikasi yang reaktif pengembang harus dapat mengidentifikasi proses yang akan ditempatkan pada baris kode dengan tantangan concurrency serta asynchronous. Pada paragraf sebelumnya juga disebutkan data flow dan reaktif, sehingga kedua hal tersebut saya pecah menjadi 3 usecase yang harus diselesaikan oleh pengembang agar dapat membuat prosesnya tetap sesuai alur. Berikut adalah penjabarannya.

## Current Object is a Future

Pada usecase berikut pengembang harus paham bahwa semua obyek di dalam proses reaktif termasuk Spring Reactive berasal dari masa depan secara asynchronous. Apabila saya pinjam istilah dari Javascript terdapat istilah `Promise`, di mana sebuah obyek tidak langsung available dan baru akan muncul di suatu saat. Karena ketika synchronous saat CPU harus mencari obyek yang berasal dari database melalui I/O maka berarti CPU akan alokasi thread yang standby menanti datangnya obyek sehingga menjadi blocking bagi thread.

Sedangkan untuk asynchronous ketika CPU harus mencari obyek harus berasal dari database melalui I/O maka CPU tidak memerlukan alokasi thread untuk standby, thread tersebut hanya akan aktif ketika I/O yang mencari data ke database menyatakan sekarang obyek tersebut telah tersedia dengan meberikan sinyal sehingga hal ini dapat dikatakan sebagai proses yang non-blocking bagi thread. Sebagai pengembang untuk menyelesaikan usecase ini adalah harus membuat kode dengan obyek yang availabilitasnya bersifat masa depan. 

## Concurrency Under the Hood

Concurrency atau kemampuan multi-tasking yaitu beberapa tugas dapat dijalankan secara bersamaan meskipun awalnya tugas-tugas tersebut tidak diberikan secara bersamaan. Terkadang prosesnya diberikan secara bergantian dan ketika selesai thread dan I/O dapat saling berbagi informasi tugas. Dengan proses asynchronous maka pengembang harus terbiasa dengan concurrency, dimana concurrency juga identik dengan multi-tasking tetapi juga harus memperhatikan urutan tugas-tugasnya agar tidak overlap.

Seperti yang dijelaskan pada bagian current object is a future ketika thread hanya aktif ketika I/O memberikan sinyal melalui event loop maka sembari menunggu I/O, thread dapat mengeksekusi tugas lainnya yang diberikan oleh CPU. Hal ini membuat proses concurrency menjadi lebih baik karena thread dapat lebih dioptimalkan. Sehingga di dalam usecase ini adalah pengembang jangan sampai membuat kode menjadi blocking karena tentunya akan menghambat kemampuan concurrency.

## Under Control in Data Flow

Pengembang harus memperhatikan urutan antar tugas agar tidak overlap, hal ini menjadi sangat krusial apabila terdapat dependensi antar tugas sehingga concurrency menyebabkan disrupsi transaksi. Pengembang harus memahami data flow di dalam proses concurrency pada Spring Reactive. Dengan adanya fungsi pada data flow untuk pemberian urutan berdasarkan prioritas dapat mengatur tugas agar tetap berada di dalam koridor.

Untuk lengkapnya terdapat referensi konsep dari ReactiveX yang akan sangat membantu pengembang untuk memahami. Pada usecase ini akan memberikan beberapa contoh dari data flow yang biasa saya terapkan di dalam pengembangan aplikasi untuk menyelesaikan usecase control di dalam Spring Reactive data flow.

**Flow Control**

| Control | Flow | Description |
| ------- | ---- | ----------- |
| **Just** | ![](https://raw.githubusercontent.com/nandcep/nandcep.github.io/refs/heads/master/data/posts/17535928251523970065314127146903.png) | Something |
| **Map**  | ![](https://raw.githubusercontent.com/nandcep/nandcep.github.io/refs/heads/master/data/posts/17535923110862547429684566955901.png) | Something |
| **Transform** | High | Something |
| **Flat Map** | ![](https://raw.githubusercontent.com/nandcep/nandcep.github.io/refs/heads/master/data/posts/17535976222887166834779068290578.png) | Something |
| **On Error Return** | ![](https://raw.githubusercontent.com/nandcep/nandcep.github.io/refs/heads/master/data/posts/1753597281720589442231892524589.png) | Something |
| **On Subscribe** | ![](https://raw.githubusercontent.com/nandcep/nandcep.github.io/refs/heads/master/data/posts/17535979922927815261571641627841.png) | Low |

**Example of Combination**

# Produktivitas

## Single is Mono

## Multiple is Flux

## Quality over Unit Testing

# Kesimpulan
