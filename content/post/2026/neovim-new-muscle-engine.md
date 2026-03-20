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

Karena Samsung baru rilis update Android 16 dengan One UI 8 terbarunya, fitur Dex jadi harus pakai eksternal monitor dan terparahnya Termux Desktop berbasis PRoot jadi melambat. Membuka VSCode harus menunggu hampir 2 menit loading baru siap pakai, itu pun lemot.

Setelah cek GitHub ternyata banyak pengguna PRoot di device Samsung yang mendapat One UI terbaru mengalami hal yang serupa. Infonya ada perubahan manajemen CPU, GPU, dan memori yang berdampak ke proses aplikasi. Satu hal yang pasti, update Samsung memang suck! 

> Jadi saya memutuskan untuk re-install Termux dan menggunakannya hanya sebagai terminal tanpa Termux Desktop

# Vim Termux 

Secara pekerjaan coding di Tablet saya hanya berkutat dengan text dan perintah. Jadi terminal saja sudah cukup. Terinspirasi dari cerita coder di era 90an, mereka menggunakan Text-User-Interface (TUI) seperti Vim, Free Prascal, dan Pacific C.

Berhubung Termux menyediakan package manager untuk install Vim, saya langsung menginstall Vim berikut dengan Git untuk keperluan manajemen repository.

```
apt update && apt upgrade -y 
apt install vim git tmux 
```

Dengan bermodal Vim dan Git sudah dapat mulai bekerja. Sedikit tambahan yaitu multiplexer dengan tmux. Tetapi memang ada yang kurang, yaitu syntax highlightning dan File tree browser agar mudah menavigasi karena mata jadi cepat lelah hanya warna hitam dan putih.

Selain masalah navigasi, juga butuh beberapa utility untuk development agar semakin lincah. Jadi butuh linting, auto complete, running split terminal, dan lainnya seperti di VSCode. Untuk terminal split masih dapat diakali dengan tmux.

Untuk setup tampilan Termux yang digunakan mengikuti petunjuk repo [berikut](https://github.com/adi1090x/termux-style), download font Nerd FiraCode Semibold dari situs [berikut](https://www.nerdfonts.com/font-downloads), dan menggunakan zsh sebagai terminal profile. Untuk update font setelah unzip di direktori Downloads kurang lebih seperti berikut:

```
cd /storage/shared/Downloads
mv FiraCode-SemiBold.ttf ~/.termux/font.ttf
termux-reload-settings
```

## Setup Neovim

Karena kurang puas, saya mencari bahan lainnya agar Vim jadi lebih powerful. Dari hasil pencarian banyak yang merekomendasikan Neovim sebuah extended version dari Vim. Karena mendukung LSP dan Treesitter. Sepemahaman saya kurang lebih sebagai berikut.

LSP adalah _Language Server Protocol_ yang menstandarisasi komunikasi antara editor dengan bahasa pemrograman. Setiap bahasa memiliki LSPnya sendiri, berfungsi agar editor dapat mengenali bahasa pemrograman, code completion, symbol navigation, melakukan referensi, dan diagnosa kode.

Treesitter adalah parser untuk teks kode menjadi lebih terstruktur seperti identifikasi symbol variable dan fungsi, karakter khusus bahasa pemrograman, syntax highlightning pewarnaan, dan hal lain terkait sintaksis. Ada 2 model yaitu AST dan CST tetapi saya bukan orang yang tepat untuk menjelaskan.

Karena Neovim mendukung 2 fitur utama tersebut dan secara popularitas sangat tinggi (> 90k stars) maka menjadi pilihan utama. Untuk instalasinya sangat mudah karena ada di package Termux. Berikut perintah untuk install dan cek versinya.

```
apt update && apt upgrade -y
apt install nvim

nvim -v
```

Untuk replace Vim dengan Neovim saya membuat alias baru di .zshrc sebagai berikut dengan perintah `vim ~\.zshrc` dan menambahkan pada baris konfigurasi lalu reload konfigurasi.

```
alias vim = nvim #lalu write dan keluar dengan perintah :wq!

cd ~
source .zshrc
```

Dari setup utama ini maka Neovim sudah siap sebagai editor utama. Untuk runningnya cukup dengan menggunakan perintah yang sama dengan Vim karena sudah dibuatkan alias yaitu `vim <file/directory>`.

## Setup NvChad

Agar dapat menambahkan fungsi dengan plugin seperti di VSCode yang dapat ditambahkan extension, NvChad menjadi pilihan yang solid. Sebetulnya ada 2 alternatif seperti LazyVim dan AstroVim, tetapi NvChad memiliki popularitas yang tertinggi dan 

## Setup Tambahan

# Pengalaman
