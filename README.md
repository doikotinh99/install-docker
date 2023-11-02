
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
