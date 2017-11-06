## 3.4 Penanganan Error Pada CAN Bus Error

Salah satu kelebihan CAN Bus adalah pada kemampuannya untuk menangani error yang terjadi pada komunikasi data. 

### 3.4.1 Arbitration

Abritation merupakan salah satu fitur penanganan error yang ada pada CAN Bus. Abritation berfungsi untuk menangani jika terjadi pengiriman data yang berbarengan antara dua atau lebih node. Pada komunikasi data multi point menggunakan EIA-485, tidak diijinkan ada dua perangkat yang mengirimkan data secara bersamaan dalam satu jaringan yang sama. Hal ini akan menyebabkan error pada data yang dikirimkan. Namun, hal ini tidak terjadi pada CAN Bus. Semua node boleh saja mengirimkan data secara bersamaan, namun pada akhirnya node yang memiliki ID lebih rendah \(prioritas lebih tinggi\) yang akan tetap mengirimkan data. Node yang lainnya akan mengalah dengan cara menunggu sampai proses pengiriman data dari node yang memiliki prioritas lebih tinggi selesai dan kemudian mengirimkan data ulang. Proses ini dikenal dengan nama arbitration. 

![](/assets/2017-11-06_134502.png)

Proses arbitration lebih jelasnya dapat dilihat pada gambar di atas. Gambar diatas menjelaskan bahwa ada dua node yang sama-sama mengirimkan data. Node tersebut memiliki ID 15 dan ID 16. Seperti yang telah dijelaskan pada materi sebelumnya, proses pengiriman data akan menggunakan data frame. Sinyal start akan dikirimkan terlebih dahulu untuk memberikan petanda bahwa akan terjadi proses pengiriman data. Setelah sinyal start dikirim, sinyal selanjutnya yang dikirimkan adalah sinyal ID \(Identifier\) dari node yang mengirimkan data. Pada contoh diatas standar CAN yang digunakan adalah CAN 2.0 A dimana ID terdiri dari 11 bit ID. Node dengan ID 15 akan mengirimkan sinyal **0b00000001111**, sedangkan node dengan ID 16 akan mengirimkan sinyal **0b00000010000**. Pada saat mentransmisikan sinyal ID pada bit ke 4 akan terjadi perbedaan antara node 15 dan node 16. Dimana node 15 sinyalnya adalah dominat \(0\) sementara node 16 sinyalnya adalah recessive \(1\). Pada saat seperti ini, sinyal dominant akan menjadi prioritas. Sehingga membuat node 16 menghentikan transmisi data sementara dan menunggu sampai transmisi data dari node 15 selesai. Proses arbitration ini menjadi jaminan tidak ada kesalahan pengiriman data walaupun ada lebih dari dua node yang mengirimkan data secara bersamaan.



