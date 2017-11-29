## 4.5 Percobaan Transmisi Data Melalui Ethernet

### 4.5.1 Percobaan Mengirim Data dengan UDP

Pada percobaan kali ini akan diuji coba pengiriman data antara dua perangkat melalui komunikasi ethernet menggunakan protokol UDP. Uji coba komunikasi data dilakukan pada Arduino dengan tambahan ethernet shield dan ethernet port pada komputer. Komputer akan membuka akses port 8888 untuk menerima data dari Arduino. IP Address yang digunakan oleh Arduino adalah "192.168.1.177". Sedangkan IP Address yang digunakan pada komputer adalah "192.168.1.2".

**Kebutuhan Komponen**

Untuk melakukan percobaan komunikasi data melalui ethernet dibutuhkan beberapa modul komponen berikut ini :

1. [Arduino Uno](https://store.arduino.cc/usa/arduino-uno-rev3) 1 pcs
2. [Ethernet Shield](https://store.arduino.cc/usa/arduino-ethernet-shield-2) 1 pcs
3. Kabel Ethernet  1 pcs

**Kebutuhan Software**

1. [Hercules](https://www.hw-group.com/products/hercules/index_en.html)

**Langkah Percobaan**

1. Hubungkan Arduino dengan ethernet shield.
2. Buatlah program pada Arduino seperti di bawah ini. Lakukan upload program dan hubungkan kabel ethernet dari arduino ke PC.

   ```cpp
   #include <SPI.h>
   #include <Ethernet.h>
   #include <EthernetUdp.h>

   // MAC Address yang digunakan
   // Masing-masing device harus unik
   byte mac[] = {
     0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED
   };

   // IP Address yang digunakan
   // IP Address harus berbeda tiap device yang terkoneksi dalam jaringan yg sama
   IPAddress ip(192, 168, 1, 177);

   EthernetUDP Udp;

   void setup() {
     // Inisialisasi ethernet dengan MAC Address dan IP Address yg didefinisikan diatas
     Ethernet.begin(mac, ip);
     // Arduino membuka koneksi port 9000 sebagai port untuk masuknya data
     Udp.begin(9000);
   }

   void loop() {
     // Arduino mengirim data ke IP Address tujuan dan port tujuan
     Udp.beginPacket("192.168.1.2", 8888);
     int randomData = random(0, 1000);
     Udp.print("Sent from Arduino : ");
     Udp.println(randomData);
     Udp.endPacket();
     delay(1000);
   }
   ```

3. Setting IP Address komputer Anda dengan menggunakan IP Address static "192.168.1.2".  
   ![](/assets/2017-11-15_100614.png)

4. Pastikan koneksi tidak bermasalah dengan cara melakukan PING pada IP Address Arduino.  
   ![](/assets/2017-11-15_100700.png)

5. Buka aplikasi Hercules, pilih tab UDP kemudian sesuaikan parameter IP Address dan Port. Klik button Listen untuk membuka koneksi pada port 8888. Setelah koneksi terbuka, maka data yang dikirimkan melalui Arduino akan muncul pada textbox Received Data di Hercules.  
   ![](/assets/2017-11-15_104105.png)

### 4.5.2 Percobaan Menerima Data dengan UDP

Pada percobaan kali ini akan diuji coba penerimaan data melalui komunikasi ethernet menggunakan protokol UDP. Arduino akan membuka akses port 9000 sebagai jalur masuknya data. Data akan dikirimkan menggunakan aplikasi Hercules pada PC. Pengiriman data pada aplikasi Hercules ditujukan pada IP Addresss dan port yang digunakan Arduino.

**Kebutuhan Komponen**

Untuk melakukan percobaan ini dibutuhkan beberapa modul komponen berikut ini :

1. [Arduino Uno](https://store.arduino.cc/usa/arduino-uno-rev3) 1 pcs
2. [Ethernet Shield](https://store.arduino.cc/usa/arduino-ethernet-shield-2) 1 pcs
3. Kabel Ethernet  1 pcs

**Langkah Percobaan**

1. Hubungkan Arduino dengan ethernet shield
2. Buatlah program dibawah ini, lakukan upload program dan koneksikan kabel ethernet ke arduino dan komputer.

   ```cpp
   #include <SPI.h>
   #include <Ethernet.h>
   #include <EthernetUdp.h>

   // MAC Address yang digunakan
   // Masing-masing device harus unik
   byte mac[] = {
     0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED
   };

   // IP Address yang digunakan
   // IP Address harus berbeda tiap device yang terkoneksi dalam jaringan yg sama
   IPAddress ip(192, 168, 1, 177);

   EthernetUDP Udp;

   // Variabel untuk menerima data
   char packetBuffer[UDP_TX_PACKET_MAX_SIZE];

   void setup() {
     // Inisialisasi ethernet dengan MAC Address dan IP Address yg didefinisikan diatas
     Ethernet.begin(mac, ip);
     // Arduino membuka koneksi port 9000 sebagai port untuk masuknya data
     Udp.begin(9000);
     Serial.begin(9600);
   }

   void loop() {
     // Cek apakah ada paket data yg masuk
     int packetSize = Udp.parsePacket();
     if (packetSize) {
       Serial.print("Received packet of size ");
       Serial.println(packetSize);
       Serial.print("From IP Address ");
       IPAddress remote = Udp.remoteIP();
       for (int i = 0; i < 4; i++) {
         Serial.print(remote[i], DEC);
         if (i < 3) {
           Serial.print(".");
         }
       }
       Serial.print(" port ");
       Serial.println(Udp.remotePort());
       // Menerima data dari UDP
       Udp.read(packetBuffer, UDP_TX_PACKET_MAX_SIZE);
       Serial.println("Data :");
       Serial.println(packetBuffer);
     }
   }
   ```

3. Buka aplikasi Hercules dan lakukan akses terhadap port yang digunakan. Untuk mengirimkan data, isi textbox Send dengan text apapun. Kemudian klik Send untuk mengirimkan data  
   ![](/assets/2017-11-15_103948.png)

4. Untuk memastikan data yang dikirim dari PC menggunakan Hercules telah diterima Arduino, bukalah Serial Monitor pada Arduino. Data yang dikirim akan ditampilkan pada Serial Monitor Arduino beserta informasi IP Address pengirim dan port yang digunakan.  
   ![](/assets/2017-11-15_103954.png)

### 4.5.3 Percobaan Mengirim Data dengan TCP \(Arduino Server - PC Client\)

Pada percobaan kali ini akan diuji coba pengiriman data melalui komunikasi ethernet menggunakan protokol TCP. Pada percobaan kali ini Arduino akan menjadi server dan PC akan menjadi client. Port yang digunakan untuk komunikasi data adalah port 23. Arduino akan membuka koneksi dan menunggu request koneksi dari client. Jika client telah terkoneksi, Arduino akan mengirimkan data ke client tersebut.

**Kebutuhan Komponen**

Untuk melakukan percobaan ini dibutuhkan beberapa modul komponen berikut ini :

1. [Arduino Uno](https://store.arduino.cc/usa/arduino-uno-rev3) 1 pcs
2. [Ethernet Shield](https://store.arduino.cc/usa/arduino-ethernet-shield-2) 1 pcs
3. Kabel Ethernet  1 pcs

**Langkah Percobaan**

1. Hubungkan Arduino dengan ethernet shield
2. Buatlah program dibawah ini

   ```cpp
   #include <SPI.h>
   #include <Ethernet.h>

   byte mac[] = {
     0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED
   };

   IPAddress ip(192, 168, 1, 177);

   // Membuka akses port 23 untuk protokol TCP
   EthernetServer server(23);

   void setup() {
     Ethernet.begin(mac, ip);
     // Membuka koneksi dan menunggu koneksi dari client
     server.begin();
   }

   void loop() {
     // Mengirim data ke client yang telah terkoneksi   
     int randomData = random(0, 1000);
     server.print("Sent from Arduino : ");
     server.println(randomData);
     delay(1000);
   }
   ```

3. Untuk melakukan ujicoba buka aplikasi Hercules, kemudian pilih tab TCP Client. Lakukan koneksi ke IP Address dan port server \(Arduino\).  
   ![](/assets/2017-11-15_105258.png)

### 4.5.4 Percobaan Menerima Data dengan TCP \(Arduino Server - PC Client\)

Pada percobaan kali ini akan di ujicoba penerimaan data pada Arduino yang berfungsi sebagai TCP Server. Data akan dikirimkan oleh client menggunakan software Hercules. Server akan melakukan pengecekan data dari client. Jika client memgirimkan data, data akan diterima dan ditampilkan ke Serial Monitor.

**Kebutuhan Komponen**

Untuk melakukan percobaan komunikasi data melalui ethernet1 dibutuhkan beberapa modul komponen berikut ini :

1. [Arduino Uno](https://store.arduino.cc/usa/arduino-uno-rev3) 1 pcs
2. [Ethernet Shield](https://store.arduino.cc/usa/arduino-ethernet-shield-2) 1 pcs
3. Kabel Ethernet  1 pcs

**Langkah Percobaan**

1. Hubungkan Arduino dengan ethernet shield
2. Buatlah program dibawah ini

   ```cpp
   #include <SPI.h>
   #include <Ethernet.h>

   byte mac[] = {
     0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED
   };

   IPAddress ip(192, 168, 1, 177);

   // Membuka akses port 23 untuk protokol TCP
   EthernetServer server(23);

   void setup() {
     Ethernet.begin(mac, ip);
     // Membuka koneksi dan menunggu koneksi dari client
     server.begin();
     Serial.begin(9600);
   }

   void loop() {
     // Cek jika ada data yang dikirim client
     EthernetClient client = server.available();
     if (client) {
       while (client.available() > 0) {
         char thisChar = client.read();
         Serial.write(thisChar);
       }
     }
   }
   ```

3. Buka software Hercules dengan mode TCP Client. Lakukan percobaan untuk mengirim data dan perhatikan data yang diterima pada Serial Monitor.  
   ![](/assets/2017-11-15_105922.png)

4. Jika pengiriman data berhasil, maka akan muncul data yang Anda kirim sebelumnya melalui  
   ![](/assets/2017-11-15_105928.png)

### 4.5.5 Percobaan Mengirim Data dengan TCP \(Arduino Client - PC Server\)

Pada percobaan kali ini Arduino akan menjadi client dan PC akan menjadi server. Hercules akan membuka koneksi dan menunggu koneksi dari client \(arduino\). Arduino akan melakukan request koneksi ke server dengan IP Address dan port yang telah dibuka aksesnya oleh server. Setelah terkoneksi, Arduino akan mengirimkan data ke Server.

**Kebutuhan Komponen**

Untuk melakukan percobaan ini dibutuhkan beberapa modul komponen berikut ini :

1. [Arduino Uno](https://store.arduino.cc/usa/arduino-uno-rev3) 1 pcs
2. [Ethernet Shield](https://store.arduino.cc/usa/arduino-ethernet-shield-2) 1 pcs
3. Kabel Ethernet  1 pcs

**Langkah Percobaan**

1. Hubungkan Arduino dengan ethernet shield
2. Buatlah program dibawah ini

   ```cpp
   #include <SPI.h>
   #include <Ethernet.h>

   byte mac[] = {
     0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED
   };

   // Alamat IP Address Client (Arduino)
   IPAddress ip(192, 168, 1, 177);

   // Alamat IP Address server
   IPAddress server(192, 168, 1, 2);

   // Mendefinisikan arduino sebagai client
   EthernetClient client;

   void setup() {
     Ethernet.begin(mac, ip);
     // Client melakukan request koneksi ke server dengan IP Adreess dan port yg didefinisikan
     client.connect(server, 7000);
   }

   void loop() {
     // Mengirim data selama client masih terkoneksi dengan server
     if (client.connected()) {
       int randomData = random(0, 1000);
       client.print("Sent from Arduino : ");
       client.println(randomData);
     }
     delay(1000);
   }
   ```

3. Buka aplikasi Hercules dengan memilih mode TCP Server. Lakukan listening koneksi pada port 7000. Pastikan data yang dikirimkan Arduino muncul pada textbox received data.

### 4.5.6 Percobaan Menerima Data dengan TCP \(Arduino Client - PC Server\)

Pada percobaan kali ini arduino yang berfungsi sebagai client akan menerima data dari server. Data akan dikirimkan melalui software Hercules yang berfungsi sebagai server.

**Kebutuhan Komponen**

Untuk melakukan percobaan komunikasi data melalui ethernet1 dibutuhkan beberapa modul komponen berikut ini :

1. [Arduino Uno](https://store.arduino.cc/usa/arduino-uno-rev3) 1 pcs
2. [Ethernet Shield](https://store.arduino.cc/usa/arduino-ethernet-shield-2) 1 pcs
3. Kabel Ethernet  1 pcs

**Langkah Percobaan**

1. Hubungkan Arduino dengan ethernet shield
2. Buatlah program dibawah ini

   ```cpp
   #include <SPI.h>
   #include <Ethernet.h>

   byte mac[] = {
     0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED
   };

   // Alamat IP Address Client (Arduino)
   IPAddress ip(192, 168, 1, 177);

   // Alamat IP Address server
   IPAddress server(192, 168, 1, 2);

   // Mendefinisikan arduino sebagai client
   EthernetClient client;

   void setup() {
     Ethernet.begin(mac, ip);
     // Client melakukan request koneksi ke server dengan IP Adreess dan port yg didefinisikan
     client.connect(server, 7000);
     Serial.begin(9600);
   }

   void loop() {
     if (client.connected()) {
       // Jika ada paket data yang masuk ke client
       if (client.available()) {
         // Menerima paket data dan menampilkan ke serial
         char c = client.read();
         Serial.print(c);
       }
     }
   }
   ```

3. Lakukan uji coba pengiriman data melalui aplikasi Hercules pada port tujuan 7000. Pastikan data diterima oleh Arduino melalui serial monitor.

### TUGAS

1. Terdapat dua buah Arduino yang saling terhubung melalui ethernet. Arduino 1 terhubung ke potensiometer yang berada pada pin A0. Sedangkan Arduino 2 terhubung ke LED yang berada pada pin 10 \(PWM\). Arduino 1 akan mengirimkan data ADC dari potensiometer ke Arduino 2. Arduino 2 akan memproses data tersebut untuk mengatur nilai PWM pada pin 10. Gunakan salah satu protokol TCP atau UDP untuk mengerjakan soal ini !



