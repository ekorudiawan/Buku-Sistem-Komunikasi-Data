## 3.2 Frame Data CAN Bus

Pada CAN Bus dikenal beberapa frame yang digunakan sebagai standar untuk melakukan transmisi data. Frame-frame tersebut adalah data frame, remote frame, error frame, overload frame. Lebih detail penjelasan dari masing-masing frame tersebut akan dijelaskan di bawah ini

### 3.2.1 Data Frame

Data frame digunakan ketika sebuah node atau perangkat ingin mengirimkan data kepada node lainnya. Data frame berisi informasi tentang ID, data length dan data itu sendiri. ID merupakan identitas node pengirim data. Pada CAN Bus, ID menentukan prioritas dari data yang ditransmisikan. Jika ada lebih dari satu node dalam satu jaringan yang mengirimkan data, maka node dengan ID terendah \(prioritas tertinggi\) yang akan tetap mengirimkan data. Sementara node lain harus menunda pengiriman data sampai data yang ditransmisikan oleh node dengan ID terendah selesai. Data length berisi informasi tentang total bytes data dalam satu kali transmisi. Maksimal jumlah byte yang diizinkan dalam satu kalu transmisi adalah 8 byte atau 64 bit data. Sementara informasi data akan disertakan tergantung dari ukuran byte yang sudah dideskripsikan sebelumnya pada data length. Misalkan data length berisi 1 byte maka hanya ada 1 byte data atau 8 bit data yang akan ditransmisikan. Selain ID, data length dan data bit, ada bit-bit lain juga yang disertakan dalam pengiriman data. Perlu diketahui bahwa data frame pada standar CAN 2.0 A dan CAN 2.0 B sedikit berbeda. Namun perbedaannya tidak terlalu signifikan, perbedaan ini dikarenakan oleh ukuran bit ID yang berbeda antara CAN 2.0 A dan CAN 2.0 B. Untuk lebih jelasnya lihat ilustrasi gambar berikut

#### Data Frame CAN 2.0 A

![](/assets/CAN-Bus-frame_in_base_format_without_stuffbits.svg)  
![](/assets/2017-11-06_131441.png)

| Nama Signal | Ukuran \(bit\) | Fungsi | Nilai |
| :---: | :---: | :---: | :---: |
| Start Frame | 1 | Sinyal penanda akan dilakukan transmisi data | Dominant \(Low\) |
| ID \(Identifier\) \(warna hijau\) | 11 | ID unik dari masing-masing perangkat yang merepresentasikan prioritas dari data yang akan dikirim | Tergantung ID dari node |
| Remote transmission request \(RTR\) | 1 | Sinyal untuk mengirim data atau melakukan request data | Dominant \(low\) untuk data frame \(node mengirimkan data\) |
| Identifier extension bit \(IDE\) | 1 | Sinyal penanda menggunakan 11 bit ID \(CAN 2.0 A\) atau 29 bit ID \(CAN 2.0 B\) | Dominant \(low\) untuk 11 bit ID \(CAN 2.0 B\) |
| Reserved bit \(r0\) | 1 | Bit data cadangan, saat ini belum digunakan | Dominant \(low\) |
| Data length code \(DLC\) \(warna kuning\) | 4 | Panjang data yang akan ditransmisikan | Tergantung panjang data yang akan ditransmisikan \(0-8\) |
| Data field \(warna merah\) | 0-64 | Data yang akan ditransmisikan | Tergantung data yang akan dikirim |
| CRC | 15 | Sinyal yang digunakan untuk melakukan pengecekan error | Tergantung hasil perhitungan CRC |
| CRC delimiter | 1 | Sinyal pembatas CRC | Recessive \(high\) |
| ACK slot | 1 | Sinyal perrnyataan bahwa transmisi data telah selesai | Node transitter akan mengirimkan sinyal recessive. Sedangkan node receiver akan mengirimkan sinyal dominant |
| ACK delimiter | 1 | Sinyal pembatas ACK | Recessive \(high\) |
| End-of-frame \(EOF\) | 7 | Sinyal petanda bahwa transmisi data sudah complete | Recessive sebanyak 7 bit data |

#### Data Frame CAN 2.0 B

![](/assets/2017-11-06_131457.png)

| Nama Signal | Ukuran \(bit\) | Fungsi | Nilai |
| :---: | :---: | :---: | :---: |
| Start Frame | 1 | Sinyal penanda akan dilakukan transmisi data | Dominant \(Low\) |
| ID \(Identifier\) \(warna hijau\) | 11 | ID unik dari masing-masing perangkat yang merepresentasikan prioritas dari data yang akan dikirim \(11 bit pertama\) | Tergantung ID dari node |
| Substitute remote request \(SRR\) | 1 |  | Recessive |
| Identifier extension bit \(IDE\) | 1 | Sinyal penanda menggunakan 11 bit ID \(CAN 2.0 A\) atau 29 bit ID \(CAN 2.0 B\) | Recessive untuk 29 bit ID \(CAN 2.0 B\) |
| ID \(Identifier\) \(warna hijau\) | 18 | ID unik dari masing-masing perangkat yang merepresentasikan prioritas dari data yang akan dikirim \(18 bit kedua\) | Tergantung ID dari node |
| Remote transmission request \(RTR\) | 1 | Sinyal untuk mengirim data atau melakukan request data | Dominant \(low\) untuk data frame \(node mengirimkan data\) |
| Reserved bit \(r1, r0\) | 2 | Bit data cadangan, saat ini belum digunakan | Dominant \(low\) |
| Data length code \(DLC\) \(warna kuning\) | 4 | Panjang data yang akan ditransmisikan | Tergantung panjang data yang akan ditransmisikan \(0-8\) |
| Data field \(warna merah\) | 0-64 | Data yang akan ditransmisikan | Tergantung data yang akan dikirim |
| CRC | 15 | Sinyal yang digunakan untuk melakukan pengecekan error | Tergantung hasil perhitungan CRC |
| CRC delimiter | 1 | Sinyal pembatas CRC | Recessive \(high\) |
| ACK slot | 1 | Sinyal perrnyataan bahwa transmisi data telah selesai | Node transitter akan mengirimkan sinyal recessive. Sedangkan node receiver akan mengirimkan sinyal dominant |
| ACK delimiter | 1 | Sinyal pembatas ACK | Recessive \(high\) |
| End-of-frame \(EOF\) | 7 | Sinyal petanda bahwa transmisi data sudah complete | Recessive sebanyak 7 bit data |

### 3.2.2 Remote Frame

Remote frame merupakan frame yang digunakan untuk melakukan request data. Umumnya pada komunikasi CAN Bus tiap node akan melakukan pengiriman data tanpa adanya permintaan terlebih dahulu. Proses pengiriman data seperti ini wajib menggunakan data frame. Selain itu bisa juga terjadi proses pengiriman data atas permintaan node lain. Proses meminta data ini mewajibkan menggunakan format remote frame. Perbedaan remote frame dengan data frame adalah sebagai berikut :

1. Pada remote frame sinyal RTR selalu bernilai recessive. Pada data frame sinyal RTR selalu bernilai dominant
2. Pada remote frame tidak terdapat data bit. Pada data frame pasti terdapat data bit
3. Pada remote frame sinyal DLC berfungsi untuk menentukan ukuran data yang akan di request. Sedangkan pada data frame sinyal DLC menentukan ukuran data yang akan dikirim 

### 3.2.3 Error Frame

Error frame merupakan sinyal yang dikirimkan jika terjadi error atau kesalahan pada proses transmisi data. Error frame terdiri dari sinyal error flag dan error delimiter. Error flag merupakan sinyal error itu sendiri, yang terdiri dari 6-12 bit sinyal recessive atau dominant. Sedangkan error delimiter merupakan sinyal yang dikirimkan setelah error flag, yang berfungsi sebagai petanda bahwa error flag telah dikirim. Sinyal error delimiter terdiri dari 8 bit sinyal dengan nilai recessive \(high\). Error frame akan dikirimkan oleh node jika node tersebut mendeteksi error pada CAN bus.

### 3.2.4 Overload Frame

Overload frame merupakan sinyal yang akan dikirimkan sebuah node jika node tersebut tidak mampu menampung data yang dikirimkan oleh node lain. Hal ini dapat terjadi karena terlalu cepatnya proses transfer data, sehingga node receiver tidak mampu mengolah data yang diterima atau disebut dengan overload. Ketika terjadi overload, node receiver dapat mengirimkan sinyal overload frame yang berfungsi untuk menginformasikan ke node transmitter agar proses pengiriman data diberi jeda waktu \(delay\).

