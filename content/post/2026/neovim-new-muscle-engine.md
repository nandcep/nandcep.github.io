---
date: 2026-03-19T00:01:00+07:00
lastmod: 2026-03-19T00:01:30+07:00
authors: ["nandra"]
title: Neovim, New Muscle Engine
slug: neovim-new-muscle-engine
tags: [technology]
draft: true
---

# VSCode, Tetapi
Visual Studio Code (VS Code) tetap jadi code editor andalan saya untuk sapu jagat coding dengan berbagai extensionnya meski bukan berkategori IDE. Selain itu gratis karena open source dengan License MIT, hanya kekurangannya adalah berbasis _Electron_. 

Saya masih setia bersama VSCode baik di MacBook Pro pribadi, Thinkpad kantor, dan Termux Desktop di Tab S9+. Berbicara _Electron_ berat, iya tapi belum ada experience karena MacBook masih Sequoia dan Thinkpad terpasang RAM 32GB. Namun untuk Tab S9+ berbeda cerita.

Karena Samsung baru rilis update Android 16 dengan One UI 8 terbarunya, fitur Dex jadi harus pakai eksternal monitor dan yang terparah Termux Desktop berbasis PRoot jadi melambat. Membuka VSCode harus menunggu hampir 2 menit loading baru siap pakai, itu pun lemot.

Setelah cek GitHub ternyata semua pengguna PRoot di device Samsung yang mendapat One UI terbaru mengalami hal yang serupa. Infonya ada perubahan manajemen CPU, GPU, dan memori yang berdampak ke proses aplikasi. Satu hal yang pasti, update Samsung memang suck! 

> Jadi saya memutuskan untuk re-install Termux dan menggunakannya hanya sebagai terminal tanpa Termux Desktop

# Vim Termux 

Secara pekerjaan coding di Tablet saya hanya berkutat dengan text dan perintah. Jadi terminal saja sudah cukup. Terinspirasi dari cerita coder di era 90an, mereka menggunakan Text-User-Interface (TUI) seperti Vim, Free Prascal, dan Pacific C.

Berhubung Termux menyediakan package manager untuk install Vim, saya langsung menginstall Vim berikut dengan Git untuk keperluan manajemen repository.

```
apt update && apt upgrade -y 
apt install vim git tmux 
```

Dengan bermodal Vim dan Git sudah dapat mulai bekerja. Sedikit tambahan yaitu multiplexer dengan tmux. Tetapi memang ada yang kurang, yaitu syntax highlightning dan File tree browser agar mudah menavigasi karena mata jadi cepat lelah hanya warna hitam dan putih.

Selain masalah navigasi, juga butuh beberapa utility untuk development agar semakin lincah. Katakanlah untuk linting, auto complete, running terminal parallel, dan lainnya seperti di VSCode. Untuk terminal sebetulnya masih dapat diakali dengan tmux.

## Setup Neovim



## Setup Nvchad

# Pengalaman
