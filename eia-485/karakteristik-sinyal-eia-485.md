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

U+ pada gambar diatas sama dengan terminal B. Sedangkan U- sama dengan terminal A. Pada gambar tersebut dapat dilihat bahwa pada saat sinyal A \(warna biru\) terletak di bawah sinyal B \(warna merah\) nilai biner yang tertulis adalah 1 atau disebut juga dengan mark. Namun pada saat sinyal A berada di atas sinyal B nilai biner yang tertulis adalah 0 atau sama dengan space. Sedangkan pada kondisi tidak ada pengiriman data, sinyal A berada di bawah sinyal B. Namun pada kondisi ini perbedaan tegangan antara terminal A dan B diantara -1.5 volt sampai +1.5 volt. Sehingga kondisi ini dianggap tidak terdefinisikan, karena logic 1 range tegangannya -1.5 volt sampai -6 volt. Sedangkan logika 0 range tegangannya antara +1.5 volt sampai 6 volt. Kondisi ini disebut juga kondisi idle atau kondisi ketika tidak ada data yang ditransmisikan.



