# Jarkom-Modul-4-C15-2021

## Anggota Kelompok

1. Gerald Elroy Van Ryan - 05111940000187
2. Riza Dwi
3. Gian Ega wijaya 


## Perhitungan VLSM

1. Melakukan pembagian subnet pada topologi. Total hingga A15.

2. Jumlah IP dapat ditentukan dengan menggabungkan jumlah semua host oleh tiap subnet & sertakan labeling netmask berdasarkan jumlah IP. 

- Tabel subnet

 ![image](https://user-images.githubusercontent.com/64303057/143678750-1114d5e4-cd74-4322-b029-f82e41cf8db6.png)

- Tabel penentuan IP & netmask

![Picture4](https://user-images.githubusercontent.com/90212308/143684421-d2da8775-aaa0-421c-96cc-7a55141a96ab.png)

Dari tabel di atas, didapatkan jumlah IP sebanyak **5845**, sehingga dapat menggunakan netmask /19.

3. Subnet besar memiliki NID **192.191.0.0** (sesuai prefix kelompok) dengan netmask /19. Perhitungan pembagian IP dilakukan dengan *tree* subnet.

![image](https://user-images.githubusercontent.com/64303057/143679084-cabcdd81-5180-4972-8f01-e970817d10fa.png)

4. Dari subnet-*tree* di atas, maka didapatlah pembagian IP sebagai berikut :

![Picture3](https://user-images.githubusercontent.com/90212308/143684559-28260db9-d717-4056-bf7e-077ce7f178d3.png)

## Topologi GNS 3

Berikut hasil topologi untuk GNS 3: 

![Picture2](https://user-images.githubusercontent.com/90212308/143684235-afd3f1cc-8bfa-4e2e-bfa6-d76436736c4b.png)


## Subnetting GNS 3

Dengan menggunakan topologi di atas, kita akan coba subnetting untuk tiap `router, client, dan server`. Address & netmask yang dipakai sesuai dengan tabel yang sudah dibuat tadi.

Ini adalah contoh subnneting pada configure FOOSHA :

```
auto eth0
iface eth0 inet static
	address 192.191.8.1
	netmask 255.255.252.0
 
auto eth1
iface eth1 inet static
 address 192.191.27.149
	netmask 255.255.255.252
 
auto eth2
iface eth2 inet static
	address 192.191.27.165
	netmask 255.255.255.252
 
auto eth3
iface eth3 inet static
	address 192.191.27.153
	netmask 255.255.255.252

auto eth4
iface eth4 inet dhcp
```

**Contoh penjelasan**:

Untuk subnet A4 dengan NID `192.191.16.0` dan netmask `255.255.252.0`. Pada Foosha, address untuk `eth0` akan ditambahkan 1 dari NID sehingga menjadi `192.191.16.1`, sedangkan untuk kliennya, `Blueno`, address-nya akan ditambahkan 1 lagi dari IP Foosha. Address-nya menjadi `192.191.16.2`. Karena `Blueno` ini adalah *client*, maka perlu ditambahkan konfigurasi *gateway*-nya, menjadi `192.191.16.1` (Gateway-nya adalah address dari `Foosha`).

## Routing GNS 3

Untuk routing sendiri ada 2 macam, yaitu sebagai berikut :

### Router dengan Router

![image](https://user-images.githubusercontent.com/64303057/143682535-6b8f0353-0445-4e6c-9f83-0d974aade12e.png)

Routing untuk antar router perlu dilakukan  pada setiap router yang bukan main router yang terconnect ke internet. Inilah yang disebut Default Routing. 

Contohnya adalah default routing dari `PUCCI` ke `WATER7`. Tambahkan kode ini pada `script.sh` di `PUCCI`:

```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.191.27.145
```

NID & Netmask-nya adalah `0.0.0.0` karena Default Routing, lalu *gateway*-nya adalah IP dari `eth1`-nya `Water7`.

### Router dengan Klien

![image](https://user-images.githubusercontent.com/64303057/143683036-eadaa442-d76e-44db-bfa3-0f7eef5f2d1b.png)

Pada router ke klien, routing dilakukan dari `FOOSHA` ke NID & Netmask dari klien yang subnetting-nya telah dilakukan. Pada contoh kali ini, dilakukanlah routing dari `FOOSHA` ke `CALMBELT`. 

Tambahkan ini pada `script.sh` di `FOOSHA`:
```
route add -net 192.191.0.0 netmask 255.255.248.0 gw 192.191.27.150
```

NID & Netmask yang dipakai berasal dari subnet A1 (sesuai pada tabel). Lalu, gateway-nya adalah IP dari `eth2`-nya `WATER7`.

Tidak hanya `Calmbelt`, FOOSHA juga perlu melakukan routing ke klien-klien lainnya **selain BLUENO** karena mereka sudah satu subnet.

### Routing pada Script.sh

Ini adalah contoh routing yang kodenya ditambahkan pada file `script.sh`.

1. FOOSHA 

```
#FOOSHA KIRI
##mengarah ke cipher
route add -net 192.191.16.0 netmask 255.255.252.0 gw 192.191.27.150
##mengarah ke jipangu
route add -net 192.191.27.0 netmask 255.255.255.128 gw 192.191.27.150
##mengarah ke calmbelt dan courtyard
route add -net 192.191.0.0 netmask 255.255.248.0 gw 192.191.27.150

#FOOSHA BAWAH
##mengarah ke maingate
route add -net 192.191.24.0 netmask 255.255.254.0 gw 192.191.27.154
##mengarah ke jabra
route add -net 192.191.20.0 netmask 255.255.252.0 gw 192.191.27.154
##mengarah ke alabasta
route add -net 192.191.27.128 netmask 255.255.255.240 gw 192.191.27.154
##mengarah ke enieslobby
route add -net 192.191.26.0 netmask 255.255.255.0 gw 192.191.27.154
##mengarah ke elena
route add -net 192.191.12.0 netmask 255.255.252.0 gw 192.191.27.154
##mengarah ke FUKUROU
route add -net 192.191.27.160 netmask 255.255.255.252 gw 192.191.27.154

#FOOSHA KE ROUTER
##mengarah ke PUCCI
route add -net 192.191.27.144 netmask 255.255.255.252 gw 192.191.27.150
## mengarah ke OIMO
route add -net 192.191.27.156 netmask 255.255.255.252 gw 192.191.27.154
```

2. GUANHAO

```
route add -net 192.191.27.128 netmask 255.255.255.240 gw 192.191.24.3
route add -net 192.191.26.0 netmask 255.255.255.0 gw 192.191.27.158
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.191.27.153
route add -net 192.191.27.160 netmask 255.255.255.252 gw 192.191.27.158
route add -net 192.191.12.0 netmask 255.255.252.0 gw 192.191.27.158
```

Tambahkan juga kode di bawah ini agar dapat terhubung dengan internet (tambahkan di FOOSHA):

```
iptables -t nat -A POSTROUTING -o eth4 -j MASQUERADE -s 192.191.0.0/19
```

Serta tambahkan kode ini pada tiap node :

```
echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Hasil ping 

Berikut adalah contoh ping pada GNS :

1. Ping `FOOSHA` dari `Jipangu` 

![image](https://user-images.githubusercontent.com/64303057/143683384-0791927d-5046-43bc-adb4-547992dfe900.png)

2. Ping `WATER7` ke `FOOSHA`

![image](https://user-images.githubusercontent.com/64303057/143683424-9782b56d-0c59-474a-a4ed-9c791dfec3ef.png)

3. Ping `PUCCI` ke `DORIKI`

![image](https://user-images.githubusercontent.com/64303057/143683454-f9b275df-fa4d-4018-9985-6a5bcba36bb0.png)

4. Ping `google.com` dari `PUCCI`

![image](https://user-images.githubusercontent.com/64303057/143683475-6b8e95fa-de1c-4c4e-a9d6-4a8364b64971.png)














