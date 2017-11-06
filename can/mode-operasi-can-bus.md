## 3.3 Mode Operasi CAN Bus

CAN Bus hanya dapat beroperasi pada mode komunikasi data simplex dan half-duplex. Hal ini dikarenakan saluran transmitter dan receiver pada CAN Bus dihubungkan menjadi satu pada terminal CAN High dan CAN Low. Sehingga proses pengiriman data antar node harus bergantian satu dengan yang lainnya. Masing-masing node dapat mengirimkan data ke semua node yang terintegrasi dalam jaringan komunikasi yang sama. Tidak ada batasan master dan slave seperti pada EIA-485.   
![](/assets/2017-11-06_131514.png)Pada gambar di atas dapat terlihat bahwa driver \(transmitter\) memiliki saluran keluar yang terhubung ke terminal CAN High dan CAN Low. Receiver juga memiliki saluran masuk yang berasal dari terminal CAN High dan CAN Low. Hal ini menyebabkan apapun data yang dikirim melalui CAN Bus akan diterima oleh semua node, termasuk diterima oleh node transmitter itu sendiri.

