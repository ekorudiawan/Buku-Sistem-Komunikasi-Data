## 3.4 Penanganan Error Pada CAN Bus Error

Salah satu kelebihan CAN Bus adalah pada kemampuannya untuk menangani error yang terjadi pada komunikasi data.

### 3.4.1 Arbitration

Abritation merupakan salah satu fitur penanganan error yang ada pada CAN Bus. Abritation berfungsi untuk menangani jika terjadi pengiriman data yang berbarengan antara dua atau lebih node. Pada komunikasi data multi point menggunakan EIA-485, tidak diijinkan ada dua perangkat yang mengirimkan data secara bersamaan dalam satu jaringan yang sama. Hal ini akan menyebabkan error pada data yang dikirimkan. Namun, hal ini tidak terjadi pada CAN Bus. Semua node boleh saja mengirimkan data secara bersamaan, namun pada akhirnya node yang memiliki ID lebih rendah \(prioritas lebih tinggi\) yang akan tetap mengirimkan data. Node yang lainnya akan mengalah dengan cara menunggu sampai proses pengiriman data dari node yang memiliki prioritas lebih tinggi selesai dan kemudian mengirimkan data ulang. Proses ini dikenal dengan nama arbitration.

![](/assets/2017-11-06_134502.png)

Proses arbitration lebih jelasnya dapat dilihat pada gambar di atas. Gambar diatas menjelaskan bahwa ada dua node yang sama-sama mengirimkan data. Node tersebut memiliki ID 15 dan ID 16. Seperti yang telah dijelaskan pada materi sebelumnya, proses pengiriman data akan menggunakan data frame. Sinyal start akan dikirimkan terlebih dahulu untuk memberikan petanda bahwa akan terjadi proses pengiriman data. Setelah sinyal start dikirim, sinyal selanjutnya yang dikirimkan adalah sinyal ID \(Identifier\) dari node yang mengirimkan data. Pada contoh diatas standar CAN yang digunakan adalah CAN 2.0 A dimana ID terdiri dari 11 bit ID. Node dengan ID 15 akan mengirimkan sinyal **0b00000001111**, sedangkan node dengan ID 16 akan mengirimkan sinyal **0b00000010000**. Pada saat mentransmisikan sinyal ID pada bit ke 4 akan terjadi perbedaan antara node 15 dan node 16. Dimana node 15 sinyalnya adalah dominat \(0\) sementara node 16 sinyalnya adalah recessive \(1\). Pada saat seperti ini, sinyal dominant akan menjadi prioritas. Sehingga membuat node 16 menghentikan transmisi data sementara dan menunggu sampai transmisi data dari node 15 selesai. Proses arbitration ini menjadi jaminan tidak ada kesalahan pengiriman data walaupun ada lebih dari dua node yang mengirimkan data secara bersamaan.

### 3.4.2 Synchronization

CAN Bus merupakan jenis komunikasi data asinkron. Dimana sinyal data akan dikirimkan tanpa sinyal clock. Proses transmisi data akan dilakukan dengan kesepakatan kecepatan transfer data yang sama pada semua node. Sehingga sebuah node dapat mengirim dan menerima data dengan timing yang sama tanpa perlu membandingkan sengan sinyal clock. Namun, pada kenyataannya terdapat sedikit perbedaan kecepatan transfer data antar node. Hal ini bisa disebabkan oleh toleransi dari beberapa komponen yang berfungsi sebagai clock generator pada masing-masing node. Perbedaan kecepatan transfer data dari masing-masing node dapat menyebabkan kesalahan pada saat mengirim ataupun menerima data. CAN Bus menggunakan proses sinkronisasi untuk menangani kesalahan ini. Proses sinkronisasi dilakukan pada saat terjadi perubahan sinyal dari recessive ke dominant. Proses sinkronisasi pertama dilakukan pada saat perubahan antara kondisi idle ke sinyal start. Dimana pada kondisi idle sinyal pada CAN Bus adalah recessive kemudian saat sinyal start ditransmisikan sinyal pada CAN Bus berubah menjadi dominant. Pada saat terjadi transisi inilah node akan mulai melakukan sinkronisasi dengan cara menghitung jumlah bit data dan lama waktu sampai terjadi transisi berikutnya. Sehingga diperoleh lama nilai t \(periode\) dari 1 bit data.

### 3.4.3 Bit Stuffing

Bit stuffing merupakan proses menambahakan beberapa bit data pada sebuah frame. Tujuan dari bit stuffing ini adalah untuk membantu proses sinkronisasi yang telah dijelaskan sebelumnya. Bit stuffing digunakan untuk memastikan transisi pada frame data cukup untuk melakukan sinkronisasi. Untuk lebih jelasnya dapat dilihat pada gambar di bawah ini.

![](/assets/CAN-Frame_mit_Pegeln_mit_Stuffbits.svg)

Pada gambar di atas dapat dilihat bahwa normalnya frame data akan dikirimkan seperti gambar pertama. Dimana ID dari node yang mengirim adalah 20. Panjang data yang akan dikirim adalah 1 byte. Sementara data yang dikirim adalah 1 diikuti dengan sinyal lainnya. Penambahan bit pada frame diatas akan dilakukan jika terdapat 5 bit yang memiliki nilai sama dan berurutan. Satu bit akan ditambahkan setelah kelima bit yang sama dan berurutan tersebut. Nilai bit nya merupakan kebalikan dari nilai lima bit yang berurutan tersebut. Pada gambar diatas perhatikan bit yang berwarna ungu. Bit yang berwarna ungu merupakan contoh dari bit stuffing.

### 3.4.4 ACK Signal

Sinyal ACK merupakan sinyal penanda bahwa tidak ada error pada saat pengiriman data. Sinyal ini akan dikirimkan oleh node transmitter dan receiver. Transmitter akan mengirimkan sinyal recessive pada bit ACK. Node receiver akan mengecek data yang diterima. Jika receiver tidak mendeteksi adanya error pada data maka node receiver akan mengirimkan sinyal dominant. Dimana sinyal dominant akan menggantikan sinyal recessive yang sebelumnya dikirim oleh transmitter. Ketika sinyal ACK bernilai dominant \(0\) ini berarti bahwa tidak ada kesalahan atau error pada saat transmisi data. Namun jika node receiver mendeteksi error maka akan mengirimkan sinyal recessive pada bit ACK. Hal ini menunjukkan bahwa ada kesalahan pada saat transmisi data. Jika dalam satu jaringan terdapat banyak node, semua node akan merespon dengan memberikan sinyal ACK tergantung data yang diterima pada masing-masing node error atau tidak. Jika ternyata hanya ada satu node yang mendeteksi error dan node lainnya tidak, maka transmitter akan menganggap tidak ada error. Transmitter akan mendeteksi error jika semua node memberikan sinyal recessive.

### 3.4.5 Interframe Spacing

Interframe spacing merupakan jarak pengiriman data antar frame. Interframe spacing berfungsi untuk memberikan jeda waktu antar pengiriman data. Interframe spacing membutuhkan minimal tiga bit recessive sebagai jarak antar frame.

### 3.4.6 Cyclic Redundancy Check

CRC \(Cyclic Redundancy Check\) merupakan 15 bit sinyal yang digunakan untuk melakukan pengecekan error terhadap data yang dikirimkan. CRC tidak hanya digunakan pada CAN Bus tetapi digunakan juga pada protokol komunikasi data lainnya contohnya Modbus.



[CAN Bus 2.0 Spesification](/assets/CAN_Bus_2.0.pdf)

