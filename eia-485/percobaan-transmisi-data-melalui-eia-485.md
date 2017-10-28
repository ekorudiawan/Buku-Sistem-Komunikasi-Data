## 2.4 Percobaan Transmisi Data Melalui EIA-485

### 2.4.1 Master Mengirim Data Ke Slave

Percobaan kali ini bertujuan untuk mengaplikasikan pengriman data melalui EIA-485. Perangkat yang digunakan adalah mikrokontroler dengan tambahan driver EIA-485. Perangkat yang akan dihubungkan sebanyak dua perangkat. Satu perangkat bertindak sebagai master atau pengendali komunikasi data. Satu perangkat lagi bertindak sebagai slave. Master akan dihubungkan dengan tombol push button dan slave akan dihubungkan dengan LED. Master akan mengirimkan data 1 byte yang bernilai 100 atau 255 tergantung dari kondisi tombol. Jika tombol ditekan master akan mengirimkan data 255, namun jika tombol tidak ditekan master akan mengirimkan data 100. Slave akan merespon pengiriman data ini dengan menyalakan LED jika data yang diterima slave 255 dan akan mematikan LED jika data yang diterima slave 100.

**Kebutuhan Komponen**

Untuk melakukan percobaan komunikasi data melaui EIA-485 dibutuhkan beberapa modul komponen berikut ini :

1. [Arduino Uno](https://store.arduino.cc/usa/arduino-uno-rev3) 2 pcs
2. [RS485 Shield](http://linksprite.com/wiki/index.php5?title=RS485_Shield_V2.1_for_Arduino) 2 pcs
3. [Modul Push Button](https://www.dfrobot.com/product-1098.html) 1 pcs
4. [Modul LED](https://www.dfrobot.com/product-490.html) 1 pcs

**Gambar Percobaan                
**![](/assets/Webp.net-resizeimage.jpg)

Koneksi

1. 485-A Transmitter ke 485-A Receiver
2. 485-B Transmitter ke 485-B Receiver
3. Modul Button ke pin 4 Transmitter
4. Modul LED ke pin 4 Receiver

**Langkah Percobaan**

1. Hubungkan semua modul seperti pada gambar percobaan diatas
2. Buatlah program pada Arduino yang berfungsi sebagai pengirim seperti berikut ini. Program berikut ini berfungsi untuk mendeteksi penekanan tombol dan mengirimkan data ke receiver melalui EIA-485. Jika tombol ditekan, data yang dikirim adalah 255 dan jika tombol tidak ditekan data yang dikirimkan adalah 100.

   ```
   #include <SoftwareSerial.h>
   #define BUTTON 4

   SoftwareSerial _485Master(3, 2);

   void setup() {
     _485Master.begin(9600);
     pinMode(BUTTON, INPUT_PULLUP);
   }

   void loop() {
     if (digitalRead(BUTTON) == LOW) {
       _485Master.write(255);
     }
     else {
       _485Master.write(100);
     }
   }
   ```

3. Buatlah program pada Arduino yang berfungsi sebagai penerima data seperti berikut ini. Program berikut ini berfungsi untuk menerima data dari transmitter. Data yang diterima akan dibandingkan nilainya. Jika data bernilai 100 maka lampu LED akan mati, jika data yang diterima bernilai 255 maka lampu LED akan menyala.

   ```
   #include <SoftwareSerial.h>
   #define LED 4

   SoftwareSerial _485Slave(3, 2);

   void setup() {
     _485Slave.begin(9600);
     pinMode(LED, OUTPUT);
   }

   void loop() {
     if (_485Slave.available()) {
       unsigned char data = _485Slave.read();
       if (data == 255) {
         digitalWrite(LED, 1);
       }
       else {
         digitalWrite(LED, 0);
       }
     }
   }
   ```

4. Lakukan uji coba dengan melakukan penekanan tombol pada arduino transmitter kemudian perhatikan nyala LED pada arduino receiver.

### 2.4.2 Master Request Data Ke Slave

Percobaan kali ini masih menggunakan perangkat yang sama seperti percobaan sebelumnya. Namun pada percobaan kali ini tombol push button dihubungkan pada slave dan LED dihubungkan pada master. Proses komunikasi data sedikit berbeda dengan percobaan yang pertama. Pada percobaan kali ini master akan melakukan request data ke slave dengan cara mengirimkan data terlebih dahulu ke slave. Nilai data yang akan dikirimkan ke slave untuk merequest data adalah 255. Slave akan merespon permintaan request data dari master. Kemudian slave akan mengirimkan data yang direquest oleh master. Data yang dikirimkan oleh slave nilainya tergantung dari kondisi penekanan tombol pada slave. Jika tombol ditekan slave akan mengirimkan data 255 dan ketika tombol tidak ditekan slave akan mengirimkan data 100. Master akan merespon data yang telah dikirimkan oleh slave atas request sebelumnya. Master akan membandingkan nilai data tersebut. Jika data bernilai 255 maka master akan menyalakan LED dan jika data yang diterima adalah 100 maka master akan mematikan LED.

**Kebutuhan Komponen**

Untuk melakukan percobaan komunikasi data melaui EIA-485 dibutuhkan beberapa modul komponen berikut ini :

1. [Arduino Uno](https://store.arduino.cc/usa/arduino-uno-rev3) 2 pcs
2. [RS485 Shield](http://linksprite.com/wiki/index.php5?title=RS485_Shield_V2.1_for_Arduino) 2 pcs
3. [Modul Push Button](https://www.dfrobot.com/product-1098.html) 1 pcs
4. [Modul LED](https://www.dfrobot.com/product-490.html) 1 pcs

**Gambar Percobaan                
**![](/assets/Webp.net-resizeimage.jpg)

Koneksi

1. 485-A Transmitter ke 485-A Receiver
2. 485-B Transmitter ke 485-B Receiver
3. Modul Button ke pin 4 Receiver
4. Modul LED ke pin 4 Transmitter

**Langkah Percobaan**

1. Hubungkan semua modul seperti pada gambar percobaan diatas
2. Buatlah program pada Arduino yang berfungsi sebagai master seperti berikut ini. Program berikut ini berfungsi untuk mengirim data 255 ke arduino slave sebagai petanda bahwa arduino master ingin membaca data pada arduino slave. Jika data yang di request telah dikirimkan oleh arduino slave maka arduino master akan menerima data tersebut dan membandingkan nilai datanya. Jika data yang diterima dari arduino slave adalah 255 maka arduino master akan menyalakan LED. Namun jika data yang diterima dari arduino slave adalah 100 maka arduino master akan mematikan LED.  
   `#include <SoftwareSerial.h>`

   `#define LED 4`

   `SoftwareSerial _485Master(3, 2);`

   `void setup() {`

   `_485Master.begin(9600);`

   `pinMode(LED, OUTPUT);`

   `}`

   `void loop() {`

   `// Kirim data ke master untuk request data`

   `_485Master.write(255);`

   `// Cek data yang dikirimkan slave`

   `if(_485Master.available()) {`

   `// Baca data dari slave`

   `unsigned char dataFromSlave = _485Master.read();`

   `if(dataFromSlave == 255) {`

   `digitalWrite(LED, 1);`

   `}`

   `else if(dataFromSlave == 0) {`

   `digitalWrite(LED, 0);`

   `}`

   `}`

   `}`

3. Buatlah program pada Arduino slave seperti berikut ini. Program berikut ini berfungsi untuk menerima data dari arduino master. Kemudian jika data yang diterima bernilai 255 atau master ingin melakukan request data, maka Arduino slave akan meresponnya dengan mengirimkan data 100 jika button pada arduino slave tidak ditekan. Jika button ditekan data yang dikirimkan oleh arduino slave adalah 255.  
   `#include <SoftwareSerial.h>`

   `#define BUTTON 4`

   `SoftwareSerial _485Slave(3, 2);`

   `void setup() {`

   `_485Slave.begin(9600);`

   `pinMode(BUTTON, INPUT_PULLUP);`

   `}`

   `void loop() {`

   `// Cek apakah ada data yang dikirimkan master`

   `if(_485Slave.available()) {`

   `// Baca data dari master`

   `unsigned char dataFromMaster = _485Slave.read();`

   `// Jika data dari master 255`

   `// Master request data`

   `if(dataFromMaster == 255) {`

   `// Cek kondisi button`

   `// Kirim data ke master`

   `if(digitalRead(BUTTON)==LOW) {`

   `_485Slave.write(255);`

   `}`

   `else{`

   `_485Slave.write(100);`

   `}`

   `}`

   `}`

   `}`

4. Lakukan ujicoba dengan melakukan penekanan tombol pada arduino slave dan perhatikan nyala lampu LED pada arduino master.



