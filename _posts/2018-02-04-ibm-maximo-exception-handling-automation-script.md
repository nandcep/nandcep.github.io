---
title: IBM Maximo Exception Handling Automation Script
date: 2018-02-04 00:00:01
categories: []
tags: [pemrograman,ibm-maximo]     # TAG names should always be lowercase
---

![](https://images.unsplash.com/photo-1475770230762-6409e81d7589?q=80&w=2832&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D)
Credit to Unsplash

Ketika mengerjakan project di kantor lama saya mendapat task untuk membuat Automation Script. Script tersebut digunakan untuk upload data person dari sebuah custom table ke table person di IBM Maximo. Nah kebetulan ada error yang kerap terjadi yaitu error duplicate data.

Lalu bagaimana caranya agar saya tahu bahwa error yang terjadi adalah error duplikasi? Berikut adalah block `try..catch` di Automation Script dan bagaimana mendapatkan jenis exception yang terjadi.

```python
try :
	nperson = personSet.add();
	... #logic statement yang berpotensi terjadi error
except MXException, e :
	errkey = str(e.getErrorKey()); #cast key error sebagai string
	if errkey == 'duplicatekey' : #jika ternyata key messagenya adalah duplicate data
		... #logic untuk ketika terjadi exception
```

Yang dapat dimanfaatkan adalah class `MXException` karena merupakan base class `Exception` di Platform Maximo. Untuk mendapatkan keynya dapat menggunakan function `getErrorKey()`. Semoga bermanfaat, terimakasih.