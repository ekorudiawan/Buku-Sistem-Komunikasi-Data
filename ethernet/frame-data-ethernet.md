## 4.2 Frame Data Ethernet

Frame data pada ethernet dapat dilihat pada gambar berikut ini  
![](/assets/2017-11-14_094627.png)

1. SSD, Start of Frame merupakan sinyal penanda awal pengiriman data.
2. Preamble, merupakan bit yang digunakan untuk melakukan sinkronisasi. Bit preamble terdiri dari 7 byte sinyal sinkronisasi dan ditambahkan 1 byte sinyal SFD yang merupakan sinyal delimiter dari preambble.
   ![](/assets/2017-11-15_083818.png)

3. MAC Header, berisi informasi tentang MAC Address pengirim \(6 byte\) dan MAC Addreess penerima \(6 byte\) serta 2 byte mencakup informasi panjang data yang akan dikirimkan atau tipe service yang digunakan pada saat pengiriman data. Pada standar IEEE802.3 2 byte terakhir berisi informasi panjang data yang akan dikirim \(0-1500\).
   ![](/assets/2017-11-15_084503.png)  

4. IP Header, berisi informasi IP Address pengirim dan IP address penerima
5. TCP/UDP Header, protokol data yang digunakan pada pengiriman data
6. Data, merupakan sinyal data yang akan dikirimkan. Ukuran data yang dapat dikirimkan dalam satu paket data minimal adalah 46 byte dan maksimal adalah 1500byte dalam sekali pengiriman frame data.
7. FCS, merupakan sinyal yang digunakan untuk pengecekan error. FCS sama halnya dengan CRC pada CAN Bus.



