---
title: Eksekusi Store Procedure dengan Java
date: 2015-07-25 00:00:01
categories: []
tags: [java,sql,pemrograman]     # TAG names should always be lowercase
---
![](https://images.unsplash.com/photo-1612629278431-0cd4cf0fb111?q=80&w=3173&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D)
_Credit to Unsplash_

Berhubung minggu malam ini sedang hujan deras dan tidak dapat keluar dari kosan, akhirnya saya iseng membuka IDE di MacBook pribadi yang lama tidak tersentuh. Tema coding pada tulisan ini adalah eksekusi stored procedure menggunakan Java. Java memiliki kemampuan untuk eksekusi store procedure dengan dukungan pustaka JDBC.

Untuk sample saya menggunakan DBMS SQL Server 2008R2 dengan pustaka JTDs. Untuk mengeksekusinya saya menggunakan class `CallableStatement` dan bukan Prepared Statement. Kenapa `CallableStatement`? Karena object yang dieksekusi store procedure sudah terkompilasi di dalam DBMS dan bukan perintah mentah seperti query DML.

> `CallableStatement` memang didesain untuk mengeksekusi store procedure karena memiliki terdapat subset yang dapat menhandle parameter out dan in.

Berikut adalah snippetnya stored procedurenya:

```sql
USE [AppDB]
GO
/****** Object:  StoredProcedure [dbo].[sp_name]    Script Date: 25/07/2015 21:35:13 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
-- =============================================
-- Author:  nandcep
-- Create date: 25-07-2015
-- Description: example only
-- =============================================
CREATE PROCEDURE [dbo].[sp_name] 
 -- Add the parameters for the stored procedure here
 @in_id varchar(50)
AS
BEGIN
 -- SET NOCOUNT ON added to prevent extra result sets from
 -- interfering with SELECT statements.
 SET NOCOUNT ON;

    -- Insert statements for procedure here
 SELECT data from dummy
 where id = @in_id; 
END
```

Baris kode di atas adalah store procedure yang akan dipanggil di dalam Java. Berikut snippet code Java untuk pemanggilan store procedure di atas dengan menggunakan object dari class `CallableStatement`:

```java
try{
   CallableStatement callableStatement = DBConnection.getConnection().prepareCall("EXEC sp_name ?"); //execute sp dengan callable, parameter diwakilkan sebagai ?
   callableStatement.setEscapeProcessing(true);
   callableStatement.setString(1, "IDX0123ZZYY"); //parameter beserta indexnya
   executed = callableStatement.execute(); //eksekusi
   ResultSet resultSet = callableStatement.getResultSet();
   while(resultSet.next()){
     data = resultSet.getString("data"); //mendapatkan data yang dihasilkan oleh store procedure (lihat baris terakhir)
   }
}
catch(Exception e){
   e.printStackTrace();
}
if(!executed){
   throw new Exception("Failed to execute store procedure");
}
```

Parameter yang dipassing dari Java ke parameter store procedure in_id menggunakan set parameter berdasarkan index beserta tipe datanya. Pada contoh di atas parameter menggunakan tipe data berupa string dan ada di urutan pertama. Untuk tahapan dan penjelasan saya telah berikan pada bagian komentar. 

Semoga bermanfaat dan terimakasih.

_Disadur dari blog lama saya di WordPress.com_