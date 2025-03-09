---
title: Efisiensi Query Join Tanpa ORM
date: 2022-02-15 00:00:01
categories: []
tags: [sql,database, pemrograman]     # TAG names should always be lowercase
---

![](https://images.unsplash.com/photo-1516844113229-18646a422956?q=80&w=2938&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D)
Credit to Unsplash

Beberapa hari yang lalu saya membaca [artikel](https://www.travisluong.com/fastapi-vs-fastify-vs-spring-boot-vs-gin-benchmark/) yang dishare pada group Telegram JVM Indonesia, topiknya menarik perhatian saya karena membahas tentang perbandingan performance (_benchmark_) antar database driver untuk framework per masing-masing bahasa pemrograman.

> Silakan dibaca terlebih dahulu artikel tersebut dan setelah membaca artikel tersebut, namun saya tidak akan membahas serta memprovokasi agar melirik bahasa pemrogramannya.
{: prompt-info }

Saya lebih tertarik untuk menelisik di bagian Spring Data JDBC yang memiliki performance optimal. Menjadi pertanyaan untuk beberapa orang kenapa Spring Data JDBC membuat aplikasi memiliki performa yang lebih optimal dibandingkan dengan menggunakan Spring Data JPA.

Pertanyaan tersebut terjawab pada artikel dari Baeldung berikut [ini](https://www.baeldung.com/spring-data-jdbc-intro) yang dapat dicermati pada bagian conclusion. Berhubung saya polyglot yang sering menggunakan ORM (Object Relational Mapping) agar merdeka dari implementasi database dan lebih fokus pada bisnis serta kode, tentunya setuju terkait kepedihan menggunakan native driver.

Kenapa saya lebih suka ORM? Tentunya dikarenakan tidak harus terlalu banyak berurusan dengan implementasi database. Relasi dapat langsung terikat di dalam kode (e.g di Spring Data kita dapat mendefinisikan kardinalitas relasi antar class dengan anotasi), _what you type is what you get_.

Kebetulan saya adalah tipikal engineer yang sangat memperhatikan efektivitas dari eksekusi proses. Jika saya rasa hasil transaksi atau query kurang optimal dengan ORM pun saya masih bisa melakukan tuning tipe fetch, native query, dan dengan custom [projection](https://www.baeldung.com/spring-data-jpa-projections) di level repository.

Namun tetap ada trade off dari penggunaan ORM di mana saya tidak dapat mengkontrol semua perintah query e.g query fetch (entah itu akan menjadi subset query atau join) yang dieksekusi oleh ORM (kalau pun bisa mentok biasanya saya main di tipenya, justru akan menjadi pertanyaan kenapa saya menggunakan ORM tetapi masih harus ribet mengatur semua query pada elemen fetch).

Spike dengan ORM (berlaku bagi engineer yang tidak bijak) juga akan sangat terasa ketika mengalami high throughputs dan ujung-ujungnya mengakibatkan performance issue tentunya. Dan jika kita sudah baca artikel di atas tentunya akan mulai mempertimbangkan penggunaan native driver di beberapa aspek bisnis misalnya yang dinilai kritikal dan menjadi tolok ukur bagi pelanggan serta atasan kita 😀

Di ORM semua table adalah class dan kolom-kolomnya adalah attributenya sehingga kita tidak perlu memusingkan perihal mapping kembali dari query ke object yang kita inginkan. Termasuk juga relasinya semua sudah diotomatiskan ke tipe relasinya pada object.

Untuk kasus tanpa ORM maka ada beberapa pendekatan yang dapat kita lakukan (dari yang terburuk hingga yang tampaknya aman, _I’m not sure though but worth to try_). Casenya semisal kita memiliki 2 tabel beserta model entitas dengan kardinalitas 1..n yaitu Posts dan Comments, kita ingin mendapatkan detail post beserta komentar-komentarnya berdasarkan id.

Relasi pada tabel-tabel berikut ini adalah 1 artikel dapat memiliki beberapa komentar dan sebaliknya 1 komentar berelasi dengan 1 artikel.

```
Tabel Posts

+----+-------------+---------+--------------+------------+
| id | post_title  | content | created_date | created_by |
+----+-------------+---------+--------------+------------+
|  1 | Lorem Ipsum | …       | …            | …          |
+----+-------------+---------+--------------+------------+
```

```
Table Comments

+----+---------+-------------------------------+--------------+------------+
| id | post_id |            message            | created_date | created_by |
+----+---------+-------------------------------+--------------+------------+
|  1 |       1 | Nice article but ….           | …            | …          |
|  2 |       1 | I think the article can be …. | …            | …          |
|  3 |       1 | Whoa, what a great ….         | …            | …          |
+----+---------+-------------------------------+--------------+------------+
```

Entitas Post merupakan pseudo of object yang dapat merepresentasikan tabel Posts beserta relasi 1..n ke tabel Comments pada attribute comments, entitas dapat diumpamakan sebagai class atau struct.

```
Entity Post

Post {
 id long
 postTitle string
 content text
 createdDate timestamp
 createdBy long
 comments arrayOf(comment)
}
```

Entitas Comment merupakan pseudo yang dapat merepresentasikan tabel Comment beserta relasi 1..1 ke tabel Posts pada attribute post, entitas berikut dapat diumpamakan sebagai class atau struct.

```
Entity Comment

Comment {
 id long
 post Post
 message text
 createdDate timestamp
 createdBy long
}
```

Tanpa menggunakan ORM maka diperlukan sedikit usaha agar dapat merelasikan antara attribute comments pada entitas Post dan attribute post pada entitas Comment.

Pendekatan yang dapat kita lakukan semisal ingin melakukan query data dari table posts untuk ditarik detail post beserta komentarnya agar menjadi collection di attribute comments adalah dengan langkah-langkah di bawah ini.

# Subquery (n+1)

select terlebih dahulu tabel Post. Scan row dan mapping kolom ke attribute pada entitas Post seperti id, postTitle, content, createdDate, dan createdBy.

```sql
select * from posts where id = ?1
```

Setelah mendapatkan table Posts maka tahap berikutnya query ke table Comments berdasarkan foreign key yaitu post_id.

```sql
select * from comments where post_id = ?1
```

Stream rows dan scan setiap row untuk mapping kolom ke entitas Comment. Setiap row mapping ke entitas Comment akan dimasukkan ke dalam array pada attribute comments di entitas Post.

# Join dan Proses Berdasarkan Normalisasi dari Hasil Query

Pada pendekatan berikut hanya dibutuhkan sekali query. Select dengan melakukan join tabel Posts dengan tabel Comments, kira-kira gambaran joinnya seperti berikut.

```sql
select
    p.id, p.post_title, p.content, p.created_by, 
    p.created_date, c.id as comment_id, c.message message, 
    c.created_by as comment_by, c.created_date as comment_date 
from posts p inner join
comments c on p.id = c.post_id where c.id = ?1
```

Maka akan menghasilkan set data dari query di atas dengan normalisasi seperti pada tabel bawah ini.

```
+----+-------------+---------+------------+--------------+------------+----------------------------+------------+--------------+
| id | post_title  | content | created_by | created_date | comment_id |          message           | comment_by | comment_date |
+----+-------------+---------+------------+--------------+------------+----------------------------+------------+--------------+
|  1 | Lorem Ipsum | ….      | ….         | ….           |          1 | Nice article but           | ….         | ….           |
|  1 | Lorem Ipsum | ….      | ….         | ….           |          2 | I think the article can be | ….         | ….           |
+----+-------------+---------+------------+--------------+------------+----------------------------+------------+--------------+
```

Lalu untuk melakukan mapping dapat dilakukan 2 kali proses yaitu :
- Row dengan indeks pertama untuk kolom id sampai dengan created_date dapat dimapping sebagai data untuk entitas Post.
- Sedangkan kolom comment_id sampai dengan comment_date pada indeks pertama dan berikutnya dapat dilakukan stream dan dimapping menjadi collection data untuk attribute comments.

# Join dan Construct Query dengan Dokumen JSON

RDBMS jaman now sekarang sudah semakin canggih, sehingga kita dapat melakukan hasil query dengan output dokumen JSON seperti database yang berorientasi dokumen e.g MongoDB.

Ekspektasi data yang dapat kita bayangkan adalah berupa seperti berikut (disesuaikan dengan entitas Post dan tabel Posts).

```json
{
 id : 1,
 post_title : "Lorem Ipsum",
 content : "....",
 created_date : "....",
 created_by : "....",
 comments : [
  {
   id : 1,
   message : "Nice article but ....",
   created_date : "....",
   created_by : "....",
   post_id : 1
  },
  {
   id : 2,
   message : "I think the article can be ....",
   created_date : "....",
   created_by : "....",
   post_id : 1
  }
 ]
}
```

Lalu bagaimana cara melakukan query agar dapat menghasilkan data seperti di atas? Saya akan memberikan contoh dengan menggunakan database Postgres. Untuk database dengan produk lain dapat menyesuaikan, setahu saya Oracle dan SQL Server pun juga mempunyai fungsi yang serupa.

Perlu dicatat pendekatan yang saya lakukan berikut ini tidak serta merta menjadi satu dokumen namun saya pecah ke beberapa kolom namun hanya dalam sekali eksekusi query.

```sql
SELECT
    row_to_json(P.*) post,
    array_to_json(array_agg(DISTINCT C.*)) comments,
FROM POSTS P
INNER JOIN COMMENTS C ON C.POST_ID = P.ID
WHERE P.ID = ?1
GROUP BY P.ID
```

Query di atas sekiranya ketika dieksekusi akan menghasilkan data set sebagai berikut. Untuk melakukan mapping menjadi lebih mudah dengan serialisasi JSON string ke data type pada entitas.

Kolom post diserialize menjadi entitas Post untuk attribute id sampai dengan createdDate, sedangan kolom comments menjadi diserialize menjadi attribute comments pada entitas Post.

```json
COLUMN POST
---------------
{
 id : 1,
 post_title : "Lorem Ipsum",
 content : "….",
 created_date : "….",
 created_by : "…."
}
```

```json
COLUMN COMMENTS
---------------
[
 {
  id : 1,
  message : "Nice article but ….",
  created_date : "….",
  created_by : "….",
  post_id : 1
 },
 {
  id : 2,
  message : "I think the article can be ….",
  created_date : "….",
  created_by : "….",
  post_id : 1
 }
]
```

# Pengukuran
Berikut adalah hasil eksekusinya (by the way, saya menggunakan bahasa pemrograman Go dengan driver PGX) :

- Pendekatan 1, execution time 5ms
- Pendekatan 2, execution time 3.124 ms
- Pendekatan 3, execution time 870µs

Dari beberapa pendekatan di atas saya menarik kesimpulan dari test eksekusi beserta time takennya sebagai berikut:
- Pendekatan nomor 3 karena hanya sekali eksekusi dan proses stream array untuk menjadi sebuah entitas.
- Sedangkan pendekatan nomor 2 juga masih cukup cepat namun agak boros di bagian proses untuk menjadi sebuah entitas.
- Pendekatan nomor 1 dirasa tidak saya rekomendasikan karena semakin banyak roundtrip eksekusi query akan membuat boros koneksi ataupun peminjaman pool koneksi dan dampaknya waktu respon akan lebih lama.

Hasil testing ini bernilai relatif tergantung casenya dan tolok ukurnya. Time taken yang lebih cepat asumsi saya menjadi penanda penggunaan komputasi baik IO, memori, dan prosesor yang lebih efisien.

Kiranya postingan ini dapat menjadi referensi atau ide bagi rekan-rekan yang bingung terkait cara serta pendekatan mapping query join ke object yang efisien tanpa ORM, terimakasih.