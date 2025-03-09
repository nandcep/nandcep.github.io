---
title: Mengembangkan Plugin BMC Remedy untuk JSON Parser
date: 2018-03-26 00:00:01
categories: []
tags: [bmc-remedy,java,pemrograman]     # TAG names should always be lowercase
---

![](https://images.unsplash.com/photo-1475770230762-6409e81d7589?q=80&w=2832&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D)
Credit to Unsplash

Berhubung sebentar lagi saya akan resign tidak ada salahnya meninggalkan jejak untuk mencatat sebagian kecil ilmu yang telah didapatkan selama bekerja. Di MII saya banyak sekali belajar secara otodidak untuk mengembangkan ilmu serta aplikasi diluar kemampuan aslinya. Hal tersebut dikarenakan memang dasarnya saya memulai karir sebagai pengembang aplikasi.

Kebetulan BMC Remedy platformnya based on Java. Untuk melakukan kustomnya tambahan fitur pada komponennya pun seperti WordPress, yaitu dengan menggunakan plugin. Plugin yang siang ini saya adalah plugin untuk consume RESTful Web Service dari eksternal. Apakah mengembangkan plugin untuk BMC Remedy itu susah? Jujur tidak sama sekali.

Pertama yang saya butuhkan adalah 2 pustaka API dari BMC Remedy AR System. Karena saya menggunakan AR System 9.1 maka pustaknya adalah `arpluginsvr91_build002.jar`, `arapi91_build002.jar`, dan `json-simple-1.1.1.jar`. Setelah itu backup terlebih dulu beberapa file config antara lain `AR.conf` dan `pluginsvr_config.xml`.

Use casenya semisal saya memiliki Middle Form untuk menampung hasil consume web service dengan nama `BMC:Learning:JSON`. Response JSON yang saya tarik berasal dari `http://domain/xxxxx.json` dengan method GET. Response bodynya katakanlah seperti di bawah ini:

```json
[
    {"Programming Language" : "Java"},
    {"Programming Language" : "C#"},
    {"Programming Language" : "PHP"},
    {"Programming Language" : "Python"}
]
```

Data pada field Programming Language akan disimpan ke Short Description pada Middle Form `BMC:Learning:JSON`. Sehingga nanti pada filter akan memiliki satu action yaitu `Set Field` dengan sourcenya adalah plugin dengan 3 parameter yaitu nama form, mapping field, dan alamat URL web service RESTful yang ingin diconsume.

Kok terkesan banyak parameternya? Agar sedikit lebih dinamis tentunya dan tidak terlalu hardcode sehingga lebih reuseable serta mudah dikembangkan di kemudian hari. Selanjutnya untuk adalah membuat New Java Project di Eclipse dan beri nama JSONParser.

Dari project tersebut referensikan 3 pustaka di atas. Dan berikut adalah penjelasan dari setiap pustakanya:

| # | Library | Description |
| -- | ------- | ----------- |
| 1  | arpluginvr91_build002.jar | Pustaka untuk interface dan implementasi plugin BMC Remedy AR System |
| 2  | arapi91_build002.jar | Pustaka untuk melakukan operation API di AR System 9.1 |
| 3  | json-simple-1.1.1.jar | Pustaka untuk encode dan decode JSON |

Untuk file berikut adalah penjelasannya:

| # | Library | Description |
| -- | ------- | ----------- |
| 1  | AR.conf | file konfigurasi AR System beserta seluruh plugin registry yang butuh diaktifkan |
| 2  | pluginsvr_config.xml | File konfigurasi plugin dan path Java Class yang akan saya gunakan |

Lalu saya membuat sebuah class dengan nama `JSONParsePlugin` berikut snippetnya

```java
package plugin.nandcep.github.io;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;
import org.json.simple.JSONArray;
import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;
import org.json.simple.parser.ParseException;
import com.bmc.arsys.pluginsvr.plugins.ARFilterAPIPlugin;
import com.bmc.arsys.pluginsvr.plugins.ARPluginContext;
import com.bmc.arsys.api.ARException;
import com.bmc.arsys.api.ARServerUser;
import com.bmc.arsys.api.Entry;
import com.bmc.arsys.api.Value;

public class JSONParserPlugin extends ARFilterAPIPlugin {

	@Override
	public List filterAPICall(ARPluginContext context, List listOfParam) throws ARException {
		String serverName = context.getARConfigEntry("Server-Connect-Name");
		int port = Integer.valueOf(context.getARConfigEntry("TCD-Specific-Port"));
		ARServerUser server = new ARServerUser(context, "", serverName);
		server.setPort(port);
		String formName = listOfParam.get(0).getValue().toString();
		String fieldId = listOfParam.get(1).getValue().toString();
		String urlStr = listOfParam.get(2).getValue().toString();
		String jsonStr = this.getJSON(urlStr);
		List results = new ArrayList();
		String [] fieldIds = fieldId.split(",");
		JSONParser jParser = new JSONParser();
		Object json = null;
		try {
			json = jParser.parse(jsonStr);
		} catch (ParseException e) {
			e.printStackTrace();
		}
		JSONArray jArr = (JSONArray) json;
		for(int i = 0; i < jArr.size(); i++) {
			JSONObject jObj = (JSONObject) jArr.get(i);
			int fieldIdx = 0;
			Entry entry = new Entry();
			for(Iterator iterator = jObj.keySet().iterator(); iterator.hasNext();) {
				String key = (String) iterator.next();
				String value = jObj.get(key).toString();
				int field = Integer.valueOf(fieldIds[fieldIdx]);
				entry.put(field, new Value(value));
				fieldIdx++;
			}
			String id = server.createEntry(formName, entry);
			results.add(new Value(id));
		}
		return results;
	}

	private String getJSON(String urlStr) {
		String jsonStr = "";
		try {
			URL url = new URL(urlStr);
			HttpURLConnection urlCn = (HttpURLConnection) url.openConnection();
			urlCn.setDoOutput(true);
			urlCn.setRequestMethod("GET");
			BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(urlCn.getInputStream()));
			String line;
			StringBuilder content = new StringBuilder();
			while((line = bufferedReader.readLine())!= null) {
				content.append(line);
				content.append(System.lineSeparator());
			}
			jsonStr = content.toString();
		} catch (Exception e1) {
			// TODO Auto-generated catch block
			e1.printStackTrace();
		}
		return jsonStr;
	}

}
```

Setelah itu saya harus modifikasi `pluginsvr_conf.xml` sebagai berikut:

```xml
<plugins>
    <name>NANDCEP.GITHUB.IO.JSONPARSER</name>
    <classname>plugin.nandcep.github.io.JSONParserPlugin </classname>
    <filename>{your AR Server location}\pluginsvr\custom\jsonparser.jar</filename>
    <pathelement type="path">{your AR Server location}\pluginsvr\custom</pathelement>
    <pathelement type="location">{your ar server location}\pluginsvr\custom\lib\json-simple-1.1.1.jar</pathelement>
    <userDefined>
        <bulk_transaction_size>32</bulk_transaction_size>
        <server_name>{your hostname}</server_name>
        <use_bulk_transaction>false</use_bulk_transaction>
        <server_port>{your RPC port}</server_port>
    </userDefined>
    ......
    ......
</plugins>

```

Dan harus modifikasi `AR.conf` sebagai berikut:

```
Server-Plugin-Alias: NANDCEP.GITHUB.IO.JSONPARSER NANDCEP.GITHUB.IO.JSONPARSER :9999
```

Selesai config lalu simpan, selanjutnya export Java Project Eclipse menjadi sebuah executable jar dengan nama `jsonparser.jar` dan tempatkan di `{lokasi instalasi AR Server}\pluginsvr\custom\`. Notes: buat folder dengan nama custom jika tidak ada sebelumnya.

Untuk tambahan eksternal library di dalam folder custom buat satu lagi folder dengan nama lib dan copy library `json-simple-1.1.1.jar` ke dalam folder lib. Karena berbasis interface `ARFilterAPIPlugin` saya diwajibkan untuk mengimplementasikan method `filterAPICall` (context, filter parameter).

Filter parameter berasal dari inputan yang ada di filter nanti. Sehingga pada bagian awal terdapat variabel nama form, field id, dan URL yang nilainya diset secara otomatis oleh action `Set Field`. Method `getJSON` adalah sebuah prosedur yang digunakan untuk menarik JSON dari sebuah URL.

Returnnya berupa string dan akan diproses dengan library json-simple. Setelah selesai semuanya maka harus restart AR System dan baru setelah itu dapat dicoba pada filter. Pada filter yang butuh untuk memanggil plugin. Saya set nama pluginnya adalah NANDCEP.GITHUB.IO.JSONPARSER.

Untuk set action, dapat mengikuti seperti langkah di bawah ini:


1. `$0$` set valuenya dengan nama form yaitu `BMC:Learning:JSON`
2. `$1$` set valuenya dengan mapping field dari return JSON dengan yang ada di form. Karena sudah disepakati field `Programming Language` akan masuk sebagai short description maka dapat set valuenya menjadi "8" sesuai dengan ID field dari Short Description. Jika semisal memiliki lebih dapat menggunakan separator koma "," i.e "8,5110001,51100002,dst"
3. `$2$` set dengan alamat url RESTful semisal `http://domain/xxxxx.json`

Semoga saya dapat mencoba kembali BMC Remedy lagi jika ada kesempatan. Semoga bermanfaat, salam.