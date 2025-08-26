### 安裝

#### 下載前端/後端程式碼(沒有程式碼可略過)

```
cd /home/docker/waytogolf
git clone https://github.com/kyria116/202508-waytogolf-backend.git backend/
git clone https://github.com/kyria116/202508-waytogolf-frontend.git frontend/
```

```
docker-compose up -d
```


#### 建立資料庫
```
CREATE DATABASE IF NOT EXISTS `waytogolf`; CREATE USER 'waytogolf'@'%' IDENTIFIED BY 'password'; GRANT ALL PRIVILEGES ON `waytogolf`.* TO 'waytogolf'@'%' WITH GRANT OPTION; FLUSH PRIVILEGES;
ALTER DATABASE `waytogolf` DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_unicode_ci;

```





#### 進入 docker php 環境

```
docker-compose exec php bash
```

#### 安裝 Laravel  (沒有程式碼才需要安裝)
```
docker-compose exec php bash
composer create-project laravel/laravel:^10 . # 安裝 Laravel 10

```

#### 設定 `.env`

```bash
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=waytogolf
DB_USERNAME=root
DB_PASSWORD=
```

#### 安裝 PHP 相關套件
```bash
docker-compose exec php bash

cd /app/backend/
composer install
```

#### 產生資料庫
```bash
docker-compose exec php bash
php artisan migrate

```


#### 開啟 storage 資料夾 www-data　寫入權，以利網頁有權限可以存取此資料夾檔案
```
cd /home/docker/waytogolf/backend
chgrp -R www-data storage/ # 若是 windows 則應該到 php container 下 chmod -R 757 storage/
```




## Gitlab CI/CD 

### 程式設定 gitlab-runner 的寫入權限

因為 Gitlab ci 是透過 gitlab-runner 去執行，避免執行時出現 Permission denied 的問題，請將以下資料夾權限開啟
```
sudo chown -R gitlab-runner:docker /home/docker/waytogolf/backend/
sudo chgrp -R www-data /home/docker/waytogolf/backend/storage/
sudo chmod g+w -R /home/docker/waytogolf/backend/storage/
```

## GCP 環境安裝
```
# 確認 ubuntu 版本為 我要的 20.x
username -a

# 安裝 docker
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" |   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo docker run hello-world

cd /home/
sudo mkdir docker
sudo usermod -aG docker shirley_lin

# 查看 docker group 裡面要有 shirley_lin
getent group
sudo chown root:docker docker
sudo chmod g+w docker/
cd /home/docker

# 放入 project code
git clone git@git.skymirror.com.tw:zscorp/docker-project/waytogolf.git
cd waytogolf/
git clone git@git.skymirror.com.tw:zscorp/waytogolf/waytogolf-backend.git backend/
git clone git@git.skymirror.com.tw:zscorp/waytogolf/waytogolf-frontend.git frontend/

docker-compose up -d


```
