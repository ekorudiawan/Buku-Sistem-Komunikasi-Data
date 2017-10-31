## 2.4 Percobaan Transmisi Data Melalui EIA-485

### 2.4.1 Master Mengirim Data Ke Slave

Percobaan kali ini bertujuan untuk mengaplikasikan pengriman data melalui EIA-485. Perangkat yang digunakan adalah mikrokontroler dengan tambahan driver EIA-485. Perangkat yang akan dihubungkan sebanyak dua perangkat. Satu perangkat bertindak sebagai master atau pengendali komunikasi data. Satu perangkat lagi bertindak sebagai slave. Master akan dihubungkan dengan tombol push button dan slave akan dihubungkan dengan LED. Master akan mengirimkan data 1 byte yang bernilai 10 atau 20 tergantung dari kondisi tombol. Jika tombol ditekan master akan mengirimkan data 20, namun jika tombol tidak ditekan master akan mengirimkan data 10. Slave akan merespon pengiriman data ini dengan menyalakan LED jika data yang diterima slave 20 dan akan mematikan LED jika data yang diterima slave 10.

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
2. Buatlah program pada Arduino yang berfungsi sebagai pengirim seperti berikut ini. Program berikut ini berfungsi untuk mendeteksi penekanan tombol dan mengirimkan data ke receiver melalui EIA-485. Jika tombol ditekan, data yang dikirim adalah 20 dan jika tombol tidak ditekan data yang dikirimkan adalah 10.

   ```cpp
   #include <SoftwareSerial.h>
   #define BUTTON 4

   SoftwareSerial _485Master(3, 2);

   void setup() {
     _485Master.begin(9600);
     pinMode(BUTTON, INPUT_PULLUP);
   }

   void loop() {
     if (digitalRead(BUTTON) == LOW) {
       _485Master.write(20);
     }
     else {
       _485Master.write(10);
     }
   }
   ```

3. Buatlah program pada Arduino yang berfungsi sebagai penerima data seperti berikut ini. Program berikut ini berfungsi untuk menerima data dari transmitter. Data yang diterima akan dibandingkan nilainya. Jika data bernilai 10 maka lampu LED akan mati, jika data yang diterima bernilai 20 maka lampu LED akan menyala.

   ```cpp
   #include <SoftwareSerial.h>
   #define LED 4

   SoftwareSerial _485Slave(3, 2);

   void setup() {
     _485Slave.begin(9600);
     pinMode(LED, OUTPUT);
   }

   void loop() {
     while (_485Slave.available() > 0) {
       unsigned char data = _485Slave.read();
       if (data == 20) {
         digitalWrite(LED, 1);
       }
       else if (data == 10) {
         digitalWrite(LED, 0);
       }
     }
   }
   ```

4. Lakukan uji coba dengan melakukan penekanan tombol pada arduino transmitter kemudian perhatikan nyala LED pada arduino receiver.

### 2.4.2 Master Request Data Ke Slave

Percobaan kali ini masih menggunakan perangkat yang sama seperti percobaan sebelumnya. Namun pada percobaan kali ini tombol push button dihubungkan pada slave dan LED dihubungkan pada master. Proses komunikasi data sedikit berbeda dengan percobaan yang pertama. Pada percobaan kali ini master akan melakukan request data ke slave dengan cara mengirimkan data terlebih dahulu ke slave. Nilai data yang akan dikirimkan ke slave untuk merequest data adalah 50. Slave akan merespon permintaan request data dari master. Kemudian slave akan mengirimkan data yang direquest oleh master. Data yang dikirimkan oleh slave nilainya tergantung dari kondisi penekanan tombol pada slave. Jika tombol ditekan slave akan mengirimkan data 20 dan ketika tombol tidak ditekan slave akan mengirimkan data 10. Master akan merespon data yang telah dikirimkan oleh slave atas request sebelumnya. Master akan membandingkan nilai data tersebut. Jika data bernilai 20 maka master akan menyalakan LED dan jika data yang diterima adalah 10 maka master akan mematikan LED.

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
2. Buatlah program pada Arduino yang berfungsi sebagai master seperti berikut ini. Program berikut ini berfungsi untuk mengirim data 50 ke arduino slave sebagai petanda bahwa arduino master ingin membaca data pada arduino slave. Jika data yang di request telah dikirimkan oleh arduino slave maka arduino master akan menerima data tersebut dan membandingkan nilai datanya. Jika data yang diterima dari arduino slave adalah 20 maka arduino master akan menyalakan LED. Namun jika data yang diterima dari arduino slave adalah 10 maka arduino master akan mematikan LED.

   ```cpp
   #include <SoftwareSerial.h>
   #define LED 4
   SoftwareSerial _485Master(3, 2);

   void setup() {
     _485Master.begin(9600);
     pinMode(LED, OUTPUT);
   }

   void loop() {
     // Kirim data ke master untuk request data
     _485Master.write(50);
     // Cek data yang dikirimkan slave
     while (_485Master.available() > 0) {
       // Baca data dari slave
       unsigned char dataFromSlave = _485Master.read();
       if (dataFromSlave == 20) {
         digitalWrite(LED, 1);
       }
       else if (dataFromSlave == 10) {
         digitalWrite(LED, 0);
       }
     }
   }
   ```

3. Buatlah program pada Arduino slave seperti berikut ini. Program berikut ini berfungsi untuk menerima data dari arduino master. Kemudian jika data yang diterima bernilai 50 atau master ingin melakukan request data, maka Arduino slave akan meresponnya dengan mengirimkan data 10 jika button pada arduino slave tidak ditekan. Jika button ditekan data yang dikirimkan oleh arduino slave adalah 20.

   ```cpp
   #include <SoftwareSerial.h>
   #define BUTTON 4
   SoftwareSerial _485Slave(3, 2);

   void setup() {
     _485Slave.begin(9600);
     pinMode(BUTTON, INPUT_PULLUP);
   }

   void loop() {
     // Cek apakah ada data yang dikirimkan master
     while (_485Slave.available() > 0) {
       // Baca data dari master
       unsigned char dataFromMaster = _485Slave.read();
       // Jika data dari master 50
       // Master request data
       if (dataFromMaster == 50) {
         // Cek kondisi button
         // Kirim data ke master
         if (digitalRead(BUTTON) == LOW) {
           _485Slave.write(20);
         }
         else {
           _485Slave.write(10);
         }
       }
     }
   }
   ```

4. Lakukan ujicoba dengan melakukan penekanan tombol pada arduino slave dan perhatikan nyala lampu LED pada arduino master.

### 2.4.2 Master Mengirim Data Ke Multi Slave

Percobaan kali ini merupakan pengembangan dari percobaan pertama. Pada percobaan kali ini perangkat yang terhubung dan saling bertukar data ada tiga \[erangkat. Satu perangkat berfungsi sebagai master dan dua perangkat lainnya berfungsi sebagai slave. Master dihubungkan dengan dua push button dan masing-masing slave dihubungkan dengan satu LED. Pada percobaan kali ini master akan mengirimkan data ke kedua slave tersebut. Data yang dikirimkan akan disertai dengan informasi address yang merupakan data 1 byte yang berisi address dari masing-masing slave. Selain address, master juga akan mengirimkan 1 byte data yang merupakan data untuk menyalakan LED pada masing-masing slave. Sehingga dalam sekali pengiriman, 2 byte data akan dikirimkan master ke semua slave secara broadcast. Slave akan merespon data yang dikirimkan oleh master dengan melakukan pengecekan terlebih dahulu terhadap nilai address. Jika nilai address sesuai dengan yang telah di definisikan pada slave maka data akan diproses oleh slave tersebut. Namun jika ternyata address tidak sesuai dengan yang telah didefinisikan pada slave, data akan diabaikan dan tidak akan diproses. Untuk infomasi tentang address dan data pada percobaan kali ini silahkan lihat tabel berikut

Tabel Definisi Address pada Slave

| Perangkat Slave | Address |
| :---: | :---: |
| Slave1 | 1 |
| Slave2 | 2 |

Tabel Definisi Data pada Slave

| Data | Kondisi LED |
| :---: | :---: |
| 10 | Mati |
| 20 | Nyala |

Contoh format data yang dikirimkan master ke Slave1 untuk menyalakan LED

```cpp
// Kirim address terlebih dahulu 
// Address Slave1 = 1
_485Master.write(1); 
// Kemudian kirim data untuk menyalakan LED = 100
_485Master.write(20);
```

**Kebutuhan Komponen**

Untuk melakukan percobaan komunikasi data melaui EIA-485 dibutuhkan beberapa modul komponen berikut ini :

1. [Arduino Uno](https://store.arduino.cc/usa/arduino-uno-rev3) 3 pcs
2. [RS485 Shield](http://linksprite.com/wiki/index.php5?title=RS485_Shield_V2.1_for_Arduino) 3 pcs
3. [Modul Push Button](https://www.dfrobot.com/product-1098.html) 2 pcs
4. [Modul LED](https://www.dfrobot.com/product-490.html) 2 pcs

**Gambar Percobaan **

Koneksi

1. 485-A Master ke 485-A Slave1 dan 485-A Slave2
2. 485-B Master ke 485-B Slave1 dan 485-B Slave2
3. Modul Button ke pin 4 dan 5 Arduino Master
4. Modul LED masing-masing dihubungkan ke pin 4 Arduino Slave1 dan Slave2

Langkah Percobaan

1. Hubungkan semua modul seperti pada gambar percobaan diatas

2. Buatlah program pada Arduino master yang berfungsi untuk mendeteksi penekanan tombol dan mengirimkan data ke slave secara broadcast. Tombol pada pin 4 berfungsi untuk menyalakan LED pada Slave 1 dan button pada pin 5 berfungsi untuk menyalakan LED pada Slave2. Data yang akan dikirimkan sesuai dengan format data yang telah dijelaskan diatas.

   ```cpp
   #include <SoftwareSerial.h>
   #define BUTTON_1 4
   #define BUTTON_2 5

   SoftwareSerial _485Master(3, 2);

   void setup() {
     _485Master.begin(9600);
     pinMode(BUTTON_1, INPUT_PULLUP);
     pinMode(BUTTON_2, INPUT_PULLUP);
   }

   void loop() {
     if (digitalRead(BUTTON_1) == LOW) {
       _485Master.write(1);
       _485Master.write(20);
     }
     else if (digitalRead(BUTTON_1) == HIGH) {
       _485Master.write(1);
       _485Master.write(10);
     }
     else if (digitalRead(BUTTON_2) == LOW) {
       _485Master.write(2);
       _485Master.write(20);
     }
     else if (digitalRead(BUTTON_2) == HIGH) {
       _485Master.write(2);
       _485Master.write(10);
     }
   }
   ```

3. Buatlah program pada Arduino Slave 1 yang berfungsi untuk merespon data dari master jika address yang dikirimkan master sesuai dengan address yang telah didefinisikan pada Slave 1.Respon data berupa kondisi LED yang nyala jika data = 200 dan LED yang mati jika data = 100

   ```cpp
   #include <SoftwareSerial.h>
   #define MY_ADDRESS 1
   #define LED 4

   SoftwareSerial _485Slave(3, 2);

   void setup() {
     Serial.begin(9600);
     _485Slave.begin(9600);
     pinMode(LED, OUTPUT);
   }

   void loop() {
     while (_485Slave.available() > 0) {
       unsigned char address = _485Slave.read();
       unsigned char data = _485Slave.read();
       if (address == MY_ADDRESS) {
         if (data == 20) {
           digitalWrite(LED, 1);
         }
         else if (data == 10) {
           digitalWrite(LED, 0);
         }
       }
       delay(10);
     }
   }
   ```

4. Buatlah program pada Arduino Slave 2 dengan program yang sama dengan Slave 1, namun definisi address berbeda

   ```cpp
   #define MY_ADDRESS 2
   ```

5. Lakukan ujicoba dengan melakukan penekanan tombol pada master dan perhatikan kondisi LED pada kedua Slave. Pastikan hasilnya sesuai dengan yang telah dideskripsikan di atas.

### 2.4.3 Master Request Data Ke Multi Slave

#### Tugas

Buatlah sistem komunikasi data menggunakan EIA-485 dimana terdapat tiga perangkat yang saling berkomunikasi. Satu perangkat bertindak sebagai master dan yang lainnya sebagai slave. Perangkat master terhubung ke dua buah LED pada pin 4 dan pin 5. Pada masing-masing perangkat slave terhubung satu buah push button. Mode komunikasi data yang digunakan adalah half-duplex dimana master akan melakukan request data dari kedua perangkat slave. Request data dilakukan dengan cara mengirim data 50 diikuti dengan address dari slave yang dituju. Request data dilakukan secara broadcast. Slave yang memiliki address yang sama dengan yang address yang dikirimkan master akan merespon dengan mengirimkan informasi data push button. Format data yang dikirimkan slave terdiri dari 2 byte data, byte pertama berisi address dari slave yang mengirimkan data. Byte kedua berisi data kondisi button. Setelah slave mengirimkan data yang direquest master, master akan menerima data tersebut dan data tersebut akan digunakan untuk meng-update kondisi LED pada pin 4 dan pin 5. LED pada pin 4 menunjukkan data dari slave 1 dan LED pada pin 5 menunjukkan data dari slave 2. 

Contoh format data yang dikirimkan master ke slave1 untuk merequest data

```cpp
// Kirim instruksi request data = 50
_485Master.write(50); 
// Kemudian kirim data ke address slave yang dituju (slave1)
_485Master.write(1);
```

Contoh format data yang dikirimkan slave1 ke master untuk mereply data yang direquest master

```cpp
// Kirim address slave pengirim
_485Slave.write(1); 
// Kemudian kirim data (sesuai kondisi button)
_485Slave.write(20);
```







