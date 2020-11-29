## Lapres Praktikum Jaringan Komputer Modul 3

---

Anggota :

1. 05111840000039 - Sitti Chofifah
2. 05111840000139 - Dohan Pranata W.

---

---

1.  Membuat topologi jaringan

- Surabaya sebagai route
- Malang sebagai DNS server
- Tuban sebagai DHCP server
- Mojokerto sebagai proxy server
- UML sisa sebagai client

2.  Surabaya ditunjuk sebagai perantara (DHCP Relay) antara DHCP Server dan Client

    Jawab :

a. Install isc-dhcp-relay di Surabaya `apt-get install isc-dhcp-relay`

![](myMediaFolder\media\image6.png)

![](myMediaFolder\media\image12.png)

b. Masukkan IP Tuban dan masukan interface dengan eth1 eth2 seperti gambar di atas

c. Buka `/etc/default/isc-dhcp-relay` di surabaya dan edit seperti gambar dibawah ini

![](myMediaFolder\media\image11.png)

d. Kemudian lakukan `service isc-dhcp-relay restart`

3.  Client pada subnet 1 mendapatkan range IP dari 192.168.0.10 sampai
    > 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200.
4.  Client pada subnet 3 mendapatkan range IP dari 192.168.1.50 sampai
    > 192.168.1.70.
5.  Client mendapatkan DNS Malang dan DNS 202.46.129.2 dari DHCP
6.  Client di subnet 1 mendapatkan peminjaman alamat IP selama 5 menit,
    > sedangkan client pada subnet 3 mendapatkan peminjaman IP selama 10
    > menit.

Jawaban no 3,4,5,6 :

a. Install isc-dhcp-server di Tuban `apt-get install isc-dhcp-server`
b. Buka `nano /etc/default/isc-dhcp-server`
c. Buka `nano /etc/dhcp/dhcpd.conf` lalu ketikkan config seperti dibawah.

//jawaban no 3

> subnet 192.168.0.0 netmask 255.255.255.0 {
> range 192.168.0.10 192.168.0.100;
> range 192.168.0.110 192.168.0.200;
> option routers 192.168.0.1;
> option broadcast-address 192.168.0.255;
> option domain-name-servers 10.151.83.98, 202.46.129.2; //jawaban no 5
> default-lease-time 300; //jawaban no 6
> max-lease-time 300;
> }

//jawaban no 4

> subnet 192.168.1.0 netmask 255.255.255.0 {
> range 192.168.1.50 192.168.1.70;
> option routers 192.168.1.1;
> option broadcast-address 192.168.1.255;
> option domain-name-servers 10.151.83.98, 202.46.129.2; //jawaban no 5
> default-lease-time 600; //jawaban no 6
> max-lease-time 600;
> }
> subnet 10.151.83.0 netmask 255.255.255.0 {
> }

Restart service isc-dhcp-server dengan perintah `service isc-dhcp-server restart`

![](myMediaFolder\media\image15.png)

d. Setting semua interfaces di masing masing UML client di `nano /etc/network/interface` seperti berikut

> auto eth0
> iface eth0 inet dhcp
> ![](myMediaFolder\media\image22.png)

e. Lakukan network restart di semua UML Client

![](myMediaFolder\media\image10.png)

7. Membuat Proxy sebagai penghubung jaringan lokal ke internet dengan syarat proxy hanya bisa dilakukan oleh Anri sebagai user TA. User autentikasi milik Anri memiliki format:
   ● User : userta_yyy
   ● Password : inipassw0rdta_yyy
   > Keterangan : yyy adalah nama kelompok masing-masing. Contoh:
   > userta_c01
   > Jawab :
   > a. Install squid pada UML Mojokerto `apt-get install squid`
   > b. Buat user dan password baru
   > ![](myMediaFolder\media\image4.png)

c. Backup config

![](myMediaFolder\media\image18.png)

d. Edit config sebagai berikut

![](myMediaFolder\media\image3.png)

e. Lalu restart

f. Atur pengaturan proxy di laptop sebagai berikut

![](myMediaFolder\media\image13.png)

g. Masuk ke browser dan coba akses suatu web, maka akan muncul tampilan > seperti berikut

![](myMediaFolder\media\image2.png)

h. Masukkan user dan password seperti yang sudah di atur , dan berikut > tampilan setelah berhasil login

![](myMediaFolder\media\image7.png)

8. Setiap hari selasa - rabu pukul 13.00-18.00 . Bu Meguri membatasi
   penggunaan internet Anri hanya pada jadwal yang telah ditentukan saja.
   Maka di luar jam tersebut Anri tidak dapat mengakses jaringan internet
   dengan proxy tersebut

9. Jadwal Bimbingan bu Meguri adalah setiap hari selasa - kamis pukul
   21.00-09.00 keesokan harinya (sampai jumat jam 09.00)

Jawaban no 8 & 9

a. Buka `nano /etc/squid/squid.conf` lalu ubah seperti berikut:

![](myMediaFolder\media\image16.png)

b. Buka dan edit seperti konfigurasi di bawah ini

![](myMediaFolder\media\image5.png)

c. Berikut hasil setelah melakukan konfigurasi pengaturan waktu akses

![](myMediaFolder\media\image17.png)

10. Setiap mengakses google.com makan di redirect menuju
    monta.if.its.ac.d

Jawab :

a. Buka `nano /etc/squid/squid.conf`

b. Tambahkan potongan syntax berikut :

> acl site dstdomain .google.com
> deny_info http://monta.if.its.ac.id all
> http_access deny site all

![](myMediaFolder\media\image16.png)
c. Berikut bukti berhasil melakukan redirect ke monta.if.its.ac.id saat mengakses google.com

![](myMediaFolder\media\image20.png)
![](myMediaFolder\media\image8.png)

11. Bu Meguri meminta Anri untuk mengubah error default squid menjadi
    error page yang dapat diunduh di wget 10.151.36.202/error403.tar.gz

![](myMediaFolder\media\image14.png)

a. Masuk ke folder /usr/share/squid/errors/en
b. Lalu rm ERR_ACCESS_DENIED
c. Kemudian lakukan wget 10.151.36.202/ERR_ACCESS_DENIED
d. Berikut hasil nya :

![](myMediaFolder\media\image21.png)

12. Karena Bu Meguri dan Anri adalah tipe orang yang pelupa , maka untuk memudahkan mereka, Anri memiliki ide ketika menggunakan proxy cukup dengan mengetikkan domain janganlupa-ta.yyy.pw dan memasukkan port 8080.

Jawab :

a. Install bind9 pada Malang dengan perintah `apt-get install bind9 -y`
b. Kemudian masuk ke `nano /etc/bind/named.conf.local`
c. Isikan konfigurasi domain janganlupa-ta.b11.pw sesuai dengan syntax berikut

> zone \"janganlupa-ta.b11.pw\"{
> type master;
> file \"/etc/bind/jarkom/janganlupa-ta.b11.pw\";
> };

![](myMediaFolder\media\image1.png)

d. Buat folder jarkom `mkdir /etc/bind/jarkom`

e. Copykan file db.local pada path /etc/bind ke dalam folder janganlupa-ta.b11.pw yang sudah dibuat dan ubah namanya menjadi jarkom

> cp /etc/bind/db.local /etc/bind/jarkom/janganlupa-ta.b11.pw

f. Buka nano /etc/bind/jarkom/janganlupa-ta.b11.pw dan ubah syntaxnya seperti gambar dibawah ini

![](myMediaFolder\media\image19.png)
g. Setelah itu lakukan restart bind9 dengan perintah `service bind9 restart`
h. Untuk mengecek apakah domain sudah bisa dijalankan, maka lakukan pengaturan proxy di laptop seperti gambar dibawah ini
![](myMediaFolder\media\image9.png)
