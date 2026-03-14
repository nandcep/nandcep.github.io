---
date: 2014-01-03T00:01:00+07:00
lastmod: 2014-01-03T00:01:00+07:00
authors: ["nandra"]
title: Fixing Runat Server Javascript ASP.NET
slug: fixing-runat-server-javascript-asp-net
tags: [programming]
---

![](https://images.unsplash.com/photo-1494961104209-3c223057bd26?q=80&w=2804&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D)
Credit to Unsplash

Saya pernah mengalami kendala ketika membangun aplikasi web berbasis ASP.NET 3.5, load file Javascript pada tag script mengalami masalah. File Javascriptnya dianggap tidak ikut terdeploy ke dalam IIS sehingga terkena “not yet run at server”.

Efeknya ada banyak variabel dan fungsi pada eksekusi Javasacript yang tidak terdenifisikan atau undefined. Error yang diakibatkan oleh “not yet run at server” dapat dicek melalui console debugger milik (IE 8/9) atau melalui debugger Visual Studio.

Jadi bagaimana cara mengatasi masalah tersebut?
```xml
<asp:ScriptManager ID="script_mgr_1" runat="server">
    <Scripts>
        <asp:ScriptReference Path="lokasi file javascript 1" />
        <asp:ScriptReference Path="lokasi file javascript 2" />
        <asp:ScriptReference Path="lokasi file javascript 3" />
        <asp:ScriptReference Path="lokasi file javascript n" />
    </Scripts>
</asp:ScriptManager>

```

Snippet kode di atas adalah solusi untuk error “_not yet run at server_” dengan tag ScriptManager dari ASP.NET di halaman ASPX yang membutuhkan. Melalui tag `ScriptReference` maka file Javascript akan terbawa ke dalam IIS atau server lokal.

Biasanya reference tersebut digunakan untuk load jQuery. Cukup set relative path dari jQuery atau Javascript filenya berdasarkan solution project di `ScriptReference` maka akan ikut terbawa ke dalam IIS. Simple, just like a piece of cake.

Disadur dari blog lama saya di *WordPress.com*