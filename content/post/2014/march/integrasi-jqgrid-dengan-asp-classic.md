---
date: 2014-03-21T00:01:00+07:00
lastmod: 2014-03-21T00:01:30+07:00
authors: ["nandra"]
title: Integrasi jqGrid dengan ASP Classic
slug: integrasi-jqgrid-dengan-asp-classic
tags: [programming]
---

Tidak jauh berbeda dengan implementasi integrasi [jqGrid](https://www.trirand.com/blog/) untuk PHP, jqGrid juga dapat digunakan untuk server side yang dikembangkan menggunakan bahasa selain PHP. Pada tulisan ini saya menggunakannya untuk website yang dibangun dengan ASP Classic.

> Terdapat 3 file VBS yang kembangkan yaitu Mathematic.asp, jqGrid.asp, dan pustaka JSON.
> 

Mathematic.asp berisi fungsi matematika yang salah satunya terdapat fungsi ceil, JQGrid.asp sebagai helper untuk operasi pada JQGrid, dan pustaka JSON untuk melakukan encode decode JSON yang dibutuhkan oleh JQGrid.

Berikut adalah VBS untuk Mathematic.asp yang di dalamnya terdapat fungsi untuk ceiling. Untuk fungsi matematika selain ceil seperti floor, round, dan sebagainya tidak saya include dalam artikel ini karena tidak berkaitan.

```visual-basic
Function ceil(n)
    Dim nref
    nref = CDbl(n)
    If Int(nref * 10 ) MOD 10 >= 0 Then
        ceil = Int(nref) + 1
    Else
        ceil = Int(nref)
    End If
End Function
```

VBS Mathematic.asp dan pustaka JSON harus diinclude ke dalam jqGrid.asp. jqGrid.asp yang dibangun juga berfungsi sebagai helper. Berikut adalah VBS tersebut beserta dengan penjelasannya. Hal pertama yang saya lakukan adalah inisialiasi variabel-variabel yang biasanya dikirimkan oleh jqGrid.

```visual-basic
Dim
    command,
	where,
	searchField,
	searchOper,
	searchString,
	ops,
	opsRef
Dim page,
	limit,
	sidx,
	sord,
	start,
	count,
	totalPage,
	ceilParam,
	lengthWhere,
	querySQL
Set command = Request("command")
```

Selanjutnya saya membuat sebuah prosedur dengan nama `constructGridWhere` untuk melakukan parsing perintah jqGrid menjadi operational where clause pada SQL. Variabel yang diolah oleh `constructGridWhere` adalah variabel global where yang telah didefinisikan sebelumnya.

Untuk menangkap request yang bersifat multi clause dari kombinasi maka saya mempergunakan `Scripting.Dictionary` sebagai collection dan setelah itu saya olah dengan perulangan `for..each` untuk melakukan concat.

```visual-basic
Sub constructGridWhere()
	Set ops = CreateObject("Scripting.Dictionary")
	Set searchField = Request("searchField")
	Set searchOper = Request("searchOper")
	Set searchString = Request("searchString")
	If Request("_search") = "true" Then
		ops.add "eq","="
		ops.add "ne","<>"
		ops.add "lt","<"
		ops.add "le","<=" ops.add "gt",">"
		ops.add "ge","=>"
		ops.add "bw","LIKE"
		ops.add "bn","NOT LIKE"
		ops.add "in","LIKE"
		ops.add "ni","NOT LIKE"
		ops.add "ew","LIKE"
		ops.add "en","NOT LIKE"
		ops.add "cn","LIKE"
		ops.add "nc","NOT LIKE"
	End If
	For Each key in ops
		If searchOper = key Then
			opsRef = ops(key)
		End If Next
		If searchOper = "eq" Then
			searchString = searchString
		End If
		If (searchOper = "bw" Or searchOper = "bn") Then
			searchString = searchString & searchString
		End If
		If searchOper = "ew" Or searchOper = "en" Then
			searchString = searchString & "%" & searchString
		End If
		If searchOper = "cn" Or searchOper = "nc" Or searchOpern = "in" Or searchOper = "ni" Then
			searchString = searchString & "%" & searchString & "%"
		End If
		where = searchField & opsRef & "'" & searchString &"'"
End Sub
```

Untuk melakukan penarikan data dari tabel pada sebuah skema di database, saya membuat sebuah prosedur dengan nama `constructGridData`. Di mana pada prosedur tersebut diperlukan eksekusi terlebih dahulu terhadap `constructGridData`.

Pada prosedur berikut adalah perintah sederhana untuk melakukan seleksi data pada satu tabel saja. Sehingga apabila diperlukan join pada beberapa tabel atau advance query tentu saja masih diperlukan modifikasi.

```visual-basic
Sub constructGridData(fieldKey, table)
	Call constructGridWhere()
	Set page = Request("page")
	Set limit = Request("rows")
	Set sidx = Request("sidx")
	Set sord = Request("sord")
	start = limit * page - limit
	If start < 0 Then
        start = 0
    End If
    Set resultSet = Server.createObject("ADODB.Recordset")
    resultSet.Open "SELECT COUNT("& fieldKey &") AS TotalRecord FROM " & table, connection
    If Not resultSet.Eof Then
        count = CInt(resultSet.Fields("TotalRecord"))
    Else
        count = 0
    End If
    resultSet.Close
    Set resultSet = Nothing ceilParam = count/limit
    If count > 0 Then
		totalPage = ceil(ceilParam)
	Else
		totalPage = 0
	End If
	If page > totalPage Then
		page = totalPage
	End If
	lengthOps = Len(opsRef)
	If Not lengthOps = 0 THEN
		querySQL = "SELECT * FROM " & table & " WHERE " & where & " ORDER BY " & sidx & " " & sord & " LIMIT " & limit
	Else
		querySQL = "SELECT * FROM " & table & " ORDER BY " & sidx & " " & sord & " LIMIT " & limit
	End If
	Set resultSet = Server.createObject("ADODB.Recordset")
	resultSet.Open querySQL, connection
End Sub
```

Untuk fungsi terakhir yang diperlukan adalah mengisi cell data dalam bentuk JSON berdasarkan collection of data (array).

```visual-basic
Function myCell(id, cellArray)
	Dim o :
		Set o = jsObject
		o("id") = id
		o("cell") = cellArray
		Set myCell = o
End Function
```

Dalam melakukan testing terhadap fungsi-fungsi tersebut atas dasar keperluan load data dengan nama `produceData`, maka saya membuat sebuah controller yang dapat mengirimkan data berdasarkan request dari jGrid table dalam bentuk format JSON. Controller ini bernama UserController.asp.

Apabila membutuhkan contoh koneksi ke database dapat melihat pada posting artikel saya yang sebelumnya.

```visual-basic
command = Request("command")
If command = "load" Then
	Call produceData()
End If

Sub produceData
	Call constructGridData("field_pk","nama_tabel")
	Set myData = jsObject()
	myData("page") = page
	myData("total") = totalPage
	myData("records") = count
	Set myData("rows") = jsArray()
	Do While Not resultSet.Eof
		id = resultSet.Fields("field_pk")
        propertyA = resultSet.Fields("kolom_1")
		propertyB = resultSet.Fields("kolom_n")
		Set myData("rows")(Null) = myCell(id, array(id,propertyA,propertyB))
		resultSet.MoveNext
		Loop myData.Flush
End Sub
```

Untuk penyelesaiannya di sisi klien, berikut adalah javascript untuk menghasilkan jqGridnya:

```jsx
$(document).ready(function () {
  $("#gridData").jqGrid({
    url: "/WebService/UserController.asp?command=load",
    mtype: "post",
    datatype: "json",
    colNames: ["ID", "Property A", "Property B"],
    colModel: [
      {
        name: "id",
        index: "id",
        width: 30,
      },
      {
        name: "propertyA",
        index: "propertyA",
        width: 30,
        sortable: true,
      },
      {
        name: "propertyB",
        index: "propertyB",
        width: 30,
        sortable: true,
      },
    ],
    autowidth: true,
    height: 280,
    rowNum: 10,
    rowList: [10, 20, 30],
    pager: "#pager",
    sortname: "id",
    viewrecords: true,
    sortorder: "asc",
    caption: "Data caption",
  });
  $("#gridData").jqGrid("navGrid", "#pager", {
    add: false,
    del: false,
    edit: false,
  });
})
```

Dari sini dapat disimpulkan cukup mudah ketika sudah terdapat base layout seperti prosedur dan fungsi yang generic dalam menghasilkan data untuk jqGrid. Sebetulnya pada tahap ini masih diperlukan custom pada query agar lebih dinamis namun perlu waktu tambahan supaya dapat tercapai.

Sekiranya bermanfaat bagi yang membutuhkan. Salam.

Disadur dari blog lama saya di *WordPress.com*