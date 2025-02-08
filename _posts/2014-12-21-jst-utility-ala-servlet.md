---
title: JSF Utility ala Servlet
date: 2014-12-21 00:00:01
categories: []
tags: [java,jsf,pemrograman]     # TAG names should always be lowercase
---
![](https://images.unsplash.com/photo-1508873535684-277a3cbcc4e8?q=80&w=2940&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D)
_Credit to Unsplash_

Berhubung sedang mempelajari dan menerapkan rapid development dengan JSF (Java Server Faces), saya menyiapkan beberapa fungsi sebagai helper seperti halnya yang biasa digunakan ketika develop dengan JSP (Java Server Page). Beberapa kebutuhan method dan function yang membuat saya sulit move-on dari JSP Servlet adalah sebagai berikut
- Get query parameter value dari URL
- Set session
- Get value from a session
- Redirect halaman

Di bawah ini adalah penjelasan dari helper yang telah saya develop.

# Get Request Parameter Function
Fungsi untuk mengabil parameter request atau query parameter dari URL. Semisal terdapat URL `https://baseurl:port/endpoint?parameter=value` maka dapat memanggil fungsinya seperti berikut `getRequestParameter("parameter")`

```java
public static String getRequestParameter(String name) {
    return (String) FacesContext
        .getCurrentInstance()
        .getExternalContext()
        .getRequestParameterMap()
        .get(name);
}
```

# Set Session Function

Method untuk menuliskan session pada web server aplikasi, semisal ingin menyimpan session dari obyek profile user yang telah login ke aplikasi. Untuk cara penggunaannya dapat mengeksekusinya sebagai berikut `setSession("USERLOGIN", userLoginResponse)`

```java
public static void setSession(String id, Object o){
        FacesContext context = FacesContext.getCurrentInstance();
        context.getExternalContext().getSessionMap().put(id, o);
}
```

# Get Session Value

Fungsi untuk mendapatkan value dari sebuah session yang tersimpan pada web server aplikasi. Semisal ingin mendapatkan session dari login dengan key `USERLOGIN` maka dapat memanggil fungsinya seperti berikut `getSession("USERLOGIN")`

```java
public static Object getSession(String id){
        FacesContext context = FacesContext.getCurrentInstance();
        return context.getExternalContext().getSessionMap().get(id);
}
```

# Page Redirect

Method untuk melakukan redirect ke sebuah URL pada aplikasi. Untuk melakukannya dapat memanggil methodnya seperti berikut `redirect("/login.jsf")`

```java
public static void redirect(String url){
        try {
                FacesContext.getCurrentInstance().getExternalContext().redirect(url);
        } catch (IOException e) {
                e.printStackTrace();
        }
}
```

# Kesimpulan

Fungsi dan method tersebut sebaiknya dibungkus ke dalam sebuah class utility serta dapat dipanggil secara static. Sehingga tidak perlu instansiasi terhadap class tersebut menjadi object. Fungsi di atas semuanya mengandalkan API servlet karena sebetulnya JSF juga dibangun di atas servlet.

Semoga helper di atas dapat bermanfaat bagi yang membutuhkan. Terimakasih.

_Disadur dari blog lama saya di WordPress.com_