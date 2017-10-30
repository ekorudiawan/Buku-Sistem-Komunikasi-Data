## 2.1 Karakteristik Sinyal EIA-485

Sinyal EIA-485 ditransmisikan melalui dua buah terminal dengan nama terminal A dan terminal B. Kedua terminal tersebut dikenal dengan nama lain antara lain sebagai berikut 

* Terminal A, nama lainnya D-, RX- atau TX-
* Terminal B, nama lainnya D+, RX+ atau TX+

Perbedaan tegangan antara terminal A dan B digunakan untuk merepresentasikan sinyal biner pada saat mentransmisikan data. Lebih detail tentang karakteristik sinyal pada EIA-485 adalah sebagai berikut :

1. Logika 1 : -1.5 Volt sampai -6 Volt atau tegangan pada terminal A lebih negatif dari pada terminal B
2. Logika 0 : +1.5 Volt sampai +6 Volt atau tegangan pada terminal A lebih positif dari pada terminal B
3. Tidak Terdefinisikan : -1.5 Volt sampai +1.5 Volt

Logika 1 pada EIA-485 biasanya disebut dengan MARK. Sedangkan logika 0 disebut dengan SPACE. Gambar sinyal EIA-485 jika digambarkan pada timing diagram dapat dilihat seperti berikut ini. 

![](/assets/487px-RS-485_waveform.svg.png)

U+ pada gambar diatas sama dengan terminal B. Sedangkan U- sama dengan terminal A.

EIA-485 dapat digunakan untuk melakukan komunikasi data untuk maksimal 32 perangkat dalam satu jalur kabel. Namun, pada pada satu jalur kabel hanya boleh ada satu perangkat yang aktif sebagai pengendali komunikasi data yang biasanya disebut dengan driver atau master. Sedangkan perangkat lainnya disebut dengan receiver atau slave.

