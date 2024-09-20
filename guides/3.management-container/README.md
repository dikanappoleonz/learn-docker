<!-- Heading -->
<h1 align="center">Management Container</h1>

## 3.1 Container Resource Limit
secara default saat container berjalan maka dia akan menggunakan semua CPU dan Memory yang tersedia pada sistem host, tentunya hal ini akan memengaruhi container lain jika resource yang di gunakan terlalu banyak. untuk itu Container Resource Limit ini perlu di atur saat pembuatan container.
 ```js
dika@docker-dika-node01:~$ sudo docker run -d --name db-mysql --memory 100m --cpus 0.5 -e MYSQL_ROOT_PASSWORD=kits123 -e MYSQL_DATABASE="Komunitas IT" -p 3306:3306 mysql:latest
```

## 3.2 Container Link 
setelah container mysql sudah terinstall disini buat container phpmyadmin dan hubungkan ke container mysql. gunakan opsi `--link` untuk menghubungkan ke container database mysql
 ```js
dika@docker-dika-node01:~$ sudo docker run -d --name phpmyadmin --link db-mysql:db -p 8000:80 phpmyadmin:latest
```

>Open browser and access ip_address:port
<p align="center">
  <img src="images/images-1.png" witdh="50%" height="50%" alt="nginx"/>
</p>
<p align="center">
  <img src="images/images-2.png" witdh="50%" height="50%" alt="nginx"/>
</p>

## 3.3 Docker Volume
docker volume seperti sebuah storage yang digunakan docker untuk menyimpan data, by default volume auto generate saat membuat ataupun menjalankan container

> 3.3.1 Mount Volume with busybox

buat directory baru dan isi dengan sebuah data sederhana
```js
dika@docker-dika-node01:~$ for i in {1..3};do mkdir data-kits/komunitas-it-$i;done && for i in {1..3};do echo "Komunitas IT #$i" > data-kits/komunitas-it-$i/data.txt;done
dika@docker-dika-node01:~$ tree
.
├── data-kits
│   ├── komunitas-it-1
│   │   └── data.txt
│   ├── komunitas-it-2
│   │   └── data.txt
│   └── komunitas-it-3
│       └── data.txt
└── snap
    └── docker
        ├── 2932
        ├── common
        └── current -> 2932

10 directories, 3 files
dika@docker-dika-node01:~$
```

create volume forkits
 ```js
dika@docker-dika-node01:~$ sudo docker volume create forkits
[sudo] password for dika: 
forkits
dika@docker-dika-node01:~$ sudo docker volume ls
DRIVER    VOLUME NAME
local     79cc97991f06eaa6d2450367be7bf3637fa18d41426d7c1316e164465c11c067
local     forkits
```

lakukan pull image `busybox` (perangkat lunak yang menggabungkan berbagai perintah dan utilitas dasar Unix/Linux ke dalam satu file eksekusi tunggal yang ringan)
```js
dika@docker-dika-node01:~$ sudo docker pull busybox:latest
latest: Pulling from library/busybox
2fce1e0cdfc5: Pull complete
Digest: sha256:c230832bd3b0be59a6c47ed64294f9ce71e91b327957920b6929a0caa8353140
Status: Downloaded newer image for busybox:latest
docker.io/library/busybox:latest
dika@docker-dika-node01:~$ sudo docker images
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
phpmyadmin   latest    2c40d71042e9   2 weeks ago     562MB
nginx        latest    39286ab8a5e1   5 weeks ago     188MB
mysql        latest    680b8c60dce6   8 weeks ago     586MB
busybox      latest    6fd955f66c23   16 months ago   4.26MB
```
deploy volume yang telah terbuat kedalam container busybox
```js
dika@docker-dika-node01:~$ sudo docker create -v /forkits --name KITS busybox
623771f81f7ab1d349504de044d66a65e9be7b351bc8b1c49ea3209499627f0a
dika@docker-dika-node01:~$ sudo docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED         STATUS    PORTS     NAMES
623771f81f7a   busybox   "sh"      9 seconds ago   Created             KITS
```
copy data yang dibuat diawal kedalam volume busybox
```js
dika@docker-dika-node01:~$ cd data-kits/
dika@docker-dika-node01:~/data-kits$ sudo docker cp . KITS:/forkits
Successfully copied 6.14kB to KITS:/forkits
```
test volume dengan membuat 1 container ubuntu dan lakukan mount ke volume busybox. jika sudah masuk kedalam container jalankan periuntah `df -h` dan `lsblk -l` untuk melihat mounted volume
```js
dika@docker-dika-node01:~$ sudo docker run -it --volumes-from KITS ubuntu /bin/bash
root@f0412ed18b88:/# df -h
Filesystem      Size  Used Avail Use% Mounted on
overlay          20G  6.0G   13G  33% /
tmpfs            64M     0   64M   0% /dev
shm              64M     0   64M   0% /dev/shm
/dev/sda2        20G  6.0G   13G  33% /forkits
tmpfs           457M     0  457M   0% /proc/asound
tmpfs           457M     0  457M   0% /proc/acpi
tmpfs           457M     0  457M   0% /proc/scsi
tmpfs           457M     0  457M   0% /sys/firmware
root@f0412ed18b88:/# lsblk -l
NAME  MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
loop0   7:0    0 74.3M  1 loop
loop1   7:1    0  132M  1 loop
loop2   7:2    0 38.8M  1 loop
sda     8:0    0   20G  0 disk
sda1    8:1    0    1M  0 part
sda2    8:2    0   20G  0 part /etc/hosts
                               /etc/hostname
                               /etc/resolv.conf
                               /forkits
sr0    11:0    1  2.6G  0 rom
```
terakhir test `ls -R` dan `cat` semua isi file data yang dimasukkan kedalam volume 
```js
root@f0412ed18b88:/# ls -R forkits/
forkits/:
komunitas-it-1  komunitas-it-2  komunitas-it-3

forkits/komunitas-it-1:
data.txt

forkits/komunitas-it-2:
data.txt

forkits/komunitas-it-3:
data.txt

root@f0412ed18b88:/# for x in {1..3}; do cat forkits/komunitas-it-$x/data.txt; done
Komunitas IT #1
Komunitas IT #2
Komunitas IT #3
```