## 4.5 Percobaan Transmisi Data Melalui Ethernet

### 4.5.1 Percobaan Mengirim Data dengan UDP

Pada percobaan kali ini akan diuji coba pengiriman data melalui komunikasi ethernet menggunakan protokol UDP.

**Kebutuhan Komponen**

Untuk melakukan percobaan komunikasi data melalui ethernet1 dibutuhkan beberapa modul komponen berikut ini :

1. [Arduino Uno](https://store.arduino.cc/usa/arduino-uno-rev3) 1 pcs
2. Ethernet Shield 2 pcs
3. Kabel Ethernet  1 pcs

**Langkah Percobaan**

1. Hubungkan Arduino dengan ethernet shield
2. Buatlah program pada Arduino seperti di bawah ini

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

3. Setting IP Address komputer Anda dengan IP Address static "192.168.1.2"  
   ![](/assets/2017-11-15_100614.png)

4. Pastikan koneksi tidak bermasalah dengan cara melakukan PING pada IP Address Arduino.  
   ![](/assets/2017-11-15_100700.png)

5. Buka aplikasi Hercules, pilih tab UDP kemudian sesuaikan parameter IP Address dan Port. Klik button Listen untuk membuka koneksi pada port 8888. Setelah koneksi terbuka, maka data yang dikirimkan melalui Arduino akan muncul pada textbox Received Data di Hercules.  
   ![](/assets/2017-11-15_104105.png)

### 4.5.2 Percobaan Menerima Data dengan UDP

Pada percobaan kali ini akan diuji coba pengiriman data melalui komunikasi ethernet menggunakan protokol UDP.

**Kebutuhan Komponen**

Untuk melakukan percobaan komunikasi data melalui ethernet1 dibutuhkan beberapa modul komponen berikut ini :

1. [Arduino Uno](https://store.arduino.cc/usa/arduino-uno-rev3) 1 pcs
2. Ethernet Shield 2 pcs
3. Kabel Ethernet  1 pcs

**Langkah Percobaan**

1. Hubungkan Arduino dengan ethernet shield
2. Buatlah program dibawah ini

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

3. asdasd

4. asdasds

### 4.5.3 Percobaan Mengirim Data dengan TCP \(Arduino Server - PC Client\)

Pada percobaan kali ini akan diuji coba pengiriman data melalui komunikasi ethernet menggunakan protokol UDP.

**Kebutuhan Komponen**

Untuk melakukan percobaan komunikasi data melalui ethernet1 dibutuhkan beberapa modul komponen berikut ini :

1. [Arduino Uno](https://store.arduino.cc/usa/arduino-uno-rev3) 1 pcs
2. Ethernet Shield 2 pcs
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
3. sdfsdff

### 4.5.4 Percobaan Menerima Data dengan TCP \(Arduino Server - PC Client\)

Pada percobaan kali ini akan diuji coba pengiriman data melalui komunikasi ethernet menggunakan protokol UDP.

**Kebutuhan Komponen**

Untuk melakukan percobaan komunikasi data melalui ethernet1 dibutuhkan beberapa modul komponen berikut ini :

1. [Arduino Uno](https://store.arduino.cc/usa/arduino-uno-rev3) 1 pcs
2. Ethernet Shield 2 pcs
3. Kabel Ethernet  1 pcs

**Langkah Percobaan**

1. Hubungkan Arduino dengan ethernet shield
2. Buatlah program dibawah ini

1. sdfsdfsdf

### 4.5.5 Percobaan Mengirim Data dengan TCP \(Arduino Client - PC Server\)

Pada percobaan kali ini akan diuji coba pengiriman data melalui komunikasi ethernet menggunakan protokol UDP.

**Kebutuhan Komponen**

Untuk melakukan percobaan komunikasi data melalui ethernet1 dibutuhkan beberapa modul komponen berikut ini :

1. [Arduino Uno](https://store.arduino.cc/usa/arduino-uno-rev3) 1 pcs
2. Ethernet Shield 2 pcs
3. Kabel Ethernet  1 pcs

**Langkah Percobaan**

1. Hubungkan Arduino dengan ethernet shield
2. Buatlah program dibawah ini
3. sdfsdfdsf

### 4.5.6 Percobaan Menerima Data dengan TCP \(Arduino Client - PC Server\)

Pada percobaan kali ini akan diuji coba pengiriman data melalui komunikasi ethernet menggunakan protokol UDP.

**Kebutuhan Komponen**

Untuk melakukan percobaan komunikasi data melalui ethernet1 dibutuhkan beberapa modul komponen berikut ini :

1. [Arduino Uno](https://store.arduino.cc/usa/arduino-uno-rev3) 1 pcs
2. Ethernet Shield 2 pcs
3. Kabel Ethernet  1 pcs

**Langkah Percobaan**

1. Hubungkan Arduino dengan ethernet shield
2. Buatlah program dibawah ini

1. sdfsdfsdf



