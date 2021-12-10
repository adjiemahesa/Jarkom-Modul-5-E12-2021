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

## Soal 2
Kalian diminta untuk mendrop semua akses HTTP dari luar Topologi kalian pada server yang merupakan DHCP Server dan DNS Server demi menjaga keamanan.

**Pembahasan**

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
* `-p icmp` : Mendefinisikan opsi port. Disini digunakan port `icmp`
* `-m connlimit` : Menyesuaikan dengan rule connlimit
* `--connlimit-above 3`: Mendefinisikan jika connlimit sudah diatas 3
* `--connlimit-mask 0` : Mendefinisikan mask yang masuk. 0 berarti akan menerima semua mask
* `-j DROP` : Melakukan DROP koneksi
