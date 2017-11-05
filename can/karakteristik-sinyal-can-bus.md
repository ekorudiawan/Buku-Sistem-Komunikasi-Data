## 3.1 Karakteristik Sinyal CAN Bus

Karakteristik sinyal pada CAN Bus lebih mirip dengan sinyal pada EIA-485. Dimana terdapat dua buat terminal kabel yang digunakan untuk melakukan transimi data. Perbedaan tegangan dari kedua terminal kabel ini yang digunakan untuk merepresentasikan sinyal high dan low dalam transmisi data. Kedua terminal pada CAN bus tersebut dikenal dengan nama CAN High dan CAN Low. Pada CAN Bus dikenal dua jenis sinyal yaitu dominant dan recessive. Dominant sama dengan logic 0 \(low\) sedangkan recessive sama dengan logic 1 \(high\).

Terdapat dua standar sinyal pada CAN Bus yaitu standar **ISO 11898-2 **atau lebih dikenal dengan nama High Speed CAN Bus. Standar kedua yaitu **ISO 11898-3 **atau lebih dikenal dengan nama Low Speed CAN Bus. Perbedaan dari kedua standar tersebut akan dijelaskan di bawah ini.

### 3.1.1 High Speed CAN Bus

High Speed CAN Bus menggunakan topologi jaringan model linear bus. Dimana semua perangkat akan terkoneksi dalam sebuah jaringan yang memiliki dua buah titik akhir \(endpoint\). Semua perangkat akan dihubungkan ke terminal CAN High dan CAN Low. Pada masing-masing endpoint akan dipasang resistor termination yang nilainya sebesar 120 ohm. Lebih jelasnya bisa dilihat pada gambar berikut ini :  
![](/assets/CAN_ISO11898-2_Network.png) 

Karakteristik sinyal pada high speed CAN bus dapat dilihat di bawah ini :

1. Recessive \(high . 1\) terjadi jika perbedaan tegangan pada terminal CAN High dan CAN Low antara -0.1 volt sampai +0.5 volt
2. Dominant \(low / 0\) terjadi jika perbedaan tegangan pada terminal CAN High dan CAN Low antara +0.9 volt sampai +5 volt

![](/assets/2017-11-05_210341.png)

![](/assets/ISO11898-2.svg)

### 3.1.2 Low Speed CAN Bus



