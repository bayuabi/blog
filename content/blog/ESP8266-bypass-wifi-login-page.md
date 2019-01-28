---
title: "ESP8266 Bypass Wifi Login Page"
date: 2019-01-26T16:37:15+07:00
draft: false
---

Ketika menggunakan ESP8266 (Wemos, Nodemcu, dll) untuk projek IoT, tentunya kita membutuhkan jaringan wifi untuk berkomunikasi melalui jaringan internet.
Dalam baris kode yang kita tulis biasanya kita diharuskan untuk memasukkan SSID dan password wifi yang kita gunakan.
Tetapi kadang kala wifi yang kita gunakan mengharuskan kita untuk mengisi sebuah halaman login, seperti wifi kampus, wifi corner, wifi kantor, dll.
Halaman login tersebut biasanya mengharuskan kita untuk memasukkan username dan password yang telah ditentukan.
Pada tutorial kali ini, saya akan menunjukkan bagaimana cara login wifi yang mengharuskan mengisi halaman login menggunakan board berbasis ESP8266.

Tulisan ini terinspirasi dari post [Ristie UB](http://ristie.ub.ac.id/proyek/134-login-nas-ub-ac-id-tanpa-web-browser.html) yang membahas bagaimana
caranya login ke halaman wifi UB (Universitas Brawijaya) tanpa harus membuka aplikasi browser.

Pada tulisan ini saya akan menggunakan halaman login wifi UB (Universitas Brawijaya) sebagai contoh.
Board yang saya gunakan untuk mencoba adalah Wemos D1 Mini, kamu bisa menggunakan board apapun yang berbasis ESP8266.
Untuk menulis baris kode, saya menggunakan Arduino IDE yang sudah diinstall board ESP8266,
kamu bisa melihat tutorial [ini](https://github.com/esp8266/Arduino) jika belum menginstall board ESP8266 pada Arduino IDE.

## Perintah login pada wifi UB
![Login wifi UB](/image/login_nas-300x269.jpg)

Seperti terlihat pada gambar di atas, halaman login wifi UB membutuhkan username dan password untuk bisa menggunakan akses wifi.
Halaman login wifi UB menggunakan protokol HTTP dengan metode POST untuk komunikasi antara client dan server.
Dengan menggunakan metode POST maka username dan password tidak mudah diketahui oleh orang lain karena tidak akan ditampilkan pada halaman browser.

![POST pada halaman login wifi UB](/image/post-wifi-ub.jpg)

Pada halaman login digunakan variabel "opr", "userName", "pwd", dan "rememberPwd" yang dikirimkan ke alamat "http://nas.ub.ac.id/ac_portal/login.php"

## Penerapan pada board ESP8266
Untuk mengakses wifi menggunakan board ESP8266, pertama kita perlu menambahkan beberapa library dan mendeklarasikan konstanta yang akan digunakan.

```c
#include <ESP8266WiFi.h>
#include <ESP8266HTTPClient.h>

const char* WIFI_SSID = "YOUR WIFI SSID";
const char* WIFI_PASS = "YOUS WIFI PASSWORD";
const char* NIM = "YOUR NIM"; //username pada halaman login wifi
const char* PASS = "YOUR SIAM PASSWORD"; //password pada halaman login wifi
```

Ganti variabel diatas sesuai dengan parameter wifi yang kamu gunakan.
Pada wifi UB, mahasiswa biasanya menggunakan NIM dan password Sistem Informasi kampus untuk login ke wifi.
Selanjutnya kita akan memulai koneksi ke wifi dengan menggunakan kode :

```c
WiFi.begin(WIFI_SSID, WIFI_PASS);
```

Untuk menggunakan protokol HTTP pada ESP8266 kita perlu menginisialisasikan pada kode seperti berikut :

```c
HTTPClient http;
http.begin("http://nas.ub.ac.id/ac_portal/login.php");
```

Karena menggunakan metode POST kita perlu menambahkan header.

```c
http.addHeader("Content-Type", "application/x-www-form-urlencoded");
```

Selanjutnya kita mengirimkan nilai variabel yang telah disebutkan di atas menggunakan metode POST

```c
String kirim = "opr=pwdLogin&userName=";
kirim += NIM;
kirim += "&pwd=";
kirim += PASS;
kirim += "&rememberPwd=1";
int httpCode = http.POST(kirim);
if(httpCode > 0){
  Serial.println(http.getString());   
}
```

Untuk mengetahui keberhasilan kode yang sudah dibuat, akan ditampilkan response dari server pada serial monitor menggunakan kode seperti berikut :

```c
if(httpCode > 0){
  Serial.println(http.getString());   
}
```

Apabila berhasil, pada serial monitor akan tampil tulisan yang sama seperti saat kita login menggunakan browser.

![Sukses Login](/image/sukses-login.jpeg)

Berikut ini adalah kode lengkapnya. Kamu juga bisa melihat di repository [github](https://github.com/bayuabi/esp8266-auto-nas/blob/master/ESP8266_AutoNAS.ino) saya.

```c
#include <ESP8266WiFi.h>
#include <ESP8266HTTPClient.h>

const char* WIFI_SSID = "YOUR WIFI SSID";
const char* WIFI_PASS = "YOUS WIFI PASSWORD";
const char* NIM = "YOUR NIM";
const char* PASS = "YOUR SIAM PASSWORD";

void setup() {
  Serial.begin(9600);
  WiFi.begin(WIFI_SSID, WIFI_PASS);
  while(WiFi.status() != WL_CONNECTED){
    delay(200);
    Serial.println("Connecting...");
  }
  HTTPClient http;
  http.begin("http://nas.ub.ac.id/ac_portal/login.php");
  http.addHeader("Content-Type", "application/x-www-form-urlencoded");
  String kirim = "opr=pwdLogin&userName=";
  kirim += NIM;
  kirim += "&pwd=";
  kirim += PASS;
  kirim += "&rememberPwd=1";
  int httpCode = http.POST(kirim);
  if(httpCode > 0){
    Serial.println(http.getString());   
  }
}

void loop() {

}
```

Sekian tulisan kali ini. Semoga bermanfaat.
Kamu dapat bertanya, memberi kritik atau saran lewat kolom komentar.

Credit:

-http://ristie.ub.ac.id/proyek/134-login-nas-ub-ac-id-tanpa-web-browser.html
