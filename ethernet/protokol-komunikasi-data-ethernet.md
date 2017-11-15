## 4.4 Protokol Komunikasi Data Ethernet

Pada komunikasi data ethernet, terdapat dua protokol komunikasi data yang dapat digunakan yaitu TCP dan UDP. Penjelasan lebih detail tentang kedua protokol tersebut dapat dilihiat di bawah ini

### Port dan Socket

Pada komunikasi data ethernet terdapat istilah port dan socket. Port merupakan jalur keluar dan masuknya data. Port pada komunikasi data ethernet didefinisakan pada TCP header atau UDP header. Port disini bukan berarti konektor untuk menghunbungkan kabel. Port pada komunikasi ethernet merupakan nomor unik yang menentukan standar protokol yang digunakan. Nomer port terdiri dari 16bit nomer unik. Berarti terdapat maksimal 65536 nomor port. Beberapa nomor port sudah digunakan untuk protokol komunikasi standar seperti port 80 untuk protokol HTTP, port 23 untuk Telnet, dll. Namun ada beberapa nomer port yang masih belum digunakan untuk protokol tertenttu. Port yang belum digunakan ini bisa kita gunakan untuk melakukan komunikasi data tanpa protokol tertentu.

Socket merupakan istilah yang digunakan untuk menunjukkan IP Address dan port yang digunakan untuk komunikasi data. Pada komunikasi ethernet data akan dikirimkan secara spesifik ke tujuan tertentu \(IP Address\) dan ke nomer port tertentu \(Port\). Serta dikirimkan dengan infromasi IP Address pengirim dan port pengirim.

### 4.4.1 UDP

UDP merupakan salah satu protokol komunikasi data pada komunikasi ethernet. Komunikasi data menggunakan protokol UDP tidak menjamin data sampai pada tujuan atau yang dikenal dengan nama **Connectionless Oriented**. Data akan dikirimkan secara broadcast ke penerima dengan IP address dan port terterntu.  
![](/assets/Picture1.png)

Gambar diatas menjelaskan proses komunikasi data menggunakan UDP. Server dan client sama-sama memulai komunikasi data dengan cara membuka akses pada nomor port yang telah ditentukan. Setelah itu data dikirimkan dari client ke server atau server ke client. Secara normal proses komunikasi akan berjalan lancar. Namun jika pada saat komunikasi data tiba-tiba koneksi terputus, baik client ataupun server tidak dapat memastikan bahwa data yang dikirim ternyata tidak sampai pada tujuan. Hal ini dikarenakan protokol UDP tidak menggunakan proses **acknowledgement**. 

### 4.4.2 TCP

TCP merupakan protokol komunikasi data pada ethernet yang dapat menjamin bahwa data yang dikirimkan sampai ke tujuan. Hal ini disebabkan karena pada TCP kedua perangkat yang saling berkomunikasi harus melakukan proses persetujuan terlebih dahulu \(acknowledgement\). Kedua perangkat harus saling sepakat terlebih dahulu untuk dapat bertukar data. Jika pada pengiriman data ternyata terdapat error atau data tidak sampai ke tujuan, data akan dikirimkan ulang oleh pengirim.  
![](/assets/Picture2.png)

Proses komunikasi data pada TCP dimulai dengan membuka akses ke port yang digunakan. Setelah itu dari sisi client melakukan request untuk membuka koneksi ke server. Proses ini memerlukan persetujuan server agar komunikasi data dapat terjadi. Setelah proses persetujuan terjadi kedua perangkat dapat saling bertukar data.

