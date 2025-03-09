---
title: Install Axis and Allies (2004) di Mac OS
date: 2015-07-19 00:00:01
categories: []
tags: [mac,teknologi,game-porting]     # TAG names should always be lowercase
---
![](https://www.gamespot.com/a/uploads/original/gamespot/images/2004/reviews/617606-915000_20041102_003.jpg)
_Credit to Gamespot_

[Axis & Allies](https://en.wikipedia.org/wiki/Axis_%26_Allies_(2004_video_game)) adalah game strategi perang dunia 2 besutan Atari yang pertama kali saya mainkan di sekitar tahun 2004. Mainnya menumpang di rumah Martdoman Sitorus karena dia satu-satunya teman SMP yang punya PC high-end di jaman itu. Berhubung game tersebut adalah game yang khusus untuk PC maka perlu diporting terlebih dahulu.

Setelah menemukan game Axis & Allies di torrent, kemarin malam saya langsung download menggunakan Bolt. Tidak sampai setengah jam 1.5 GB sudah selesai jaringan kost mendapatkan 800 kb/s. Step yang saya jalankan agar dapat memainkannya di MacBook:

1. Nyalakan VM Windows XP.
2. Install Daemon Tools, mount, install gamenya.
3. Jangan lupa overwrite dengan cracknya agar tidak perlu mount CD lagi.
4. Copy installed game ke lokal Mac.
5. Buka Wineskin dan buatkan satu wrapper, misal saya beri nama “Axis & Allies”.
6. Klik show content untuk wrappernya, move folder yang tadi udah di pindah ke local Mac ke `drive_c/Program Files/`.
6. Double click Wineskin di dalam wrapper. Di bagian Advance, set Windows EXEnya i.e `/Program Files/Atari/Axis & Allies/AA.exe`.
7. Pada bagian Screen saya set full screen dan adjust resolusi sesuai real screen resolution yang ada di MacBook.
8. Jangan tick “Use Mac Driver instead of X11” soalnya layarnya jadi kedip-kedip / flickr.
9. Test terlebih dahulu sebelum release.

Langkah-langkahnya di atas sebetulnya sama dengan ketika porting game DotA, AoE, DivineKids, dan lain sebagainya. Saatnya bernostalgila. Sampai dengan pukul 2.15 pagi ini saya baru selesai bermain game Axis & Allies. Durasi sekali skirmish cukup lama hingga 2.5 jam.

_Disadur dari blog lama saya di WordPress.com_