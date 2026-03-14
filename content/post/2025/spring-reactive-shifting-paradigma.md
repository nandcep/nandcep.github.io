---
date: 2025-06-26T00:01:00+07:00
lastmod: 2025-06-26T00:01:30+07:00
authors: ["nandra"]
title: Spring Reactive, Shifting Paradigma
slug: spring-reactive-shifting-paradigma
tags: [arsitektur]
---

![](https://images.unsplash.com/photo-1625315714730-d0830cd368bd?q=80&w=3132&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D)
Credit to Unsplash

Sudah hampir 3 bulan berkutat dengan teknologi yang powerful dan seru yaitu Spring Reactive. Powerful karena meningkatkan produktivitas pengembangan aplikasi, seru karena hal baru yang merubah paradigma serta pola pikir. Karena saya cepat bosan jadi mencoba pengalaman baru. By the way, intro tentang reaktif dan non-blocking dapat dibaca pada post [berikut](https://nandcep.github.io/post/2022/reactive-system-in-a-nutshell/).

Saya tidak akan membuktikan peningkatan performa terhadap aplikasi. Lebih membahas pengalaman bagaimana pemrograman reaktif merubah pola pikir dari yang sebelumnya imperatif adalah berbeda secara keseluruhan. Biarlah peningkatan performa serta resiliensi menjadi bonusnya karena perlu benchmark lebih lanjut dengan load serta performance testing. 

Dengan memahami sifat dasarnya maka akan mempermudah ketika mentransformasi pola pikir imperatif ke reaktif. Hal yang menjadi tantangan dari imperatif menuju reaktif adalah tentang kebiasaan dalam pengetikan kode, jadi untuk membantu membentuk kebiasaan yang baru harus dimulai dengan perkenalan.

# Spring Reactive

Spring Reactive adalah pendekatan agar pengembang dapat membangun aplikasi secara non-blocking I/O, standar Spring Reactive selaras dengan JSR-365 untuk spesifikasi async event dan reactive RestAPI. Jelas sangat berbeda dengan Spring konvensional dikarenakan semua komponen Spring konvensional tidak memiliki interopabilitas di Spring Reactive.

Berikut adalah table untuk contoh pustaka pengganti dari Spring konvensional ke reaktif.

| Spring Konvensional | Spring Reactive | Deskripsi |
| --- | --- | --- |
| Spring MVC | Spring WebFlux | Pustaka yang berubah untuk web stack termasuk dari web server yang sebelumnya Tomcat by default menjadi Netty. |
| Spring Data Redis | Spring Data Redis Reactive | Pustaka yang berubah untuk manajamen data pada Redis. |
| Spring Data | Spring Data Reactive | Pustaka yang berubah untuk manajemen data dari sebelumnya JPA menjadi R2DBC. |
| Spring Rest Template | Spring WebClient | Pustaka yang berubah untuk pemanggilan atau request HTTP dari web service. |

Dari table tersebut dapat dipahami Spring Reactive adalah sebuah set pustaka yang berbeda dari Spring konvensional.

Perbedaan secara reaktif juga berbeda secara signifikan secara sintaksis. Deklarasi kode secara reaktif sangat fungsional dengan ekspresi lambda dalam mengolah data, di mana saya dituntut memahami data flow sehingga tidak mengakibatkan perubahan pada alur proses bisnis dan ada celah pada error handling. Tidak lagi berpatokan pada implisit sekuensial.

Spring Reactive adalah versi lengkap dari pustaka RxJava karena meliputi dari web framework, data, dan security bahkan komponen lainnya. Dikarenakan RxJava sendiri adalah pustaka terpisah dari ekosistem Spring sehingga untuk melengkapi menjadi solusi yang benar-benar reaktif masih memerlukan pustaka lainnya yang mendukung. 

Sehingga dengan Spring Reactive maka sebagai pengembang Spring tidak memerlukan lagi pustaka RxJava.

# Experience yang Dirasakan

Ketika membangun aplikasi yang reaktif saya harus dapat mengidentifikasi proses yang akan ditempatkan pada baris kode dengan sifat eksekusinya asynchronous juga memungkinkan prosesnya concurrent. Berikut pengalaman baru yang saya dapatkan dari pemrograman secara reaktif.

## Current Object is a Future

Hal menarik bahwa semua obyek di dalam proses reaktif termasuk Spring Reactive berasal dari masa depan karena semua instruksi berjalan secara asynchronous. Apabila saya pinjam istilah dari Javascript terdapat istilah `Promise`, di mana semua obyek tidak dapat diprediksi secara langsung keberadaannya dan baru akan muncul ketika telah diproses. 

Eksekusi pemanggilan sebuah obyek secara asynchronous contohnya ketika harus mencari data ke database. CPU tidak memerlukan alokasi thread untuk standby terhadap availability data tersebut. Thread tersebut hanya menginstruksikan kepada I/O untuk mencari data ke database lalu pergi mengerjakan hal lain dan kembali lagi ketika I/O memberikan signal data sudah available.

Sehingga hal ini dapat dikatakan sebagai proses yang non-blocking bagi thread. Ini adalah hal yang unik bagi saya karena pembuatan kode dengan obyek yang availabilitynya adalah masa depan.

## Concurrency Under the Hood

Concurrency atau kemampuan multi-tasking yaitu beberapa tugas dapat dijalankan secara bersamaan meskipun awalnya tugas-tugas tersebut tidak diberikan secara bersamaan. Dengan proses asynchronous maka saya harus terbiasa dengan concurrency, di mana juga tampak identik dengan multi-tasking namun harus memperhatikan urutannya agar tidak overlap.

Seperti yang dijelaskan pada bagian *current object is a future* ketika thread hanya kembali aktif ketika I/O memberikan sinyal melalui event loop maka sembari menunggu I/O, thread dapat mengeksekusi tugas lainnya yang diberikan oleh CPU. Hal ini membuat proses concurrency menjadi lebih baik karena thread dapat lebih dioptimalkan. 

Sehingga di dalam hal ini ini adalah saya sebagai pengembang jangan sampai membuat kode menjadi blocking karena tentunya akan menghambat kemampuan concurrency. Bisa rugi karena harusnya thread dapat mengerjakan beberapa hal secara concurrent justru terhambat karena kode saya yang blocking eksekusi.

## Under Control in Data Flow

Berbicara tentang urutan harus memperhatikan prioritas antar tugas agar tidak overlap, hal ini menjadi sangat krusial apabila terdapat dependensi antar tugas sehingga concurrency menyebabkan disrupsi pada transaksi atau proses bisnis. Dengan adanya fungsi pada data flow untuk pemberian urutan prioritas dapat mengatur tugas agar tetap berada pada koridor.

Saya benar-benar harus memahami data flow di dalam proses concurrency pada Spring Reactive. Untuk lengkapnya terdapat referensi konsep dari ReactiveX yang telah saya ambil dan sangat membantu saya untuk memahami bagaimana cara kerja data flow. Kata flow di sini akan saya berikan sinomim sebagai stream yang diatur oleh operator.

## Data Stream

| Operator | Deskripsi | Flow |
| --- | --- | --- |
| `Just` | Operator yang digunakan untuk inisialisasi atau emit obyek ke dalam stream, obyek pada stream ini bersifat future dan dapat diamati serta diproses oleh fungsi-fungsi lainnya. Sebuah fungsi utama dari reactive data stream. | ![](https://reactivex.io/documentation/operators/images/just.c.png)  |
| `Map` | <p>Operator yang digunakan untuk mendapatkan obyek yang dijanjikan dari sebuah stream, proses ini menjadi synchronous karena obyek menjadi sebuah value yang harus direturn.</p> <p>i.e mendapatkan data</p> pelanggan | ![](https://reactivex.io/documentation/operators/images/map.i.png) |
| `FlatMap` | <p>Operator yang digunakan untuk mendapatkan return dari perintah asynchronous lain, proses ini merupakan non-blocking karena returnnya adalah sebuah stream.</p><p>i.e mendapatkan data pelanggan lalu mengirimkan email ke pelanggan tersebut</p> | ![](https://reactivex.io/documentation/operators/images/flatMap.c.png) |
| `OnSubscribe` | Operator yang digunakan untuk melakukan trigger terhadap sebuah stream, tanpa fungsi ini maka stream akan diabaikan atau tidak dieksekusi | ![](https://reactivex.io/documentation/operators/images/subscribeOn.c.png) |
| `OnErrorReturn` | Operator yang digunakan untuk melakukan `try..catch` terhadap sebuah stream, sehingga ketika terjadi sebuah exception atau error akan ditangkap dan ditangani oleh perintah ini. Digunakan sebagai chain command dari `Map` dan `FlatMap`. | ![](https://reactivex.io/documentation/operators/images/Catch.png) |

## Chain Everywhere, Think Short

Pengetikan kode sangat beruntun dalam mengalirkan data. Setiap operator benar-benar membuat saya spesifik dan berpikir singkat apa yang harus dilakukan di dalam fungsi lambda tersebut. Misal di dalam map, onErrorMap, switchIfEmpty, dan flatMap. Semua harus serba spesifik.

```jsx
Mono<Finishing> someFinishing(start) {
	return needToFinish.execute(start).map(data -> {
		// some procedure to process mapped data
	}).onErrorMap(ex -> {
		// some step to throw error on mapping in needToFinish
	}).switchIfEmpty(
		// if empty then what?
	).flatMap(result -> {
		return letsFinish.execute().map(finishing -> {
			// then do something different after the data is mapped
		}).onErrorMap(ex -> {
			// some step to throw error in the finishing from letsFinish	
		});
	});
}
```

Sehingga yang biasanya diketikan secara eksplisit panjang secara vertikal harus dichunk atau dicacah ke setiap fungsi lambda. Tidak ada lagi juga nested throw exception karena dibungkus juga ke setiap chain operator yang nanti detailnya akan saya bahas di bagian berikutnya.

## Say Goodbye to Overflow with Backpressure

Dengan menggunakan Mono atau Flux ternyata mempermudah saya dalam mengolah sebuah obyek. Mono biasa digunakan untuk obyek tunggal sedangkan Flux untuk obyek yang sifatnya bersifat multi atau collection. 

Dengan Flux bersifat multi yang diabstraksikan ke dalam satu variable maka ada keuntungan misal

```jsx
Flux<MyDataList> cdrs = cdrRepository.findByMsisdn(msisdn) //all data is collected by producer
return cdrs.map(PageResult::response); //all data read by consumer
```

yaitu data dari Flux tidak perlu perulangan secara manual dan tidak perlu dikirimkan lengkap dari producer.

1. **Producer**: saya punya 1000 data yang sudah ditarik.
2. **Consumer**: tapi memori saya hanya mampu menampung 10 data untuk diproses saat ini.
3. **Negosiasi** producer hanya mengirimkan 10 data untuk diproses oleh consumer dan akan mengirimkan lagi ketika consumer meminta lagi sisa data berdasarkan alokasi memori yang dimiliki hingga lengkap.

## More Robust on Error Handling

```jsx
try {
	// some logic
} catch (EmptyRowException ex) {
	throw new NotFoundException(404);
}
if (invalid) {
	throw new InvalidException(400);
}
//more logic
```

Tidak ada lagi nested try..catch seperti kode di atas yang membungkus logika utama, semua menjadi chain command dari sebuah fungsi. Atau dengan bahasa yang lebih baik menjadi sebuah event berkelanjutan semisal terdapat error.

```jsx
cdrRepository.findByMsisdn(msisdn).map(data -> {
		// some logic to more logic
})
.onErrorMap(ex -> new InvalidException(400);
.switchIfEmpty(Mono.error(new NotFoundException(404))
```

Exception menjadi sebuah obyek yang direturn dan otomatis akan dijadikan referensi error yang ditampilkan ke user. Membuat kode lebih bersih dan tidak banyak keyword `throw` di dalam editor, ini hal yang paling membuat saya merasa senang.

## Longer Unit Testing

Karena semua obyek pada reaktif berada di masa depan maka semua terbungkus dengan Mono atau Flux. Sehingga apabila memerlukan pengecekan value dari obyek harus melakukan blocking atau menanti sampai obyek tersebut muncul.

```jsx
@Test
@DisplayName("[case-type: positive] should return transaction_id when success bulk save data")
void shouldSuccessBulkSave() {
	when(dependencyRepository.saveAll(anyCollection()))
	  .thenReturn(Flux.just(ClassA.builder().build()));
  TransactionResponse response = myService.execute().block(); // wait until the value is returned
  assertNotNull(response);
}
```

# Kesimpulan

Ini adalah pengalaman saya menggunakan Spring Reactive, menambah pemikiran ternyata reaktif seperti pola Monad daripada formula matematika eksplisit untuk pengolahan data karena fokusnya kepada “konteks” untuk pengolahan data dibandingkan fokus nilai dari datanya itu sendiri. Jadi mungkin akan sangat cocok untuk pengolahan data kompleks.