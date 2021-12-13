# Jarkom-Modul-5-D02-2021

**Nama Anggota Kelompok D2 :**
- Sabrina Lydia Simanjuntak (05111940000107)
- Zulfayanti Sofia Solichin (05111940000189)
- Nadia Tiara Febriana (05111940000217)
 
(A) Tugas pertama kalian yaitu membuat topologi jaringan sesuai dengan rancangan yang diberikan Luffy dibawah ini:

<img width="499" alt="Screen Shot 2021-12-11 at 19 03 07" src="https://user-images.githubusercontent.com/72669398/145675908-767385f8-49ed-495e-8a73-6b0891bbb90c.png">

Keterangan : 	Doriki adalah DNS Server
		Jipangu adalah DHCP Server
		Maingate dan Jorge adalah Web Server
		Jumlah Host pada Blueno adalah 100 host
		Jumlah Host pada Cipher adalah 700 host
		Jumlah Host pada Elena adalah 300 host
		Jumlah Host pada Fukurou adalah 200 host

(B) Karena kalian telah belajar subnetting dan routing, Luffy ingin meminta kalian untuk membuat topologi tersebut menggunakan teknik CIDR atau VLSM. setelah melakukan subnetting, 

Melakukan subnetting VLSM sebagai berikut

<img width="1111" alt="Screen Shot 2021-12-11 at 21 23 04" src="https://user-images.githubusercontent.com/72669398/145679990-9b37d3ad-9176-4c94-b459-318e31742ee2.png">

Setelah dikelompokkan menjadi beberapa subnet, dapat dilakukan pembagian IP sebagai berikut

<img width="643" alt="Screen Shot 2021-12-11 at 21 23 26" src="https://user-images.githubusercontent.com/72669398/145679998-423b2975-3d2c-4727-857f-cf6fb3efe3f8.png">

Dimana NID dan broadcast addressnya dapat dicari menggunakan tree sebagai berikut

<img width="539" alt="Screen Shot 2021-12-11 at 21 23 40" src="https://user-images.githubusercontent.com/72669398/145680007-78112335-4ee0-4b75-afaf-2c1dbef30b19.png">

Kemudian lakukan konfigurasi network interface pada setiap device sesuai pembagian subnet sebagai berikut

**FOOSHA**
```
auto lo
iface lo inet loopback
 
auto eth0
iface eth0 inet static
address 192.168.122.2
netmask 255.255.255.252
gateway 192.168.122.1
 
# Water7 (A4-)
auto eth1
iface eth1 inet static
address 10.22.0.1
netmask 255.255.255.252
 
#Guanhao (A5-)
auto eth2
iface eth2 inet static
address 10.22.0.5
netmask 255.255.255.252
```
**WATER7**
```
auto lo
iface lo inet loopback
 
#Foosha (A5+)
auto eth0
iface eth0 inet static
address 10.22.0.2
netmask 255.255.255.252
 
#Blueno (A2-)
auto eth1
iface eth1 inet static
address 10.22.0.129
netmask 255.255.255.128
 
#Doriki Jipangu (A1-)
auto eth2
iface eth2 inet static
address 10.22.0.9
netmask 255.255.255.248
 
#Cipher (A3-)
auto eth3
iface eth3 inet static
address 10.22.4.1
netmask 255.255.252.0
```

**GUANHAO**
```
auto lo
iface lo inet loopback
 
#Foosha (A5+)
auto eth0
iface eth0 inet static
address 10.22.0.6
netmask 255.255.255.252
 
#Elena (A6-)
auto eth1
iface eth1 inet static
address 10.22.2.1
netmask 255.255.254.0
 
#Maingate Jorge (A8-)
auto eth2
iface eth2 inet static
address 10.22.0.17
netmask 255.255.255.248
 
#Fukurou (A7-)
auto eth3
iface eth3 inet static
address 10.22.1.1
netmask 255.255.255.0
```

**DORIKI**
```
auto lo
iface lo inet loopback
 
#Water7 (A1+)
auto eth0
iface eth0 inet static
address 10.22.0.10
netmask 255.255.255.248
gateway 10.22.0.9
```

**JIPANGU**
```
auto lo
iface lo inet loopback
 
#Water7 (A1++)
auto eth0
iface eth0 inet static
address 10.22.0.11
netmask 255.255.255.248
gateway 10.22.0.9
```
 
**JORGE**
```
auto lo
iface lo inet loopback
 
#Guanhao (A8+)
auto eth0
iface eth0 inet static
address 10.22.0.18
netmask 255.255.255.248
gateway 10.22.0.17
```

**MAINGATE**
```
auto lo
iface lo inet loopback
 
#Guanhao (A8++)
auto eth0
iface eth0 inet static
address 10.22.0.19
netmask 255.255.255.248
gateway 10.22.0.17
```
 
**BLUENO**
```
auto lo
iface lo inet loopback
 
#Water7 (A2+)
auto eth0
iface eth0 inet static
address 10.22.0.130
netmask 255.255.255.128
gateway 10.22.0.129
```

CIPHER
```
auto lo
iface lo inet loopback
 
#Water7 (A3+)
auto eth0
iface eth0 inet static
address 10.22.4.2
netmask 255.255.252.0
gateway 10.22.4.1
```
 
**ELENA**
```
auto lo
iface lo inet loopback
 
#Guanhao (A6+)
auto eth0
iface eth0 inet static
address 10.22.2.2
netmask 255.255.254.0
gateway 10.22.2.1
```
 
**FUKUROU**
```
auto lo
iface lo inet loopback
 
#Guanhao (A7+)
auto eth0
iface eth0 inet static
address 10.22.1.2
netmask 255.255.255.0
gateway 10.22.1.1
```

(C) Kalian juga diharuskan melakukan Routing agar setiap perangkat pada jaringan tersebut dapat terhubung.
Melakukan konfigurasi routing pada setiap router berdasarkan NID yang sudah didapatkan sebagai berikut
 
**FOOSHA**
```
#A4
route add -net 10.22.0.128 netmask 255.255.255.128 gw 10.22.0.2 #A2
route add -net 10.22.4.0 netmask 255.255.252.0 gw 10.22.0.2 #A3
route add -net 10.22.0.8 netmask 255.255.255.248 gw 10.22.0.2 #A1
 
#A5
route add -net 10.22.2.0 netmask 255.255.254.0 gw 10.22.0.6 #A6
route add -net 10.22.1.0 netmask 255.255.255.0 gw 10.22.0.6 #A7
route add -net 10.22.0.16 netmask 255.255.255.248 gw 10.22.0.6 #A8
```

**WATER7**
```
#Default
route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.22.0.1
```
 
**GUANHAO**
```
#Default
route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.22.0.5
```
 
Kemudian jalankan perintah `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.22.0.0/16` pada FOOSHA dan `echo nameserver 192.168.122.1 > /etc/resolv.conf` pada device yang lain agar semua device dapat terhubung ke internet.

(D) Tugas berikutnya adalah memberikan ip pada subnet Blueno, Cipher, Fukurou, dan Elena secara dinamis menggunakan bantuan DHCP server. Kemudian kalian ingat bahwa kalian harus setting DHCP Relay pada router yang menghubungkannya.

Menginstall DHCP server pada node JIPANGU dengan command `apt-get install isc-dhcp-server -y` . Lalu membuat node FOOSHA, GUANHAO, dan WATER7 sebagai DHCP relay dengan menginstall `apt-get install isc-dhcp-relay`. Kemudian masukkan konfigurasi sebagai berikut
 
**DHCP Relay FOOSHA**

<img width="548" alt="Screen Shot 2021-12-11 at 21 24 00" src="https://user-images.githubusercontent.com/72669398/145680021-ca6f7680-c5d7-4b01-be70-4b170983ca17.png">

**DHCP Relay GUANHAO**

<img width="549" alt="Screen Shot 2021-12-11 at 21 24 11" src="https://user-images.githubusercontent.com/72669398/145680032-49513af1-f071-4dd1-9ae7-5ad4ce5a2e5f.png">

**DHCP Relay WATER7**

<img width="547" alt="Screen Shot 2021-12-11 at 21 24 25" src="https://user-images.githubusercontent.com/72669398/145680041-6372d1b4-3899-44e4-af5a-562165881a6d.png">

Kemudian pada node JIPANGU masukkan `INTERFACES="eth0"` pada file `/etc/default/isc-dhcp-server` untuk mendapatkan layanan dari DHCP Server.

<img width="636" alt="Screen Shot 2021-12-11 at 21 24 37" src="https://user-images.githubusercontent.com/72669398/145680045-1d2de058-25bc-4dbc-b7ab-ad92395b562f.png">

Kemudian ubah script `/etc/network/interfaces` pada node-node Client menjadi sebagai berikut
 
```
auto eth0
iface eth0 inet dhcp
```
 
Lalu, restart node Client.


# Soal no 1
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Foosha menggunakan iptables, tetapi Luffy tidak ingin menggunakan MASQUERADE.

```
iptables -t nat -A POSTROUTING -s 10.22.0.0/21 -o eth0 -j SNAT --to-source 192.168.122.2
```
Syntax di atas diatur pada router FOOSHA, karena FOOSHA adalah satu-satunya router yang terhubung ke cloud melalui eth0. maka, `-t nat` NAT Table pada `-A POSTROUTING` POSTROUTING chain digunakan untuk `-j SNAT` mengubah yang awalnya berupa private IPv4 address yang memiliki 16-bit blok dari private IP addresses yaitu `-s 10.22.0.0/21` menjadi `--to-source 192.168.122.2` .

# Soal no 2
Kalian diminta untuk mendrop semua akses HTTP dari luar Topologi kalian pada server yang merupakan DHCP Server dan DNS Server demi menjaga keamanan.

```
iptables -A FORWARD -d 10.22.0.8/29 -i eth0 -p tcp --dport 80 -j DROP
```

Syntax di atas merupakan DNS Server DORIKI pada DHCP Server JIPANGU. Dengan menggunakan `-A FORWARD` untuk menyaring paket dengan `-p tcp` dari luar topologi menuju DHCP Server JIPANGU dan DNS Server DORIKI pada subnet yang sama yaitu A1 `-d 10.22.0.8/29` dimana `--dport 80` akses SSH yang memiliki port 80 (HTTP) yang masuk ke DHCP Server JIPANGU dan DNS Server DORIKI melalui `-i eth0` paket masuk dari eth0 dari JIPANGU dan DORIKI agar `-j DROP` di DROP.


# Soal no 3
Karena kelompok kalian maksimal terdiri dari 3 orang. Luffy meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.

```
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```
di atas merupakan syntax DHCP Server JIPANGU dan DNS Server DORIKI. Dengan menggunakan `-A INPUT`  untuk menyaring paket dengan `-p icmp` yang masuk agar dibatasi `-m connlimit --connlimit-above 3` hanya maksimal 3 koneksi `--connlimit-mask 0`, sehingga selebihnya akan `-j DROP` di DROP.


Kemudian kalian diminta untuk membatasi akses ke Doriki yang berasal dari subnet Blueno, Cipher, Elena dan Fukuro dengan beraturan sebagai berikut

# Soal no 4
Akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.

Syntax di bawah diatur pada DNS Server DORIKI,

Pada syntax pertama yang merupakan batas akses doriki dari Blueno kami menggunakan : 
```
iptables -A INPUT -s 10.22.0.128/25 -d 10.22.0.8/29 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
```

Kemudian selanjutnya pada syntax kedua pada batas akses Doriki dari Blueno kami menggunakan : 
```
iptables -A INPUT -s 10.22.0.128/25 -j REJECT
```

Kemudian selanjutnya pada syntax ketiga pada batas akses Doriki dari Cipher kami menggunakan : 
```
iptables -A INPUT -s 10.22.4.0/22 -d 10.22.0.8/29 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
```

Kemudian selanjutnya pada syntax keempat pada batas akses Doriki dari Cipher kami menggunakan : 
```
iptables -A INPUT -s 10.22.4.0/22 -j REJECT
```

# Soal no 5
Akses dari subnet Elena dan Fukuro hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya.
Selain itu di reject

Syntax di bawah diatur pada DNS Server DORIKI:

Pada syntax pertama yang merupakan batas akses doriki dari Elena kami menggunakan : 
```
iptables -A INPUT -s 10.22.2.0/23 -m time --timestart 15:01 --timestop 06:59 -j ACCEPT
```

Kemudian selanjutnya pada syntax kedua pada batas akses Doriki dari Elena kami menggunakan : 
```
iptables -A INPUT -s 10.22.2.0/23 -j REJECT
```

Kemudian selanjutnya pada syntax ketiga pada batas akses Doriki dari Fukurou kami menggunakan : 
```
iptables -A INPUT -s 10.22.1.0/24 -m time --timestart 15:01 --timestop 06:59 -j ACCEPT
```

Kemudian selanjutnya pada syntax keempat pada batas akses Doriki dari Fukurou kami menggunakan : 
```
iptables -A INPUT -s 10.22.1.0/24 -j REJECT
```

# Soal no 6
Karena kita memiliki 2 Web Server, Luffy ingin Guanhao disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate

Luffy berterima kasih pada kalian karena telah membantunya. Luffy juga mengingatkan agar semua aturan iptables harus disimpan pada sistem atau paling tidak kalian menyediakan script sebagai backup.

