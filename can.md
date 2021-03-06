# 3. CAN \(Controller Area Network\)

CAN atau yang biasa disebut dengan CAN Bus merupakan standar komunikasi data yang pada awalnya digunakan pada industri otomotif. CAN Bus digunakan untuk menghubungkan antar sub-system pada kendaraan agar bisa saling terhubung dan bertukar data tanpa menggunakan banyak kabel. CAN pertama kali dikembangkan oleh perusahaan [Robert Bosch GmbH](https://en.wikipedia.org/wiki/Robert_Bosch_GmbH) dan secara resmi dirilis sebagai standar komunikasi data pada industri otomotif oleh [Society of Automotive Engineers](https://en.wikipedia.org/wiki/Society_of_Automotive_Engineers) \(SAE\). Saat ini standar komunikasi CAN yang dirilis telah mencapai versi 2.0 dan dibagi menjadi dua part yaitu CAN 2.0 A dan CAN 2.0 B. Sampai saat ini standar CAN masih tetap dikembangkan oleh Bosch. Rilis paling terbaru adalah standar CAN FD \(CAN with Flexible Data-Rate\) yang dikeluarkan pada tahun 2012. Walaupun CAN FD merupakan versi paling terbaru, namun standar CAN 2.0 A dan CAN 2.0 B masih banyak digunakan. Untuk saat ini CAN tidak hanya digunakan pada industri otomotif saja, namun sudah merambah ke industri automation dan aviation.

CAN Bus dapat digunakan untuk komunikasi data multi point sama seperti EIA-485. Ini berarti dalam satu jaringan CAN bus bisa terdapat lebih dari dua perangkat yang dapat bertukar data. Tidak ada master dan slave pada komunikasi CAN, semua perangkat yang terhubung dalam satu jaringan disebut dengan node. Masing-masing node dapat berkomunikasi sesama node lainnya selama masih dalam satu jaringan yang sama. Untuk standar CAN 2.0 A sendiri maksimal node yang dapat terkoneksi dalam satu jaringan komunikasi adalah sebanyak 2048 node. Sedangkan untuk standar CAN 2.0 B node yang dapat terhubung bisa lebih banyak lagi yaitu 536.870.912 node. Banyaknya node yang dapat terhubung sebetulnya tergantung dari kapasitas bit ID yang disediakan oleh standar CAN itu sendiri. Untuk standar CAN 2.0 A bit ID yang disediakan adalah 11 bit, sehingga dalam satu jaringan maksimal node yang boleh terhubung adalah 2^11 node. Sedangkan untuk standar CAN 2.0 B bit ID yang disediakan adalah 29 bit. Hal ini tentunya membuat CAN 2.0 B memiliki jumlah node yang lebih banyak dalam satu jaringan.

## 

## 



