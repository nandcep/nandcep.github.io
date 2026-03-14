---
date: 2026-01-10T00:01:00+07:00
lastmod: 2026-01-10T00:01:30+07:00
authors: ["nandra"]
title: Perspektif DevSecOps dari Seorang Developer
slug: perspektif-devsecops-dari-seorang-developer
tags: [arsitektur]
---

# 👨🏻‍💻 Intro Pengalaman

Catatan berikut adalah sudut pandang yang saya dapatkan selama bertugas sebagai tim IT DevSecOps. Sebuah pandangan bagaimana seorang pengembang aplikasi di era sekarang yang harus tetap memiliki ownership dan sesuai value, value yang ditanamkan dari perusahaan di awal saya meniti karir di IT yaitu *“always strive for excellence”*

> Any fool can write code that a computer can understand. Good programmers write code that humans can understand - Martin Fowler
> 

Makna dari kutipan di atas ini sangat luas, that humans can understand juga berarti dapat terus dikembangkan. Mudah dikembangkan berarti memiliki **kualitas serta keamanan yang teruji**. Tidak dipungkiri meskipun kesempurnaan hanya milik Tuhan, namun kualitas dan keamanan menjadi asuransi bahwa setidaknya aplikasi sudah dimitigasi error dan celahnya.

Selama berkarir sebagai pengembang aplikasi saya selalu membayangkan akan selalu berkutat dengan bahasa pemrograman, mengetik sintaks, melakukan debugging, endless testing, dan shipping ke production dengan semangat. Kurang lebih bayangan tersebut sudah terwujud hampir 13 tahun selama saya melakukan pengembangan sistem.

Sense saya di tahun 2012 - 2016 mengembangkan aplikasi adalah membuat aplikasi lalu diuji oleh Quality Assurance untuk dilanjut dengan uji performa sesuai indikasi jumlah pengguna maksimal, dan terakhir diretas oleh IT Security. Apabila tidak ada temuan atau setidaknya sudah layak maka dapat deploy. Dari semua tahapan itu saya banyak mendapatkan pengalaman.

Apa saja pengalaman dari masalah yang sering saya hadapi dan bagaimana menyikapinya sebelum mengenal DevSecOps?

---

> 1. Banyak temuan setelah ditesting baik secara fungsi dan keamanan.

**Identifikasi detail dan celah requirement.** Semua requirement harus didetailkan, dari fungsi apa saja yang harus dibuat dan diuji. Sehingga menjadi test case yang utuh agar mempercepat ketika nanti ada perubahan saya dapat melihat flow mana yang terdampak dengan menjalankan test. 

Dari daftar fungsi tersebut juga melihat apa saja dependensi atau ketergantungannya apakah butuh format spesifikasi, validasi, dan integrasi.

> 2. Masih banyak bug tetapi denial terhadap kualitas dan temuan, kurang empati. 

**Diawali mereplikasi temuan bug dan issue**. Telinga dan mata masing-masing ada 2 dan mulut hanya 1, artinya mendengar dan melihat harus lebih besar porsinya. Jika ada temuan turunkan ego dan lebih penasaran bagaimana ditemukannya kekurangan dan celah. 

Acapkali mencoba sendiri secara langsung end to end (E2E) flow sebelum saya menyerahkan ke tim QA dan Security. Dan apabila masih ada temuan memilih direct call apa yang mereka lakukan dan screen capture untuk didokumentasikan secara kolaboratif.

> 3. Aplikasi lambat dan berat tetapi berlindung dibalik nama keterbatasan fungsional, produk, dan waktu. 

**Anti inferior, simplifikasi, dan tuning performa.** Sikap tidak membatasi diri sangat penting, menurunnya performa memang benar ada 2 yaitu kesalahan pengembang atau produknya jelek. Yang terparah adalah kombinasi dari keduanya. 

Yang dilakukan adalah mengenali barang yang digunakan. Baik itu tools, database, dan teknologi pendukung lainnya. Simplifikasi query dan coding contohnya agar efisien. Atau menawarkan perubahan metode misal *flow* *synchronous* menjadi *asynchronous* secara multi thread tetapi tidak merubah *outcome*. 

Atau menawarkan tools alternatif, patch, atau upgrade apabila memang sudah relevan sekali dengan issuenya. Intinya mencoba dulu analisa dan perbaiki, apabila memang perlu komitmen dari principle jangan ragu open ticket untuk meminta asistensi dan troubleshoot bersama. Tidak ada barang rusak yang tidak dapat diperbaiki atau diperbaharui.

> 4. Tidak mau upgrade dan merasa sudah aman padahal bobrok, yang penting aplikasi jalan.

**Up to date dengan teknologi dan mental paranoid dengan nama baik.** Roda bergulir, waktu berputar. Teknologi yang sudah usang tetapi nyaman memang harus tetap diupdate dan diupgrade. Karena filosofi “*The only constant is change*” memang nyata. 

Saya selalu berusaha tidak ada temuan di fase Vulnerability Asessment dari IT Security baik itu kode dan fungsi, rasanya bangga sekali apabila temuannya levelnya minor saja bahkan tidak ada temuan. 

Kenapa? Artinya dapat menghemat waktu di dalam SDLC dan juga meringankan beban orang lain yang nanti akan melanjutkan pengembangan aplikasi. Saya tidak ingin dikenal dengan “*Itu lho Adi yang buat kita rugi karena fraud dan kena bobol*”

Bagi kantor dan pribadi, jeleknya nama baik adalah resiko yang harus ditanggung bersama. 

> 5. Kabur dan taichi ke tim arsitektur dan operation ketika “*mlempem”* beraksi, tidak terlacak kurangnya, dan membuat mahal biaya.

**Forecast arsitektur fisik dan kemampuan layanan.** Harga itu relatif. Bagi saya pribadi lebih baik mahal di depan asal masuk akal. Misal aplikasi customer facing yang jumlah traffic seharinya lebih dari 1000 orang transaksi per menit. Kalau dari hasil survey pasar harganya *n* miliar IDR tetapi teruji saya tidak akan mencari yang sekian *n* juta IDR tetapi banyak muslihat.

Di atas saya membandingkan dengan penyedia jasa, baik itu infrastruktur, produk atau jasa aplikasi, dan juga tim supportnya. Kalau sudah bayar mahal, sepakat scope of worknya, dan tahu apa saja RACI maka saya sebagai pengembang tentunya tidak boleh melempar bagian Responsibility dan Action ke tim lain.

Secara pragmatis dan general, arsitek itu abstrak dan operation itu konstan. Abstrak adalah apa yang didesain, implementasinya dapat dibentuk dengan berbagai komponen dari sipil pekerjaan. Kalau konstan artinya apa yang dikerjakan itu sudah tetap, apabila tidak jelas artinya bukan pekerjaan operation.

Di dalam proses SDLC dan operation harus jadi:
1. Si paling paham bagaimana aplikasi dibuat, tidak melempar-lemparkan issue dan temuan jadi harus paham error yang ditemukan. Harus investigasi terlebih dulu apakah benar kesalahan atau sekedar ketidaktahuan ketika ada masalah.
2. Si paling teliti seberapa cost dari pembuatan aplikasi, menyiapkan baseline dalam bentuk komparasi penggunaan infrastruktur di setiap environment agar siap pakai dan sesuai budget dengan estimasi jumlah user dari tim bisnis.
3. Si gerak cepat dengan memperjelas temuan misal merapikan log, dokumentasi error code, dan menambahkan *tracer* di dalam aplikasi sehingga tim operation dapat menjalankan tugasnya.

---

Hal di atas semua gratis didapatkan dari kesalahan dan ketidakpahaman, sedih memang harus lewat ujian hidup yang sering terlambat diketahui. Tetapi dilihat sisi positifnya kalau dijadikan pembelajaran mental maka setidaknya kecil kemungkinan terulang lagi ke depannya. Dari hal itu semua, permasalahan di atas semakin mudah diatasi terjadi setelah mengadopsi DevSecOps.

# 🎯 Transformasi DevSecOps

Semua pemecahan tersebut di dalam pekerjaan sekarang menurut saya semakin dipermudah dengan adanya konsep DevSecOps (Development, Security, and Operation). Banyak proses dari validasi kualitas dan keamanan sedapat mungkin dialokasikan pada fase development. Tentunya standar dan prosesnya dari masing-masing peran.

Semua proses dan tahapannya pun dipermudah dengan dikenalkannya CI/CD pipeline juga tools yang terintegrasi di dalamnya. Dengan harapan pengembang dipermudah dalam mengidentifikasi serta semakin dewasa. Perubahan perspektif dari pengembang konvensional menjadi pengembang yang mengadopsi cara kerja DevSecOps adalah sebagai berikut.

## Kualitas adalah Hasil Kerja

Dengan diterapkan CI/CD pipeline, saya terbantu melalui tools pemindai seperti linter, test coverage, dan code quality analysis. Tidak ada lagi alasan saya tidak tahu di mana kesalahan saya. Karena di dalam CI/CD pipeline tersebut semua standar kualitas sudah dijabarkan dan dapat diselesaikan.

Kode yang tidak sesuai kaidah akan terkena linter dan diberi tahu di mana letak kesalahannya. Semakin percaya diri karena kode memenuhi requirement dengan membuat test cases yang nyata melalui unit tests. Semua hasilnya akan terukur dengan rating seperti maintainability dan code quality di code quality analysis.

Dengan alat deteksi dan workflow yang tepat maka dapat disesuaikan juga dengan bahasa pemrograman yang digunakan. Membuat terpacu di dalam menghasilkan aplikasi yang optimal untuk mencapai rating tertinggi. Harapannya dapat meningkatkan salary saya juga tentunya. Ada hasil ada income katanya.

## Keamanan adalah Poin Utama

Di dalam CI/CD pipeline juga dapat dilengkapi bahkan wajib mengadopsi tools pemindai keamanan. Misal seperti dependency scanning, secret detection, SAST, dan container scanning. Tidak ada lagi alasan bagi saya untuk bilang saya tidak paham kenapa dan bagaimana aplikasi penuh celah secara kode aplikasi.

Pustaka yang usang dan terekspos celah keamanan (CVE) akan ditolak oleh dependency scanning. Hardcoded value yang sensitif pasti terkena secret detection. Sedangkan sintaksis yang beresiko pasti akan tervalidasi oleh SAST. Dan container yang menggunakan runtime yang End of Life karena memiliki celah keamanan pasti dicegat container scanning.

Proses keamanan memang sangat berisik karena CVE dan OWASP selalu update, jadi jangan heran sudah diperbaiki bulan lalu tetapi di periode yang berdekatan akan terkena temuan baru. Namun penanganannya sangat mudah. Cukup perhatikan log serta bermodalkan Google dan AI, semua temuan report di dalam CI/CD pipeline dapat ditanyakan.  

Proses berkala ini butuh kedewasaan, karena kejahatan terus berkembang sehingga harus aware dan teliti terkait temuan. Memang masih ada pengembang yang tidak teliti melihat temuan dengan dalih pipeline error bahkan ada yang ngeyel. Sedangkan log dan report sangatlah jelas. Memang betul butuh kedewasaan, jadi saya harus ikut sabar dalam menjelaskan kepada mereka.

## Up to Date adalah Keniscayaan

Dengan adanya CI/CD pipeline yang embedded proses quality dan security membuat harus upgrade komponen menjadi tidak terhindarkan. Upgrade tidak lagi dilakukan secara periodik sirkular yang perlu review bulanan atau tahunan. Yang di mana menjadi akal-akalan pengembang untuk melakukan CR atau parahnya malah butuh cost yang mahal tanpa evaluasi menyeluruh.

Saya pernah berdebat dengan partner teknologi karena untuk security fix harus mengeluarkan biaya yang sama dengan membuat aplikasi baru. Di mana partner justru menyarankan re-engineering dengan framework lain. Atasan dan tim bisnis pun menjadi panik karena fokusnya hanya temuan security terkait End of Life komponen untuk CVE yang critical.

Padahal aplikasi tersebut juga pembuat awalnya dari partner yang dimaksud. Karena geram dengan saran tersebut partner, saya mengeluarkan ultimatum untuk cut-off kerjasama di mana sebetulnya diakibatkan partner yang lalai dalam pemeliharaan khususnya klausul overhaul komponen. Simple dan berhasil membuat harganya menjadi masuk akal.

Dari hal tersebut atasan saya mengajukan perlunya kebutuhan upgrade yang harus dapat diidentifikasi lewat temuan dalam waktu singkat yaitu menit lewat CI/CD pipeline dan diselesaikan saat itu juga dengan update komponen. Tidak lagi menjadi effort di kemudian hari karena menjadi bagian dari delivery sekaligus dapat fokus dengan fitur baru.

# DevSecOps: Kebiasaan adalah Kecepatan

Kecepatan delivery di dalam DevSecOps sangat dipengaruhi oleh kebiasaan. Di mana kecepatan sangat bergantung dari best practice engineering. Misal unit tests maka saya diharuskan suka dengan Test Driven Development (TDD), menjalankan Git strategy dengan sebelumnya memahami model Trunk Based Development (TBD), dan hemat waktu dengan templating. 

## Suka Testing

Terkait TDD saya membiasakan mindful sebelum membuat kode perlu menyiapkan kerangka fungsi yang akan ditest terlebih dahulu baru implementasinya. Saya membuat terlebih dahulu daftar fungsinya beserta ekspektasinya agar mudah diuji hasilnya saat selesai implementasi. Jadi tidak merasa unit test dibuat hanya demi memenuhi CI/CD pipeline.

Unit tests juga bermanfaat ketika handover, melalui testing dengan test case dapat menjadi warisan ke penerus agar dapat mengetahui detail cara kerja fungsi mulai dari validasi hingga proses bisnisnya secara teknis. Sehingga ketika melakukan perubahan dapat mengetahui dampaknya ke flow yang sebelumnya.

## Branching Sederhana

Saya sangat anti dengan Git Flow dan alergi branch dengan nama environment seperti development, SIT, UAT, dan production tapi tidak benci dengan pengembangnya. Simplenya Git Flow membuat proses review dan rebase kode menjadi membingungkan terkait mana yang paling latest serta faktor dari long lived branch yang menggantung tidak bertuan entah kenapa.

Saya mengalami petaka dari Git Flow ketika feature yang tidak naik-naik tetapi menyenggol fungsi existing, lalu ketika siap maka pull request ke branch environment lain yang pasti akan banyak sekali changes sekaligus menyebabkan code conflict karena ada tim lain yang hotfix di branch target. Merge back juga menyita waktu karena harus resolve conflict di banyak file.

Dengan TBD maka menjadi solusinya, pengembang wajib membuat branch feature yang harus pull request ke main branch untuk dapat melakukan deployment. Ketika pengembang commit akan langsung tervalidasi oleh CI pipeline. Ketika selesai Merge Review maka branch feature masuk ke main branch dan dihapus, lanjut eksekusi CD pipeline ke environment non-production.

Dengan begitu proses review akan lebih singkat, perubahan sudah pasti spesifik dari feature branch tertentu. Main branch adalah branch yang paling latest jadi merge back pasti harus selalu dari main branch. Bagaimana naik ke production? Yaitu melakukan tagging di main branch pada commit dengan semantic version. Repositori pun menjadi lebih clean dan clear.

## Time Saving

Terkait templating saya juga mewajibkan diri untuk selalu melakukan abstraksi dan DRY (Don’t Repeat Yourself). Misal untuk manifest Kube saya membiasakan menggunakan Helm template yang tinggal re-use template dengan mengisi valuesnya lewat YAML value. Perintah aplikasi menggunakan package manager atau Makefile. Dengan begitu waktu jadi lebih hemat.

Dampaknya apa? CI/CD pipeline jadi tidak perlu banyak custom, semua aplikasi Go dan NodeJS yang memiliki berbagai framework memiliki perintah yang sama dengan perintah `make test` atau `make build`. Aplikasi berbasis Java dengan frameworknya cukup dengan Maven seperti `mvn clean install` atau `mvn clean verify`. 

Terkait untuk menghindari perubahan value terhadap konfigurasi harus menjalankan CI/CD karena dianggap mengubah kode maka memisahkan repositori untuk aplikasi dengan konfigurasi. Harapannya ketika mengubah manifest maka tidak perlu membuang waktu dengan menunggu hasil dan dapat ditrigger melalui CD pipeline.

Dan yang paling disuka dari tujuan time saving adalah harus mengautomasi banyak hal mulai dari migrasi database, VA URL, hingga integration tests. Dengan melakukan shift-left proses ke dalam CI/CD yang tadinya harus manual di server dan terpisah membuat waktu semakin efektif untuk hal lain yang lebih penting.

# ✍🏽 Kesimpulan

DevSecOps sangat mempermudah saya dalam menganalisa masalah, mengukur level delivery, dan selalu update dalam mengembangkan aplikasi. Di mana untuk mengevaluasinya kebenarannya dapat mengadopsi DORA Metrics sehingga dapat mengidentifikasi maturity level saya di dalam bekerja sebagai pengembang aplikasi.

Juga percaya bahwa dengan perkembangan teknologi AI akan membuat DevSecOps menjadi bantu loncatan yang lebih signifikan semisal melakukan automasi code review terhadap standarisasi seperti konvensi kode internal. Tentunya masih banyak use case yang dapat melengkapi dan diselesaikan dengan DevSecOps.