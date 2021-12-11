# Jarkom-Modul-5-E12-2021
Anggota Kelompok E12:
 * Rahadian Adjie Mahesa  - 05111940000221
 * Afifan Syafaqi Yahya   - 05111940000234
 * Ahmad Luthfi Hanif     - 05111940000179

### Prefix IP
Prefix IP kami adalah `10.35`

<hr/>

## Pembuatan Topologi dan Persiapan
### Topologi GNS3
![image](https://user-images.githubusercontent.com/55140514/145554246-68775871-b719-4356-a662-8194504364bd.png)

Keterangan :
* Doriki adalah DNS Server
* Jipangu adalah DHCP Server
* Maingate dan Jorge adalah Web Server
* Jumlah Host pada Blueno adalah 100 host
* Jumlah Host pada Cipher adalah 700 host
* Jumlah Host pada Elena adalah 300 host
* Jumlah Host pada Fukurou adalah 200 host

### Subnetting dengan VLSM
Pertama-tama kita membuat *subnetting* terlebih dahulu. Disini kami menggunakan cara `VLSM`. Langkah pertama yang kita lakukan dalam pembuatan *subnetting* nya adalah menentukkan subnet terlebih dahulu seperti berikut

![image](https://user-images.githubusercontent.com/55140514/145554044-9eb24a13-c3f6-4b3f-a192-d7e709b96c79.png)

Setelah menentukkan subnet-subnetnya kita melakukan penjumlahan IP beserta dengan subnetnya:
| Subnet | Jumlah | Length |
|:------:|:------:|:------:|
| A1     | 3      | /29    |
| A2     | 101    | /25    |
| A3     | 701    | /22    |
| A4     | 2      | /30    |
| A5     | 2      | /30    |
| A6     | 301    | /23    |
| A7     | 201    | /24    |
| A8     | 3      | /29    |
|        | 1314   | /21    |

Berdasarkan perhitungan kami di table tersebut, jumlah ip yang didapatkan adalah `1314` dan root menggunakan netmask `/21`. Setelah kita menentukan itu, kita sekarang menyusun pohonnya seperti berikut

![image](https://user-images.githubusercontent.com/55140514/145555378-07eb115a-d2db-4110-b16e-fb341da656ca.png)

Dengan sudah tersusunnya pohon tersebut maka kita lanjut dengan melakukan konfigurasi setiap node. Sebelum itu, kita melakukan `Pembagian IP` lagi untuk IP tiap Node seperti berikut

| Subnet |   Node   |      IP     |   Subnet Mask   | Length |
|:------:|:--------:|:-----------:|:---------------:|:------:|
| A1     | Water7   | 10.35.7.129 | 255.255.255.248 | /29    |
| A1     | Jipangu  | 10.35.7.130 | 255.255.255.248 |        |
| A1     | Doriki   | 10.35.7.131 | 255.255.255.248 |        |
| A2     | Water7   | 10.35.7.1   | 255.255.255.128 | /25    |
| A2     | Blueno   | 10.35.7.2   | 255.255.255.128 |        |
| A3     | Water7   | 10.35.0.1   | 255.255.252.0   | /22    |
| A3     | Cipher   | 10.35.0.2   | 255.255.252.0   |        |
| A4     | Foosha   | 10.35.7.145 | 255.255.255.252 | /30    |
| A4     | Water7   | 10.35.7.146 | 255.255.255.252 |        |
| A5     | Foosha   | 10.35.7.149 | 255.255.255.252 | /30    |
| A5     | Guanhao  | 10.35.7.150 | 255.255.255.252 |        |
| A6     | Guanhao  | 10.35.4.1   | 255.255.254.0   | /23    |
| A6     | Elena    | 10.35.4.2   | 255.255.254.0   |        |
| A7     | Guanhao  | 10.35.6.1   | 255.255.255.0   | /24    |
| A7     | Fukurou  | 10.35.6.2   | 255.255.255.0   |        |
| A8     | Guanhao  | 10.35.7.137 | 255.255.255.248 | /29    |
| A8     | Maingate | 10.35.7.138 | 255.255.255.248 |        |
| A8     | Jorge    | 10.35.7.139 | 255.255.255.248 |        |

Lalu, untuk konfigurasinya seperti berikut
* Foosha
```
auto eth0
iface eth0 inet dhcp
hwaddress ether a6:2a:80:42:69:8a

auto eth1
iface eth1 inet static
	address 10.35.7.145
	netmask 255.255.255.252

auto eth2
iface eth2 inet static
	address 10.35.7.149
	netmask 255.255.255.252
```
* Water7
```
auto eth0
iface eth0 inet static
	address 10.35.7.146
	netmask 255.255.255.252
	gateway 10.35.7.145

auto eth1
iface eth1 inet static
	address 10.35.7.1
	netmask 255.255.255.128

auto eth2
iface eth2 inet static
	address 10.35.7.129
	netmask 255.255.255.248

auto eth3
iface eth3 inet static
	address 10.35.0.1
	netmask 255.255.252.0
```
* Guanhao
```
auto eth0
iface eth0 inet static
	address 10.35.7.150
	netmask 255.255.255.252
	gateway 10.35.7.149

auto eth1
iface eth1 inet static
	address 10.35.4.1
	netmask 255.255.254.0

auto eth2
iface eth2 inet static
	address 10.35.7.137
	netmask 255.255.255.248

auto eth3
iface eth3 inet static
	address 10.35.6.1
	netmask 255.255.255.0
```
* Jipangu
```
auto eth0
iface eth0 inet static
	address 10.35.7.130
	netmask 255.255.255.248
	gateway 10.35.7.129
```
* Doriki
```
auto eth0
iface eth0 inet static
	address 10.35.7.131
	netmask 255.255.255.248
	gateway 10.35.7.129
```
* Maingate
```
auto eth0
iface eth0 inet static
	address 10.35.7.138
	netmask 255.255.255.248
	gateway 10.35.7.137
```
* Jorge
```
auto eth0
iface eth0 inet static
	address 10.35.7.139
	netmask 255.255.255.248
	gateway 10.35.7.137
```
* Blueno
```
#auto eth0
#iface eth0 inet static
#	address 10.35.7.2
#	netmask 255.255.255.128
#	gateway 10.35.7.0

auto eth0
iface eth0 inet dhcp
```
* Cipher
```
#auto eth0
#iface eth0 inet static
#	address 10.35.0.2
#	netmask 255.255.252.0
#	gateway 10.35.0.0

auto eth0
iface eth0 inet dhcp
```
* Elena
```
#auto eth0
#iface eth0 inet static
#	address 10.35.4.2
#	netmask 255.255.254.0
#	gateway 10.35.4.0

auto eth0
iface eth0 inet dhcp
```
* Fukurou
```
#auto eth0
#iface eth0 inet static
#	address 10.35.6.2
#	netmask 255.255.255.0
#	gateway 10.35.6.0

auto eth0
iface eth0 inet dhcp
```
<hr/>

### Note
* Pada Blueno, Cipher, Elena, dan Fukurou IP yang sesuai dengan pembagian IP di *comment* kan sehingga memakai yang `inet dhcp` sesuai dengan ketentuan soal nomor `D`

<hr/>

### Routing
Kemudian dilakukan Routing pada `Foosha` sebagai berikut
```
# Arah Water7
route add -net 10.35.7.128 netmask 255.255.255.248 gw 10.35.7.146
route add -net 10.35.7.0 netmask 255.255.255.128 gw 10.35.7.146
route add -net 10.35.0.0 netmask 255.255.252.0 gw 10.35.7.146

#Arah Guanhao
route add -net 10.35.4.0 netmask 255.255.254.0 gw 10.35.7.150
route add -net 10.35.6.0 netmask 255.255.255.0 gw 10.35.7.150
route add -net 10.35.7.136 netmask 255.255.255.248 gw 10.35.7.150
```

### Instalasi DHCP Server dan Relay
Pada node `Jipanggu` dilakukan instalasi DHCP Server dengan `apt-get install isc-dhcp-server` dan pada router `Water7` dan `Guanhao` dilakukan instalasi DHCP Relay dengan apt-get install `isc-dhcp-relay`

Pada `Jipanggu` di `/etc/default/isc-dhcp-server` diisi sama seperti berikut
![image](https://user-images.githubusercontent.com/55140514/145561819-e0be8dbb-0355-47c5-88ee-838ba7b9db84.png)

Lalu di konfigurasi dhcp yang `/etc/dhcp/dhcpd.conf` ditambahkan subnet-subnet dari 4 node yang menggunakan dhcp tersebut seperti berikut

![image](https://user-images.githubusercontent.com/55140514/145562336-26690902-6d30-467a-8b6b-d146ba9087d2.png)

![image](https://user-images.githubusercontent.com/55140514/145562421-869a40d5-5b5c-41b2-aaf0-2b03fa7ef384.png)

![image](https://user-images.githubusercontent.com/55140514/145562577-6e01314c-0df3-4d32-9c56-39c7e5ec0196.png)

Lalu, untuk DHCP Relay pada `Water7` dan `Guanhao` diisi seperti berikut
* Water7

![image](https://user-images.githubusercontent.com/55140514/145562851-01ae8e3e-31f8-48cb-9c4f-1b0523205ece.png)

* Guanhao

![image](https://user-images.githubusercontent.com/55140514/145562931-926ffd94-cebd-4c58-b88e-5a8fdbf2109c.png)

<hr/>

## Soal 1
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Foosha menggunakan iptables, tetapi Luffy tidak ingin menggunakan MASQUERADE.

**Pembahasan**
1. Pada soal ini akan dilakukan iptables agar semua node dapat mengakses internet.
2. Pada soal ini juga diminta agar konfigurasi iptables tidak menggunakan `MASQUERADE`. Maka yang digunakan adalah `SNAT` dari chain `POSTROUTING` yang ada di dalam *table* `NAT`
3. Konfigurasi perintah iptables sebagai berikut 
```
iptables -t nat -A POSTROUTING -s 10.35.0.0/21 -o eth0 -j SNAT --to-source 192.168.122.89
```
### Testing
Kita mencoba ping `google.com` dari node lain. Disini kita menggunakan node `ELENA`

![image](https://user-images.githubusercontent.com/55140514/145565463-514f5fd6-2e47-41d2-be37-a82522acdad6.png)

### Note
* `-t nat` : Menggunakan Table NAT
* `-A POSTROUTING` : Meng-*append* chain POSTROUTING yang mengubah asal paket setelah routing
* `-s 10.35.0.0/21` : Menunjukkan source dari paket yaitu subnet 10.35.0.0/21
* `-o eth0` : Mendefinisikan interface yang dilihta untuk paket keluarnya. Disini digunakan eth0 FOOSHA
* `-j SNAT` : Menggunakan SNAT untuk mengubah dari mana koneksi terjadi
* `--to-source 192.168.122.89` : Asal koneksi terjadi. Disini kami menggunakan `Fixed Address` 192.168.122.89

<hr/>

## Soal 2
Kalian diminta untuk mendrop semua akses HTTP dari luar Topologi kalian pada server yang merupakan DHCP Server dan DNS Server demi menjaga keamanan.

**Pembahasan**
1. Pada soal ini kita diminta untuk melakukan pembatasan akses dari luar topologi sehingga jika mengakses ke DNS dan DHCP Server akan di-*drop*. Disini kami menggunakan chain `FORWARD` yang melakukan filter pada paket yang akan diteruskan.
2. Selanjutnya kita akan menggunakan `-d` dimana akan menjadi destinasi untuk filternya. Disini kami menggunakan subnet yang ada `DNS dan DHCP Server`.
3. Pada soal ini untuk percobaan kami menambahkan `PC` yang menyambung dengan `Foosha` yang berperan sebagai PC dari luar topologi dikarenakan menggunakan IP dan Subnet Mask yang berbeda. Oleh karena itu rule akan diarahkan ke PC tersebut (Disini berada di `eth3` Foosha)
4. Pada Foosha akan ditambakan rule sebagai berikut:  
```
iptables -A FORWARD -d 10.35.7.128/29 -i eth3 -p tcp --dport 80 -j DROP
```  
Hal ini dimaksudkan agar paket-paket yang menuju(dalam artian dari luar) ke port 80 (HTTP) akan di-drop.

### Testing
* Dibuatkan PC dari luat bernama `testing`.

![image](https://user-images.githubusercontent.com/55140514/145678947-4a1271e8-15c9-4fe4-a7bc-cd8402ce14e5.png)


* Melakukan `nmap` dengan `-p` ke port `80` ke `IP DNS atau DHCP Server` menjadi `nmap -p 80 10.35.7.130` dan `nmap -p 80 10.35.7.131`. Hasil sebagai berikut

![image](https://user-images.githubusercontent.com/55140514/145678892-0e97a0c7-160a-4207-aec8-14c183f93cee.png)

* Bisa dilihat bahwa jika kita melakukan `nmap` akan terjadi tulisan `filtered`.

### Note
* `-A FORWARD` : Meng-*append* chain POSTROUTING yang melakukan filter pada paket yang akan diteruskan
* `-d 10.35.7.128/29` : Menunjukkan destinasi yaitu subnet 10.35.7.128/29
* `-i eth3` : Mendefinisikan interface yang dilihat untuk paket yang masuk. Disini digunakan eth3 FOOSHA'
* `-p tcp` : Mendefinisikan opsi protocol port. Disini digunakan port `tcp`
* `--dport 80` : Port yang dilihat adalah port 80
* `-j DROP` : Melakukan DROP koneksi

<hr/>

## Soal 3
Karena kelompok kalian maksimal terdiri dari 3 orang. Luffy meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.

**Pembahasan**
1. Untuk melakukan pembatasan koneksi kita menggunakan aturan `connlimit`. Pada aturan tersebut kita mengatur berapa banyak koneksi yang bisa dijalankan dengan `--connlimit-above` dan yang menggunakan mask apa dengan `--connlimit-mask`
2. Karena yang diminta membatasi baik DHCP dan DNS maka kita akan memasukkan perintah di node `Jipanggu` sebagai DHCP Server dan `Doriki` sebagai DNS Server
3. Konfigurasi perintah iptables sebagai berikut 
```
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```

### Testing
Dilakukan ping ke DNS Server dan DHCP Server dari 4 Node. Disini kita mencoba ping ke IP DHCP Server yaitu IP `Jipanggu`. Pertama pada node `Elena` seperti berikut

![image](https://user-images.githubusercontent.com/55140514/145567662-3ac80558-3531-41aa-a9d7-e85a3e26dae8.png)

Lalu kedua pada node `Blueno` seperti berikut

![image](https://user-images.githubusercontent.com/55140514/145567689-39477cd9-f6e0-4f4d-a46d-b3feb3bf2252.png)

Ketiga pada node `Cipher`

![image](https://user-images.githubusercontent.com/55140514/145567735-64dfd849-cad9-4eb8-8070-1287347059c8.png)

Dan Terakhir pada node `Fukurou`

![image](https://user-images.githubusercontent.com/55140514/145567809-9ae1ba30-a579-451f-9dd7-987cb157f262.png)

Bisa dilihat bahwa pada node `Fukurou` tidak bisa melakukan ping ke IP DHCP Server karena sudah ada 3 koneksi yang melakukan ping sebelumnya

### Note
* `-A INPUT` : Meng-*append* chain INPUT yang memfilter paket yang masuk
* `-p icmp` : Mendefinisikan opsi protocol port. Disini digunakan port `icmp`
* `-m connlimit` : Menyesuaikan dengan rule connlimit
* `--connlimit-above 3`: Mendefinisikan jika connlimit sudah diatas 3
* `--connlimit-mask 0` : Mendefinisikan mask yang masuk. 0 berarti akan menerima semua mask
* `-j DROP` : Melakukan DROP koneksi

<hr/>

## Soal 4 dan 5
Kemudian kalian diminta untuk membatasi akses ke Doriki yang berasal dari subnet Blueno, Cipher, Elena dan Fukuro dengan beraturan sebagai berikut

4. Akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.
5. Akses dari subnet Elena dan Fukuro hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya.

**Pembahasan**
1. Untuk membatas waktu akses kita menggunakan rule `time` yang bisa mendefinisikan range waktu dengan `--timestart` untuk mulai waktu dan `--timestop` untuk akhir waktu.
2. Pada **Soal 4** kita juga menggunakan `--weekdays` untuk mendefinisikan harinya. Sedangkan untuk **Soal 5** kita tidak perlu melakukan itu karena yang diminta adalah setiap hari
3. Konfigurasi perintah iptables sebagai berikut 
* **Soal4**
```
#Blueno
iptables -A INPUT -s 10.35.7.0/25 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 10.35.7.0/25 -j REJECT

#Cipher
iptables -A INPUT -s 10.35.0.0/22 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 10.35.0.0/22 -j REJECT
```
* **Soal 5**
```
#Elena
iptables -A INPUT -s 10.35.4.0/23 -m time --timestart 00:00 --timestop 06:59 -j ACCEPT
iptables -A INPUT -s 10.35.4.0/23 -m time --timestart 15:01 --timestop 23:59 -j ACCEPT
iptables -A INPUT -s 10.35.4.0/23 -j REJECT

#Fukurou
iptables -A INPUT -s 10.35.6.0/24 -m time --timestart 00:00 --timestop 06:59 -j ACCEPT
iptables -A INPUT -s 10.35.6.0/24 -m time --timestart 15:01 --timestop 23:59 -j ACCEPT
iptables -A INPUT -s 10.35.6.0/23 -j REJECT
```
4. Untuk `Soal 4` source nya akan dirubah menjadi subnet `Blueno` dan `Cipher` dan dimasukkan hari `Mon,Tue,Wed,Thu` atau Senin sampai Kamis pada `--weekdays`. Lalu, dilakukan `REJECT` untuk secara umum agar hanya bisa diakses menggunakan peraturan yang telah kita buat.
5. Untuk `Soal 5` sama seperti Soal 4 tetapi kita gunakan source untuk subnet `Elena` dan `Fukurou`, tidak digunakan `--weekdays` karena yang ditentukan adalah setiap hari, serta dibuat untuk 2 range waktu yaitu `00:00 - 06:59` dan `15:01 - 23:59` karena waktunya tidak bisa melebihi 23:59 serta agar sesuai dengan ketentuan yang diberikkan soal. 

### Testing
Dilakukan ping ke `Doriki` pada dua waktu tertentu. Untuk **Soal 4** waktu yang digunakan adalah `Kamis, 09 Desember 2021 10:00:00` dan `Kamis, 09 Desember 2021 22:00:00`. Node yang digunakan adalah `Blueno` dan Hasil seperti berikut
* Kamis, 09 Desember 2021 10:00:00

![image](https://user-images.githubusercontent.com/55140514/145572518-d5d9f4c7-31d2-49f0-b65b-5f6160ef9cdb.png)

* Kamis, 09 Desember 2021 22:00:00 

![image](https://user-images.githubusercontent.com/55140514/145572572-15386a29-954e-4b63-8fdd-454c24c70679.png)


Lalu, untuk **Soal 5** digunakan waktu yang sama tetapi pada node `Elena`. Perlu diperhatikan bahwa di soal ini waktu akses nya adalah pukul `15:01 hingga 06:59`

* Kamis, 09 Desember 2021 22:00:00

![image](https://user-images.githubusercontent.com/55140514/145573037-7c7857c3-9b67-4c9d-bea8-e479ec355b3e.png)

* Kamis, 09 Desember 2021 10:00:00 

![image](https://user-images.githubusercontent.com/55140514/145573064-0db3d0fe-7fb2-419d-9135-8e601e7f7a26.png)

Bisa dilihat bahwa saat berada diluar waktu yang ditentukan node tidak bisa melakukan ping

### Note
* `-A INPUT` : Meng-*append* chain INPUT yang memfilter paket yang masuk
* `-s [IP Subnet]` : Menunjukkan source dari paket yaitu subnet
* `-m time` : Menyesuaikan dengan rule time
* `--timestart` : Waktu mulai
* `--timestop` : Waktu berhenti
* `--weekdays` : Mendefinisikan hari-hari kerja
* `-j ACCEPT / REJECT`: Menerima atau menolak koneksi

## Soal 6
Karena kita memiliki 2 Web Server, Luffy ingin Guanhao disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate

**Pembahasan**
1. Pada soal ini kita diminta untuk melakukan *Load Balancing*. Maka pada iptables kita akan menggunakan 
2. 

