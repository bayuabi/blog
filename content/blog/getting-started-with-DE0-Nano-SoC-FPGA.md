---
title: "Getting Started With DE0 Nano SoC FPGA"
date: 2019-01-29T00:43:08+08:00
draft: false
---

Pada tulisan kali ini saya akan berbagi mengenai pemrogaman FPGA menggunakan bahasa pemrogaman VHDL.
Untuk yang belum mengerti mengenai apa itu FPGA dan VHDL bisa dilihat dulu sekilas tentang keduanya di link berikut :
- [FPGA](https://id.wikipedia.org/wiki/FPGA)
- [VHDL](https://id.wikipedia.org/wiki/VHDL)

Board FPGA yang akan saya gunakan adalah DE0 Nano SoC. 
Pada board tersebut terdapat Sytem on Chip (SoC) yang terdiri dari FPGA dan Hard Processing System (HPS) ARM 9 yang menggunakna chip Cyclone V.

Download file [De0-Nano-SoC Manual](https://drive.google.com/open?id=11Id_EzIWZZTbf9bBxO8D6MsoqyguA6mb) untuk mengetahui lebih lanjut tentang spesifikasi dari DE0 Nano SoC.

## Instalasi Software
Download Quartus Prime di website [Intel](http://fpgasoftware.intel.com/?edition=lite)

Pilih Quartus Prime, ModelSim, dan device Cyclone V.

Pastikan file yang anda pilih sesuai dengan OS yang kamu gunakan.
Pada tutorial ini saya menggunakan OS Windows.

Jalankan file setup dan ikuti langkah-langkahnya 

Untuk melihat apakah board terdeteksi komputer, tancapkan kabel power dan USB.
Buka device manager dan pastikan device JTAG Progammer sudah terinstall dengan sempurna. 

## Membuat Project Baru
Jalankan software Quartus Prime, dan pilih New Project Wizard

Beri nama dan simpan project pada folder yang kamu inginkan.

![File Name](/image/2019/getting-started-de0-nano-soc-fpga/save_file.PNG)

Pilih Empty Project pada Project Type

Klik Next pada Add File

Pada Family, Device, and Board Settings pilih jenis chip sesuai board yang kamu gunakan.
Biasanya terdapat tulisan di IC board. Untuk board yang saya gunakan, target device saya adalah 5CSEMA4U23C6.

![Device Choose](/image/2019/getting-started-de0-nano-soc-fpga/chip-choose.PNG)

Klik Next pada EDA Tool Setting

Klik Finish pada Summary

Pada toolbar File pilih New kemudian klik VHDL File

![New VHDL File](/image/2019/getting-started-de0-nano-soc-fpga/New.PNG)

## Menulis Program VHDL

Pada tutorial ini, kita akan membuat sebuah rangkaian kombinatorial sederhana yaitu multiplexer sesuai dengan rangkaian berikut :

![Rangkaian Multiplexer](/image/2019/getting-started-de0-nano-soc-fpga/Rangkaian-Kombinasional.PNG)

Pada bahasa pemrogaman VHDL yang pertama kita tulis adalah library yang kita gunakan.
Kita akan menggunakan library dasar pada VHDL yaitu sebagai berikut

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
```

Selanjutnya kita menulis entity. 
Entity pada VHDL seperti pendeklarasian modul yang akan kita buat.
Pada Entity kita bisa mendeklarasikan Port (Input/Output), Generic (seperti konstanta), dan deklarasi yang lainnya.

```vhdl
entity multiplexer is
	Port(
		x, y, s : in STD_LOGIC;
		m : out STD_LOGIC
	);
end entity;
```

Maksud dari kode diatas adalah, kita mendeklarasikan Port yang akan digunakan.
Sesuai dengan gambar rangkaian di atas, x, y, s menjadi input dari rangkaian sehingga pada kode ditulis 'in' dan m sebagai output sehingga ditulis 'out'.
Kode STD_LOGIC pada VHDL seperti tipe data yang digunakan. STD_LOGIC merupakan pendeklarasian untuk input/ouput 1 bit.

Selanjutnya kita menulis architecture pada modul yang dibuat. Architecture merupakan isi dari program VHDL yang dibuat.

```vhdl
architecture dataflow of multiplexer is
begin
	m <= (x and (not s)) or (s and y);
end dataflow;
```

Pada program diatas dapat mudah dimengerti bagaimana implementasi gerbang logika sesuai gambar diatas dengan menggunakan syntax 'and', 'or', 'not'.
Selain itu pada vhdl juga bisa membuat gerbang 'xor', dan 'xnor'.
Maksud dari kata 'dataflow' diatas hanyalah variabel, kamu bisa menggantinya sesuai dengan kemauan.
Dataflow sendiri merupakan salah satu jenis modelling pada VHDL. Selain dataflow juga terdapat Structural dan Behavioral modelling.
Kamu bisa membaca [disini](https://buzztech.in/vhdl-modelling-styles-behavioral-dataflow-structural/) untuk mengetahui perbedaan antara ketiganya.

Berikut ini kode lengkapnya:

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity multiplexer is
	Port(
		x, y, s : in STD_LOGIC;
		m : out STD_LOGIC
	);
end entity;

architecture dataflow of multiplexer is
begin
	m <= (x and (not s)) or (s and y);
end dataflow;
```

Simpan file VHDL, beri nama sesuai dengan nama entity yang kamu buat, kalau pada program diatas namanya 'multiplexer'.

Jadikan file multiplexer.vhd menjadi top level entity dengan klik kanan pada file kemudian pilih 'Set as top level entity'

![Top Level Entity](/image/2019/getting-started-de0-nano-soc-fpga/Top-Level.png)

## Analysis dan Compile

Setelah membuat program, langkah selanjutnya adalah proses Analysis dan Compile.
Sebelum compile keseluruhan program, terlebih dahulu kita periksa apakah kode yang kita buat sudah benar apa belum dengan menggunakan tool Analysis and Elaboration dengan memilih 
logo seperti berikut.

![Analize and Elaboration](/image/2019/getting-started-de0-nano-soc-fpga/analize.PNG)

Selanjutnya pada toolbar Assigments pilih menu Pin Planner untuk menyambungkan program dengan hardware yang kita gunakan.
Untuk mengetahui pin pada DE0-Nano-SoC, buka link file DE0-Nano-SoC Manual diatas.
Tulis pin yang akan digunakan pada Pin Planner.

Saya menggunakan Switch yang ada pada board DE0-Nano-SoC sebagai input dan LED sebagai output.

![Pin Table](/image/2019/getting-started-de0-nano-soc-fpga/Pin-Table.PNG)

![Pin Planner](/image/2019/getting-started-de0-nano-soc-fpga/Pin-Planner.PNG)

Apabila sudah selesai pada bagian Pin Planner, Compile program dengan memilih tool Compile.

![Compile](/image/2019/getting-started-de0-nano-soc-fpga/Compile.png)

Pastikan tidak terjadi error pada program yang kita tulis.

## Synthesize (Upload) program pada Board

Untuk upload program pada board, pilih tool Programmer.

Klik 'Hardware Setup' dan pilih DE-SoC [USB-x].

Klik 'Auto Detect' dan pilih device yang kamu gunakan, seperti 5CSEMA4.

Klik kiri pada gambar chip FPGA, yang ada tulisan 5CSEMA4, kemudian klik kanan dan pilih Change File.

Pilih file dengan ekstensi .sof pada folder output_file.

Centang Program/Configure.

Klik 'Start' untuk memulai pemrogaman.

Apabila berhasil akan muncul tulisan 100% (Success) seperti gambar dibawah.

![Sukses](/image/2019/getting-started-de0-nano-soc-fpga/sukses.PNG)

Ubah posisi switch dan lihat perubahan pada LED. Buktikan kebenaran logika sesuai gambar rangkaian gerbang diatas.

![Board](/image/2019/getting-started-de0-nano-soc-fpga/board.JPG)

Sekian tulisan kali ini, semoga bisa bermanfaat. 

Nantikan tulisan-tulisan tentang FPGA selanjutnya pada blog ini.