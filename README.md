
# Install docker

## 1 Add Docker's official GPG key:
```
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg 
```

## 2 Add the repository to Apt sources:
``` 
echo \
"deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
"$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update 
```

## 3 Install the Docker packages.
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo docker run hello-world
```
# Image

## liệt kê các images

```
docker images
```
## tìm kiếm các images
- tra các images tại https://hub.docker.com/
```
docker search keyword
```

## tải image
``` 
docker pull ubuntu:22.04
```

## xoá image 
```
docker image rm ubuntu:22.04
```

# container

## chạy container

```
docker run -it ubuntu:22.04
```
- -i là tương tác với container được tạo ra
- -t là kết nối container đó với terminal

## kiểm tra danh sách các container

- kiểm tra các container đang chạy
```
docker ps
```

- kiểm tra cả các container đang dùng
```
docker ps -a
```
- chạy lại container đã dừng
```
docker start idContainer
```

- vào lại terminal của container

```
docker attach idContainer
```

- để thoát container mà không dừng thì ấn tổ hợp phím CTRL + P + Q

- để dừng container 
```
docker stop idContainer
```

- đặt tên cho container
```
docker run -it --name "name_container" -h hostname ubuntu:22.04
```

- xoá container

```
docker rm idContainer
```

- nếu container đang hoạt động mà vẫn muốn xoá thì thêm -f

```
docker rm -f idContainer
```

# Volume

## Ánh xạ thư mục máy host vào container
```
docker run -it -v /opt/lampp/htdocs/www/docker:/home/docker imageId
```

- /opt/lampp/htdocs/www/docker : đây là tư mục của máy host muốn ánh xạ
- /home/docker : đây là thư mục của container
- imageId : id của image muốn tại container

## chia sẻ dữ liệu giữa các container
```
docker run -it --volumes-from containerId imageId
```
- containerId là id của container hoặc name của container
- imageId là id của image muốn tạo ra container

## Tạo volume

- kiểm tra các volume đang có.
```
docker volume ls
```

- tạo volume
```
docker volume create nameVolume
```

- kiểm tra thông tin của volume
```
docker volume  inspect nameVolume
```

- xoá volume
```
docker volume rm www
```

## gán volume vào 1 container
```
docker run -it --mount source=nameVolume,target=/home/www imageID
```
- nameVolume là tên của volume
- /home/www là thư mục của container được volume ánh xạ vào
- imageID là id của image muốn tạo ra container
## tạo volume gắn với thư mục của máy host
```
docker volume create --opt device=/opt/lampp/htdocs/www --opt type=none --opt o=bind www
```
- /opt/lampp/htdocs/www dường dẫn file của máy host
- www là tên của volume

- gắn volume vừa tạo vào container
```
docker run -it -v www:/home/www imageId
```
# Network

## danh sách docker
```
docker network ls
```

## kiểm tra thông tin của network

```
docker network inspect bridge
```

## Ánh xạ cổng từ máy host vào container

```
docker run -it -p 8888:80 busybox
```

- 8888 là cổng của máy host (127.0.0.1:8888)
- 80 là cổng của container
- busybox là image chưa các câu lệnh của linux

## Tạo network
```
docker network create --driver bridge network1
```
- bridge là loại mạng
- network1 là tên mạng muốn tạo ra

## Xoá network
```
docker network rm network1
```
- network1 là tên mạng muốn xoá

## kết nối một container đang chạy vào network
```
docker network connect nameNetwork nameContainer
```

# Cài đặt php-fpm

```
docker pull php:8.1-fpm
docker run -d --name c-php -h php -v /opt/lampp/htdocs/www:/home/www --network network1 b2
docker exec -it c-php bash
```
# Cài đặt httpd

```

```

## config httpd.conf

```
LoadModule proxy_module modules/mod_proxy.so
LoadModule proxy_fcgi_module modules/mod_proxy_fcgi.so

AddHandler "proxy:fcgi://c-php:9000" .php
```