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

# Vanilla Termux 

Secara pekerjaan coding di Tablet saya hanya berkutat dengan text dan perintah. Terinspirasi dari cerita coder di era 90an, mereka menggunakan Text-User-Interface (TUI) seperti Vim, Free Prascal, dan Pacific C. Jadi harusnya terminal saja cukup dengan menggunakan [Termux](https://github.com/termux/termux-app) untuk full coding.

Berhubung Termux sudah menyediakan manager untuk install keperluan development maka segera install Git, network tools, perkakas development lainnya, termasuk `tmux`.

```
apt update && apt upgrade -y 
apt install zsh git curl wget tmux golang nodejs-lts openjdk-21
```

Sedikit untuk kosmetik agar tampilan lebih menggugah selera bekerja juga tidak lupa: 
1. Install [Termux Styling] untuk ganti tema agar tidak monoton, penjelasan ada pada link.
2. Update font Nerd FiraCode Semibold dari situs [berikut](https://www.nerdfonts.com/font-downloads).
3. ZSH dihias dengan [oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh) agar lebih ceria. 

Untuk font yang telah diunduh misal pada folder Downloads, langkahnya kurang lebih sebagai berikut:
```
cd /storage/shared/Downloads
mv FiraCode-SemiBold.ttf ~/.termux/font.ttf
termux-reload-settings
```

Dengan bermodal peralatan dan segala kosmetiknya maka sudah siap bekerja. By the way `tmux` dipakai untuk multiplexer split terminal karena malas switch antar tab session. Tetapi memang pemain utamanya belum disiapkan, yaitu apa editor yang akan digunakan untuk mulai coding. Dari hasil pencarian, disarankan menggunakan editor yang support LSP dan Treesitter.

Apa itu LSP dan Treesitter?
| Feature | Description |
| --- | --- |
| _Language Server Protocol (LSP)_ | yang menstandarisasi komunikasi antara editor dengan bahasa pemrograman. Setiap bahasa memiliki LSPnya sendiri, berfungsi agar editor dapat mengenali bahasa pemrograman, code completion, symbol navigation, melakukan referensi, dan diagnosa kode. |
| _Treesitter_ | parser untuk teks kode menjadi lebih terstruktur seperti identifikasi symbol variable dan fungsi, karakter khusus bahasa pemrograman, syntax highlightning pewarnaan, dan hal lain terkait sintaksis. Ada 2 model yaitu AST dan CST tetapi saya bukan orang yang tepat untuk menjelaskan. |

Ada beberapa opsi yaitu Vim dan Neovim, tetapi yang mendukung minimal effort adalah Neovim jadi dipilihlah Neovim.
 

## Setup Neovim

Karena kurang puas, saya mencari bahan lainnya agar Vim jadi lebih powerful. 

Treesitter adalah 
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
