## 3.5 Percobaan Transmisi Data Melalui CAN Bus

### 3.5.1 Komunikasi Dua Node

Pada percobaan kali ini akan dilakukan uji coba komunikasi data antara dua node. Node pertama merupakan arduino dan sensor suhu LM35 dengan disertai CAN Bus shield. Untuk selanjutnya node ini disebut dengan temperature sensor node. Sedangkan node kedua merupakan node dengan CAN Bus shield saja. Untuk selanjutnya node ini disebut dengan display node. Pada percobaan kali ini temperature sensor node akan membaca tegangan analog dari sensor menggunakan ADC. Kemudian nilai ADC pada temperature sensor node akan dikirimkan melalui CAN Bus ke display node. Display node tugasnya hanya menerima data dari temperature sensor node. Kemudian menampilkan datanya melalui USB to serial dalam bentuk temperature. Pengiriman data akan dilakukan secara terus menerus dengan jeda waktu tertentu.

**Kebutuhan Komponen**

Untuk melakukan percobaan komunikasi data melalui CAN Bus dibutuhkan beberapa modul komponen berikut ini :

1. [Arduino Uno](https://store.arduino.cc/usa/arduino-uno-rev3) 2 pcs
2. [CAN Bus Shield](http://wiki.seeed.cc/CAN-BUS_Shield_V1.2/) 2 pcs
3. [LM35 Sensor](https://www.dfrobot.com/wiki/index.php/DFRobot_LM35_Linear_Temperature_Sensor_%28SKU:DFR0023%29) 1 pcs

**Koneksi**

1. LM35 Pada temperature sensor node terhubung pada pin A0
2. CAN High pada temperature sensor node terhubung ke CAN High display node
3. CAN Low pada temperature sensor node terhubung ke CAN Low display node

**Langkah Percobaan**

1. Hubungkan semua komponen sesuai dengan koneksi diatas
2. Buatlah program pada temperature sensor node. Temperature sensor node memiliki ID : 1. Program ini akan membaca nilai ADC pada pin A0. ADC pada arduino memiliki resolusi 10bit \(0-1023\). Nilai ADC akan disimpan pada varibel dengan tipe data integer dengan kapasitas penyimpanan data 16bit \(pada arduino uno\). Jika data nilai ADC ini ingin dikirimkan melalui CAN Bus, maka data 16 bit harus dipecah menjadi 2 buah data 8 bit \(1byte\). Data tersebut akan dikirimkan melalui CAN Bus dengan baudrate 500Kbs.

   ```cpp
   #include <mcp_can.h>
   #include <SPI.h>
   #define SPI_CS_PIN  10
   #define LED_ERR     13
   #define TEMP_SENSOR A0
   #define NODE_ID     1

   MCP_CAN CAN(SPI_CS_PIN);

   void setup()
   {  
     pinMode(LED_ERR, 1); // LED Error Indicator  
     pinMode(TEMP_SENSOR, 0); // LM35 Sensor
     digitalWrite(LED_ERR, 0); // Default off
     // Inisialisasi CAN Bus dengan Baudrate 500 Kbps
     // Jika error nyalakan LED
     while (CAN_OK != CAN.begin(CAN_500KBPS))              
     {
       digitalWrite(LED_ERR, 1);
       delay(100);
     }
   }

   void loop() {
     // Variabel untuk menampung 2 byte data yang akan dikirimkan
     unsigned char tempNodeData[2] = {0 , 0};

     // Baca nilai ADC dari sensor LM35
     // Nilai ADC default-nya 10 bit
     // Tipe data int berukuran 16 bit (2 byte) pada arduino uno
     int adcValue = analogRead(TEMP_SENSOR);

     // Tipe data unsigned char berukuran 8 bit (1 byte)
     // adcValue (2 byte data) dipecah menjadi 2 bagian
     // Disimpan pada 2 indeks array dgn tipe data unsigned char (1 byte)
     tempNodeData[0] = lowByte(adcValue);
     tempNodeData[1] = highByte(adcValue);

     // Kirim data via CAN Bus
     // NODE_ID      : 1 (ID dari node telah didefinisikan diatas)
     // Mode 0       : Menggunakan Standar CAN 2.0 A (11 bit ID)
     // Data Length  : 2 byte
     // Data         : tempNodeData
     CAN.sendMsgBuf(NODE_ID, 0, 2, tempNodeData);

     // Delay 50ms antar pengiriman data
     delay(50);
   }
   ```

3. Buatlah program pada display node. Display node memiliki ID : 2. Program ini akan membaca 2 byte data dari CAN Bus. Kemudian melakukan kalkulasi untuk mendapatkan nilai temperature dari sensor. Data tersebut akan ditampilkan ke serial monitor.

   ```cpp
   #include <SPI.h>
   #include "mcp_can.h"

   #define SPI_CS_PIN  10
   #define LED_ERR     13
   #define NODE_ID     2

   MCP_CAN CAN(SPI_CS_PIN);                                    

   void setup()
   {
     pinMode(LED_ERR, 1); // LED Error Indicator 
     digitalWrite(LED_ERR, 0); // Default off
     Serial.begin(9600); // Inisialisasi Serial Port Untuk Menampilkan data
     // Inisialisasi CAN Bus dengan baudrate 500 Kbps
     // Jika error nyalakan LED
     while (CAN_OK != CAN.begin(CAN_500KBPS))              
     {
       digitalWrite(LED_ERR, 1);
       delay(100);
     }
   }

   void loop()
   {
     // Variabel untuk menampung ukuran data yg akan diterima
     unsigned char dataLength = 0; 
     // Variabel yg digunakan untuk menampung data (2 byte data yg akan diterima)
     unsigned char dataBuffer[2]; 

     // Check jika ada data yg masuk
     if (CAN_MSGAVAIL == CAN.checkReceive())           
     {
       // Membaca data dari CAN Bus
       // Simpan ke variabel dataBuffer
       CAN.readMsgBuf(&dataLength, dataBuffer);    
       // Membaca ID dari pengirim data
       unsigned char senderID = CAN.getCanId();
       // Jika data yang diterima dari ID = 1
       if (senderID == 1) {
         // 2 byte data yg terpisah digabungkan menjadi int (2 byte)
         int adcFromNode = word(dataBuffer[1], dataBuffer[0]);
         // Perhitungan sensor suhu
         float temp = ((float)adcFromNode * 500.0) / 1023.0;
         // Kirim data ke serial port
         Serial.print("ID: ");
         Serial.print(senderID);
         Serial.print(" Data: ");
         Serial.print(dataBuffer[1], HEX);
         Serial.print(" ");
         Serial.print(dataBuffer[0], HEX);
         Serial.print(" Temp: ");
         Serial.print(temp);
         Serial.println(" C");
       }
     }
   }
   ```

4. Lakukan percobaan dengan melihat nilai data pada Serial Monitor Arduino. Kemudian screen shoot pdata yang ditampilakan oleh display node. Lakukan pemanasan pada sensor suhu untuk memastikan data yang diterima juga ikut berubah.

#### Tugas 1.

Tambahkan satu node lagi pada jaringan CAN Bus. Node ketiga terhubung dengan potensiometer yang dihubungkan pada pin A1 arduino. Node ini akan mengirimkan data ADC sama seperti temperature sensor node. Node ini memiliki ID : 3. Pada display node, tambahkan program agar bisa menampilkan data dari node yang ketiga.

### 3.5.2 Komunikasi Dua Node Menggunakan Request

Pada percobaan kali ini sistem komunikasi data yang digunakan menggunakan request data. Dengan menggunakan request data temperature sensor node tidak terus-menerus mengirimkan data. Request data akan dilakukan oleh display node terlebih dahulu dengan cara mengirimkan data 100. Kemudian temperature sensor node akan merespon dengan mengirimkan data yang di request jika ID dan format data request sesuai. Pada percobaan kali ini display node dan temperature sensor node harus dapat mengirimkan dan menerima data.

**Kebutuhan Komponen**

Untuk melakukan percobaan komunikasi data melalui CAN Bus dibutuhkan beberapa modul komponen berikut ini :

1. [Arduino Uno](https://store.arduino.cc/usa/arduino-uno-rev3) 2 pcs
2. [CAN Bus Shield](http://wiki.seeed.cc/CAN-BUS_Shield_V1.2/) 2 pcs
3. [LM35 Sensor](https://www.dfrobot.com/wiki/index.php/DFRobot_LM35_Linear_Temperature_Sensor_%28SKU:DFR0023%29) 1 pcs

**Koneksi**

1. LM35 Pada temperature sensor node terhubung pada pin A0
2. CAN High pada temperature sensor node terhubung ke CAN High display node
3. CAN Low pada temperature sensor node terhubung ke CAN Low display node

**Langkah Percobaan**

1. Hubungkan semua komponen sesuai dengan koneksi diatas
2. Buatlah program pada temperature sensor node. Program pada temperature sensor node akan menerima data request dari display node. Data yang diterima berupa 1 byte data. Jika data tersebut sesuai format yang sudah ditentukan \(100\), maka akan diteruskan. Kemudian temperature sensor node akan mereply dengan mengirimkan data kembali ke display node.

   ```cpp
   #include <mcp_can.h>
   #include <SPI.h>

   #define SPI_CS_PIN  10
   #define LED_ERR     13
   #define TEMP_SENSOR A0
   #define NODE_ID     1

   MCP_CAN CAN(SPI_CS_PIN);

   void setup()
   {
     pinMode(LED_ERR, 1); // LED Error Indicator
     pinMode(TEMP_SENSOR, 0); // LM35 Sensor
     digitalWrite(LED_ERR, 0); // Default off
     Serial.begin(9600);
     // Inisialisasi CAN Bus dengan Baudrate 500 Kbps
     // Jika error nyalakan LED
     while (CAN_OK != CAN.begin(CAN_500KBPS))
     {
       digitalWrite(LED_ERR, 1);
       delay(100);
     }
   }

   void loop() {
     // Variabel untuk menampung 2 byte data yang akan dikirimkan
     unsigned char tempNodeData[2] = {0 , 0};
     // Variabel untuk menampung ukuran data yg akan diterima
     unsigned char dataLength = 0;
     // Variabel yg digunakan untuk menampung data (2 byte data yg akan diterima)
     unsigned char dataBuffer[1];

     if (CAN_MSGAVAIL == CAN.checkReceive()) {
       CAN.readMsgBuf(&dataLength, dataBuffer);
       unsigned char senderID = CAN.getCanId();
       Serial.print("ID : ");
       Serial.print(senderID);
       Serial.print(" Data : ");
       Serial.println(dataBuffer[0], DEC);
       // Check ID pengirim
       if (senderID == 2) {
         delay(100);
         if (dataBuffer[0] == 100) {
           // Baca nilai ADC dari sensor LM35
           // Nilai ADC default-nya 10 bit
           // Tipe data int berukuran 16 bit (2 byte) pada arduino uno
           int adcValue = analogRead(TEMP_SENSOR);

           // Tipe data unsigned char berukuran 8 bit (1 byte)
           // adcValue (2 byte data) dipecah menjadi 2 bagian
           // Disimpan pada 2 indeks array dgn tipe data unsigned char (1 byte)
           tempNodeData[0] = lowByte(adcValue);
           tempNodeData[1] = highByte(adcValue);

           // Kirim data via CAN Bus
           // NODE_ID      : 1 (ID dari node telah didefinisikan diatas)
           // Mode 0       : Menggunakan Standar CAN 2.0 A (11 bit ID)
           // Data Length  : 2 byte
           // Data         : tempNodeData
           if(CAN.sendMsgBuf(NODE_ID, 0, 2, tempNodeData)==CAN_OK) {
             Serial.println("Send ok");
           }
         }
       }
     }
   }
   ```

3. Buatlah program pada display node. Program pada display node akan mengirimkan data 1 byte terlebih dahulu. Data tersebut bernilai 100. Data ini digunakan untuk memberikan tanda pada temperature sensor node untuk melakukan request data. Setelah data dikirimkan, display node akan menunggu reply data dari temperature sensor node. Setelah data reply diterima data akan ditampilkan ke serial monitor.

   ```cpp
   #include <SPI.h>
   #include "mcp_can.h"

   #define SPI_CS_PIN  10
   #define LED_ERR     13
   #define NODE_ID     2

   MCP_CAN CAN(SPI_CS_PIN);

   void setup()
   {
     pinMode(LED_ERR, 1); // LED Error Indicator
     digitalWrite(LED_ERR, 0); // Default off
     Serial.begin(9600); // Inisialisasi Serial Port Untuk Menampilkan data
     // Inisialisasi CAN Bus dengan baudrate 500 Kbps
     // Jika error nyalakan LED
     while (CAN_OK != CAN.begin(CAN_500KBPS))
     {
       digitalWrite(LED_ERR, 1);
       delay(100);
     }
   }

   void loop()
   {
     // Variabel untuk menampung ukuran data yg akan diterima
     unsigned char dataLength = 0;
     // Variabel yg digunakan untuk menampung data (2 byte data yg akan diterima)
     unsigned char dataBuffer[2];

     unsigned char tempNodeData[1] = {100};
     CAN.sendMsgBuf(NODE_ID, 0, 1, tempNodeData);

     // Tunggu sampai ada reply dari sensor node
     while (CAN.checkReceive() != CAN_MSGAVAIL);

     if (CAN_MSGAVAIL == CAN.checkReceive())
     {
       // Membaca data dari CAN Bus
       // Simpan ke variabel dataBuffer
       CAN.readMsgBuf(&dataLength, dataBuffer);
       // Membaca ID dari pengirim data
       unsigned char senderID = CAN.getCanId();
       // Jika data yang diterima dari ID = 1
       if (senderID == 1) {
         // 2 byte data yg terpisah digabungkan menjadi int (2 byte)
         int adcFromNode = word(dataBuffer[1], dataBuffer[0]);
         // Perhitungan sensor suhu
         float temp = ((float)adcFromNode * 500.0) / 1023.0;
         // Kirim data ke serial port
         Serial.print("ID : ");
         Serial.print(senderID);
         Serial.print(" Data : ");
         Serial.print(dataBuffer[1], HEX);
         Serial.print(" ");
         Serial.println(dataBuffer[0], HEX);
         Serial.print("Temp : ");
         Serial.print(temp);
         Serial.println(" C");
       }
     }
     delay(1000);
   }
   ```

4. Lakukan percobaan diatas dengan cara melihat perubahan nilai data yang diterima display node pada serial monitor.

#### Tugas 2.

Tambahkan satu node lagi pada jaringan CAN Bus. Node ketiga terhubung dengan potensiometer yang dihubungkan pada pin A1 arduino. Diplay node akan melakukan request data pada kedua node tersebut. Node akan merespon jika request data ditujukan untuk ID-nya.

#### Tugas 3.

Terdapat dua buah node pada jaringan CAN Bus \(Node A dan Node B\). Kedua node tersebut terhubung dengan temperature sensor \(pin A0\) dan LED \(pin 13\). LED pada node A akan menyala jika temperature sensor pada node B melebihi 35 celcius. LED pada node B akan menyala jika temperature sensor pada node A melebihi 35 celcius. Kedua node saling berkomunikasi untuk bertukar data. Node A dan Node B harus dapat mengirim dan menerima data.  



