## 1.2 Frame Data EIA-232

Frame data dapat juga disebut dengan format data. Standar EIA-232 mengatur format data sedemikian rupa agar terstandarisasi.  Pada saat pengiriman data terjadi, sinyal yang dikirimnya tidak hanya sinyal data saja, namun ada tambahan sinyal lain yang dikirimkan bersamaan dengan satu paket data. Sinyal lain ini disertakan dengan maksud dan tujuan tertentu. Kumpulan sinyal data dan sinyal-sinyal tambahan ini disebut dengan frame data. 

Frame data pada EIA-232 terdiri dari :

1. Start Bit, berukuran 1 bit dengan nilai selalui 0 yang berfungsi sebagai petanda awal pengiriman data
2. Data Bit, berukuran 5, 6, 7 atau 8 bit merupakan data yang akan ditransmisikan
3. Parity, berukuran 1 bit yang dapat digunakan untuk pengecekan error pada pertukaran data
4. Stop Bit, berukuran 1 atau 2 bit dengan nilai selalu 1 sebagai petanda akhir pengiriman data

![](/assets/frame-format.png)Pada gambar di atas dapat dilihat bahwa satu frame data selalu terdiri dari start bit, data bit, parity dan stop bit. Syarat ini multak jika ingin menggunakan standar EIA-232. Namun untuk sinyal parity bisa digunakan atau tidak, sifatnya optional. Pada gambar di atas dapat dilihat bahwa pada saat idle atau tidak ada data yang ditransmisikan, kondisi sinyal akan selalu high. Data ditransmisikan diawali dengan mengirim start bit yang nilainya selalu low. Sinyal start bit ini bertujuan untuk memberikan petanda bahwa transmisi data akan segera dilakukan. Setelah start bit ditransmisikan, kemudian diikuti oleh 8 bit data. Besarnya data bit yang ditransmisikan juga tergantung dengan yang diinginkan. Bisa dipilih 5, 6, 7 atau 8 bit data dalam 1 frame data. Pada umumnya data bit yang digunakan adalah 8 bit. Setelah 8 bit data ditransmisikan kemudian diikuti oleh parity bit. Parity bit berguna sebagai sinyal untuk pengecekan error. Jika parity bit digunakan, Anda dapat memilih dua mode yaitu ODD atau EVEN. Pengecekan error menggunakan parity bit sangat sederhana, yaitu dengan menghitung banyaknya sinyal high pada data bit. Metode pengecekan error tersebut lebih jelasnya seperti berikut 

* ODD \(ganjil\),  jika jumlah sinyal high pada data bit adalah ganjil maka bit parity akan bernilai 0 \(low\). Jika jumlah sinyal high pada data bit adalah genap maka bit parity akan bernilai 1 \(high\)
* EVEN \(genap\), jika jumlah sinyal high pada data bit adalah ganjil maka bit parity akan bernilai 1 \(high\). Jika jumlah sinyal high pada data bit adalah genap maka bit parity akan bernilai 0 \(low\)

Untuk lebih jelasnya dapat dilihat pada ilustrasi tabel berikut ini

Tabel Pengecekan Error Pada Bit Parity

| Data Bit | Jumlah Bit 1 \(high\) | Even | Odd |
| :---: | :---: | :---: | :---: |
| 00000000 | 0 | 0 | 1 |
| 01010001 | 3 | 1 | 0 |
| 01101001 | 4 | 0 | 1 |
| 01111111 | 7 | 1 | 0 |

Sinyal terakhir yang dikirimkan pada satu frame data adalah stop bit. Sinyal stop bit berfungsi untuk menandakan bahwa pengiriman data sudah berakhir. Sinyal stop bit dapat menggunakan 1 bit sinyal atau 2 bit sinyal, tergantung konfigurasi yang digunakan. Perlu digarisbawahi bahwa format frame data antara dua perangkat yang saling berkomunikasi harus sama. Jika tidak sama kemungkinan besar proses komunikasi data akan error.

