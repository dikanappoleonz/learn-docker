<!-- Heading -->
<h1 align="center">Basic Command</h1>

## Docker Search
Docker Serach digunakan untuk mencari image pada repository. by default menggunakan repository docker hub

> Example: Search Nginx
```js
dika@Docker-1:~$ sudo docker search nginx
[sudo] password for dika: 
NAME                              DESCRIPTION                                     STARS     OFFICIAL
nginx                             Official build of Nginx.                        20206     [OK]
nginx/nginx-prometheus-exporter   NGINX Prometheus Exporter for NGINX and NGIN…   43        
nginx/nginx-ingress               NGINX and  NGINX Plus Ingress Controllers fo…   94        
nginx/unit                        This repository is retired, use the Docker o…   63        
nginx/nginx-quic-qns              NGINX QUIC interop                              1         

kasmweb/nginx                     An Nginx image based off nginx:alpine and in…   8
rancher/nginx                                                                     2
redash/nginx                      Pre-configured nginx to proxy linked contain…   2
youstin/nginx                                                                     0
bitnamicharts/nginx                                                               0
rapidfort/nginx                   RapidFort optimized, hardened image for NGINX   15
paketobuildpacks/nginx                                                            0
wodby/nginx                       Generic nginx                                   2
bitwarden/nginx                   The Bitwarden nginx web server acting as a r…   13
starojanje/nginx                                                                  0
rasa/nginx                        Rasa X nginx server                             2
gluufederation/nginx               A customized NGINX image containing a consu…   1
arm64v8/nginx                     Official build of Nginx.                        50
nsilpanisong/nginx                Repository used to test nginx                   0
mtinny/nginx                      https://github.com/mtinny/k8s-nginx-toolbox     0
```

## Pull Image
pull image digunakan untuk mengambil image dari registry atau repository. by default tag yang digunakan adalah latest jika tidak mendefiniskan tag 
> Example: Pull Nginx
```js
dika@Docker-1:~$ sudo docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
a2318d6c47ec: Pull complete
095d327c79ae: Pull complete
bbfaa25db775: Pull complete
7bb6fb0cfb2b: Pull complete
0723edc10c17: Pull complete
24b3fdc4d1e3: Pull complete
3122471704d5: Pull complete
Digest: sha256:04ba374043ccd2fc5c593885c0eacddebabd5ca375f9323666f28dfd5a9710e3
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
```

## Docker Image
1. Melihat daftar list image
```js
dika@Docker-1:~$ sudo docker image ls
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
nginx        latest    39286ab8a5e1   4 weeks ago   188MB
```

2. Menghapus image
```js
dika@Docker-1:~$ sudo docker image rm nginx
Untagged: nginx:latest
Untagged: nginx@sha256:04ba374043ccd2fc5c593885c0eacddebabd5ca375f9323666f28dfd5a9710e3
Deleted: sha256:39286ab8a5e14aeaf5fdd6e2fac76e0c8d31a0c07224f0ee5e6be502f12e93f3
Deleted: sha256:d71f9b66dd3f9ef3164d7023cc99ce344d209decd5d6cd56166c0f7a2f812c06
Deleted: sha256:634d30adf8a2232256b2871e268c8f0fdb2c348374cd8510920a76db56868e16
Deleted: sha256:f230be3f4e104c7414b7ce9c8d301f37061b4e06afe010878ea55f858d89f7f3
Deleted: sha256:c5210c8480131b7dbc5ad8adc425d68cd7a8848ee2e07de3c69cb88a4b8fd662
Deleted: sha256:d4f588811a337e0b01da46772d02f7f82ee5f9baff6886365ffb912d455f4f53
Deleted: sha256:d73e21a1e27b0184b36f6578c8d0722a44da253bc74cd72e9788763f4a4de08f
Deleted: sha256:8e2ab394fabf557b00041a8f080b10b4e91c7027b7c174f095332c7ebb6501cb

dika@Docker-1:~$ sudo docker image ls
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
```

## Docker Run
docker run digunakan untuk membuat dan menjalankan container. jika image sumber tidak ada maka akan secara otomatis pull dari repository default docker hub.
> Example: create container nginx
```js
dika@Docker-1:~$ sudo docker run -d --name web-kits -p 8080:80 nginx:latest
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
a2318d6c47ec: Pull complete
095d327c79ae: Pull complete
bbfaa25db775: Pull complete
7bb6fb0cfb2b: Pull complete
0723edc10c17: Pull complete
24b3fdc4d1e3: Pull complete
3122471704d5: Pull complete
Digest: sha256:04ba374043ccd2fc5c593885c0eacddebabd5ca375f9323666f28dfd5a9710e3
Status: Downloaded newer image for nginx:latest
03ed4f00840d50a2c15ebf922cab2676d7ee975673d93de557e91075974178ce
```
> Options pada docker run
```js
-d		//untuk menjalankan container dalam mode dedicated atau berjalan di latar belakang.
--name		//digunakan untuk memberikan sebuah nama untuk container
-p 8080:80	//port pertama digunakan untuk mendefinisikan port pada host. dan port kedua adalah port didalam container
```

>Open browser and access ip_address:port
<p align="center">
  <img src="images/images-1.png" witdh="50%" height="50%" alt="nginx"/>
</p>

