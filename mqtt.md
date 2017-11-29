# 5. MQTT \(Message Queue Telemetry Transport\)

**Reference : **

1. [http://mqtt.org/](http://mqtt.org/)
2. [MQTT Protocol V3.1.1](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.pdf)
3. GFDSF

## 5.1.1 Percobaan MQTT

Pada percobaan kali ini akan diuji coba pengiriman data menggunakan protokol MQTT. MQTT broker yang digunakan adalah  [https://io.adafruit.com](https://io.adafruit.com). Pada percobaan kali ini digunakan board mikrokontroler Wemos yang sudah terintegrasi dengan modul komunikasi WiFi.

**Kebutuhan Komponen**

Untuk melakukan percobaan komunikasi data melalui ethernet dibutuhkan beberapa modul komponen berikut ini :

1. Wemos 1 pcs
2. LM35 1 pcs
3. LED 1 pcs
4. Access Point 1 pcs

Koneksi

1. LM35 - pin A0 Wemos \(Gunakan tegangan supply 3,3V\)
2. LED - pin D7 Wemos

**Langkah Percobaan**

1. Buatlah akun di url berikut ini [https://io.adafruit.com](https://io.adafruit.com)
2. Buatlah dashboard baru pada akun Anda dengan nama My Dashboard  
   ![](/assets/2017-11-28_091224.png)![](/assets/2017-11-28_091346.png)

3. Masuk ke dahboard yang telah Anda buat sebelumnya  
   ![](/assets/2017-11-28_091358.png)

4. Buat block baru dengan cara memilih menu '+'  
   ![](/assets/2017-11-28_091450.png)

5. Tambahkan block Toggle pada dashboard Anda dengan nama **led\_control**  
   ![](/assets/2017-11-28_091508.png)![](/assets/2017-11-28_091617.png)![](/assets/2017-11-28_091632.png)![](/assets/2017-11-28_091644.png)

6. Buatlah block baru dengan tipe Gauge dengan nama **potensiometer** dan konfigurasi seperti berikut ini  
   ![](/assets/2017-11-28_091800.png)

7. Buatlah block baru dengan tipe Line Chart dengan nama **temperature** dan konfigurasi seperti berikut ini  
   ![](/assets/2017-11-28_091957.png)

8. Buatlah block baru dengan tipe Slider dengan nama **led\_pwm\_control** dan konfigurasi seperti berikut ini  
   ![](/assets/2017-11-28_092122.png)

9. Pada Arduino buatlah program berikut ini

   ```c
   #include <ESP8266WiFi.h>
   #include "Adafruit_MQTT.h"
   #include "Adafruit_MQTT_Client.h"

   /************************* WiFi Access Point *********************************/
   // Sesuaikan dengan access point yang digunakan

   #define WLAN_SSID       "Andromax-M3Z-00CC"
   #define WLAN_PASS       "37925456"

   /************************* Adafruit.io Setup *********************************/
   // Sesuaikan dengan akun masing-masing

   #define AIO_SERVER      "io.adafruit.com"
   #define AIO_SERVERPORT  1883                   // port 8883 untuk SSL
   #define AIO_USERNAME    "eko_rudiawan"
   #define AIO_KEY         "6ca793307d394350977c4b2db3e382af"

   // Create an ESP8266 WiFiClient class to connect to the MQTT server.
   WiFiClient client;
   //WiFiFlientSecure untuk koneksi SSL
   //WiFiClientSecure client;

   // Setup MQTT client untuk koneksi ke MQTT broker
   Adafruit_MQTT_Client mqtt(&client, AIO_SERVER, AIO_SERVERPORT, AIO_USERNAME, AIO_KEY);

   // Feed data untuk di publish
   Adafruit_MQTT_Publish potensiometer = Adafruit_MQTT_Publish(&mqtt, AIO_USERNAME "/feeds/potensiometer");
   Adafruit_MQTT_Publish temperature = Adafruit_MQTT_Publish(&mqtt, AIO_USERNAME "/feeds/temperature");
   //Adafruit_MQTT_Publish led_control_pub = Adafruit_MQTT_Publish(&mqtt, AIO_USERNAME "/feeds/led_control");

   // Feed data untuk di subscribe
   Adafruit_MQTT_Subscribe led_control = Adafruit_MQTT_Subscribe(&mqtt, AIO_USERNAME "/feeds/led_control");
   Adafruit_MQTT_Subscribe led_pwm_control = Adafruit_MQTT_Subscribe(&mqtt, AIO_USERNAME "/feeds/led_pwm_control");

   void MQTT_connect();

   void setup() {
     pinMode(LED_BUILTIN, OUTPUT);
     pinMode(D7, OUTPUT);
     Serial.begin(115200);
     delay(10);

     Serial.println(F("MQTT Client"));

     // Koneksi ke akses point
     Serial.println();
     Serial.print("Connecting to ");
     Serial.println(WLAN_SSID);

     WiFi.begin(WLAN_SSID, WLAN_PASS);
     while (WiFi.status() != WL_CONNECTED) {
       delay(500);
       Serial.print(".");
     }
     Serial.println();

     Serial.println("WiFi connected");
     Serial.println("IP address: "); Serial.println(WiFi.localIP());

     // Aktifkan subscription
     mqtt.subscribe(&led_control);
     mqtt.subscribe(&led_pwm_control);
   }

   void loop() {
     MQTT_connect();

     Adafruit_MQTT_Subscribe *subscription;
     while ((subscription = mqtt.readSubscription(5000))) {
       if (subscription == &led_control) {
         Serial.print(F("led_control data: "));
         String led_data((char *)led_control.lastread);
         Serial.println(led_data);
         if (led_data.equals("ON")) {
           digitalWrite(LED_BUILTIN, LOW);
         }
         else if (led_data.equals("OFF")) {
           digitalWrite(LED_BUILTIN, HIGH);
         }
       }
       if (subscription == &led_pwm_control) {
         Serial.print(F("led_pwm_control data: "));
         char *pwm_value = (char *)led_pwm_control.lastread;
         Serial.println(pwm_value);
         int pwm = atoi(pwm_value);
         analogWrite(D7, pwm);
       }
     }

     // Publish potensiometer data
     int adc0 = analogRead(A0);

     Serial.print(F("\nPublish potensiometer data : "));
     Serial.print(adc0);
     Serial.println();

     if (!potensiometer.publish(adc0)) {
       Serial.println(F("Publish Failed"));
     } else {
       Serial.println(F("Publish OK!"));
     }

     // Publish temperature data
     float millivolts = (adc0 / 1024.0) * 3300;
     float temp = millivolts / 10;

     Serial.print(F("\nPublish temperature data : "));
     Serial.print(temp);
     Serial.println();

     if (!temperature.publish(temp)) {
       Serial.println(F("Publish Failed"));
     } else {
       Serial.println(F("Publish OK!"));
     }
   }

   void MQTT_connect() {
     int8_t ret;

     if (mqtt.connected()) {
       return;
     }

     Serial.print("Connecting to MQTT... ");

     uint8_t retries = 3;
     while ((ret = mqtt.connect()) != 0) {
       Serial.println(mqtt.connectErrorString(ret));
       Serial.println("Retrying MQTT connection in 5 seconds...");
       mqtt.disconnect();
       delay(5000);  // wait 5 seconds
       retries--;
       if (retries == 0) {
         while (1);
       }
     }
     Serial.println("MQTT Connected!");
   }
   ```

10. Lakukan kompilasi dan upload program dengan memilih tipe board **Wemos D1**

11. Lakukan percobaan dengan cara merubah nilai pada dashboard Anda.

## 5.1.2 Tugas MQTT

1. Buatlah sebuah aplikasi kontrol LED menggunakan MQTT. Terdapat dua client \(Wemos\) yang terkoneksi ke MQTT broker. Wemos 1 terkoneksi dengan push button dan Wemos 2 terkoneksi dengan LED. Wemos 1 melakukan publish data ke MQTT broker sedangkan Wemos 2 melakukan subscribe data dari MQTT broker. Jika push button pada Wemos 1 ditekan LED pada Wemos 2 akan menyala, begitu juga sebaliknya !



