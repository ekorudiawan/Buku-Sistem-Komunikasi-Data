## 4.2 Frame Data Ethernet

Frame data pada ethernet dapat dilihat pada gambar berikut ini  
![](/assets/2017-11-14_094627.png)

1. SSD, Start of Frame merupakan sinyal penanda awal pengiriman data.
2. Preamble, merupakan bit yang digunakan untuk melakukan sinkronisasi. Bit preamble terdiri dari 7 byte sinyal sinkronisasi dan ditambahkan 1 byte sinyal SFD yang merupakan sinyal delimiter dari preambble.  
   ![](/assets/2017-11-15_083818.png)

3. MAC Header, berisi informasi tentang MAC Address pengirim \(6 byte\) dan MAC Addreess penerima \(6 byte\) serta 2 byte mencakup informasi panjang data yang akan dikirimkan atau tipe service yang digunakan pada saat pengiriman data. Pada standar IEEE802.3 2 byte terakhir berisi informasi panjang data yang akan dikirim \(0-1500\).  
   ![](/assets/2017-11-15_084503.png)

4. IP Header, berisi informasi IP Address pengirim dan IP address penerima. TCP/UDP Header, protokol data yang digunakan pada pengiriman data. Data, merupakan sinyal data yang akan dikirimkan. Ukuran panjang data \(IP Header + TCP/UDP Header + DATA\) yang dapat dikirimkan dalam satu paket data minimal adalah 46 byte dan maksimal adalah 1500byte dalam sekali pengiriman frame data.  
   ![](/assets/2017-11-15_090055.png)  
   ![](/assets/2017-11-15_091555.png)  
  
   **Version**: Version no. of Internet Protocol used \(e.g. IPv4\).

   **IHL**: Internet Header Length; Length of entire IP header.

   **DSCP**: Differentiated Services Code Point; this is Type of Service.

   **ECN**: Explicit Congestion Notification; It carries information about the congestion seen in the route.

   **Total Length**: Length of entire IP Packet \(including IP header and IP Payload\).

   **Identification**: If IP packet is fragmented during the transmission, all the fragments contain same identification number. to identify original IP packet they belong to.

   **Flags**: As required by the network resources, if IP Packet is too large to handle, these ‘flags’ tells if they can be fragmented or not. In this 3-bit flag, the MSB is always set to ‘0’.

   **Fragment Offset**: This offset tells the exact position of the fragment in the original IP Packet.

   **Time to Live**: To avoid looping in the network, every packet is sent with some TTL value set, which tells the network how many routers \(hops\) this packet can cross. At each hop, its value is decremented by one and when the value reaches zero, the packet is discarded.

   **Protocol**: Tells the Network layer at the destination host, to which Protocol this packet belongs to, i.e. the next level Protocol. For example protocol number of ICMP is 1, TCP is 6 and UDP is 17.

   Header Checksum: This field is used to keep checksum value of entire header which is then used to check if the packet is received error-free.

   **Source Address**: 32-bit address of the Sender \(or source\) of the packet.

   **Destination Address**: 32-bit address of the Receiver \(or destination\) of the packet.

   **Options**: This is optional field, which is used if the value of IHL is greater than 5. These options may contain values for options such as Security, Record Route, Time Stamp, etc.  
   ![](/assets/92926-tcp_udp_headers.jpg)

5. FCS, merupakan sinyal yang digunakan untuk pengecekan error. FCS sama halnya dengan CRC pada CAN Bus. Sinyal FCS pada ethernet memiliki panjang 4 bytes.



Berdasarkan packet data diatas dalam sekali penmgiriman data menggunakan ethernet jumlah byte minimal dan maksimal yang dikirim adalah sebagai berikut.

![](/assets/2017-11-15_093340.png)



