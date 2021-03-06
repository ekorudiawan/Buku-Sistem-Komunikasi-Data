## 1.1 Karakteristik Sinyal EIA-232

Pengiriman data dan penerimaan data pada EIA-232 menggunakan jalur yang berbeda \(transmitted data dan received data\). Dimana jalur data tersebut memiliki karakteristik sinyal khusus pada saat mengirimkan sinyal digital. 

Karakteristik sinyal yang dimiliki oleh EIA-232 adalah sebagai berikut :

**Transmitter :**

1. Logika 1 : -5 Volt sampai -25 Volt
2. Logika 0 : +5 Volt sampai +25 Volt
3. Tidak Terdefinisikan : -5 Volt sampai +5 Volt

**Receiver : **

1. Logika 1 : -3 Volt sampai -25 Volt
2. Logika 0 : +3 Volt sampai +25 Volt
3. Tidak Terdefinisikan : -3 Volt sampai +3 Volt

EIA-232 membutuhkan level tegangan yang cukup besar -25 Volt sampai +25 Volt untuk merepresentasikan data biner 0 dan 1. Hal ini bertujuan untuk mengurangi tengangan drop jika kabel yang digunakan untuk pengiriman data cukup panjang. Pada umumnya level tegangan logika yang digunakan pada mikroprosesor adalah 0 Volt sampai +5 Volt atau biasanya disebut dengan level tegangan TTL \(Transistor Transistor Logic\). Level tegangan tersebut harus dikonversi menjadi -25 Volt sampai +25 Volt agar dapat dihubungkan dengan perangkat standar EIA-232. Pengkonversian level tegangan dapat dilakukan dengan cara menambahkan IC driver standar EIA-232 contohnya IC MAX232. Gambar 2 berikut menjelaskan tentang koneksi minimal yang dapat digunakan untuk menghubungkan perangkat yang memiliki standar EIA-232.

![](/assets/2017-10-25_115015.png)Gambar 2. Koneksi DTE dan DCE yang Dihubungkan Melalui EIA-232

Koneksi diatas menjelaskan bahwa Line Driver atau disebut juga Transmitter pada perangkat DTE dihubungkan dengan Line Receiver pada perangkat DCE. Sedangkan Line Receiver pada perangkat DTE dihubungkan ke Line Driver pada perangkat DCE. Selain itu Signal Common dari kedua perangkat harus digabungkan dikarenakan Signal Common menjadi tegangan referensi pada saat pengiriman sinyal.

Kecepatan maksimal transmisi data EIA-232 ditentukan oleh panjang kabel yang digunakan. Semakin panjang kabel yang digunakan maka induktansi akan meningkat yang menyebabkan tegangan drop semakin besar. Kecepatan transmisi data standar yang dapat digunakan pada EIA-232 adalah sebagai berikut : 110, 300, 600, 1200, 2400, 4800, 9600, 19200, 38400, 57600 dan 115200 bps \(bit per second\). Hubungan antara kecepatan transmisi data dan panjang kabel dapat dilihat pada Tabel 1.

Tabel 1. Hubungan Kecepatan Transmisi Data dan Panjang Kabel

| Baudrate \(bps\) | Panjang Kabel \(m\) |
| :--- | :--- |
| 110 | 850 |
| 300 | 800 |
| 600 | 700 |
| 1200 | 500 |
| 2400 | 200 |
| 4800 | 100 |
| 9600 | 70 |
| 19200 | 50 |
| 115200 | 20 |

## 



