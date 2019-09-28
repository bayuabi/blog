---
title: "Self Driving Car"
date: 2019-04-26T16:37:15+07:00
draft: false
---

![Self Driving Car](/image/2019/self-driving-car/icon.JPG)

NOTE : Baris kode pada project ini dapat dilihat disini : https://github.com/bayuabi/opencv-autonomous-car

Pada proyek kali ini, saya akan membuat sebuat prototype self-driving car.
Proyek ini terinspirasi dari beberapa hal, salah dua-nya adalah Autopilot mobil Tesla yang semakin ciamik
dan obrolan ringan bareng bapak2 dosen yang menginspirasi untuk merealisasikan projek ini.
Proyek ini saya kerjakan bareng beberapa adek tingkat yang terobsesi dengan dunia riset, dan yang akan semakin
saya paksa untuk lebih terjerumus ke dunia riset ini. Mereka adalah Evan, Monifa, Ghifari dan Maya dari TE'18.

## Hardware
- Raspberry Pi 3 Model B+ --> Proses gambar dari kamera
- Raspberry Pi Camera --> Pengambil gambar
- Arduino --> Pengatur gerak motor, disini saya menggunakan Arduino disamping GPIO dari Raspberry Pi langsung karena butuh 4 pin PWM untuk driver motor,
kalau menggunakan Raspberry akan sedikit membebani proses komputasi. Komunikasi antara Raspberry dengan Arduino menggunakan Serial UART.
- Driver Motor L298N 
- Battery Lithium Ion 3.7 Volt --> disusun seri 3, total tegangan sekitar 12 volt.
- Battery Management System (BMS) 3 sel 
- Stepdown LM2596 --> stepdown ke 5V untuk catu Arduino dan Raspberry

## Software
- Bahasa Python pada Raspberry dan C++ pada Arduino
- OpenCV untuk proses pengolahan gambar
- Plan : Tensorflow untuk Machine Learning

Sekarang proyek ini masih pada tahap perakitan hardware. Seluruh update akan segera saya tulis disini.

## Update 10 Mei 2019 (23:11)
Kontrol mobil menggunakan keyboard. Video menyusul.

## Update 15 Mei 2019 (00:12)
Bisa mengenalali garis, meskipun input masih gambar, belum live video.

![Lane Detected!](/image/2019/self-driving-car/lane-detect-1.png)

Proses :

- Ubah gambar menjadi Gray
- Deteksi tepi menggunakan algoritma Canny Edge Detector (Watch here for detailed information about Canny Edge Detector : https://www.youtube.com/watch?v=sRFM5IEqR2w)
- Menemukan garis menggunakan algoritma Hough Transform (great explanation : https://www.youtube.com/watch?v=4zHbI-fFIlI)
 




