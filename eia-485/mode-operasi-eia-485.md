## 2.3 Mode Operasi EIA-485

### Mode Dua Kabel \(Two Wire Network\)

Pada mode operasi dua kabel, jalur transmitter dan receiver pada tiap perangkat dihubungkan menjadi satu. Terminal A dan terminal B dari tiap perangkat dihubungkan semuanya dalam jalur kabel yang sama. Untuk lebih jelasnya bisa dilihat pada Gambar 3.

![](/assets/2017-10-25_210502.png)

Gambar 3. Model Komunikasi EIA-485 Two Wire Network

Pada mode dua kabel EIA-485 beroperasi secara half duplex. Hanya ada satu perangkat yang dapat mengirimkan data dalam satu jaringan data. Jika perangkat lain ingin mengirimkan data, maka proses pengiriman data harus dilakukan secara bergantian antara satu perangkat dengan perangkat lainnya.

### Mode Empat Kabel \(Four Wire Network\)

Pada mode empat kabel jalur tranmisi data antara pengirim dan penerima terpisah. Untuk lebih jelasnya dapat dilihat pada Gambar 4.

![](/assets/2017-10-25_211552.png)

Gambar 4. Model Komunikasi EIA-485 Four Wire Network

Pada model komunikasi seperti ini, disyaratkan terdapat satu buah perangkat yang bertindak sebagai master. Perangkat yang lainnya bertindak sebagai slave. Pada mode komunikasi four wire, master dapat mengirimkan data ke semua slave. Slave juga dapat mengirimkan data ke master. Namun, slave tidak dapat mengirimkan data ke sesama slave. Proses pengiriman data dari slave ke master juga harus dilakukan secara bergantian antara masing-masing perangkat slave. Pada model komunikasi four wire EIA-485 dapat beroperasi secara full duplex, karena jalur transmitter dan receiver pada perangkat master telah dipisah menggunakan jalur kabel yang berbeda. Sehingga memungkinkan master untuk mengirim dan menerima data dalam waktu yang sama.

### Mode Repeater

Jika dalam satu jaringan komunikasi data EIA-485 diinginkan perangkat yang berkomunikasi lebih dari 32 perangkat. Maka disyaratkan menggunakan repeater. Model komunikasi menggunakan repeater dapat dilihat pada Gambar 5.

![](/assets/2017-10-25_212823.png)Gambar 5. EIA-485 Menggunakan Repeater

Pada Gambar 5 repeater dipasang untuk  menghubungkan local station dengan remote station. Area local station bisa dihubungkan maksimal 32 perangkat, begitu juga dengan remote station.

