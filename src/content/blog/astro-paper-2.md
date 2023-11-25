---
author: Rusli Anwar
pubDatetime: 2023-11-25T13:22:00Z
title: Apa itu KernelSU: Solusi Root Kernel-Based untuk Android
postSlug: apa-itu-kernelsu-solusi-root-kernel-based-untuk-android
featured: true
draft: false
tags:
  - docs
description:
  Apa itu KernelSU dan bagaimana cara kerjanya.
---

Melakukan rooting pada Android itu ibarat memberi kita kendali penuh dalam mengatur sistem operasi di ponsel kita. Kita bisa mengubah tampilan, menghapus aplikasi bawaan (bloatware) yang tidak penting, dan mengatur semuanya sesuai kebutuhan. Namun, rooting juga ada bahayanya kalau kamu itu belum memiliki pemahaman teknis dasar. Jika lengah bisa saja hp yang ingin di root malah menjadi softbrick sehingga menimbulkan error dan yang paling fatal adalah crash alias mati total.

Nah untuk itu ada developer asal negeri tirai bambu dengan akun github bernama Tiann, asal Shen Zhen, China, yang menciptakan metode rooting Android berbasis kernel bernama KernelSU. Kemudian, di bulan desember 2022 dia nge rilis untuk kali pertama dengan versi v0.1.3.

KernelSU ibarat alat rooting yang mudah digunakan dan dikenal stabil. Cara kerjanya seperti menginstal patch pada kernel (dalam bahasa opreker, menambal kernel) perangkat Kita, yang memberi Kita akses penuh tanpa mengubah file sistem itu sendiri. Jadi, KernelSU diklaim lebih aman dibandingkan alat rooting serupa yang sudah kita kenal seperti SuperSU, Magisk ataupun Magisk Delta.

![Static Site generator Flow](@assets/images/KernelSU-logo.png)

## Table of contents

## Apa itu KernelSU?

KernelSU adalah solusi root berbasis kernel untuk perangkat Generic Kernel Image Android. Metode ini bekerja dalam mode Linux kernel dan memberikan izin root ke aplikasi pada lapisan diatas userspace secara langsung di ruang kernel. KernelSU juga menyediakan sistem modul melalui overlayfs, yang memungkinkan untuk memuat module ke dalam sistem. KernelSU sendiri merupakan proyek open-source di bawah lisensi GPL-3.

## Fitur KernelSU

* **Berbasis kernel:** KernelSU bekerja dalam mode kernel dan tidak memerlukan aplikasi pengguna.
* **Kontrol akses Whitelist:** Hanya aplikasi yang diberikan izin root yang dapat mengakses `su`, aplikasi lain tidak dapat mengakses `su`.
* **Dukungan modul:** KernelSU mendukung modifikasi `/system` tanpa sistem melalui overlayfs, bahkan dapat membuat sistem dapat ditulis (R/W).
* **KernelSU** juga menyediakan mekanisme untuk dapat memodifikasi berkas-berkas pada partisi `/system`.

## Persyaratan Instalasi

Sebelum melakukan rooting menggunakan KernelSU ada beberapa hal yang harus diperhatikan, berikut ini syarat untuk dapat menggunakan KernelSU:

* Perangkat harus menjalankan `Android 12` atau lebih tinggi.
* Perangkat harus memiliki kernel versi `5.10` atau lebih tinggi.
* Bootloader perangkat harus dalam keadaan sudah di-unlock.
* Saya menyarankan perangkat yang kamu gunakan sudah memiliki `Custom Recovery`.

Selain syarat di atas, ada beberapa hal yang perlu diperhatikan sebelum menggunakan KernelSU, yaitu:

* KernelSU belum mendukung semua jenis perangkat Android.
* KernelSU tidak selalu berfungsi dengan Custom ROM  terbaru. Jika Kamu menggunakan Custom ROM khusus, sebaiknya periksa apakah ROM tersebut sudah kompatibel dan tersedia dengan kernel yang mendukung KernelSU.
* KernelSU dapat menyebabkan masalah terhadap aplikasi tertentu. Jika Kamu mengalami masalah dengan aplikasi tertentu setelah menginstal * KernelSU, Kamu dapat mencoba `A/B` testing atau mencopot pemasangan (Uninstall) KernelSU terlebih dahulu untuk melihat dan memastikan, apakah masalah tersebut dapat teratasi.

## Tahap Uji Coba Kompatibilitas

Periksa apakah perangkat yang kamu gunakan sudah mendukung, dengan cara:

1. Unduh aplikasi manajer KernelSU dari halaman resmi github releases, kemudian instal aplikasi dan buka aplikasi KernelSU.
2. Setelah membukan, jika aplikasi menunjukkan pesan `Unsupported`, menandakan bahwa kamu harus mengkompilasi kernel perangkat kamu sendiri (tidak saya rekomendasikan) karena butuh pemahaman yang mendalam terkait build kernel itu sendiri.
3. Namun, pesan yang menunjukkan `Not installed`, 95% maka perangkat kamu sudah mendukung KernelSU.

**Catatan:** Disini saya tidak akan memberikan panduan dari awal melakukan instalasi, karena seperti yang sudah saya jelaskan diatas, bahwa belum banyak perangkat yang mendukung untuk melakukan rooting menggunakan KernelSU.

## Mengelola Izin Root dengan KernelSU

Setelah Kamu melakukan root pada perangkat Kamu dengan KernelSU, Kamu dapat menggunakan aplikasi KernelSU untuk mengelola izin root. Kamu dapat mengaktifkan atau menonaktifkan akses root ke aplikasi, serta mengatur izin untuk masing-masing aplikasi yang ada dengan hanya sekali aksi.

Untuk mengelola dan mengizinkan akses root dengan KernelSU, buka aplikasi dan ketuk tab `SuperUser`. Kamu akan melihat daftar semua aplikasi di perangkat. Selanjutnya, kamu dapat mengaktifkan atau menonaktifkan akses root untuk setiap aplikasi dengan mengetuk tombol `SuperUser` di sebelah nama aplikasi tersebut, sesimpel itu.

## Pengalaman Menggunakan KernelSU

Selama kurang lebih 10 bulan sejak pertama kali menggunakan KernelSU (Februari 2023), saya hanya mendapati beberapa masalah kecil yang mampu saya perbaiki sendiri, terlebih dengan tampilan dan menu yang sederhana tentu hal ini menjadi nilai tambah bagi KernelSU. Sejak pertama kali nge-root dan nge-unlock ditahun awal-awal 2011, dari semua metode root yang pernah saya coba, yang praktis dan aman bisa dibilang, KernelSU sebagai juaranya. Walaupun, bisa dibilang metode ini terasa familiar dengan Magisk delta namun hal yang mendasari keduanya adalah metode rootingnya. KernelSU bekerja dengan memodifikasi pada kernel, sedangkan Magisk Delta bekerja dengan memodifikasi kode sumber Magisk itu sendiri.

## Kesimpulan

Melakukan rooting perangkat Android dengan KernelSU adalah proses yang relatif aman dan mudah. Namun, penting untuk diingat bahwa rooting memiliki beberapa risiko. Jika tidak nyaman dengan risikonya, sebaiknya biarkan perangkat Kamu tidak di-root dan biarkan dalam kondisi default.

Jika Kamu memutuskan untuk melakukan root pada perangkat Kamu dengan KernelSU, dan siap menerima risiko yang ada, pastikan untuk mengikuti instruksi dengan seksama. Dan yang terpenting, pastikan untuk membuat cadangan data (backup data) sebelum memulai rooting. Dengan cara ini, jika terjadi kesalahan, Kamu dapat mengembalikan perangkat ke keadaan semula.

Saya harap artikel ini bermanfaat! Jika memiliki pertanyaan tentang rooting perangkat Android dengan KernelSU, feel free to contact me.
