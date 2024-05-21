# TUTORIAL SEDERHANA:

## Usual Command 
### Show all available container
> `docker container ls --all`

### Running only container
> `docker container ls `

### Show all images:
> `docker images`

### Remove/Delete Container:
> `docker container rm {name container}`

### Stop Running Container :
> `docker container stop {name_container}`

#### Donwload/Pull images
> `docker pull {images_name}:{tag}`

### Run Continer:
> `docker container start {name_container}`

### Create Container from images:
> `docker container create --name {name} {image:tag}` <br>
* name -> you spesify the name<br>
* tag -> optional, depend on available tag you downloaded.
 

### Expose container port:

> `docker container create --name {conatiner_name} -p {access_port}:{internal_port} {image_name}:{tag}` <br>
Example: <br>
`docker container create --name mongoserver1 -p 8080:27017 go-lang:1.14.1`
`docker container start mongoserver1`

### Remove/delete image
> `docker image rm mongo:{tag}`

Note: <br>
Hanya bisa di delete jika tidak ada container lagi yang menggunakan. 


# MEMBUAT IMAGE DOCKER
## Image docker
Jika sebelumnya kita hanya download dari registry server, sekarang kita buat dari awal

Misalnya sudah ada project yang sudah selesai, maka selanjutnya kita membuat docker file. <br>
Buat file di project direktori dengan nama Dockerfile
Jika misal mau menggunakan image yang sudah ada, maka tinggal copy saja semua file yang kita punya ke base yang kita punya

Dockerfile:
```
FROM {image_name}:{tag}
COPY {file_name} {location in image}
CMD [{command to running the file, separate with ""}]
```

Example: <br>
Misal kita ingin menajalankan sebuah file main.go dengan image golang:1.11.4
```
FROM golang:1.11.4
COPY main.go /app/main.go
CMD ['"go", "run", "/app/main.go"]
```

Build the docker: <br>
`docker build --tag {name}:{tag} {docker_file_location}` <br>
Example: <br>
`docker build --tag app-golang:0.1 .`

`.` -> current location <br>
`{tag}`\` -> custom whatever you want.


## Push images to registry
Misalnya mau menggunakan docker hub, maka tutorial lengkap ada di website masing-masing <br>
Pastikan membuat sebuah images dulu lewat website, selanjutnya baru push ke registry server

Example:<br>
Buat tag baru <br>
`docker tag {image_name}:{tag} {username}}/{image_name}:{tag}` <br>
Example: <br>
`docker tag app-golang:1.0 khannedy/app-golang:1.0`

tag bagian akhir harus ada 

### Login to registry server
`docker login` <br>
---> input username and password

### Push to registry
`docker push {username}/{images_name}:{tag}` <br>
Example: <br>
`docker push khannedy/app-golang:1.0`

# CREATE NETWORK
### Create network
> `docker network create {network_name}`
### connet to network
> `docker network connect {network_name} {container_name}`

### Check connected coninter to a network:
> `docker container inspect {container_name}` <br>
> ---> harus cek satu-satu untuk setiap container

# DOCKER COMPOSE
Jika membutuhkan sebuah file yang bisa melakukan running terhadap berbagai
command sekaligus (misal untuk running banyak container sekaligus dengan berbagai setup env dan networknya) 
bisa pakai docker compose dengan formal yaml
Secara default namanya: docker-compose.yaml

for more: https://www.youtube.com/watch?v=6yJ4xHkuKNE&list=PL-CtdCApEFH-A7jBmdertzbeACuQWvQao&index=19

> `docker-compose up` -> akan langsung dibuat containernya <br>
> `docker-compose down` -> sekaligus menghapus container (hati-hati database), termasuk network <br>
> `docker-compose stop` -> tidak menghapus container <br>
> `docker-compose start` -> cuman menjalankan container yang sudah ada

`docker-compose` -> default file name of docker compose based on yaml, jika file name diganti, maka harus disesuaikan.

# DATA MANAGEMENT in DOCKER
Terkadang kita tidak bisa membuat aplikasi yang stateless, misalnya aplikasi basis data
pasti sifatnya statefull. Lalu jika misal container di hapus, tentu data yang sudah ada juga terhapus
Maka agar datanya tidak di hapus juga, bisa buat VOLUME untuk digunakan secara share antara 
aplikasi di docker dengan hardisk fisik saat ini. Untuk setup ini bisa gunakan perintah `docker volume`

Untuk lebih jelas cek video berikut:
https://www.youtube.com/watch?v=rXtKTnWow24&list=PL-CtdCApEFH-A7jBmdertzbeACuQWvQao&index=20

## Masuk ke Container
Jadi pada dasarnya continer itu berisikan OS linux, jadi apakah kita bisa mengaksesnya secara langsung
dan melakukan interaksi? tentu

> `docker exec -t -i {container_name} {path/}`

example:
> `docker exec -t -i redis /bin/bash`

Kalo sudah selesai tinggal pakai command exit
> `exit`