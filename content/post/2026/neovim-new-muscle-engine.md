---
date: 2026-03-19T00:01:00+07:00
lastmod: 2026-03-19T00:01:30+07:00
authors: ["nandra"]
title: Neovim, New Muscle Engine
slug: neovim-new-muscle-engine
tags: [technology]
---

# VSCode, Tetapi
Visual Studio Code (VS Code) tetap jadi code editor andalan saya untuk sapu jagat coding dengan berbagai extensionnya meski bukan berkategori IDE. Selain itu gratis karena open source dengan License MIT, hanya kekurangannya adalah berbasis _Electron_. 

![](https://lh3.googleusercontent.com/pw/AP1GczPQ4iyU1oMXcD9n6LgamNj3AWgQSH3OsEAX8QEN4DXCUd_9otqQMgf372Agnx04T89ISi33_cnyQwg-ba_gfpRG9rwYVoH9Gsihw0VlDHSDkSm0550p8iVLOhTmyLSZAxpMgZ6s-FXHSxKb86T0vfNGAw=w2372-h1483-s-no-gm?authuser=0)

Saya masih setia bersama VSCode baik di MacBook Pro pribadi, Thinkpad kantor, dan Termux Desktop di Tab S9+. Berbicara _Electron_ berat, iya tapi belum ada experience karena MacBook masih Sequoia dan Thinkpad terpasang RAM 32GB. Namun untuk Tab S9+ berbeda cerita.

Karena Samsung baru rilis update Android 16 dengan One UI 8 terbarunya, fitur Dex jadi harus pakai eksternal monitor dan terparahnya Termux Desktop berbasis PRoot jadi melambat. Membuka VSCode harus menunggu hampir 2 menit loading baru siap pakai, itu pun lemot.

Setelah cek GitHub ternyata banyak pengguna PRoot di device Samsung yang mendapat One UI terbaru mengalami hal yang serupa. Infonya ada perubahan manajemen CPU, GPU, dan memori yang berdampak ke proses aplikasi. Satu hal yang pasti, update Samsung memang suck! 

> Jadi saya memutuskan untuk re-install Termux dan tanpa Termux Desktop, selamat tinggal VSCode untuk Tab S9+

# Vanilla Termux 

Secara pekerjaan coding di Tablet saya hanya berkutat dengan text dan perintah. Terinspirasi dari cerita coder di era 90an, mereka menggunakan Text-User-Interface (TUI) seperti Vim, Free Prascal, dan Pacific C. Jadi harusnya terminal saja cukup dengan menggunakan [Termux](https://github.com/termux/termux-app) untuk full coding.

Berhubung Termux sudah menyediakan manager untuk install keperluan development maka segera install Git, network tools, perkakas development lainnya, termasuk `tmux`.

```shell
apt update && apt upgrade -y 
apt install zsh git curl wget tmux golang nodejs-lts openjdk-21
```

Sedikit untuk kosmetik agar tampilan lebih menggugah selera bekerja juga tidak lupa: 
1. Install [Termux Styling](https://github.com/termux/termux-styling) untuk ganti tema agar tidak monoton, penjelasan ada di repo.
2. Update font Nerd FiraCode Semibold dari situs [berikut](https://www.nerdfonts.com/font-downloads).
3. ZSH dihias dengan [oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh) agar lebih ceria. 

Untuk font yang telah diunduh misal pada folder Downloads, langkahnya kurang lebih sebagai berikut:
```shell
cd /storage/shared/Downloads
mv FiraCode-SemiBold.ttf ~/.termux/font.ttf
termux-reload-settings
```

Dengan bermodal peralatan dan kosmetiknya maka siap lanjut memilah editor. By the way `tmux` dipakai untuk multiplexer terminal karena malas switch antar tab session. Untuk editor dari pertimbangan pencarian, yaitu disarankan editor yang support LSP dan Treesitter.

Apa itu LSP dan Treesitter?
| Feature | Description |
| --- | --- |
| _Language Server Protocol (LSP)_ | yang menstandarisasi komunikasi antara editor dengan bahasa pemrograman. Setiap bahasa memiliki LSPnya sendiri, berfungsi agar editor dapat mengenali bahasa pemrograman, code completion, symbol navigation, melakukan referensi, dan diagnosa kode. |
| _Treesitter_ | parser untuk teks kode menjadi lebih terstruktur seperti identifikasi symbol variable dan fungsi, karakter khusus bahasa pemrograman, syntax highlightning pewarnaan, dan hal lain terkait sintaksis. Ada 2 model yaitu AST dan CST tapi saya bukan orang yang tepat untuk menjelaskan. |

Ada beberapa opsi yaitu Vim dan Neovim, yang paling minim effort untuk setup adalah Neovim. Jadi pilihan sudah ditentukan yaitu Neovim.
 
## Setup Neovim

Termux sudah menyediakan package untuk install Neovim, jadi dapat langsung menjalankan perintah berikut termasuk cek versinya.
```shell
apt update && apt upgrade -y
apt install nvim

nvim -v
```

## Setup LazyVim

Neovim tampak menjanjikan karena dapat ditambahkan plugin seperti VSCode, unggul ringan berbasis Lua tetapi ada effort lebih pasang plugin. Karena butuh tampilan yang _VSCode like_ maka ada beberapa opsi yaitu LazyVim, NvChad, dan Astro Vim. 

> Dari ketiganya saya baru coba LazyVim dan NvChad.

Sebetulnya NvChad lebih populer di GitHub namun akhirnya keputusan tetap ke LazyVim, setelah mencoba langsung dan membandingkan ternyata LazyVim menawarkan banyak fitur bawaan di awal sedangkan NvChad masih perlu pasang plugin. Sehingga pada akhirnya lanjut dengan LazyVim.

Untuk install dapat mengikuti panduan dari dokumentasi [berikut](https://www.lazyvim.org). Khusus di Termux, ternyata butuh menginstall beberapa package sesuai kebutuhan saya yang diperlukan karena entah kenapa di NvChad sebelumnya sukses tetapi di LazyVim tidak berjalan.

2 error tersebut adalah:
1. Mason terdapat error tidak dapat install stylelua terkait `unsupported platform`.
2. LSP Go tidak berfungsi karena `SIGSYS: bad system call`.

Kemungkinan karena package yang didownload oleh NeoVim tidak compatible dengan platform Android, sehingga perlu menggunakan package yang ada di Android lalu nanti disetup terakhir. Berikut scriptnya:

```shell 
apt install lua-language-server #to fix stylua error
go install golang.org/x/tools/gopls@latest #to fix SIGNAL error
go install golang.org/x/tools/cmd/goimports@latest #to fix SIGNAL error
```

## Setup LSP dan Treesitter

Saya sendiri menambahkan beberapa plugin yang dibutuhkan beberapa di antaranya.

| Plugin | Fungsi |
| --- | --- |
| [Symbol Outline](https://github.com/hedyhli/outline.nvim) | Plugin yang bermanfaat untuk explore variable dan fungsi yang ada pada sebuah file coding. |
| [Neo Tests](https://github.com/nvim-neotest/neotest) | Plugin yang bermanfaat untuk explore daftar test dan eksekusinya. | 
| [Dadbod UI](https://github.com/kristijanhusak/vim-dadbod-ui) | Plugin yang bermanfaat untuk akses database dan Redis. |
| [Sonar Linter](https://gitlab.com/schrieveslaach/sonarlint.nvim) | Plugin yang bermanfaat sebagai linter validasi Sonar karena code rank biasa menggunakan SonarQube |

Lainnya untuk tweak konfigurasi Mason dan LSP menyesuikan daftar LSP yang diinstal serta detail setting yang dibutuhkan. Setup Mason dan LSP termasuk untuk handle error dengan mengarahkan Lua LSP, gopls, dan goimports ke bawaan OS. Untuk lengkapnya dapat ada di repo [personal saya](https://github.com/nandcep/nvim-sync).

Untuk LSP mengikuti kebutuhan kerja jadi butuh sedikit yang diinstall lewat `MasonInstall`. Beberapa di antaranya jdtls, yaml-language-server, dan typescript-language-server. Sedangkan Treesitter dapat diinstall dengan `TSInstall`. Beberapa di antaranya java, go, yaml, dan typescript.  

# Pengalaman

So far NeoVim dengan LazyVim membentuk kebiasaan baru yang biasanya GUI centric, sekarang full mekanik keyboard. Jujur hampir tidak pernah menggunakan mouse, membiasakan diri lewat shortcut keyboard yang mudah dihapal. Muscle memory agar lebih produktif dan cepat pengoperasiannya.

![](https://lh3.googleusercontent.com/pw/AP1GczM9OGZ6HXImfhG_Ank6Q0Extw7OpyXYiuj7d_ONg3xwT6VAKr0GKRinA4_7NEflhglnoYspgELN5dzkbLZOISWyCoglXRv5l0h21QVTfSjBRU1tUmkyFPkcbU1NddRyOFOu7QsDGeceb8N7g_l2SgiEEQ=w2372-h1483-s-no-gm?authuser=0)

Saya sendiri butuh seminggu rutin menggunakan baru agak terbiasa dan tulisan ini saya buat di NeoVim. Dari segi tampilan, meskipun Text-User-Interface namun susunannya mirip dengan VSCode yang ada di laptop. Jadi mata masih enak melihatnya, kelebihannya jelas NeoVim lebih ringan dan hemat baterai.

Dengan begini maka tidak ragu lagi, dapat dijadikan editor utama di Tab S9+ sebagai alternatif VSCode kalau tidak di depan laptop. Last but not least ada beberapa package tambahan yang menurut saya sangat cocok menjadi pendaking NeoVim yaitu:

1. Glow, aplikasi berbasis Go untuk render Markdown di terminal.
2. GitUI, aplikasi berbasis Rust untuk manajemen Git dengan Text-User-Interface.
