# Jarkom-Modul-4-C15-2021

## Perhitungan VLSM

1. Melakukan pembagian subnet pada topologi. Total hingga A15.

2. Jumlah IP dapat ditentukan dengan menggabungkan jumlah semua host oleh tiap subnet & sertakan labeling netmask berdasarkan jumlah IP. 

- Tabel subnet

 ![image](https://user-images.githubusercontent.com/64303057/143678750-1114d5e4-cd74-4322-b029-f82e41cf8db6.png)

- Tabel penentuan IP & netmask

![image](https://user-images.githubusercontent.com/64303057/143678704-4817d57c-da64-473d-9e0c-7eda199358a9.png)

Dari tabel di atas, didapatkan jumlah IP sebanyak **5845**, sehingga dapat menggunakan netmask /19.

3. Subnet besar memiliki NID **192.191.0.0** (sesuai prefix kelompok) dengan netmask /19. Perhitungan pembagian IP dilakukan dengan *tree* subnet.

![image](https://user-images.githubusercontent.com/64303057/143679084-cabcdd81-5180-4972-8f01-e970817d10fa.png)

4. Dari subnet-*tree* di atas, maka didapatlah pembagian IP sebagai berikut :

## Topologi GNS 3

Berikut hasil topologi untuk GNS 3: 

![image](https://user-images.githubusercontent.com/64303057/143679521-3782a43a-23d7-487a-b276-04129e973efc.png)



