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

