## 2.4 Percobaan Transmisi Data Melalui EIA-485

### 2.4.1 Transmitter Mengirim Data Ke Receiver

**Kebutuhan Komponen**

Untuk melakukan percobaan komunikasi data melaui EIA-485 dibutuhkan beberapa modul komponen berikut ini :

1. [Arduino Uno](https://store.arduino.cc/usa/arduino-uno-rev3) 2 pcs
2. [RS485 Shield](http://linksprite.com/wiki/index.php5?title=RS485_Shield_V2.1_for_Arduino) 2 pcs
3. Modul Push Button 1 pcs
4. Modul LED 1 pcs

**Gambar Percobaan  
**![](/assets/Webp.net-resizeimage.jpg)

Koneksi

1. 485-A Transmitter ke 485-A Receiver
2. 485-B Transmitter ke 485-B Receiver
3. Modul Button ke pin 4 Transmitter
4. Modul LED ke pin 4 Receiver

**Langkah Percobaan**

1. Hubungkan semua modul seperti pada gambar percobaan diatas
2. Buatlah program pada Arduino yang berfungsi sebagai pengirim seperti berikut ini. Program berikut ini berfungsi untuk mendeteksi penekanan tombol dan mengirimkan data ke receiver melalui EIA-485. Jika tombol ditekan, data yang dikirim adalah 255 dan jika tombol tidak ditekan data yang dikirimkan adalah 0.
   `#include <SoftwareSerial.h>   `

   `#define BUTTON 4   `

   `SoftwareSerial 485Transmitter(3, 2);   `

   `   void setup() {   `

   `  485Transmitter.begin(9600);   `

   `  pinMode(BUTTON, INPUT_PULLUP);   `

   `}   `

   `   void loop() {   `

   `  if (digitalRead(BUTTON) == LOW) {   `

   `    485Transmitter.write(255);   `

   `  }   `

   `  else {   `

   `    485Transmitter.write(0);   `

   `  }   `

   `}`

3. Buatlah program pada Arduino yang berfungsi sebagai penerima data seperti berikut ini. Program berikut ini berfungsi untuk menerima data dari transmitter. Data yang diterima akan dibandingkan nilainya. Jika data bernilai 0 maka lampu LED akan mati, jika data yang diterima bernilai 255 maka lampu LED akan menyala.
   `#include <SoftwareSerial.h>   `

   `#define LED 4   `

   `SoftwareSerial 485Receiver(3, 2);   `

   `   void setup() {   `

   `  485Transmitter.begin(9600);   `

   `  pinMode(LED, OUTPUT);   `

   `}   `

   `   void loop() {   `

   `  if (485Receiver.available()) {   `

   `    unsigned char data = 485Receiver.read();   `

   `    if (data == 255) {   `

   `      digitalWrite(LED, 1);   `

   `    }   `

   `    else {   `

   `      digitalWrite(LED, 0);   `

   `    }   `

   `  }   `

   `}`

4. Lakukan uji coba dengan melakukan penekanan tombol pada arduino transmitter kemudian perhatikan nyala LED pada arduino receiver.  



