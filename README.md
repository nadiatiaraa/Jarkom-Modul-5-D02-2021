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

(C) Kalian juga diharuskan melakukan Routing agar setiap perangkat pada jaringan tersebut dapat terhubung.

(D) Tugas berikutnya adalah memberikan ip pada subnet Blueno, Cipher, Fukurou, dan Elena secara dinamis menggunakan bantuan DHCP server. Kemudian kalian ingat bahwa kalian harus setting DHCP Relay pada router yang menghubungkannya.

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

