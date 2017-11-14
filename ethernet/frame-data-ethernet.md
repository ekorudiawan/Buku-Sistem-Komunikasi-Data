## 4.2 Frame Data Ethernet

Frame data pada ethernet dapat dilihat pada gambar berikut ini   
![](/assets/2017-11-14_094627.png)

1. SSD, Start of Frame merupakan sinyal penanda awal pengiriman data
2. Preamble, merupakan bit yang digunakan untuk melakukan sinkronisasi dan ditambahkan dengan 8bit untuk delimiter
3. MAC Header, berisi informasi tentang MAC Address pengirim dan penerima serta protokol yang digunakan.
4. IP Header, berisi informasi IP Address pengirim dan IP address penerima
5. TCP/UDP Header, protokol data yang digunakan pada pengiriman data
6. Data, merupakan data yang akan dikirimkan. Ukuran data yang dapat dikirimkan dalam satu paket data maksimal adalah 
7. FCS, merupakan sinyal yang digunakan untuk pengecekan error. FCS sama halnya dengan CRC pada CAN Bus.



